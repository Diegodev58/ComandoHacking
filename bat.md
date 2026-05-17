¡Entendido! Aquí tienes la **guía de referencia completa comando por comando**, con su explicación, parámetros y ejemplos prácticos para que armes tus propios scripts de auditoría.

---

# 📘 Guía de Comandos CMD para Auditoría de PC
## Referencia completa: Comando → Qué hace → Parámetros → Ejemplo

---

## 🧱 1. COMANDOS BÁSICOS DEL SCRIPT

### `@echo off`
**Qué hace:** Oculta la visualización de los comandos en pantalla mientras se ejecuta el script. Solo muestra los resultados, no los comandos mismos.

**Uso:** Siempre va en la primera línea del `.bat`.

```bat
@echo off
echo Hola, esto se ve en pantalla
REM "echo off" en sí mismo no se ve
```

---

### `echo`
**Qué hace:** Muestra un mensaje de texto en la consola o lo escribe en un archivo.

**Parámetros clave:**
- `echo texto` → Muestra texto en pantalla
- `echo texto > archivo.txt` → Crea/sobrescribe archivo con ese texto
- `echo texto >> archivo.txt` → Agrega texto al final del archivo (sin borrar)
- `echo.` → Muestra una línea en blanco

**Ejemplos:**
```bat
@echo off
echo Hola mundo
echo. 
echo Esto va en un archivo > C:\reporte.txt
echo Linea 1 >> C:\reporte.txt
echo Linea 2 >> C:\reporte.txt
```

---

### `pause`
**Qué hace:** Detiene la ejecución y espera a que el usuario presione una tecla.

**Parámetros clave:**
- `pause` → Muestra "Presione una tecla para continuar..."
- `pause >nul` → Espera tecla pero NO muestra el mensaje

**Ejemplos:**
```bat
@echo off
echo Auditoria completada
pause
echo Continuando...
pause >nul
```

---

### `title`
**Qué hace:** Cambia el título de la ventana de la consola.

```bat
@echo off
title Auditoria de PC - %computername%
echo Ventana renombrada
pause
```

---

### `color`
**Qué hace:** Cambia los colores de fondo y texto de la consola.

**Sintaxis:** `color XY` donde X = color fondo, Y = color texto

| Código | Color |
|--------|-------|
| 0 | Negro |
| 1 | Azul |
| 2 | Verde |
| 3 | Aguamarina |
| 4 | Rojo |
| 5 | Púrpura |
| 6 | Amarillo/Marrón |
| 7 | Blanco (default) |
| A | Verde lima |
| B | Cian claro |
| C | Rojo claro |
| D | Magenta |
| E | Amarillo |
| F | Blanco brillante |

**Ejemplos:**
```bat
@echo off
color 0A    REM Fondo negro, texto verde
color 1F    REM Fondo azul, texto blanco
color 4E    REM Fondo rojo, texto amarillo
```

---

### `cls`
**Qué hace:** Limpia toda la pantalla de la consola.

```bat
@echo off
echo Texto que se borrará
timeout /t 2 >nul
cls
echo Pantalla limpia
```

---

### `set`
**Qué hace:** Crea o modifica variables de entorno dentro del script.

**Parámetros clave:**
- `set var=valor` → Crea variable
- `set "var=valor"` → Versión segura (maneja espacios)
- `%var%` → Usa el valor de la variable
- `set /a var=5+3` → Operaciones matemáticas

**Ejemplos:**
```bat
@echo off
set "nombre=PC-AUDITORIA"
set "ruta=C:\Reportes"
set /a total=10+5

echo Nombre: %nombre%
echo Ruta: %ruta%
echo Total: %total%
```

---

### `setlocal enabledelayedexpansion`
**Qué hace:** Permite usar variables dentro de bucles (`for`) y que cambien su valor dinámicamente.

**Sintaxis especial:** Usa `!var!` en lugar de `%var%` dentro de bucles.

```bat
@echo off
setlocal enabledelayedexpansion
set contador=0

for %%A in (1 2 3) do (
    set /a contador=!contador!+1
    echo Iteracion !contador!
)
```

---

### `chcp`
**Qué hace:** Cambia la página de códigos (codificación de caracteres).

**Valores comunes:**
- `chcp 65001` → UTF-8 (soporta ñ, tildes, emojis)
- `chcp 1252` → Windows Western (ANSI)
- `chcp 850` → DOS Multilingual

```bat
@echo off
chcp 65001 >nul
echo Caracteres especiales: ñ á é í ó ú
```

> `>nul` oculta el mensaje de confirmación.

---

### `REM` o `::`
**Qué hace:** Agrega comentarios en el script. El CMD ignora esa línea.

```bat
@echo off
REM Esto es un comentario
:: Esto también es un comentario
echo Hola
```

---

## 📁 2. COMANDOS DE ARCHIVOS Y CARPETAS

### `mkdir` (o `md`)
**Qué hace:** Crea una o más carpetas.

**Parámetros clave:**
- `mkdir carpeta` → Crea carpeta
- `mkdir "carpeta con espacios"` → Crea carpeta con espacios
- `mkdir C:\ruta\carpeta` → Crea con ruta absoluta

**Ejemplos:**
```bat
@echo off
mkdir C:\Auditorias
mkdir "C:\Auditorias\Reporte del dia"
mkdir C:\Test1 C:\Test2 C:\Test3
```

---

### `rmdir` (o `rd`)
**Qué hace:** Elimina carpetas.

**Parámetros clave:**
- `/s` → Elimina carpeta y TODO su contenido (subcarpetas y archivos)
- `/q` → Modo silencioso (no pregunta confirmación)

**Ejemplos:**
```bat
@echo off
rmdir C:\CarpetaVacia
rmdir /s /q "C:\CarpetaConContenido"
```

> ⚠️ **¡CUIDADO!** `/s /q` borra todo sin preguntar.

---

### `del`
**Qué hace:** Elimina archivos.

**Parámetros clave:**
- `/f` → Fuerza eliminación de archivos de solo lectura
- `/q` → Silencioso
- `/s` → Elimina archivos en subcarpetas también
- `*.txt` → Patrón comodín

**Ejemplos:**
```bat
@echo off
del C:\temp\archivo.txt
del /f /q "C:\temp\*.log"
del /s /q C:\temp\*.tmp
```

---

### `copy`
**Qué hace:** Copia archivos de un lugar a otro.

**Parámetros clave:**
- `/y` → Sobrescribe sin preguntar
- `/v` → Verifica que la copia sea correcta

**Ejemplos:**
```bat
@echo off
copy C:\origen\datos.txt D:\backup\
copy /y "C:\mi archivo.txt" "D:\copia.txt"
copy C:\origen\*.csv D:\backup\
```

---

### `xcopy`
**Qué hace:** Copia avanzada: archivos, carpetas, subcarpetas, atributos.

**Parámetros clave:**
- `/e` → Copia subcarpetas vacías también
- `/i` → Si el destino no existe, asume que es carpeta
- `/h` → Copia archivos ocultos y del sistema
- `/y` → Sobrescribe sin preguntar
- `/d:MM-DD-YYYY` → Solo copia archivos modificados después de esa fecha

**Ejemplos:**
```bat
@echo off
xcopy C:\Proyecto D:\Backup\Proyecto /e /i /h /y
xcopy C:\Users\%username%\Documents D:\BackupDocs /e /i /y
```

---

### `move`
**Qué hace:** Mueve archivos o renombra carpetas.

**Ejemplos:**
```bat
@echo off
move C:\temp\archivo.txt D:\nueva\
move C:\CarpetaVieja C:\CarpetaNueva
```

---

### `ren` (o `rename`)
**Qué hace:** Renombra archivos o carpetas.

**Ejemplos:**
```bat
@echo off
ren C:\temp\viejo.txt nuevo.txt
ren "C:\temp\mi archivo.txt" "archivo_final.txt"
```

---

### `type`
**Qué hace:** Muestra el contenido de un archivo de texto en pantalla.

**Ejemplos:**
```bat
@echo off
type C:\Windows\System32\drivers\etc\hosts
type C:\reporte.txt
```

---

### `type nul >`
**Qué hace:** Crea un archivo vacío.

**Ejemplos:**
```bat
@echo off
type nul > C:\nuevo.txt
type nul > C:\log\archivo.log
```

---

### `dir`
**Qué hace:** Lista archivos y carpetas.

**Parámetros clave:**
- `/s` → Incluye subcarpetas
- `/b` → Formato simple (solo nombres, sin encabezados)
- `/a` → Muestra archivos con atributos específicos (`-h` ocultos, `s` sistema)
- `/o` → Ordena (`n` por nombre, `s` por tamaño, `d` por fecha)

**Ejemplos:**
```bat
@echo off
dir C:\Windows
dir /s /b C:\Users\%username%\Documents
dir /a:h C:\    REM Archivos ocultos
dir /o:s C:\    REM Ordenado por tamaño
```

---

### `cd`
**Qué hace:** Cambia el directorio actual.

**Parámetros clave:**
- `cd carpeta` → Entra a carpeta
- `cd ..` → Sube un nivel
- `cd \` → Va a la raíz
- `cd /d D:\carpeta` → Cambia de unidad Y carpeta

**Ejemplos:**
```bat
@echo off
cd C:\Windows\System32
cd ..
cd /d D:\Datos
```

---

### `pushd` / `popd`
**Qué hace:** `pushd` guarda la ubicación actual y cambia a otra. `popd` vuelve a la ubicación guardada.

**Ejemplos:**
```bat
@echo off
pushd C:\Windows\System32
echo Ahora estoy en: %cd%
dir *.dll
popd
echo Volvi a: %cd%
```

---

## 🖥️ 3. COMANDOS DE SISTEMA Y HARDWARE

### `systeminfo`
**Qué hace:** Muestra información detallada del sistema operativo, hardware y red.

**Parámetros clave:**
- `/fo table` → Formato tabla
- `/fo csv` → Formato CSV
- `/fo list` → Formato lista
- `| findstr "texto"` → Filtra líneas que contengan "texto"
- `| findstr /C:"texto exacto"` → Filtra texto exacto

**Ejemplos:**
```bat
@echo off
systeminfo > C:\info_completa.txt

systeminfo | findstr /C:"Nombre de host" > C:\nombre_pc.txt
systeminfo | findstr /B /C:"OS" /C:"Memoria" /C:"Procesador" > C:\resumen.txt

systeminfo /fo csv > C:\info.csv
```

---

### `wmic`
**Qué hace:** Windows Management Instrumentation Command-line. Accede a información profunda del sistema.

**Sintaxis:** `wmic [clase] get [campos] [formato]`

**Formatos de salida:**
- `/format:list` → Lista (campo: valor)
- `/format:csv` → Valores separados por comas
- `/format:table` → Tabla

**Clases útiles para auditoría:**

| Clase | Información |
|-------|-------------|
| `cpu` | Procesador |
| `memorychip` | Memoria RAM |
| `baseboard` | Placa base |
| `bios` | BIOS |
| `diskdrive` | Discos físicos |
| `logicaldisk` | Unidades/particiones |
| `nic` | Adaptadores de red |
| `os` | Sistema operativo |
| `qfe` | Actualizaciones/hotfixes |
| `product` | Software instalado |
| `startup` | Programas de inicio |
| `process` | Procesos en ejecución |
| `useraccount` | Cuentas de usuario |

**Ejemplos:**
```bat
@echo off
:: Procesador
wmic cpu get Name, NumberOfCores, MaxClockSpeed /format:list

:: RAM
wmic memorychip get Capacity, Speed, Manufacturer /format:csv

:: Discos
wmic diskdrive get Model, Size, InterfaceType /format:table

:: Sistema operativo
wmic os get Caption, Version, OSArchitecture /format:list

:: Actualizaciones instaladas
wmic qfe get HotFixID, InstalledOn, Description /format:csv

:: Programas de inicio
wmic startup get Caption, Command, Location /format:list

:: Guardar en archivo
wmic cpu get Name /format:list > C:\cpu.txt
```

---

### `fsutil`
**Qué hace:** Utilidades avanzadas del sistema de archivos.

**Subcomandos útiles:**
- `fsutil fsinfo drives` → Lista unidades disponibles
- `fsutil fsinfo ntfsinfo C:` → Info detallada de NTFS en C:
- `fsutil file createnew archivo.txt 1024` → Crea archivo de 1024 bytes

**Ejemplos:**
```bat
@echo off
fsutil fsinfo drives
fsutil fsinfo ntfsinfo C:
fsutil fsinfo volumeinfo D:
```

---

## 🌐 4. COMANDOS DE RED

### `ipconfig`
**Qué hace:** Muestra configuración TCP/IP.

**Parámetros clave:**
- `/all` → Configuración completa de TODOS los adaptadores
- `/release` → Libera IP (DHCP)
- `/renew` → Renueva IP (DHCP)
- `/flushdns` → Limpia caché DNS
- `/displaydns` → Muestra caché DNS

**Ejemplos:**
```bat
@echo off
ipconfig /all > C:\red_completa.txt
ipconfig | findstr /C:"IPv4" > C:\ips.txt
ipconfig /displaydns > C:\dns_cache.txt
```

---

### `ping`
**Qué hace:** Verifica conectividad con otro equipo/envía paquetes de prueba.

**Parámetros clave:**
- `-n 4` → Envía 4 paquetes (por defecto 4)
- `-t` → Ping continuo (hasta que lo detengas con Ctrl+C)
- `-l 1024` → Tamaño del paquete en bytes
- `-a` → Resuelve nombre del host

**Ejemplos:**
```bat
@echo off
ping google.com -n 4
ping 192.168.1.1 -n 2
ping -a 192.168.1.100
```

---

### `tracert`
**Qué hace:** Muestra la ruta que siguen los paquetes hasta llegar al destino (salto por salto).

**Parámetros clave:**
- `-d` → No resuelve nombres de dominio (más rápido)
- `-h 15` → Máximo 15 saltos

**Ejemplos:**
```bat
@echo off
tracert google.com
tracert -d 8.8.8.8
```

---

### `netstat`
**Qué hace:** Muestra conexiones de red, puertos abiertos y estadísticas.

**Parámetros clave:**
- `-a` → Todas las conexiones y puertos en escucha
- `-n` → Muestra direcciones y puertos en formato numérico
- `-o` → Muestra el PID del proceso asociado
- `-b` → Muestra el nombre del programa (requiere admin)
- `-anob` → Combinación de todo (requiere admin)

**Ejemplos:**
```bat
@echo off
netstat -an > C:\puertos.txt
netstat -ano > C:\puertos_con_pid.txt
netstat -anob > C:\puertos_con_programas.txt
```

---

### `arp`
**Qué hace:** Muestra y modifica la tabla ARP (relación IP ↔ MAC).

**Parámetros clave:**
- `-a` → Muestra todas las entradas ARP
- `-d` → Borra tabla ARP

**Ejemplos:**
```bat
@echo off
arp -a > C:\tabla_arp.txt
```

---

### `route`
**Qué hace:** Muestra y modifica la tabla de rutas de red.

**Parámetros clave:**
- `print` → Muestra tabla de rutas
- `add` → Agrega ruta
- `delete` → Elimina ruta

**Ejemplos:**
```bat
@echo off
route print > C:\rutas.txt
route print | findstr 0.0.0.0
```

---

### `netsh`
**Qué hace:** Shell de configuración de red. Permite configurar casi todo de red.

**Subcomandos útiles:**
- `netsh wlan show profiles` → Redes WiFi guardadas
- `netsh wlan show profile name="Red" key=clear` → Muestra contraseña WiFi
- `netsh advfirewall show allprofiles` → Estado del firewall
- `netsh interface show interface` → Interfaces de red

**Ejemplos:**
```bat
@echo off
netsh wlan show profiles > C:\wifi_redes.txt
netsh interface show interface > C:\interfaces.txt
netsh advfirewall show currentprofile > C:\firewall.txt
```

---

### `nslookup`
**Qué hace:** Consulta DNS para resolver nombres de dominio a IP.

**Ejemplos:**
```bat
@echo off
nslookup google.com > C:\dns_google.txt
nslookup -type=mx gmail.com
```

---

## 🔒 5. COMANDOS DE SEGURIDAD Y USUARIOS

### `net user`
**Qué hace:** Gestiona cuentas de usuario locales.

**Parámetros clave:**
- `net user` → Lista todos los usuarios
- `net user nombre` → Muestra detalles de un usuario
- `net user nombre /add` → Crea usuario
- `net user nombre /delete` → Elimina usuario
- `net user nombre /active:yes` → Activa usuario

**Ejemplos:**
```bat
@echo off
net user > C:\usuarios.txt
net user Administrador > C:\admin_detalles.txt
```

---

### `net localgroup`
**Qué hace:** Gestiona grupos locales de usuarios.

**Parámetros clave:**
- `net localgroup` → Lista grupos
- `net localgroup Administradores` → Muestra miembros del grupo
- `net localgroup "Remote Desktop Users"` → Miembros de Escritorio Remoto

**Ejemplos:**
```bat
@echo off
net localgroup > C:\grupos.txt
net localgroup Administradores > C:\admins.txt
net localgroup "Usuarios del escritorio remoto" > C:\rdp_users.txt
```

---

### `net accounts`
**Qué hace:** Muestra la política de contraseñas del sistema.

**Ejemplos:**
```bat
@echo off
net accounts > C:\politica_passwords.txt
```

---

### `sc`
**Qué hace:** Gestiona servicios de Windows (Service Control).

**Parámetros clave:**
- `sc query` → Lista servicios
- `sc query nombre` → Estado de un servicio específico
- `sc query type= service state= running` → Solo servicios en ejecución
- `sc query type= service start= auto` → Servicios que inician automáticamente

**Ejemplos:**
```bat
@echo off
sc query > C:\todos_servicios.txt
sc query type= service state= running > C:\servicios_activos.txt
sc query WinDefend > C:\defender_estado.txt
```

> ⚠️ Nota: Después de `type=` y `state=` hay un espacio antes del valor.

---

### `tasklist`
**Qué hace:** Lista procesos en ejecución.

**Parámetros clave:**
- `/v` → Información detallada (usuario, título ventana, memoria)
- `/fi "STATUS eq RUNNING"` → Filtra por estado
- `/fi "IMAGENAME eq chrome.exe"` → Filtra por nombre
- `/svc` → Muestra servicios dentro de cada proceso
- `/fo csv` → Formato CSV

**Ejemplos:**
```bat
@echo off
tasklist > C:\procesos.txt
tasklist /v > C:\procesos_detalle.txt
tasklist /fi "MEMUSAGE gt 100000" > C:\procesos_pesados.txt
```

---

### `taskkill`
**Qué hace:** Finaliza procesos.

**Parámetros clave:**
- `/im nombre.exe` → Mata por nombre de imagen
- `/pid 1234` → Mata por ID de proceso
- `/f` → Fuerza el cierre
- `/t` → Mata también procesos hijos

**Ejemplos:**
```bat
@echo off
taskkill /im notepad.exe /f
taskkill /pid 1234 /f /t
```

---

### `schtasks`
**Qué hace:** Gestiona tareas programadas.

**Parámetros clave:**
- `/query` → Lista tareas
- `/fo LIST /v` → Formato lista detallada
- `/fo TABLE` → Formato tabla

**Ejemplos:**
```bat
@echo off
schtasks /query > C:\tareas.txt
schtasks /query /fo LIST /v > C:\tareas_detalle.txt
```

---

### `reg`
**Qué hace:** Interactúa con el registro de Windows.

**Parámetros clave:**
- `query` → Consulta valores
- `add` → Agrega valores
- `delete` → Elimina valores
- `/s` → Recursivo (subclaves)
- `/v nombre` → Valor específico
- `/ve` → Valor por defecto

**Ejemplos:**
```bat
@echo off
:: Software instalado (32 bits)
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall" /s > C:\software32.txt

:: Software instalado (64 bits)
reg query "HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall" /s > C:\software64.txt

:: Programas de inicio del usuario
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /s > C:\inicio_usuario.txt

:: Programas de inicio del sistema
reg query "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /s > C:\inicio_sistema.txt
```

---

### `powercfg`
**Qué hace:** Configura opciones de energía.

**Parámetros clave:**
- `/batteryreport` → Genera reporte de batería (portátiles)
- `/energy` → Genera reporte de eficiencia energética

**Ejemplos:**
```bat
@echo off
powercfg /batteryreport /output C:\bateria.html
powercfg /energy /output C:\energia.xml
```

---

### `driverquery`
**Qué hace:** Lista drivers instalados.

**Parámetros clave:**
- `/v` → Detallado
- `/fo csv` → Formato CSV

**Ejemplos:**
```bat
@echo off
driverquery > C:\drivers.txt
driverquery /v /fo csv > C:\drivers.csv
```

---

## 🔄 6. COMANDOS DE CONTROL DE FLUJO

### `if`
**Qué hace:** Ejecuta comandos condicionalmente.

**Sintaxis comunes:**
- `if exist archivo` → Si el archivo existe
- `if not exist archivo` → Si NO existe
- `if "%var%"=="valor"` → Comparación de texto
- `if errorlevel 1` → Si el último comando falló

**Ejemplos:**
```bat
@echo off
set "archivo=C:\test.txt"

if exist "%archivo%" (
    echo El archivo existe
) else (
    echo El archivo NO existe
)

if "%username%"=="Administrador" (
    echo Eres admin
) else (
    echo No eres admin
)
```

---

### `for`
**Qué hace:** Bucle que repite comandos.

**Sintaxis comunes:**

**1. Iterar sobre archivos:**
```bat
@echo off
for %%A in (*.txt) do (
    echo Archivo: %%A
)
```

**2. Iterar con rango numérico:**
```bat
@echo off
for /L %%N in (1,1,10) do (
    echo Numero: %%N
)
REM (inicio, paso, fin) → del 1 al 10 de 1 en 1
```

**3. Iterar sobre resultado de comando:**
```bat
@echo off
for /f "tokens=*" %%L in ('dir /b') do (
    echo Linea: %%L
)
```

**4. Iterar sobre archivo línea por línea:**
```bat
@echo off
for /f "tokens=*" %%L in (C:\lista.txt) do (
    echo Contenido: %%L
)
```

**5. Iterar sobre carpetas:**
```bat
@echo off
for /d %%D in (C:\Users\*) do (
    echo Carpeta: %%D
)
```

**6. Tokens (dividir líneas en columnas):**
```bat
@echo off
for /f "tokens=1,2,3 delims= " %%A in ("Hola Mundo Aqui") do (
    echo 1: %%A
    echo 2: %%B
    echo 3: %%C
)
```

---

### `goto`
**Qué hace:** Salta a una etiqueta específica del script.

**Ejemplos:**
```bat
@echo off
goto inicio

:error
echo Ocurrio un error
goto fin

:inicio
echo Bienvenido al script
goto fin

:fin
echo Fin del script
pause
```

---

### `call`
**Qué hace:** Llama a otro script `.bat` y vuelve al original cuando termina.

**Ejemplos:**
```bat
@echo off
echo Script principal
call otro_script.bat
echo Volvi al principal
```

---

### `exit`
**Qué hace:** Cierra la ventana del CMD o termina el script.

**Parámetros clave:**
- `/b` → Solo termina el script, no cierra la ventana
- `0` → Éxito
- `1` → Error

**Ejemplos:**
```bat
@echo off
if not exist "C:\importante.txt" (
    echo ERROR: Archivo no encontrado
    exit /b 1
)
echo Todo OK
exit /b 0
```

---

## ⬇️ 7. COMANDOS DE DESCARGA Y RED AVANZADA

### `curl`
**Qué hace:** Descarga archivos desde internet. Incluido en Windows 10/11.

**Parámetros clave:**
- `-o archivo` → Guarda con nombre específico
- `-O` → Guarda con nombre original del servidor
- `-L` → Sigue redirecciones
- `-s` → Silencioso
- `-I` → Solo muestra headers
- `--insecure` o `-k` → Ignora errores de certificado SSL

**Ejemplos:**
```bat
@echo off
curl -o C:\descarga\archivo.zip https://ejemplo.com/archivo.zip
curl -L -o C:\descarga\imagen.jpg https://picsum.photos/1920/1080
curl -s -o C:\temp\pagina.html https://www.google.com
```

---

### `powershell` (desde `.bat`)
**Qué hace:** Ejecuta comandos de PowerShell dentro de un `.bat`.

**Parámetros clave:**
- `-Command "comando"` → Ejecuta comando directo
- `-ExecutionPolicy Bypass` → Evita restricciones de ejecución
- `-WindowStyle Hidden` → Ejecuta sin mostrar ventana

**Ejemplos:**
```bat
@echo off
:: Descargar archivo
powershell -Command "Invoke-WebRequest -Uri 'https://ejemplo.com/archivo.exe' -OutFile 'C:\descarga\archivo.exe'"

:: Obtener info del sistema
powershell -Command "Get-ComputerInfo | Select WindowsVersion, TotalPhysicalMemory, CsProcessors | Format-List" > C:\info_ps.txt

:: Listar apps de Windows Store
powershell -Command "Get-AppxPackage | Select Name, Version | Format-Table" > C:\apps_store.txt
```

---

## 📝 8. REDIRECCIONAMIENTO Y TUBERÍAS

### `>` (sobrescribir)
**Qué hace:** Envía la salida a un archivo. Si existe, lo borra y crea nuevo.

```bat
@echo off
echo Reporte del dia > C:\reporte.txt
ipconfig > C:\red.txt
```

---

### `>>` (agregar)
**Qué hace:** Agrega la salida al final del archivo sin borrar lo existente.

```bat
@echo off
echo === INICIO === > C:\log.txt
echo Linea 1 >> C:\log.txt
echo Linea 2 >> C:\log.txt
echo === FIN === >> C:\log.txt
```

---

### `|` (pipe / tubería)
**Qué hace:** Toma la salida de un comando y la envía como entrada a otro.

```bat
@echo off
:: Filtrar systeminfo
systeminfo | findstr /C:"Windows"

:: Contar líneas
dir /s /b | find /c /v ""

:: Buscar en procesos
tasklist | findstr "chrome"
```

---

### `2>nul` (ocultar errores)
**Qué hace:** Redirige los mensajes de error a "la nada" (no se muestran ni se guardan).

```bat
@echo off
:: Si la carpeta no existe, no muestra error
dir C:\CarpetaInexistente 2>nul

:: Si el comando falla, sigue ejecutando
reg query "HKLM\ClaveInexistente" 2>nul
```

---

### `>nul` (ocultar salida normal)
**Qué hace:** Oculta la salida exitosa del comando.

```bat
@echo off
:: No muestra "Página de códigos activa: 65001"
chcp 65001 >nul

:: No muestra confirmación de copia
copy archivo.txt C:\backup\ >nul
```

---

### `> archivo 2>&1` (todo a un archivo)
**Qué hace:** Guarda tanto la salida normal como los errores en el mismo archivo.

```bat
@echo off
comando_que_puede_fallar > C:\resultado.txt 2>&1
```

---

## ⏱️ 9. COMANDOS DE TIEMPO Y FECHA

### `timeout`
**Qué hace:** Espera una cantidad de segundos.

**Parámetros clave:**
- `/t 5` → Espera 5 segundos
- `/nobreak` → No se puede interrumpir con tecla
- `-1` → Espera indefinidamente

**Ejemplos:**
```bat
@echo off
echo Esperando 3 segundos...
timeout /t 3 /nobreak
echo Continuando...
```

---

### `date` / `time`
**Qué hace:** Muestra o establece fecha/hora del sistema.

**Variables automáticas:**
- `%date%` → Fecha actual (formato del sistema)
- `%time%` → Hora actual

**Ejemplos:**
```bat
@echo off
echo Fecha: %date%
echo Hora: %time%

:: Guardar en variable
set "ahora=%date% %time%"
echo Momento: %ahora%
```

---

### `choice`
**Qué hace:** Pide al usuario que elija una opción.

**Parámetros clave:**
- `/c YNC` → Opciones válidas: Y, N, C
- `/m "mensaje"` → Mensaje a mostrar
- `/t 10 /d Y` → Espera 10 segundos, default Y

**Ejemplos:**
```bat
@echo off
choice /c SN /m "Deseas continuar"
if %errorlevel%==1 echo Elegiste SI
if %errorlevel%==2 echo Elegiste NO
```

---

## 🛠️ 10. EJEMPLOS COMPLETOS LISTOS PARA USAR

Ahora que conoces los comandos, aquí tienes **scripts completos** que combinan todo para auditorías reales.

---

### 📋 Ejemplo 1: Mini Auditoría Rápida
```bat
@echo off
chcp 65001 >nul
title Mini Auditoria
color 0A

set "carpeta=C:\Auditoria_%computername%_%date:/=-%"
mkdir "%carpeta%" 2>nul

echo === AUDITORIA RAPIDA === > "%carpeta%\resumen.txt"
echo Fecha: %date% %time% >> "%carpeta%\resumen.txt"
echo. >> "%carpeta%\resumen.txt"

echo [+] Sistema...
systeminfo | findstr /C:"Nombre" /C:"Windows" /C:"Procesador" >> "%carpeta%\resumen.txt"

echo [+] Red...
ipconfig | findstr /C:"IPv4" >> "%carpeta%\resumen.txt"

echo [+] Disco...
wmic logicaldisk get DeviceID,FreeSpace,Size /format:list >> "%carpeta%\resumen.txt"

echo [+] Usuarios...
net user >> "%carpeta%\resumen.txt"

echo [+] Procesos activos...
tasklist >> "%carpeta%\procesos.txt"

echo Auditoria guardada en: %carpeta%
pause
```

---

### 📋 Ejemplo 2: Inventario de Hardware
```bat
@echo off
setlocal enabledelayedexpansion
chcp 65001 >nul

set "salida=C:\Hardware_%computername%.txt"

(
    echo ==========================================
    echo    INVENTARIO DE HARDWARE
    echo    Equipo: %computername%
    echo    Fecha: %date%
    echo ==========================================
    echo.
    echo --- PROCESADOR ---
) > "%salida%"

wmic cpu get Name, NumberOfCores, NumberOfLogicalProcessors, MaxClockSpeed /format:list >> "%salida%"

(
    echo.
    echo --- MEMORIA RAM ---
) >> "%salida%"

for /f "tokens=*" %%L in ('wmic memorychip get Capacity /value ^| find "="') do (
    set "linea=%%L"
    set "ram=!linea:Capacity=Capacidad (bytes)!"
    echo !ram! >> "%salida%"
)

wmic memorychip get Speed, Manufacturer, PartNumber /format:list >> "%salida%"

(
    echo.
    echo --- ALMACENAMIENTO ---
) >> "%salida%"

wmic diskdrive get Model, Size, InterfaceType, MediaType /format:list >> "%salida%"

(
    echo.
    echo --- RED ---
) >> "%salida%"

wmic nic where "NetEnabled='TRUE'" get Name, MACAddress, Speed /format:list >> "%salida%"

echo Inventario guardado en: %salida%
pause
```

---

### 📋 Ejemplo 3: Verificador de Seguridad
```bat
@echo off
chcp 65001 >nul
title Verificador de Seguridad
color 0C

set "reporte=C:\Seguridad_%computername%.txt"

(
    echo ==========================================
    echo    VERIFICACION DE SEGURIDAD
    echo ==========================================
    echo.
    echo --- USUARIOS CON PRIVILEGIOS ---
) > "%reporte%"

net localgroup Administradores >> "%reporte%"

(
    echo.
    echo --- SERVICIOS INUSUALES ---
) >> "%reporte%"

sc query type= service start= auto state= running | findstr /C:"SERVICE_NAME" /C:"DISPLAY_NAME" >> "%reporte%"

(
    echo.
    echo --- PUERTOS ABIERTOS ---
) >> "%reporte%"

netstat -ano | findstr "LISTENING" >> "%reporte%"

(
    echo.
    echo --- PROGRAMAS DE INICIO ---
) >> "%reporte%"

wmic startup get Caption, Command /format:list >> "%reporte%"

reg query "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /s >> "%reporte%" 2>nul

echo Reporte de seguridad: %reporte%
pause
```

---

### 📋 Ejemplo 4: Descarga Automática de Herramientas
```bat
@echo off
chcp 65001 >nul
title Descargador de Auditoria
color 0E

set "descargas=C:\Herramientas_Auditoria"
mkdir "%descargas%" 2>nul

echo [1/3] Descargando Sysinternals Suite...
curl -L -o "%descargas%\SysinternalsSuite.zip" https://download.sysinternals.com/files/SysinternalsSuite.zip

echo [2/3] Descargando Process Explorer...
curl -L -o "%descargas%\ProcessExplorer.zip" https://download.sysinternals.com/files/ProcessExplorer.zip

echo [3/3] Descargando Autoruns...
curl -L -o "%descargas%\Autoruns.zip" https://download.sysinternals.com/files/Autoruns.zip

echo.
echo Archivos descargados en: %descargas%
dir "%descargas%"
pause
```

---

### 📋 Ejemplo 5: Auditoría de Red Completa
```bat
@echo off
chcp 65001 >nul
set "carpeta=C:\Auditoria_Red_%date:/=-%"
mkdir "%carpeta%"

echo [+] IP completa...
ipconfig /all > "%carpeta%\01_ipconfig.txt"

echo [+] Puertos y conexiones...
netstat -anob > "%carpeta%\02_netstat.txt" 2>nul

echo [+] Tabla ARP...
arp -a > "%carpeta%\03_arp.txt"

echo [+] Rutas...
route print > "%carpeta%\04_rutas.txt"

echo [+] WiFi guardadas...
netsh wlan show profiles > "%carpeta%\05_wifi_perfiles.txt"

echo [+] Firewall...
netsh advfirewall show allprofiles > "%carpeta%\06_firewall.txt"

echo [+] DNS cache...
ipconfig /displaydns > "%carpeta%\07_dns.txt"

echo [+] Interfaces...
wmic nic get Name, NetConnectionID, MACAddress, Speed, NetEnabled /format:csv > "%carpeta%\08_interfaces.csv"

echo Red auditada en: %carpeta%
pause
```

---

### 📋 Ejemplo 6: Script con Menú Interactivo
```bat
@echo off
chcp 65001 >nul
title Menu de Auditoria
color 0B

:inicio
cls
echo ==========================================
echo    MENU DE AUDITORIA
echo ==========================================
echo.
echo  [1] Informacion del Sistema
echo  [2] Hardware
echo  [3] Red
echo  [4] Seguridad
echo  [5] Todo (completa)
echo  [6] Salir
echo.
echo ==========================================
set /p opcion="Elige una opcion (1-6): "

if "%opcion%"=="1" goto sistema
if "%opcion%"=="2" goto hardware
if "%opcion%"=="3" goto red
if "%opcion%"=="4" goto seguridad
if "%opcion%"=="5" goto todo
if "%opcion%"=="6" goto salir
goto inicio

:sistema
cls
systeminfo | more
pause
goto inicio

:hardware
cls
wmic cpu get Name, MaxClockSpeed /format:list
wmic memorychip get Capacity, Speed /format:list
pause
goto inicio

:red
cls
ipconfig | findstr /C:"IPv4" /C:"Mascara"
pause
goto inicio

:seguridad
cls
net user
net localgroup Administradores
pause
goto inicio

:todo
cls
echo Ejecutando auditoria completa...
REM (Aqui iria todo el script maestro)
pause
goto inicio

:salir
exit
```

---

## 📊 Tabla Rápida de Referencia

| Necesitas... | Comando |
|-------------|---------|
| Info del sistema | `systeminfo` |
| Procesador | `wmic cpu get Name` |
| RAM | `wmic memorychip get Capacity,Speed` |
| Discos | `wmic diskdrive get Model,Size` |
| Particiones | `wmic logicaldisk get DeviceID,FreeSpace,Size` |
| Red completa | `ipconfig /all` |
| Puertos abiertos | `netstat -ano` |
| Usuarios | `net user` |
| Grupos/Admins | `net localgroup Administradores` |
| Servicios activos | `sc query type= service state= running` |
| Procesos | `tasklist /v` |
| Software instalado | `wmic product get Name,Version` |
| Programas de inicio | `wmic startup get Caption,Command` |
| Actualizaciones | `wmic qfe get HotFixID,InstalledOn` |
| Drivers | `driverquery /v` |
| WiFi guardadas | `netsh wlan show profiles` |
| Descargar archivo | `curl -o archivo URL` |
| Crear carpeta | `mkdir ruta` |
| Guardar en archivo | `comando > archivo.txt` |
| Agregar a archivo | `comando >> archivo.txt` |
| Ocultar errores | `comando 2>nul` |
| Filtrar salida | `comando | findstr "texto"` |
| Esperar X segundos | `timeout /t X` |

---

¿Quieres que profundice en algún comando específico o que te arme un script personalizado para una auditoría en particular (por ejemplo: solo seguridad, solo rendimiento, o para ejecutar en red)?
