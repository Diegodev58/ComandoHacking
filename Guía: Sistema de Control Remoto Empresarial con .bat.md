¡Entendido! Te voy a dar una guía completa para crear un **sistema de control y soporte remoto empresarial** con `.bat`, enfocado en:

- ✅ Saber qué programas están abiertos
- ✅ Controlar aplicaciones y navegador remotamente
- ✅ Instalar software sin ir físicamente
- ✅ Bloquear instalaciones no autorizadas
- ✅ Capturas de pantalla para soporte
- ✅ Logs de actividad (ventanas, procesos, comandos)
- ⚠️ **Advertencias legales claras** sobre monitoreo

---

# 🏢 Guía: Sistema de Control Remoto Empresarial con `.bat`

> **⚠️ IMPORTANTE LEGAL Y ÉTICO:**
> Todo monitoreo de PCs de empleados requiere:
> 1. **Consentimiento explícito** por escrito (política interna visible)
> 2. **Aviso previo** al usuario: "Este equipo es monitoreado por el departamento de TI"
> 3. **Cumplimiento** de leyes laborales y de privacidad (LOPD/GDPR según país)
> 4. **Finalidad legítima**: soporte técnico, seguridad, prevención de fugas de datos
> 5. **NUNCA** usar para robar contraseñas personales, banca, o espionaje personal

---

## 📋 ÍNDICE

1. [Módulo 1: Ver programas abiertos en tiempo real](#1-ver-programas-abiertos)
2. [Módulo 2: Controlar navegador y aplicaciones](#2-controlar-navegador-y-apps)
3. [Módulo 3: Instalaciones remotas silenciosas](#3-instalaciones-remotas)
4. [Módulo 4: Bloquear instalaciones no autorizadas](#4-bloquear-instalaciones)
5. [Módulo 5: Capturas de pantalla para soporte](#5-capturas-de-pantalla)
6. [Módulo 6: Logs de actividad del usuario](#6-logs-de-actividad)
7. [Script Maestro: Panel de Control Remoto](#7-script-maestro)

---

## 1. Ver Programas Abiertos

### 🔹 Listar procesos activos con detalle
```bat
@echo off
chcp 65001 >nul
title Programas Abiertos - %computername%
color 0B

set "carpeta=C:\ControlRemoto\%computername%_%date:/=-%"
mkdir "%carpeta%" 2>nul

echo [+] Escaneando programas abiertos...

:: Lista completa con usuario y memoria
tasklist /v /fo csv > "%carpeta%\procesos_completos.csv"

:: Solo programas con ventana visible (excluye procesos del sistema)
tasklist /v /fi "STATUS eq RUNNING" | findstr /V "System" | findstr /V "svchost" > "%carpeta%\programas_usuario.txt"

:: Guardar también en formato legible
(
    echo ============================================
    echo    PROGRAMAS ABIERTOS - %date% %time%
    echo    Equipo: %computername%
    echo    Usuario: %username%
    echo ============================================
    echo.
) > "%carpeta%\reporte_programas.txt"

tasklist /v /fi "SESSIONNAME eq Console" >> "%carpeta%\reporte_programas.txt"

echo [✓] Reporte guardado en: %carpeta%
pause
```

### 🔹 Detectar navegadores abiertos y páginas (por título de ventana)
```bat
@echo off
chcp 65001 >nul

set "reporte=C:\ControlRemoto\navegadores_abiertos.txt"

(
    echo === NAVEGADORES DETECTADOS ===
    echo Fecha: %date% %time%
    echo.
) > "%reporte%"

:: Buscar Chrome, Edge, Firefox, Opera
tasklist /fi "IMAGENAME eq chrome.exe" /fo table >> "%reporte%" 2>nul
tasklist /fi "IMAGENAME eq msedge.exe" /fo table >> "%reporte%" 2>nul
tasklist /fi "IMAGENAME eq firefox.exe" /fo table >> "%reporte%" 2>nul
tasklist /fi "IMAGENAME eq opera.exe" /fo table >> "%reporte%" 2>nul

echo Reporte de navegadores: %reporte%
type "%reporte%"
pause
```

---

## 2. Controlar Navegador y Aplicaciones

### 🔹 Abrir navegador con URL específica (control remoto)
```bat
@echo off
chcp 65001 >nul
title Control Remoto - Navegador

:: Abrir Chrome con intranet corporativa
start chrome.exe --start-fullscreen "https://portal.empresa.com"

:: O abrir Edge en modo kiosk (bloqueado, solo esa página)
start msedge.exe --kiosk "https://soporte.empresa.com" --edge-kiosk-type=fullscreen

echo Navegador controlado abierto.
pause
```

### 🔹 Cerrar aplicaciones específicas remotamente
```bat
@echo off
chcp 65001 >nul
title Cerrar Aplicaciones No Autorizadas

echo === CONTROL DE APLICACIONES ===
echo.

:: Cerrar juegos conocidos
taskkill /im steam.exe /f /t >nul 2>&1 && echo [X] Steam cerrado
taskkill /im epicgameslauncher.exe /f /t >nul 2>&1 && echo [X] Epic Games cerrado
taskkill /im battle.net.exe /f /t >nul 2>&1 && echo [X] Battle.net cerrado

:: Cerrar software de mensajería no corporativo
taskkill /im telegram.exe /f /t >nul 2>&1 && echo [X] Telegram cerrado
taskkill /im whatsapp.exe /f /t >nul 2>&1 && echo [X] WhatsApp cerrado

:: Cerrar navegadores no corporativos (si se requiere)
:: taskkill /im opera.exe /f /t >nul 2>&1

echo.
echo Aplicaciones no autorizadas cerradas.
pause
```

### 🔹 Abrir programa específico para el usuario
```bat
@echo off
chcp 65001 >nul

:: Abrir Excel con archivo corporativo
start excel.exe "C:\Empresa\Plantillas\reporte_mensual.xlsx"

:: Abrar calculadora (para soporte)
start calc.exe

:: Abrir Panel de control
start control.exe

:: Abrir configuración de red
start ncpa.cpl

echo Programas abiertos para el usuario.
pause
```

---

## 3. Instalaciones Remotas Silenciosas

### 🔹 Instalar .MSI sin interacción del usuario
```bat
@echo off
chcp 65001 >nul
title Instalacion Remota Silenciosa
color 0A

set "instalador=C:\Deploy\software.msi"
set "log=C:\Deploy\instalacion.log"

echo [+] Instalando software remotamente...
echo [i] No cierre esta ventana...

:: /quiet = sin interfaz | /norestart = no reinicia | /log = registro
msiexec /i "%instalador%" /quiet /norestart /log "%log%"

if %errorlevel%==0 (
    echo [✓] Instalacion exitosa.
) else (
    echo [X] Error. Ver log: %log%
)

pause
```

### 🔹 Instalar .EXE silencioso (parámetros comunes)
```bat
@echo off
chcp 65001 >nul

:: Google Chrome silencioso
"C:\Deploy\ChromeSetup.exe" /silent /install

:: Firefox silencioso
"C:\Deploy\FirefoxSetup.exe" /S

:: 7-Zip silencioso
"C:\Deploy\7z.exe" /S

:: Notepad++ silencioso
"C:\Deploy\npp.exe" /S

:: Adobe Reader (ejemplo)
:: "C:\Deploy\AcroRead.exe" /sAll /rs /msi EULA_ACCEPT=YES

echo Instalacion silenciosa completada.
pause
```

### 🔹 Script de despliegue masivo (ejecutar en múltiples PCs)
```bat
@echo off
chcp 65001 >nul
title Despliegue Masivo Empresarial

:: Lista de PCs (o leer de archivo)
for %%E in (PC-VENTAS1 PC-VENTAS2 PC-RRHH1 PC-CONTABILIDAD) do (
    echo [+] Conectando a %%E...
    
    :: Copiar instalador a PC remota
    xcopy "C:\Deploy\software.msi" "\\%%E\C$\Temp\" /Y /Q >nul 2>&1
    
    if %errorlevel%==0 (
        :: Ejecutar instalacion remota via PSExec (requiere Sysinternals)
        psexec \\%%E -s -d msiexec /i "C:\Temp\software.msi" /quiet /norestart
        echo [✓] %%E: Instalacion iniciada.
    ) else (
        echo [X] %%E: No se pudo conectar.
    )
)

echo.
echo Despliegue finalizado.
pause
```

> **Requiere:** PsExec de Sysinternals y credenciales de administrador de dominio/red.

---

## 4. Bloquear Instalaciones No Autorizadas

### 🔹 Activar restricción de instalador (política local)
```bat
@echo off
chcp 65001 >nul
title Bloqueo de Instalaciones No Autorizadas
color 0C

echo [+] Aplicando politicas de restriccion...

:: Deshabilitar Windows Installer para usuarios estandar
reg add "HKLM\Software\Policies\Microsoft\Windows\Installer" /v DisableMSI /t REG_DWORD /d 2 /f >nul 2>&1

:: Deshabilitar ejecucion de archivos desde %TEMP% (común en malware)
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v DisallowRun /t REG_DWORD /d 1 /f >nul 2>&1

:: Crear lista de programas bloqueados (ejemplo)
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\DisallowRun" /v 1 /t REG_SZ /d "utorrent.exe" /f >nul 2>&1
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\DisallowRun" /v 2 /t REG_SZ /d "steam.exe" /f >nul 2>&1

:: Deshabilitar instalacion de apps de Windows Store (opcional)
reg add "HKLM\Software\Policies\Microsoft\WindowsStore" /v RequirePrivateStoreOnly /t REG_DWORD /d 1 /f >nul 2>&1

echo [✓] Instalaciones bloqueadas para usuarios estandar.
echo [i] Solo administradores pueden instalar ahora.
pause
```

### 🔹 Script para "quitar permisos" y restaurar (cuando el técnico necesita instalar)
```bat
@echo off
chcp 65001 >nul
title Modo Instalacion - Tecnico

echo === MODO INSTALACION PARA TECNICO ===
echo.

:: Quitar restriccion temporalmente
reg delete "HKLM\Software\Policies\Microsoft\Windows\Installer" /v DisableMSI /f >nul 2>&1
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v DisallowRun /f >nul 2>&1

echo [✓] Modo instalacion ACTIVADO.
echo [i] El tecnico puede instalar software ahora.
echo.
pause

:: Al cerrar, volver a bloquear
echo [+] Restaurando bloqueos...
reg add "HKLM\Software\Policies\Microsoft\Windows\Installer" /v DisableMSI /t REG_DWORD /d 2 /f >nul 2>&1
echo [✓] Bloqueos restaurados.
pause
```

---

## 5. Capturas de Pantalla para Soporte

### 🔹 Tomar captura de pantalla y guardar (PowerShell desde .bat)
```bat
@echo off
chcp 65001 >nul
title Captura de Pantalla Remota

set "carpeta=C:\ControlRemoto\%computername%\Capturas"
mkdir "%carpeta%" 2>nul

set "archivo=%carpeta%\captura_%time::=-%.png"

echo [+] Tomando captura de pantalla...

powershell -ExecutionPolicy Bypass -Command "Add-Type -AssemblyName System.Windows.Forms; Add-Type -AssemblyName System.Drawing; $screen = [System.Windows.Forms.Screen]::PrimaryScreen.Bounds; $bitmap = New-Object System.Drawing.Bitmap $screen.Width, $screen.Height; $graphics = [System.Drawing.Graphics]::FromImage($bitmap); $graphics.CopyFromScreen($screen.Location, [System.Drawing.Point]::Empty, $screen.Size); $bitmap.Save('%archivo%'); $graphics.Dispose(); $bitmap.Dispose()"

if exist "%archivo%" (
    echo [✓] Captura guardada: %archivo%
) else (
    echo [X] Error al capturar.
)

pause
```

### 🔹 Captura automática cada X segundos (modo supervisión visible)
```bat
@echo off
chcp 65001 >nul
title Modo Supervision - Capturas Periodicas
color 0E

set "carpeta=C:\ControlRemoto\%computername%\Timelapse"
mkdir "%carpeta%" 2>nul

echo ============================================
echo    MODO SUPERVISION ACTIVO
echo ============================================
echo.
echo [AVISO] Este equipo esta siendo monitoreado
echo por el Departamento de TI para soporte.
echo.
echo Presiona Ctrl+C para detener.
echo ============================================

:loop
set "archivo=%carpeta%\cap_%date:/=-%_%time::=-%.png"
powershell -ExecutionPolicy Bypass -Command "Add-Type -AssemblyName System.Windows.Forms; Add-Type -AssemblyName System.Drawing; $s=[System.Windows.Forms.Screen]::PrimaryScreen.Bounds; $b=New-Object System.Drawing.Bitmap $s.Width,$s.Height; $g=[System.Drawing.Graphics]::FromImage($b); $g.CopyFromScreen($s.Location,[System.Drawing.Point]::Empty,$s.Size); $b.Save('%archivo%'); $g.Dispose();$b.Dispose()" >nul 2>&1

echo [+] Captura: %archivo%
timeout /t 30 /nobreak >nul
goto loop
```

---

## 6. Logs de Actividad del Usuario

### 🔹 Registrar ventana activa y programas ejecutados (visible/auditable)
```bat
@echo off
chcp 65001 >nul
title Log de Actividad - Control Empresarial
color 0A

set "carpeta=C:\ControlRemoto\%computername%\Logs"
mkdir "%carpeta%" 2>nul
set "log=%carpeta%\actividad_%date:/=-%.txt"

(
    echo ============================================
    echo    LOG DE ACTIVIDAD - EMPRESA
    echo    Equipo: %computername%
    echo    Usuario: %username%
    echo    Inicio: %date% %time%
    echo    AVISO: Monitoreo autorizado por TI
    echo ============================================
    echo.
) > "%log%"

echo [+] Iniciando monitoreo de actividad...
echo [i] Registrando cada 10 segundos. Ctrl+C para detener.

:monitoreo
:: Fecha y hora actual
set "ahora=%date% %time%"

:: Obtener título de ventana activa via PowerShell
for /f "delims=" %%W in ('powershell -Command "Add-Type @'
using System;
using System.Runtime.InteropServices;
public class WinAPI {
    [DllImport("user32.dll")]
    public static extern IntPtr GetForegroundWindow();
    [DllImport("user32.dll")]
    public static extern int GetWindowText(IntPtr hWnd, System.Text.StringBuilder text, int count);
}
'@; $hwnd=[WinAPI]::GetForegroundWindow(); $text=New-Object System.Text.StringBuilder 256; [WinAPI]::GetWindowText($hwnd,$text,256); $text.ToString()"') do set "ventana=%%W"

:: Obtener proceso de la ventana activa
for /f "tokens=1 delims= " %%P in ('powershell -Command "Get-Process | Where-Object {$_.MainWindowHandle -eq (Get-ForegroundWindow)} | Select-Object -ExpandProperty ProcessName"') do set "proceso=%%P"

:: Escribir en log
echo [%ahora%] Ventana: %ventana% | Proceso: %proceso% >> "%log%"

:: Capturar también lista de procesos nuevos cada minuto
echo [%ahora%] --- Snapshot de procesos --- >> "%log%"
tasklist /v /fi "SESSIONNAME eq Console" >> "%log%"

timeout /t 10 /nobreak >nul
goto monitoreo
```

### 🔹 Log de archivos abiertos/recientes
```bat
@echo off
chcp 65001 >nul

set "log=C:\ControlRemoto\%computername%\archivos_recientes.txt"

(
    echo === ARCHIVOS RECIENTES DEL USUARIO ===
    echo Fecha: %date% %time%
    echo.
) > "%log%"

echo --- Carpeta Recent --- >> "%log%"
dir "%USERPROFILE%\Recent" /s /b >> "%log%" 2>nul

echo. >> "%log%"
echo --- Descargas --- >> "%log%"
dir "%USERPROFILE%\Downloads" /s /b >> "%log%" 2>nul

echo. >> "%log%"
echo --- Documentos --- >> "%log%"
dir "%USERPROFILE%\Documents" /s /b >> "%log%" 2>nul

echo Log guardado: %log%
pause
```

---

## 7. Script Maestro: Panel de Control Remoto

Este es el script **definitivo** que une todo para el técnico de TI:

```bat
@echo off
setlocal enabledelayedexpansion
chcp 65001 >nul
title PANEL DE CONTROL REMOTO - TI EMPRESARIAL
color 0B

:: Crear estructura
set "base=C:\ControlRemoto\%computername%_%date:/=-%_%time::=-%"
mkdir "%base%\Capturas" 2>nul
mkdir "%base%\Logs" 2>nul
mkdir "%base%\Programas" 2>nul

:menu
cls
echo ============================================
echo    PANEL DE CONTROL REMOTO - TI
echo    Equipo: %computername%
echo    Usuario: %username%
echo    Fecha: %date%
echo ============================================
echo.
echo  [1] Ver programas abiertos ahora
echo  [2] Cerrar aplicaciones no autorizadas
echo  [3] Abrir navegador (soporte/control)
echo  [4] Tomar captura de pantalla
echo  [5] Iniciar log de actividad (ventanas)
echo  [6] Instalar software silencioso
echo  [7] Bloquear instalaciones (modo usuario)
echo  [8] Desbloquear instalaciones (modo tecnico)
echo  [9] Ver archivos recientes del usuario
echo  [10] Reporte completo de sistema
echo  [11] Salir
echo.
echo ============================================
set /p op="Seleccione opcion: "

if "%op%"=="1" goto ver_programas
if "%op%"=="2" goto cerrar_apps
if "%op%"=="3" goto control_navegador
if "%op%"=="4" goto captura
if "%op%"=="5" goto log_actividad
if "%op%"=="6" goto instalar
if "%op%"=="7" goto bloquear
if "%op%"=="8" goto desbloquear
if "%op%"=="9" goto archivos_recientes
if "%op%"=="10" goto reporte_completo
if "%op%"=="11" exit
goto menu

:ver_programas
cls
echo === PROGRAMAS ABIERTOS ===
tasklist /v /fi "SESSIONNAME eq Console" | more
echo.
echo Guardando en: %base%\Programas\
tasklist /v /fo csv > "%base%\Programas\procesos.csv"
pause
goto menu

:cerrar_apps
cls
echo === CERRANDO APLICACIONES NO AUTORIZADAS ===
taskkill /im steam.exe /f /t >nul 2>&1 && echo [X] Steam
taskkill /im epicgameslauncher.exe /f /t >nul 2>&1 && echo [X] Epic
taskkill /im telegram.exe /f /t >nul 2>&1 && echo [X] Telegram
taskkill /im utorrent.exe /f /t >nul 2>&1 && echo [X] uTorrent
echo.
echo Aplicaciones no corporativas cerradas.
pause
goto menu

:control_navegador
cls
echo === CONTROL DE NAVEGADOR ===
echo [1] Abrir portal corporativo
echo [2] Abrir soporte remoto
echo [3] Cerrar todos los navegadores
set /p nav="Opcion: "
if "%nav%"=="1" start chrome.exe "https://portal.empresa.com"
if "%nav%"=="2" start msedge.exe "https://soporte.empresa.com/remote"
if "%nav%"=="3" (
    taskkill /im chrome.exe /f /t >nul 2>&1
    taskkill /im msedge.exe /f /t >nul 2>&1
    taskkill /im firefox.exe /f /t >nul 2>&1
    echo Navegadores cerrados.
)
pause
goto menu

:captura
cls
echo [+] Capturando pantalla...
set "img=%base%\Capturas\cap_%time::=-%.png"
powershell -ExecutionPolicy Bypass -Command "Add-Type -AssemblyName System.Windows.Forms; Add-Type -AssemblyName System.Drawing; $s=[System.Windows.Forms.Screen]::PrimaryScreen.Bounds; $b=New-Object System.Drawing.Bitmap $s.Width,$s.Height; $g=[System.Drawing.Graphics]::FromImage($b); $g.CopyFromScreen($s.Location,[System.Drawing.Point]::Empty,$s.Size); $b.Save('%img%'); $g.Dispose();$b.Dispose()"
echo [✓] Guardada: %img%
pause
goto menu

:log_actividad
cls
echo === LOG DE ACTIVIDAD ===
echo [AVISO] Se registraran ventanas activas cada 10 seg.
echo Presiona Ctrl+C para detener.
echo.
set "logfile=%base%\Logs\actividad.txt"
echo Inicio log: %date% %time% > "%logfile%"
:logloop
for /f "delims=" %%W in ('powershell -Command "Add-Type @'
using System;using System.Runtime.InteropServices;public class W{[DllImport(\"user32.dll\")]public static extern IntPtr GetForegroundWindow();[DllImport(\"user32.dll\")]public static extern int GetWindowText(IntPtr hWnd,System.Text.StringBuilder t,int c);}'@;$h=[W]::GetForegroundWindow();$t=New-Object System.Text.StringBuilder 256;[W]::GetWindowText($h,$t,256);$t.ToString()"') do (
    echo [%date% %time%] Ventana activa: %%W >> "%logfile%"
)
timeout /t 10 /nobreak >nul
goto logloop

:instalar
cls
echo === INSTALACION SILENCIOSA ===
set /p msi="Ruta del .MSI: "
msiexec /i "%msi%" /quiet /norestart /log "%base%\instalacion.log"
echo [✓] Instalacion iniciada. Ver log: %base%\instalacion.log
pause
goto menu

:bloquear
cls
echo [+] Bloqueando instalaciones...
reg add "HKLM\Software\Policies\Microsoft\Windows\Installer" /v DisableMSI /t REG_DWORD /d 2 /f >nul
echo [✓] Usuarios estandar NO pueden instalar.
pause
goto menu

:desbloquear
cls
echo [+] Desbloqueando para tecnico...
reg delete "HKLM\Software\Policies\Microsoft\Windows\Installer" /v DisableMSI /f >nul 2>&1
echo [✓] Modo tecnico activo. Puedes instalar ahora.
echo [i] Recuerda volver a bloquear despues.
pause
goto menu

:archivos_recientes
cls
echo === ARCHIVOS RECIENTES ===
echo --- Recent ---
dir "%USERPROFILE%\Recent" /b /s 2>nul
echo.
echo --- Descargas ---
dir "%USERPROFILE%\Downloads" /b 2>nul
pause
goto menu

:reporte_completo
cls
echo [+] Generando reporte completo...
systeminfo > "%base%\systeminfo.txt"
ipconfig /all > "%base%\ipconfig.txt"
wmic cpu get Name /format:list > "%base%\cpu.txt"
wmic memorychip get Capacity /format:list > "%base%\ram.txt"
wmic diskdrive get Model,Size /format:list > "%base%\discos.txt"
wmic product get Name,Version /format:csv > "%base%\software.csv"
net user > "%base%\usuarios.txt"
echo [✓] Reporte completo en: %base%
pause
goto menu
```

---

## ⚖️ Checklist Legal para Implementar en tu Empresa

Antes de usar estos scripts, asegúrate de:

| Requisito | ¿Cumplido? |
|-----------|------------|
| ✅ Política escrita de uso aceptable firmada por el empleado | [ ] |
| ✅ Aviso visible en pantalla: "Este equipo es monitoreado por TI" | [ ] |
| ✅ Finalidad legítima documentada (soporte, seguridad, cumplimiento) | [ ] |
| ✅ Acceso restringido solo a personal de TI autorizado | [ ] |
| ✅ Los datos se almacenan en servidor seguro, no local | [ ] |
| ✅ Tiempo de retención definido (ej: 30 días) | [ ] |
| ✅ Procedimiento para que el empleado solicite sus datos | [ ] |
| ✅ No monitorear cuentas personales/banca del empleado | [ ] |

---

## 🚀 Próximos pasos recomendados

1. **Guarda el Panel de Control** como `PanelTI.bat`
2. **Ejecútalo como Administrador** (clic derecho → Ejecutar como administrador)
3. **Para red:** Usa PsExec o PowerShell Remoting para ejecutar en múltiples PCs
4. **Automatiza:** Programa con Task Scheduler capturas/logs diarios
5. **Centraliza:** Guarda todo en `\\Servidor\ControlRemoto\%computername%\`

