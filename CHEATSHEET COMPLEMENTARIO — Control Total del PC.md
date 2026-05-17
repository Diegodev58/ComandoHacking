Aquí tienes la **parte final y más avanzada** de la guía. Cubre todo lo que faltaba: **audio, webcam, cierre de sesión, desinstalación, bloqueo físico del equipo, impresoras, eventos,** y herramientas externas que se invocan desde `.bat` para tener un control total.

---

# 🔊📷🚪 CHEATSHEET COMPLEMENTARIO — Control Total del PC

---

## 🔊 AUDIO Y VOLUMEN (Nativo + NirCmd)

CMD nativo no controla audio directamente, pero puedes usar **NirCmd** (exe portable de 43KB, sin instalar) o PowerShell. Descarga `nircmd.exe` y ponlo en `C:\Windows\` o junto a tu `.bat`.

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `nircmd.exe setsysvolume 65535` | Volumen general al **100%** (rango 0-65535). | `nircmd.exe setsysvolume 65535` |
| `nircmd.exe setsysvolume 32768` | Volumen al **50%**. | `nircmd.exe setsysvolume 32768` |
| `nircmd.exe setsysvolume 0` | Volumen a **0%** (silenciar). | `nircmd.exe setsysvolume 0` |
| `nircmd.exe mutesysvolume 1` | **Mutea** el audio (1=on, 0=off). | `nircmd.exe mutesysvolume 1` |
| `nircmd.exe mutesysvolume 0` | **Desmutea** el audio. | `nircmd.exe mutesysvolume 0` |
| `nircmd.exe changesysvolume -2000` | Baja volumen **-2000** unidades (bajito). | `nircmd.exe changesysvolume -5000` |
| `nircmd.exe changesysvolume 2000` | Sube volumen **+2000** unidades. | `nircmd.exe changesysvolume 5000` |
| `nircmd.exe setappvolume "chrome.exe" 0` | Silencia **solo Chrome**. | `nircmd.exe setappvolume "chrome.exe" 0` |
| `nircmd.exe setappvolume "chrome.exe" 1` | Restaura volumen de Chrome al 100%. | `nircmd.exe setappvolume "chrome.exe" 1` |
| `nircmd.exe setsysvolume 65535 "Micrófono"` | Sube volumen del **micrófono** al 100%. | `nircmd.exe setsysvolume 65535 "Micrófono"` |
| `nircmd.exe mutesysvolume 1 "Micrófono"` | **Mutea micrófono** (evita que usuario hable). | `nircmd.exe mutesysvolume 1 "Micrófono"` |
| `nircmd.exe speak text "Hola usuario"` | Hace que Windows **hable** por los parlantes (TTS). | `nircmd.exe speak text "Atencion, reinicio programado"` |
| `powershell -Command "(New-Object -ComObject WScript.Shell).SendKeys([char]174)"` | Baja volumen con tecla multimedia (si el teclado la tiene). | Para scripts sin NirCmd |
| `powershell -Command "(New-Object -ComObject WScript.Shell).SendKeys([char]175)"` | Sube volumen con tecla multimedia. | Alternativa portable |

> **Tip:** Si no puedes usar NirCmd, sube `nircmd.exe` a `\\Servidor\Compartido\` y llámalo con ruta absoluta: `"\\Servidor\Tools\nircmd.exe" setsysvolume 0`.

---

## 📷 WEBCAM / CÁMARA

CMD no tiene comando nativo para cámara, pero puedes usar **PowerShell + .NET**, **ffmpeg** (exe portable), o **nircmd**.

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `nircmd.exe execmd ffplay -f dshow -i video="NombreCamara"` | Muestra preview de la webcam (requiere ffmpeg). | Para verificar que funciona |
| `ffmpeg -f dshow -i video="USB Camera" -vframes 1 "foto.jpg"` | Toma **1 foto** con la webcam y la guarda. | `ffmpeg -f dshow -i video="USB Camera" -vframes 1 "C:\Audit\foto.jpg"` |
| `ffmpeg -f dshow -i video="USB Camera" -t 10 "video.avi"` | Graba **10 segundos** de video de la webcam. | `ffmpeg -f dshow -i video="USB Camera" -t 10 "C:\Audit\clip.avi"` |
| `ffmpeg -list_devices true -f dshow -i dummy` | Lista **nombres exactos** de cámaras y micrófonos. | Antes de usar, ejecuta esto para saber el nombre |
| `powershell -Command "Add-Type -AssemblyName System.Windows.Forms; $c=New-Object System.Windows.Forms.WebBrowser; ..."` | No sirve para webcam nativa. | Mejor usar ffmpeg |
| `powershell -Command "camera = Get-PnpDevice -Class Camera"` | Lista cámara como dispositivo Plug & Play. | `Get-PnpDevice -Class Camera \| Format-List` |

> **Script completo para foto con webcam:**
```bat
@echo off
set "cam=USB Camera"
set "salida=C:\ControlRemoto\%computername%_webcam_%time::=-%.jpg"

:: Listar dispositivos primero (opcional)
:: ffmpeg -list_devices true -f dshow -i dummy 2>&1 | findstr "Camera"

echo [+] Capturando foto...
ffmpeg -y -f dshow -i video="%cam%" -vframes 1 "%salida%" 2>nul

if exist "%salida%" (
    echo [✓] Foto guardada: %salida%
) else (
    echo [X] No se detecto webcam o esta en uso.
)
pause
```

---

## 🚪 CONTROL DE SESIÓN, BLOQUEO Y ENERGÍA

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `logoff` | Cierra sesión del usuario **inmediatamente**. | `logoff` |
| `logoff 1` | Cierra sesión del usuario en sesión 1 (Terminal Server). | `logoff 1` |
| `rundll32.exe user32.dll,LockWorkStation` | **Bloquea la pantalla** (Win+L). El usuario debe poner contraseña. | `rundll32.exe user32.dll,LockWorkStation` |
| `tsdiscon` | Desconecta sesión de Escritorio Remoto (no cierra apps). | `tsdiscon` |
| `shutdown /s /t 0 /f` | **Apaga** el PC ahora. `/f` fuerza cierre de apps. | `shutdown /s /t 0 /f` |
| `shutdown /r /t 0 /f` | **Reinicia** el PC ahora. | `shutdown /r /t 0 /f` |
| `shutdown /r /t 60 /c "Mensaje"` | Reinicia en **60 segundos** con mensaje visible. | `shutdown /r /t 60 /c "Actualizando, no apague"` |
| `shutdown /a` | **Cancela** apagado/reinicio programado. | `shutdown /a` |
| `shutdown /s /t 60 /m \\PC01` | Apaga **PC remota** en red en 60 seg. | `shutdown /s /t 0 /m \\PC-VENTAS1` |
| `shutdown /r /t 0 /m \\PC01 /f` | Reinicia **PC remota** forzosamente. | `shutdown /r /t 0 /m \\PC-RRHH1 /f` |
| `nircmd.exe exitwin poweroff` | Apaga PC (alternativa a shutdown). | `nircmd.exe exitwin poweroff` |
| `nircmd.exe exitwin reboot` | Reinicia PC. | `nircmd.exe exitwin reboot` |
| `nircmd.exe exitwin logoff` | Cierra sesión. | `nircmd.exe exitwin logoff` |
| `nircmd.exe exitwin lockws` | Bloquea estación de trabajo. | `nircmd.exe exitwin lockws` |
| `nircmd.exe standby` | Modo **suspender/standby**. | `nircmd.exe standby` |
| `powercfg /hibernate on` | Activa hibernación. | `powercfg /hibernate on` |
| `powercfg /hibernate off` | Desactiva hibernación. | `powercfg /hibernate off` |
| `powercfg /setactive SCHEME_MIN` | Cambia a plan de energía **Ahorro de energía**. | `powercfg /setactive SCHEME_MIN` |
| `powercfg /setactive SCHEME_MAX` | Cambia a plan de energía **Alto rendimiento**. | `powercfg /setactive SCHEME_MAX` |
| `powercfg /setactive SCHEME_BALANCED` | Cambia a plan **Equilibrado**. | `powercfg /setactive SCHEME_BALANCED` |
| `powercfg /list` | Muestra todos los planes de energía disponibles. | `powercfg /list` |

---

## ❌ DESINSTALACIÓN DE PROGRAMAS

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `wmic product where "Name like '%%Chrome%%'" get Name` | Busca software por nombre (comodín `%%`). | Busca antes de desinstalar |
| `wmic product where "Name='Google Chrome'" call Uninstall` | **Desinstala** programa por nombre exacto. | `wmic product where "Name='Google Chrome'" call Uninstall` |
| `wmic product where "Name like '%%Java%%'" call Uninstall` | Desinstala TODO lo que contenga "Java". | ⚠️ Cuidado con comodines amplios |
| `msiexec /x "{GUID}" /quiet /norestart` | Desinstala por **GUID** (identificador único del programa). | `msiexec /x "{8A1F5E9A-2B3C...}" /quiet` |
| `msiexec /x "C:\app.msi" /quiet` | Desinstala usando el archivo MSI original. | `msiexec /x "C:\Deploy\app.msi" /quiet` |
| `powershell -Command "Get-WmiObject Win32_Product \| Where-Object {$_.Name -like '*Chrome*'} \| ForEach-Object { $_.Uninstall() }"` | Desinstala vía PowerShell buscando por nombre. | Más potente que wmic |
| `reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall" /s /f "Chrome"` | Busca en registro para encontrar GUID de desinstalación. | `> "busqueda.txt"` |
| `"C:\Program Files\app\uninstall.exe" /S` | Ejecuta desinstalador silencioso del programa. | Depende de cada software |
| `setup.exe /uninstall /silent` | Algunos programas usan este parámetro. | `"C:\Program Files\app\setup.exe" /uninstall /silent` |

> **Truco para encontrar el GUID antes de desinstalar:**
```bat
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall" /s /f "NombrePrograma" > C:\guids.txt
```

---

## 💬 MENSAJES Y COMUNICACIÓN CON EL USUARIO

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `msg * "Texto del mensaje"` | Envía **popup emergente** a todas las sesiones del PC. | `msg * "El tecnico va a controlar su PC. No cierre nada."` |
| `msg /SERVER:PC01 * "Texto"` | Envía mensaje a **PC remota** en la red. | `msg /SERVER:PC-VENTAS1 * "Reinicio en 5 min"` |
| `msg /TIME:10 * "Texto"` | Mensaje que se cierra solo tras **10 segundos**. | `msg /TIME:10 * "Actualizacion lista"` |
| `nircmd.exe infobox "Texto" "Titulo"` | Ventana de información con botón Aceptar. | `nircmd.exe infobox "Soporte remoto activo" "TI Empresa"` |
| `nircmd.exe qboxcom "¿Reiniciar?" "Confirmar" "shutdown /r /t 0"` | Cuadro Sí/No que ejecuta comando si elige Sí. | `nircmd.exe qboxcom "Reiniciar?" "TI" "shutdown /r /t 0"` |
| `nircmd.exe trayballoon "TI" "Soporte iniciado" "shell32.dll,22" 5000` | Notificación tipo globo en la bandeja por **5 seg**. | Alerta sutil al usuario |
| `powershell -Command "Add-Type -AssemblyName System.Windows.Forms; [System.Windows.Forms.MessageBox]::Show('Mensaje','TI')"` | Ventana emergente nativa .NET. | Alternativa sin NirCmd |
| `echo ^G` o `echo ` (Ctrl+G en editor) | Emite **beep** del parlante interno. | `echo ` (alerta sonora) |

---

## 🖨️ IMPRESORAS

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `wmic printer get Name,Default,PortName,Status /format:list` | Lista impresoras instaladas. | `> "impresoras.txt"` |
| `wmic printer where "Default='TRUE'" get Name` | Muestra cuál es la **impresora por defecto**. | `> "default.txt"` |
| `wmic printer where "Name='HP LaserJet'" call SetDefaultPrinter` | Cambia impresora **por defecto**. | `wmic printer where "Name='HP'" call SetDefaultPrinter` |
| `net stop spooler` | **Detiene** el servicio de cola de impresión. | Para limpiar trabajos atascados |
| `net start spooler` | **Inicia** el servicio de cola de impresión. | `net start spooler` |
| `del /q /f /s "%systemroot%\System32\spool\printers\*.*"` | Borra **todos los trabajos** atascados en cola. | Limpia cola de impresión |
| `rundll32.exe printui.dll,PrintUIEntry /dl /n "NombreImpresora"` | **Elimina** impresora del sistema. | `rundll32 printui.dll,PrintUIEntry /dl /n "HP"` |
| `rundll32.exe printui.dll,PrintUIEntry /in /n "\\Servidor\HP"` | **Instala** impresora de red por nombre UNC. | `rundll32 printui.dll,PrintUIEntry /in /n "\\Server\HP"` |
| `powershell -Command "Get-Printer \| Format-Table"` | Lista impresoras vía PowerShell. | `> "printers.txt"` |

---

## 📜 EVENTOS DEL SISTEMA Y LOGS

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `wevtutil qe System /c:10 /f:text` | Últimos **10 eventos** del Sistema en texto plano. | `> "eventos.txt"` |
| `wevtutil qe Security /c:20 /f:text` | Últimos **20 eventos** de Seguridad. | Requiere admin |
| `wevtutil qe Application /c:5 /f:text` | Últimos **5 eventos** de Aplicaciones. | `> "apps.txt"` |
| `wevtutil epl System "C:\System.evtx"` | Exporta **todo el log** del Sistema a archivo `.evtx`. | Para auditoría forense |
| `powershell -Command "Get-EventLog -LogName System -Newest 50"` | 50 eventos recientes vía PowerShell. | `> "eventos50.txt"` |
| `powershell -Command "Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4624,4625} -MaxEvents 20"` | Eventos de **inicio de sesión** exitoso (4624) y fallido (4625). | Auditoría de accesos |
| `powershell -Command "Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4648} -MaxEvents 10"` | Intentos de **logueo explícito** con credenciales. | Detecta accesos sospechosos |

---

## 🧰 HERRAMIENTAS EXTERNAS PORTABLES (para usar desde .bat)

| Herramienta | Para qué sirve | Ejemplo de uso en .bat |
|-------------|----------------|------------------------|
| **NirCmd** | Audio, volumen, apagar, mensajes, screenshots, clics automáticos. | `nircmd.exe savescreenshot "cap.jpg"` |
| **ffmpeg** | Webcam, grabación de pantalla, audio, video. | `ffmpeg -f dshow -i video="Cam" -vframes 1 foto.jpg` |
| **PsExec** | Ejecutar comandos en PCs remotas de la red. | `psexec \\PC01 -s cmd /c "ipconfig"` |
| **ScreenCapture** (NirSoft) | Capturas de pantalla avanzadas. | `screencapture.exe /filename cap.png` |
| **DevCon** (Windows SDK) | Habilitar/deshabilitar dispositivos (webcam, USB). | `devcon disable *USB\VID_*` |
| **Handle** (Sysinternals) | Saber qué proceso tiene abierto un archivo. | `handle.exe "archivo.doc"` |

---

## 🎯 COMANDOS EXTRAS PARA CONTROL TOTAL

| Comando | Descripción corta | Ejemplo útil |
|---------|-------------------|--------------|
| `nircmd.exe savescreenshot "C:\cap.jpg"` | **Screenshot** sin PowerShell (más rápido). | `nircmd.exe savescreenshot "C:\Audit\cap.jpg"` |
| `nircmd.exe savescreenshotwin "C:\cap.jpg" "titulo ventana"` | Captura **solo una ventana** específica. | `nircmd.exe savescreenshotwin "excel.jpg" "Libro1"` |
| `nircmd.exe sendmouse left click` | **Clic izquierdo** automático donde esté el mouse. | Automatización de formularios |
| `nircmd.exe sendkeypress a` | **Presiona tecla** A. | `nircmd.exe sendkeypress ctrl+s` |
| `nircmd.exe setdisplay 1920 1080 32` | Cambia **resolución de pantalla**. | `nircmd.exe setdisplay 1920 1080 32` |
| `nircmd.exe setprocesspriority "chrome.exe" high` | Pone proceso en prioridad **Alta**. | `nircmd.exe setprocesspriority "app.exe" high` |
| `nircmd.exe killprocess "notepad.exe"` | Mata proceso por nombre (alternativa a taskkill). | `nircmd.exe killprocess "notepad.exe"` |
| `nircmd.exe emptybin` | **Vaciar papelera** de reciclaje. | `nircmd.exe emptybin` |
| `nircmd.exe clipboard set "texto"` | Copia texto al **portapapeles**. | `nircmd.exe clipboard set "Contraseña: 1234"` |
| `nircmd.exe monitor off` | **Apaga el monitor** (la PC sigue encendida). | `nircmd.exe monitor off` |
| `nircmd.exe monitor on` | **Enciende el monitor**. | `nircmd.exe monitor on` |
| `nircmd.exe flashwin "titulo" 10` | Hace **parpadear** la ventana en la barra 10 veces. | Llamar atención del usuario |
| `nircmd.exe dlg "Clase" "Titulo" click "Aceptar"` | Hace **clic automático** en botón de ventana. | Automatizar instaladores |
| `nircmd.exe win hide ititle "Calculadora"` | **Oculta** la ventana de la calculadora. | `nircmd.exe win hide ititle "Notepad"` |
| `nircmd.exe win show ititle "Calculadora"` | **Muestra** ventana oculta. | `nircmd.exe win show ititle "Notepad"` |
| `nircmd.exe win close ititle "Advertencia"` | **Cierra** ventana por título sin matar proceso. | Cerrar popups molestos |
| `nircmd.exe win max ititle "Excel"` | **Maximiza** ventana. | `nircmd.exe win max ititle "Excel"` |
| `nircmd.exe win min ititle "Excel"` | **Minimiza** ventana. | `nircmd.exe win min ititle "Excel"` |
| `nircmd.exe win activate ititle "Soporte"` | **Trae al frente** una ventana. | `nircmd.exe win activate ititle "Chat"` |
| `nircmd.exe exec hide "C:\app.exe"` | Ejecuta programa **completamente oculto** (sin ventana). | `nircmd.exe exec hide "C:\monitor.exe"` |
| `nircmd.exe execmd "comando"` | Ejecuta comando CMD sin mostrar ventana negra. | `nircmd.exe execmd "ipconfig > C:\red.txt"` |
| `devcon disable *USB\VID_1234*` | **Deshabilita** dispositivo USB por ID (webcam, pendrive). | Bloquear cámara físicamente |
| `devcon enable *USB\VID_1234*` | **Habilita** dispositivo USB. | Restaurar cámara |

---

## 🗂️ ESTRUCTURA DE CARPETAS RECOMENDADA PARA TU SISTEMA

```
C:\ControlRemoto\
├── Scripts\
│   ├── Auditoria.bat
│   ├── PanelTI.bat
│   ├── Bloquear.bat
│   └── InstalarSilencioso.bat
├── Tools\
│   ├── nircmd.exe
│   ├── ffmpeg.exe
│   ├── psexec.exe
│   └── devcon.exe
├── Logs\
│   └── %computername%\
│       ├── actividad.txt
│       ├── programas.txt
│       └── capturas\
├── Deploy\
│   └── (instaladores .msi y .exe)
└── Reportes\
    └── (archivos finales comprimidos)
```

---

## 🧩 MINI-PLANTILLA BASE PARA ARMAR SCRIPTS RÁPIDOS

Copia esto y solo reemplaza `[COMANDO]` por cualquiera de la tabla:

```bat
@echo off
setlocal enabledelayedexpansion
chcp 65001 >nul
title Script de Control - %computername%
color 0B

set "fecha=%date:/=-%"
set "hora=%time::=-%"
set "hora=%hora: =0%"
set "carpeta=C:\ControlRemoto\%computername%_%fecha%_%hora%"
mkdir "%carpeta%" 2>nul

echo === INICIO: %date% %time% === > "%carpeta%\log.txt"

:: --- AQUI PEGAS TUS COMANDOS ---
[COMANDO] >> "%carpeta%\log.txt" 2>&1
:: -------------------------------

echo === FIN === >> "%carpeta%\log.txt"
echo [✓] Listo. Revisa: %carpeta%
pause
```

---

Con esto ya tienes la **guía completa y definitiva** para crear cualquier tipo de script de control remoto empresarial: desde auditar una PC hasta apagarla, silenciarla, ver por su webcam, desinstalarle programas, enviarle mensajes o bloquearle la pantalla.

¿Te falta algún comando específico o quieres que te arme un **script maestro final** que una TODO (audio + webcam + sesión + desinstalar + instalar + logs + capturas) en un solo panel?
