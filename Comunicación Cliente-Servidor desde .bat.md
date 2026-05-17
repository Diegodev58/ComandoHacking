Aquí tienes la guía para convertir tu `.bat` en un **agente cliente-servidor** que se comunica con un backend mediante **HTTP + JSON**. Como `.bat` puro no puede manejar JSON nativamente, usamos **`curl`** (para lo básico) y **PowerShell** (para lo complejo) dentro del script.

---

# 🌐 Comunicación Cliente-Servidor desde `.bat`

## 1. Enviar JSON simple al servidor (POST)

### Opción A: Con `curl` (Windows 10/11 tiene curl nativo)
```bat
@echo off
chcp 65001 >nul

:: Variables
set "server=http://192.168.1.100:8080"
set "pc=%computername%"
set "user=%username%"

:: Crear JSON en archivo temporal
(
echo {
echo   "pc": "%pc%",
echo   "user": "%user%",
echo   "evento": "login",
echo   "timestamp": "%date% %time%"
echo }
) > "%TEMP%\data.json"

:: Enviar POST con JSON
curl -X POST ^
  -H "Content-Type: application/json" ^
  -d "@%TEMP%\data.json" ^
  "%server%/api/reporte"

echo [✓] Datos enviados
pause
```

### Opción B: Con PowerShell (más robusto, escapa caracteres mejor)
```bat
@echo off
chcp 65001 >nul

set "server=http://192.168.1.100:8080"

powershell -Command "$body=@{pc='%computername%';user='%username%';evento='login';timestamp='%date% %time%'} | ConvertTo-Json; Invoke-RestMethod -Uri '%server%/api/reporte' -Method POST -ContentType 'application/json' -Body $body"

echo [✓] JSON enviado vía PowerShell
```

---

## 2. Subir un archivo (ZIP de auditoría) al servidor

### Multipart con `curl` (subir archivo binario)
```bat
@echo off
set "server=http://192.168.1.100:8080"
set "archivo=C:\Audit\reporte.zip"

curl -X POST ^
  -F "file=@%archivo%" ^
  -F "pc=%computername%" ^
  -F "user=%username%" ^
  "%server%/api/upload"

echo [✓] Archivo subido
```

### Subir con PowerShell (alternativa si curl falla)
```bat
@echo off
set "server=http://192.168.1.100:8080"
set "archivo=C:\Audit\reporte.zip"

powershell -Command "$file='%archivo%';$pc='%computername%';$user='%username%';$uri='%server%/api/upload';$form=@{pc=$pc;user=$user;file=Get-Item $file};Invoke-RestMethod -Uri $uri -Method POST -Form $form"
```

---

## 3. Recibir órdenes del servidor (GET + parsear respuesta)

Tu PC cliente consulta al servidor cada X minutos: *"¿Tienes alguna tarea para mí?"*

### Pedir comando al servidor
```bat
@echo off
set "server=http://192.168.1.100:8080"
set "pc=%computername%"

:: Consultar tarea pendiente
curl -s "%server%/api/tarea?pc=%pc%" > "%TEMP%\respuesta.json"

:: Mostrar respuesta cruda
type "%TEMP%\respuesta.json"
```

### Parsear JSON con PowerShell y actuar
```bat
@echo off
set "server=http://192.168.1.100:8080"
set "pc=%computername%"

:: Obtener respuesta y extraer campo "accion"
for /f "delims=" %%A in ('powershell -Command "$r=Invoke-RestMethod -Uri '%server%/api/tarea?pc=%pc%';$r.accion"') do set "accion=%%A"

echo [i] Servidor ordena: %accion%

if "%accion%"=="auditar" goto hacer_auditoria
if "%accion%"=="reiniciar" shutdown /r /t 0
if "%accion%"=="bloquear" rundll32.exe user32.dll,LockWorkStation
if "%accion%"=="captura" goto tomar_captura
if "%accion%"=="nada" echo Sin tareas pendientes.

goto fin

:hacer_auditoria
echo [+] Ejecutando auditoria...
systeminfo > "%TEMP%\audit.txt"
curl -F "file=@%TEMP%\audit.txt" -F "pc=%pc%" "%server%/api/upload"
goto fin

:tomar_captura
powershell -Command "Add-Type -AssemblyName System.Windows.Forms;Add-Type -AssemblyName System.Drawing;$s=[System.Windows.Forms.Screen]::PrimaryScreen.Bounds;$b=New-Object System.Drawing.Bitmap $s.Width,$s.Height;$g=[System.Drawing.Graphics]::FromImage($b);$g.CopyFromScreen($s.Location,[System.Drawing.Point]::Empty,$s.Size);$b.Save('%TEMP%\cap.png');$g.Dispose();$b.Dispose()"
curl -F "file=@%TEMP%\cap.png" -F "pc=%pc%" "%server%/api/upload"

:fin
echo [✓] Ciclo completado.
```

---

## 4. Sistema completo: Agente que se registra y reporta

Este es un **script cliente** que se ejecuta en cada PC y habla con tu servidor:

```bat
@echo off
setlocal enabledelayedexpansion
chcp 65001 >nul

:: ================= CONFIGURACIÓN =================
set "server=http://TU-SERVIDOR:8080"
set "pc=%computername%"
set "user=%username%"
set "intervalo=60"

:loop
echo [%time%] Consultando servidor...

:: -------- 1. REGISTRAR ESTADO (heartbeat) --------
powershell -Command "$body=@{pc='%pc%';user='%user%';estado='online';hora='%time%';fecha='%date%'} | ConvertTo-Json; try { Invoke-RestMethod -Uri '%server%/api/heartbeat' -Method POST -ContentType 'application/json' -Body $body -TimeoutSec 10 } catch { 'Error envio' }" >nul 2>&1

:: -------- 2. PEDIR TAREA --------
for /f "delims=" %%T in ('powershell -Command "try { $r=Invoke-RestMethod -Uri '%server%/api/tarea?pc=%pc%' -TimeoutSec 10; $r.accion } catch { 'nada' }"') do set "tarea=%%T"

echo [i] Tarea recibida: %tarea%

:: -------- 3. EJECUTAR TAREA --------
if "%tarea%"=="auditoria" goto do_auditoria
if "%tarea%"=="captura" goto do_captura
if "%tarea%"=="procesos" goto do_procesos
if "%tarea%"=="cerrar_app" goto do_cerrar_app
if "%tarea%"=="mensaje" goto do_mensaje
if "%tarea%"=="bloquear_pc" goto do_bloquear
if "%tarea%"=="subir_archivo" goto do_subir
if "%tarea%"=="nada" goto esperar

:: Tarea desconocida
call :reportar "error" "Tarea desconocida: %tarea%"
goto esperar

:: -------- TAREAS IMPLEMENTADAS --------

:do_auditoria
echo [+] Ejecutando auditoria...
set "out=%TEMP%\audit_%pc%.txt"
(
    echo === AUDITORIA %pc% ===
    echo Fecha: %date% %time%
    echo.
    echo --- SISTEMA ---
) > "%out%"
systeminfo >> "%out%" 2>&1
ipconfig /all >> "%out%" 2>&1
tasklist /v >> "%out%" 2>&1
curl -F "file=@%out%" -F "pc=%pc%" -F "tipo=auditoria" "%server%/api/upload" >nul 2>&1
call :reportar "ok" "Auditoria completada y subida"
goto esperar

:do_captura
echo [+] Capturando pantalla...
set "img=%TEMP%\cap_%pc%_%time::=-%.png"
powershell -Command "Add-Type -AssemblyName System.Windows.Forms;Add-Type -AssemblyName System.Drawing;$s=[System.Windows.Forms.Screen]::PrimaryScreen.Bounds;$b=New-Object System.Drawing.Bitmap $s.Width,$s.Height;$g=[System.Drawing.Graphics]::FromImage($b);$g.CopyFromScreen($s.Location,[System.Drawing.Point]::Empty,$s.Size);$b.Save('%img%');$g.Dispose();$b.Dispose()" >nul 2>&1
if exist "%img%" (
    curl -F "file=@%img%" -F "pc=%pc%" -F "tipo=captura" "%server%/api/upload" >nul 2>&1
    call :reportar "ok" "Captura subida"
)
goto esperar

:do_procesos
echo [+] Enviando lista de procesos...
tasklist /v /fo csv > "%TEMP%\procesos.csv"
curl -F "file=@%TEMP%\procesos.csv" -F "pc=%pc%" -F "tipo=procesos" "%server%/api/upload" >nul 2>&1
call :reportar "ok" "Procesos enviados"
goto esperar

:do_cerrar_app
for /f "delims=" %%A in ('powershell -Command "try { $r=Invoke-RestMethod -Uri '%server%/api/tarea?pc=%pc%'; $r.parametro } catch { '' }"') do set "app=%%A"
if not "%app%"=="" (
    taskkill /im "%app%" /f /t >nul 2>&1
    call :reportar "ok" "Aplicacion cerrada: %app%"
) else (
    call :reportar "error" "No se especifico app a cerrar"
)
goto esperar

:do_mensaje
for /f "delims=" %%M in ('powershell -Command "try { $r=Invoke-RestMethod -Uri '%server%/api/tarea?pc=%pc%'; $r.parametro } catch { '' }"') do set "msg=%%M"
if not "%msg%"=="" (
    msg * "%msg%"
    call :reportar "ok" "Mensaje enviado al usuario"
)
goto esperar

:do_bloquear
rundll32.exe user32.dll,LockWorkStation
call :reportar "ok" "PC bloqueada"
goto esperar

:do_subir
for /f "delims=" %%F in ('powershell -Command "try { $r=Invoke-RestMethod -Uri '%server%/api/tarea?pc=%pc%'; $r.parametro } catch { '' }"') do set "ruta=%%F"
if exist "%ruta%" (
    curl -F "file=@%ruta%" -F "pc=%pc%" -F "tipo=archivo" "%server%/api/upload" >nul 2>&1
    call :reportar "ok" "Archivo subido: %ruta%"
) else (
    call :reportar "error" "Archivo no existe: %ruta%"
)
goto esperar

:: -------- FUNCIONES --------

:reportar
set "estado=%~1"
set "mensaje=%~2"
powershell -Command "$body=@{pc='%pc%';estado='%estado%';mensaje='%mensaje%';hora='%time%'} | ConvertTo-Json; Invoke-RestMethod -Uri '%server%/api/resultado' -Method POST -ContentType 'application/json' -Body $body -TimeoutSec 10" >nul 2>&1
goto :eof

:esperar
echo [%time%] Esperando %intervalo% segundos...
timeout /t %intervalo% /nobreak >nul
goto loop
```

---

## 5. Estructura del Backend (qué necesitas en el servidor)

Tu servidor debe exponer estos endpoints mínimos:

| Método | Endpoint | Qué hace |
|--------|----------|----------|
| `POST` | `/api/heartbeat` | Recibe: `{pc, user, estado, hora}`. Marca la PC como online. |
| `GET` | `/api/tarea?pc=PC01` | Devuelve JSON: `{"accion":"captura","parametro":""}` o `{"accion":"nada"}`. |
| `POST` | `/api/resultado` | Recibe: `{pc, estado, mensaje, hora}`. Log de lo que hizo la PC. |
| `POST` | `/api/upload` | Recibe `multipart/form-data` con archivo + metadatos (pc, tipo). |

### Ejemplo de respuesta JSON del servidor (para `/api/tarea`):
```json
{
  "accion": "auditoria",
  "parametro": ""
}
```

```json
{
  "accion": "cerrar_app",
  "parametro": "steam.exe"
}
```

```json
{
  "accion": "mensaje",
  "parametro": "Reinicio programado en 5 minutos"
}
```

---

## 6. Tabla rápida: Comandos de red en `.bat`

| Necesitas... | Comando |
|-------------|---------|
| Enviar JSON por POST | `curl -X POST -H "Content-Type: application/json" -d "@archivo.json" URL` |
| Enviar JSON por POST (PowerShell) | `Invoke-RestMethod -Uri URL -Method POST -Body $json -ContentType "application/json"` |
| Subir archivo | `curl -F "file=@ruta" -F "campo=valor" URL` |
| Descargar archivo | `curl -o "destino" "URL"` |
| Consultar API (GET) | `curl -s "URL"` |
| Parsear JSON en `.bat` | PowerShell: `$r=Invoke-RestMethod -Uri URL; $r.campo` |
| Escapar comillas en JSON | Usa `^` antes de `"` en curl, o PowerShell directo |
| Headers personalizados | `curl -H "Authorization: Bearer TOKEN" URL` |
| Timeout en curl | `curl --connect-timeout 10 -m 30 URL` |
| Timeout en PowerShell | `Invoke-RestMethod ... -TimeoutSec 10` |
| Ignorar certificado SSL inválido | `curl -k URL` o PowerShell: `-SkipCertificateCheck` |

---

## 7. Tips para producción

| Problema | Solución |
|----------|----------|
| El PC no tiene curl | Usa PowerShell (`Invoke-RestMethod`) que siempre existe en Win 7+ |
| JSON con tildes o ñ se rompe | Guarda el `.bat` en codificación **UTF-8 con BOM** o usa PowerShell |
| El servidor está en internet | Usa `https` + token en header: `-H "X-Token: ABC123"` |
| Necesitas que el agente inicie con Windows | Crea tarea programada o ponlo en `shell:startup` |
| Necesitas ocultar la ventana del agente | Usa `powershell -WindowStyle Hidden` o compila a `.exe` con BatToExe |
| El servidor devuelve arrays `[...]` | PowerShell: `$r[0].accion` para el primer elemento |

---

## 8. Mini-panel de control en el servidor (concepto)

Desde tu servidor, puedes tener una web o script que envíe órdenes. Ejemplo mínimo en Python/Flask:

```python
from flask import Flask, request, jsonify
app = Flask(__name__)

tareas = {}  # pc -> {"accion": "...", "parametro": "..."}

@app.route('/api/tarea')
def tarea():
    pc = request.args.get('pc')
    t = tareas.get(pc, {"accion":"nada"})
    tareas[pc] = {"accion":"nada"}  # Limpiar después de entregar
    return jsonify(t)

@app.route('/api/upload', methods=['POST'])
def upload():
    file = request.files['file']
    pc = request.form['pc']
    file.save(f"uploads/{pc}_{file.filename}")
    return "OK"

@app.route('/orden/<pc>/<accion>')
def orden(pc, accion):
    tareas[pc] = {"accion": accion, "parametro": request.args.get('p','')}
    return f"Orden {accion} enviada a {pc}"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```

Con esto, desde el navegador del técnico:
```
http://servidor:8080/orden/PC01/captura
http://servidor:8080/orden/PC01/cerrar_app?p=steam.exe
http://servidor:8080/orden/PC01/mensaje?p=Hola+usuario
```

---

