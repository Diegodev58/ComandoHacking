Aquí tienes la **cheatsheet definitiva y ultra completa** de comandos CMD para armar tus propios scripts de control remoto, auditoría y administración de PCs. Cada uno con descripción corta, clara y un ejemplo listo para copiar.

---

# 🛠️ CHEATSHEET CMD — Control Remoto & Auditoría Empresarial

---

## 📁 ARCHIVOS Y CARPETAS

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `mkdir "ruta"` | Crea carpetas, incluso anidadas con rutas largas. | `mkdir "C:\Control\%computername%\Logs"` |
| `rmdir /s /q "ruta"` | Borra carpeta y TODO dentro sin preguntar. | `rmdir /s /q "C:\Temp\Basura"` |
| `del /f /q "archivo"` | Borra archivo. `/f` fuerza, `/q` silencioso. | `del /f /q "C:\Temp\*.tmp"` |
| `copy "origen" "destino"` | Copia archivo. `/y` sobrescribe sin avisar. | `copy /y "app.exe" "\\PC01\C$\Temp\"` |
| `xcopy "origen" "destino" /e /i /h /y` | Copia carpetas, subcarpetas, ocultos y forzado. | `xcopy "C:\Deploy" "\\PC02\C$\Deploy" /e /i /h /y` |
| `move "origen" "destino"` | Mueve o renombra archivos/carpetas. | `move "C:\log.txt" "D:\Backup\"` |
| `ren "viejo" "nuevo"` | Renombra archivo o carpeta. | `ren "log.txt" "log_ayer.txt"` |
| `type "archivo"` | Muestra contenido de un archivo de texto. | `type "C:\Windows\System32\drivers\etc\hosts"` |
| `type nul > "archivo"` | Crea archivo vacío al instante. | `type nul > "C:\nuevo.txt"` |
| `dir /s /b "ruta"` | Lista archivos con rutas completas, recursivo. | `dir /s /b "C:\Users\%username%\Documents"` |
| `dir /a:h /s /b` | Lista archivos OCULTOS en todo el disco. | `dir /a:h /s /b C:\` |
| `cd /d "ruta"` | Cambia de carpeta Y de unidad (C: a D:). | `cd /d D:\Datos` |
| `pushd "ruta"` | Guarda ubicación actual y salta a otra. | `pushd C:\Windows` |
| `popd` | Vuelve a la ubicación guardada por `pushd`. | `popd` |

---

## 🖥️ SISTEMA E INFORMACIÓN

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `systeminfo` | Info completa de Windows, hardware, red, parches. | `systeminfo > "C:\Audit\sys.txt"` |
| `systeminfo \| findstr /C:"texto"` | Filtra systeminfo por palabra clave exacta. | `systeminfo \| findstr /C:"Windows"` |
| `ver` | Muestra la versión de Windows. | `ver` |
| `whoami` | Muestra usuario actual con dominio. | `whoami > "C:\user.txt"` |
| `%computername%` | Variable: nombre del PC. | `echo %computername%` |
| `%username%` | Variable: usuario logueado. | `echo %username%` |
| `%userdomain%` | Variable: dominio o grupo de trabajo. | `echo %userdomain%` |
| `%date%` | Variable: fecha del sistema. | `echo %date%` |
| `%time%` | Variable: hora del sistema. | `echo %time%` |
| `%cd%` | Variable: carpeta actual. | `echo %cd%` |
| `%temp%` | Variable: ruta de archivos temporales del usuario. | `echo %temp%` |
| `%systemroot%` | Variable: carpeta de Windows (C:\Windows). | `echo %systemroot%` |
| `chcp 65001` | Activa UTF-8 (tildes, ñ, emojis). | `chcp 65001 >nul` |
| `cls` | Limpia la pantalla de la consola. | `cls` |
| `title "texto"` | Cambia el título de la ventana CMD. | `title Soporte Remoto - %computername%` |
| `color 0A` | Cambia colores: fondo 0 (negro), texto A (verde). | `color 0B` |
| `pause` | Espera tecla del usuario. | `pause` |
| `pause >nul` | Espera tecla pero sin mostrar mensaje. | `pause >nul` |
| `timeout /t 5 /nobreak` | Espera 5 segundos. `/nobreak` evita que el usuario salte. | `timeout /t 10` |
| `exit /b 0` | Cierra el script con código 0 (éxito). | `exit /b 0` |
| `start "" "programa"` | Abre programa sin bloquear el script. | `start "" "notepad.exe"` |

---

## 🔧 HARDWARE (WMIC)

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `wmic cpu get Name,NumberOfCores,MaxClockSpeed /format:list` | Info del procesador. | Guarda en `.txt` directo con `>` |
| `wmic memorychip get Capacity,Speed,Manufacturer /format:csv` | Info de cada módulo RAM. | `> "ram.csv"` |
| `wmic baseboard get Manufacturer,Product,SerialNumber /format:list` | Placa madre. | `> "placa.txt"` |
| `wmic bios get Manufacturer,Version,ReleaseDate /format:list` | BIOS/UEFI. | `> "bios.txt"` |
| `wmic diskdrive get Model,Size,InterfaceType,MediaType /format:list` | Discos físicos. | `> "discos.txt"` |
| `wmic logicaldisk get DeviceID,Size,FreeSpace,FileSystem /format:csv` | Unidades (C:, D:) con espacio libre. | `> "particiones.csv"` |
| `wmic path win32_videocontroller get Name,AdapterRAM /format:list` | Tarjeta de video. | `> "gpu.txt"` |
| `wmic desktopmonitor get Name,ScreenHeight,ScreenWidth /format:list` | Monitores conectados. | `> "monitores.txt"` |
| `wmic nic get Name,NetConnectionID,MACAddress,Speed /format:list` | Adaptadores de red. | `> "red.txt"` |
| `wmic os get Caption,Version,OSArchitecture /format:list` | Sistema operativo. | `> "os.txt"` |
| `wmic qfe get HotFixID,InstalledOn,Description /format:csv` | Parches/actualizaciones instaladas. | `> "updates.csv"` |
| `wmic product get Name,Version,Vendor,InstallDate /format:csv` | Software instalado (puede tardar). | `> "software.csv"` |
| `wmic startup get Caption,Command,Location /format:list` | Programas que inician con Windows. | `> "inicio.txt"` |
| `wmic process get Name,ProcessId,CommandLine /format:csv` | Procesos en ejecución con rutas. | `> "procesos.csv"` |
| `wmic useraccount get Name,SID,Status /format:csv` | Cuentas de usuario del sistema. | `> "users.csv"` |
| `wmic service get Name,State,StartMode /format:csv` | Servicios de Windows. | `> "servicios.csv"` |

> **Truco:** Todos los `wmic` permiten `where` para filtrar. Ejemplo: `wmic process where "Name='chrome.exe'" get ProcessId`.

---

## 🌐 RED Y CONECTIVIDAD

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `ipconfig /all` | Configuración completa de red de todos los adaptadores. | `> "red.txt"` |
| `ipconfig \| findstr /C:"IPv4"` | Solo muestra las IPs. | `ipconfig \| findstr /C:"IPv4"` |
| `ipconfig /displaydns` | Muestra caché DNS (páginas visitadas recientemente). | `> "dns.txt"` |
| `ipconfig /flushdns` | Limpia caché DNS. | `ipconfig /flushdns` |
| `netstat -ano` | Conexiones activas y puertos abiertos con PID. | `> "puertos.txt"` |
| `netstat -anob` | Igual pero muestra el NOMBRE del programa (necesita admin). | `> "puertos_programas.txt"` |
| `netstat -ano \| findstr "LISTENING"` | Solo puertos en escucha (servidores locales). | `> "listen.txt"` |
| `arp -a` | Tabla ARP: IPs de la red local con sus MAC. | `> "arp.txt"` |
| `route print` | Tabla de rutas de red. | `> "rutas.txt"` |
| `ping 8.8.8.8 -n 4` | Comprueba conectividad con 4 paquetes. | `ping google.com -n 2` |
| `tracert -d google.com` | Muestra la ruta hasta el destino. `-d` evita resolver nombres (más rápido). | `tracert 1.1.1.1` |
| `nslookup google.com` | Resuelve IP de un dominio. | `nslookup empresa.com` |
| `netsh wlan show profiles` | Lista redes WiFi guardadas en el PC. | `> "wifi.txt"` |
| `netsh wlan show profile name="Red" key=clear` | Muestra contraseña de una WiFi guardada (admin). | `netsh wlan show profile name="Oficina" key=clear` |
| `netsh advfirewall show allprofiles` | Estado del firewall (Dominio, Privado, Público). | `> "firewall.txt"` |
| `netsh advfirewall firewall show rule name=all` | Lista TODAS las reglas del firewall. | `> "reglas.txt"` |
| `netsh interface show interface` | Interfaces de red activas/inactivas. | `> "interfaces.txt"` |
| `curl -o "archivo" "URL"` | Descarga archivo desde internet. | `curl -o "app.exe" "https://.../app.exe"` |
| `curl -L -o "archivo" "URL"` | Descarga siguiendo redirecciones. | `curl -L -o "foto.jpg" "https://.../img.jpg"` |
| `curl -s -o "archivo" "URL"` | Descarga en silencio (sin barra de progreso). | `curl -s -o "tmp.zip" "https://.../tmp.zip"` |

---

## 👤 USUARIOS, SEGURIDAD Y PERMISOS

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `net user` | Lista usuarios locales del PC. | `> "usuarios.txt"` |
| `net user "nombre"` | Detalle completo de un usuario. | `net user Administrador` |
| `net localgroup` | Lista grupos locales. | `> "grupos.txt"` |
| `net localgroup Administradores` | Muestra miembros del grupo Admin. | `> "admins.txt"` |
| `net localgroup "Usuarios del escritorio remoto"` | Quién puede entrar por RDP. | `> "rdp.txt"` |
| `net accounts` | Política de contraseñas (longitud, expiración). | `> "politica.txt"` |
| `query user` | Sesiones activas en el PC. | `> "sesiones.txt"` |
| `query session` | Mismo que query user. | `query session` |
| `tasklist /v` | Procesos con usuario que los ejecuta y título de ventana. | `> "procesos_detalle.txt"` |
| `tasklist /fi "USERNAME eq %username%"` | Procesos del usuario actual. | `tasklist /fi "USERNAME eq juan"` |
| `taskkill /im "programa.exe" /f /t` | Mata proceso por nombre. `/f` fuerza, `/t` mata hijos. | `taskkill /im chrome.exe /f /t` |
| `taskkill /pid 1234 /f` | Mata proceso por ID numérico. | `taskkill /pid 5678 /f` |
| `sc query WinDefend` | Estado del servicio de Windows Defender. | `sc query WinDefend` |
| `sc query type= service state= running` | Servicios que están corriendo ahora. | `> "servicios_activos.txt"` |
| `sc query type= service start= auto` | Servicios configurados para iniciar automático. | `> "servicios_auto.txt"` |
| `driverquery /v /fo csv` | Drivers instalados. | `> "drivers.csv"` |
| `powercfg /batteryreport` | Reporte HTML de batería (portátiles). | `powercfg /batteryreport /output "bat.html"` |

---

## 📝 REGISTRO DE WINDOWS (REG)

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `reg query "ruta" /s` | Lee una clave del registro y todas sus subclaves. | `reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall" /s` |
| `reg query "ruta" /v "valor"` | Lee un valor específico. | `reg query "HKLM\..." /v "DisplayName"` |
| `reg add "ruta" /v "nombre" /t REG_DWORD /d 1 /f` | Crea/modifica valor numérico. `/f` fuerza sin preguntar. | `reg add "HKCU\..." /v "Bloqueo" /t REG_DWORD /d 1 /f` |
| `reg delete "ruta" /v "nombre" /f` | Borra un valor del registro. | `reg delete "HKCU\..." /v "Bloqueo" /f` |
| `reg export "ruta" "archivo.reg"` | Exporta una clave a archivo `.reg`. | `reg export "HKCU\Software" "backup.reg"` |

> **Claves útiles para auditoría:**
> - `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall` → Software 64 bits
> - `HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall` → Software 32 bits
> - `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` → Inicio del usuario
> - `HKLM\Software\Microsoft\Windows\CurrentVersion\Run` → Inicio del sistema

---

## 🔄 CONTROL DE FLUJO (LÓGICA DEL SCRIPT)

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `set "var=valor"` | Crea variable. Comillas seguras para espacios. | `set "ruta=C:\Archivos"` |
| `set /a var=5+3` | Variable numérica con operación matemática. | `set /a total=%num1%+%num2%` |
| `echo %var%` | Muestra valor de variable. | `echo %ruta%` |
| `echo texto > "archivo"` | Escribe texto en archivo (sobrescribe). | `echo Hola > "C:\hola.txt"` |
| `echo texto >> "archivo"` | Agrega texto al final del archivo. | `echo Linea >> "C:\log.txt"` |
| `echo.` | Escribe línea en blanco. | `echo.` |
| `if exist "archivo" ( ... )` | Ejecuta si el archivo existe. | `if exist "C:\app.exe" (echo Si existe)` |
| `if not exist "archivo" ( ... )` | Ejecuta si NO existe. | `if not exist "C:\app.exe" (echo Falta)` |
| `if "%var%"=="valor" ( ... )` | Compara texto exacto. | `if "%username%"=="admin" (...)` |
| `if errorlevel 1 ( ... )` | Ejecuta si el comando anterior falló. | `if errorlevel 1 (echo Error)` |
| `for %%A in (*.txt) do ( ... )` | Repite por cada archivo `.txt` en la carpeta. | `for %%A in (*.log) do del "%%A"` |
| `for /L %%N in (1,1,10) do ( ... )` | Bucle numérico del 1 al 10. | `for /L %%N in (1,1,5) do echo %%N` |
| `for /f "tokens=*" %%L in ('comando') do ( ... )` | Procesa resultado de un comando línea por línea. | `for /f "tokens=*" %%L in ('dir /b') do echo %%L` |
| `for /f "tokens=1,2 delims= " %%A in ("texto") do (...)` | Divide línea en columnas por espacio. | `for /f "tokens=1,2 delims= " %%A in ("Hola Mundo") do echo %%A` |
| `goto etiqueta` | Salta a una etiqueta del script. | `goto fin` |
| `:etiqueta` | Define punto de destino para `goto`. | `:fin` |
| `call "otro.bat"` | Ejecuta otro `.bat` y vuelve al original. | `call "modulo.bat"` |
| `setlocal enabledelayedexpansion` | Permite cambiar variables dentro de bucles (`!var!`). | Al inicio del script |
| `!var!` | Usa variable dinámica dentro de `for` o `if`. | `set /a cont=!cont!+1` |
| `set /p var="Pregunta: "` | Pide input al usuario y guarda en variable. | `set /p nombre="Tu nombre: "` |
| `choice /c YNC /m "Elije"` | Menú rápido: Y/N/C. Devuelve `%errorlevel%`. | `if %errorlevel%==1 echo Si` |

---

## 🔴 REDIRECCIONAMIENTO Y FILTROS

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `> "archivo"` | Guarda salida en archivo (sobrescribe). | `ipconfig > "red.txt"` |
| `>> "archivo"` | Agrega salida al archivo (conserva lo anterior). | `echo Fin >> "red.txt"` |
| `>nul` | Oculta salida exitosa del comando. | `chcp 65001 >nul` |
| `2>nul` | Oculta mensajes de error. | `dir C:\NoExiste 2>nul` |
| `> "archivo" 2>&1` | Guarda salida Y errores juntos. | `comando > "log.txt" 2>&1` |
| `\| more` | Muestra resultado paginado (presiona espacio). | `systeminfo \| more` |
| `\| findstr "texto"` | Filtra líneas que contengan el texto. | `tasklist \| findstr "chrome"` |
| `\| findstr /C:"texto exacto"` | Filtra texto exacto con espacios. | `systeminfo \| findstr /C:"Windows 10"` |
| `\| findstr /V "texto"` | Muestra líneas que NO contienen el texto. | `tasklist \| findstr /V "System"` |
| `\| find /c /v ""` | Cuenta líneas totales del resultado. | `dir /s /b \| find /c /v ""` |
| `&&` | Ejecuta siguiente comando SOLO si el anterior tuvo éxito. | `mkdir "C:\Test" && echo OK` |
| `\|\|` | Ejecuta siguiente comando SOLO si el anterior FALLÓ. | `mkdir "C:\Test" \|\| echo Error` |

---

## 🚀 POWERDESK DESDE BAT (Superpoderes)

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `powershell -Command "..."` | Ejecuta código PowerShell directo desde `.bat`. | `powershell -Command "Get-Date"` |
| `powershell -ExecutionPolicy Bypass -Command "..."` | Evita restricciones de ejecución de scripts. | `powershell -ExecutionPolicy Bypass -Command "..."` |
| `powershell -WindowStyle Hidden -Command "..."` | Ejecuta PowerShell sin ventana visible. | Para monitoreo silencioso |
| `powershell Compress-Archive -Path "Carpeta" -DestinationPath "zip.zip"` | Comprime carpeta a ZIP. | `powershell Compress-Archive -Path "C:\Logs" -DestinationPath "C:\Logs.zip"` |
| `powershell Expand-Archive -Path "zip.zip" -DestinationPath "C:\Destino"` | Descomprime ZIP. | `powershell Expand-Archive -Path "app.zip" -DestinationPath "C:\App"` |
| `powershell Get-Process` | Lista procesos (alternativa a tasklist). | `powershell Get-Process \| findstr "chrome"` |
| `powershell Stop-Process -Name "chrome"` | Mata proceso por nombre. | `powershell Stop-Process -Name "notepad"` |
| `powershell Get-AppxPackage` | Apps instaladas de Microsoft Store. | `> "store_apps.txt"` |
| `powershell Get-ComputerInfo` | Info mega completa del sistema. | `> "info_pc.txt"` |
| `powershell Test-Connection -ComputerName PC01 -Count 2` | Ping desde PowerShell. | `powershell Test-Connection -ComputerName 8.8.8.8 -Count 2` |
| `powershell Invoke-WebRequest -Uri "URL" -OutFile "archivo"` | Descarga archivo (alternativa a curl). | `powershell Invoke-WebRequest -Uri "https://..." -OutFile "C:\app.exe"` |
| `powershell Add-Type ... [DllImport("user32.dll")] ... GetForegroundWindow()` | Obtiene ventana activa del usuario. | Ver ejemplo en sección 6 del script maestro anterior |

---

## 📦 INSTALACIÓN Y DESPLIEGUE

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `msiexec /i "app.msi" /quiet /norestart` | Instala .MSI sin ventanas ni reinicio. | `msiexec /i "7z.msi" /quiet /norestart` |
| `msiexec /x "app.msi" /quiet` | Desinstala .MSI silenciosamente. | `msiexec /x "app.msi" /quiet` |
| `msiexec /i "app.msi" /log "inst.log"` | Instala y guarda log detallado. | `msiexec /i "app.msi" /quiet /log "C:\inst.log"` |
| `"setup.exe" /S` | Instalador EXE silencioso (común). | `"FirefoxSetup.exe" /S` |
| `"setup.exe" /silent` | Otro parámetro de silencio. | `"ChromeSetup.exe" /silent` |
| `"setup.exe" /verysilent` | Máximo silencio (Inno Setup). | `"app.exe" /verysilent` |
| `psexec \\PC01 -s -d "comando"` | Ejecuta comando en PC remota como SYSTEM. | `psexec \\PC01 -s -d msiexec /i "C:\app.msi" /quiet` |
| `psexec \\PC01 -u admin -p pass "comando"` | Ejecuta remoto con credenciales. | `psexec \\PC01 -u admin -p 1234 cmd /c "ipconfig"` |

> **PsExec** requiere descargar Sysinternals Suite.

---

## 🎯 ESPECIALES PARA CONTROL Y BLOQUEO

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `taskkill /f /im explorer.exe && start explorer.exe` | Reinicia el escritorio de Windows. | Útil para refrescar políticas |
| `shutdown /r /t 0 /f` | Reinicia PC inmediatamente. `/f` fuerza cierre de apps. | `shutdown /r /t 0 /f` |
| `shutdown /s /t 0 /f` | Apaga PC inmediatamente. | `shutdown /s /t 0 /f` |
| `shutdown /a` | Cancela apagado/reinicio programado. | `shutdown /a` |
| `rundll32.exe user32.dll,LockWorkStation` | Bloquea la pantalla de Windows. | `rundll32.exe user32.dll,LockWorkStation` |
| `tsdiscon` | Desconecta sesión de Escritorio Remoto. | `tsdiscon` |
| `logoff` | Cierra sesión del usuario actual. | `logoff` |
| `msg * "Mensaje"` | Envía mensaje emergente a todos los usuarios del PC. | `msg * "Reinicio programado en 5 min"` |
| `runas /user:admin "comando"` | Ejecuta comando como otro usuario. | `runas /user:DOMINIO\admin "cmd"` |
| `net share` | Carpetas compartidas del PC. | `> "shares.txt"` |
| `net use` | Unidades de red conectadas. | `> "unidades_red.txt"` |
| `net session` | Sesiones abiertas al PC desde red. | `> "sesiones_red.txt"` |

---

## 🖼️ CAPTURAS Y VISUALES (vía PowerShell)

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `powershell Add-Type -AssemblyName System.Windows.Forms; [System.Windows.Forms.SendKeys]::SendWait('%{F4}')` | Envía teclas al programa activo (Alt+F4). | Cierra ventana activa |
| `powershell -Command "(Get-WmiObject Win32_Process -Filter \"Name='chrome.exe'\").Terminate()"` | Mata proceso por WMI. | Alternativa a taskkill |
| `powershell -Command "Get-WmiObject Win32_ComputerSystem \| Select Manufacturer,Model"` | Marca y modelo del PC. | `> "modelo.txt"` |
| `powershell -Command "Get-EventLog -LogName System -Newest 20"` | Últimos 20 eventos del sistema. | `> "eventos.txt"` |
| `powershell -Command "Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4624} -MaxEvents 10"` | Últimos inicios de sesión exitosos. | Auditoría de accesos |

---

## 🧩 VARIABLES ESPECIALES ÚTILES

| Variable | Qué contiene |
|----------|--------------|
| `%computername%` | Nombre del equipo en red |
| `%username%` | Usuario que ejecuta el script |
| `%userdomain%` | Dominio o grupo de trabajo |
| `%date%` | Fecha actual (formato regional) |
| `%time%` | Hora actual |
| `%cd%` | Carpeta desde donde se ejecuta el script |
| `%~dp0` | Carpeta donde está el archivo `.bat` (útil para rutas relativas) |
| `%temp%` | Carpeta temporal del usuario (`C:\Users\X\AppData\Local\Temp`) |
| `%tmp%` | Igual que `%temp%` |
| `%systemroot%` | `C:\Windows` |
| `%programfiles%` | `C:\Program Files` |
| `%programfiles(x86)%` | `C:\Program Files (x86)` |
| `%appdata%` | `C:\Users\X\AppData\Roaming` |
| `%localappdata%` | `C:\Users\X\AppData\Local` |
| `%public%` | `C:\Users\Public` |
| `%random%` | Número aleatorio entre 0 y 32767 |
| `%errorlevel%` | Código de salida del último comando (0 = éxito) |
| `%cmdextversion%` | Versión de extensiones del CMD |
| `%highestnumanodenumber%` | Número de procesadores lógicos |

---

## 🗺️ MAPA DE FLUJO PARA ARMAR TU SCRIPT

```
1. CABECERA
   @echo off
   setlocal enabledelayedexpansion
   chcp 65001 >nul

2. VARIABLES
   set "carpeta=C:\Logs\%computername%_%date:/=-%"
   set "archivo=%carpeta%\log.txt"

3. PREPARAR
   mkdir "%carpeta%" 2>nul

4. AUDITAR / CONTROLAR / MONITOREAR
   [comando] > "%archivo%"
   [comando] >> "%archivo%"
   [comando] 2>nul

5. DECISIONES
   if exist "archivo" ( ... ) else ( ... )
   for %%A in (...) do ( ... )

6. FINALIZAR
   echo [✓] Listo
   pause / exit /b 0
```

---

Con esta tabla puedes **combinar cualquier comando con redirección (`>`, `>>`, `2>nul`) y lógica (`if`, `for`)** para crear scripts infinitos: desde un simple verificador de red hasta un panel de control remoto completo para toda tu empresa.

¿Quieres que te arme una **plantilla base vacía** con la estructura lista para que solo copies y pegues comandos de esta tabla?
