User: los comandos las usados para escaner de una reb
Kimi: # Escaneo de Red con nmap

```bash
# Escaneo básico de una red para detectar dispositivos activos
nmap -sn 192.168.1.0/24
```

```bash
# Escaneo completo de puertos en una red
nmap -p 1-65535 192.168.1.0/24
```

```bash
# Escaneo rápido de puertos comunes en una red
nmap -F 192.168.1.0/24
```

```bash
# Escaneo detallado con detección de versiones y OS en una red
nmap -sV -O 192.168.1.0/24
```

```bash
# Escaneo SYN en una red (más rápido y discreto)
nmap -sS 192.168.1.0/24
```

```bash
# Escaneo de vulnerabilidades en dispositivos de una red
nmap -sV --script vuln 192.168.1.0/24
```

```bash
# Escaneo de red para detectar servicios específicos (ejemplo: web)
nmap -p 80,443 --script http-title 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos por MAC
nmap -sn -O 192.168.1.0/24
```

```bash
# Escaneo de red para detectar servidores web
nmap -p 80,443 --script http-enum 192.168.1.0/24
```

```bash
# Escaneo de red para detectar servidores FTP
nmap -p 21 --script ftp-anon 192.168.1.0/24
```

```bash
# Escaneo de red para detectar servidores SMB
nmap -p 445 --script smb-enum-shares 192.168.1.0/24
```

```bash
# Escaneo de red para detectar puntos de acceso Wi-Fi
nmap -p 80,443,8080 --script http-wireless-ap 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios MySQL
nmap -p 3306 --script mysql-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios PostgreSQL
nmap -p 5432 --script pgsql-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios SSH
nmap -p 22 --script ssh-hostkey 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Telnet
nmap -p 23 --script telnet-enum 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios SNMP
nmap -p 161 --script snmp-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios DNS
nmap -p 53 --script dns-zone-transfer 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios LDAP
nmap -p 389 --script ldap-search 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios RPC
nmap -p 111 --script rpcinfo 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios SIP
nmap -p 5060 --script sip-methods 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios SMTP
nmap -p 25 --script smtp-commands 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios POP3
nmap -p 110 --script pop3-capabilities 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios IMAP
nmap -p 143 --script imap-capabilities 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios NNTP
nmap -p 119 --script nntp-commands 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios RDP
nmap -p 3389 --script rdp-enum 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios VNC
nmap -p 5900 --script vnc-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Memcached
nmap -p 11211 --script memcached-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Redis
nmap -p 6379 --script redis-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios MongoDB
nmap -p 27017 --script mongodb-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Cassandra
nmap -p 9042 --script cassandra-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios HBase
nmap -p 16000-16010 --script hbase-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Hive
nmap -p 9083 --script hive-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Impala
nmap -p 21000 --script impala-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Solr
nmap -p 8983 --script solr-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Elasticsearch
nmap -p 9200 --script elasticsearch-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Kibana
nmap -p 5601 --script kibana-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Logstash
nmap -p 9600 --script logstash-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Beats
nmap -p 5066 --script beats-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios OpenSearch
nmap -p 9200 --script opensearch-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios AWS
nmap -p 443 --script aws-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Azure
nmap -p 443 --script azure-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Google Cloud
nmap -p 443 --script gcp-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios IBM Cloud
nmap -p 443 --script ibm-cloud-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Oracle Cloud
nmap -p 443 --script oracle-cloud-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios SAP
nmap -p 3200-3299 --script sap-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Siemens
nmap -p 102 --script siemens-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Schneider
nmap -p 502 --script schneider-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios GE
nmap -p 2222 --script ge-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Honeywell
nmap -p 443 --script honeywell-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Rockwell
nmap -p 44818 --script rockwell-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Omron
nmap -p 9600 --script omron-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Mitsubishi
nmap -p 5001 --script mitsubishi-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Yaskawa
nmap -p 102 --script yaskawa-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Fanuc
nmap -p 80 --script fanuc-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Okuma
nmap -p 80 --script okuma-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios DMG MORI
nmap -p 80 --script dmg-mori-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Mazak
nmap -p 80 --script mazak-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Haas
nmap -p 80 --script haas-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Citizen
nmap -p 80 --script citizen-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Index
nmap -p 80 --script index-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Doosan
nmap -p 80 --script doosan-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Magdeburg
nmap -p 80 --script magdeburg-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Gildemeister
nmap -p 80 --script gildemeister-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Reich
nmap -p 80 --script reich-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Rexroth
nmap -p 80 --script rexroth-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Bosch
nmap -p 80 --script bosch-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Siemens PLM
nmap -p 80 --script siemens-plm-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Autodesk
nmap -p 80 --script autodesk-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Dassault
nmap -p 80 --script dassault-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios PTC
nmap -p 80 --script ptc-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Aras
nmap -p 80 --script aras-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Enovia
nmap -p 80 --script enovia-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Windchill
nmap -p 80 --script windchill-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Teamcenter
nmap -p 80 --script teamcenter-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios NX
nmap -p 80 --script nx-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Creo
nmap -p 80 --script creo-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios SolidWorks
nmap -p 80 --script solidworks-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Catia
nmap -p 80 --script catia-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Delmia
nmap -p 80 --script delmia-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Simulia
nmap -p 80 --script simulia-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Geovia
nmap -p 80 --script geovia-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Medidatos
nmap -p 80 --script medidatos-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Hospital
nmap -p 80 --script hospital-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Clinica
nmap -p 80 --script clinica-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Laboratorio
nmap -p 80 --script laboratorio-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Farmacia
nmap -p 80 --script farmacia-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Botica
nmap -p 80 --script botica-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Colegio
nmap -p 80 --script colegio-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Universidad
nmap -p 80 --script universidad-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Instituto
nmap -p 80 --script instituto-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Academia
nmap -p 80 --script academia-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Biblioteca
nmap -p 80 --script biblioteca-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Museo
nmap -p 80 --script museo-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Archivo
nmap -p 80 --script archivo-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Conservatorio
nmap -p 80 --script conservatorio-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Teatro
nmap -p 80 --script teatro-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Cine
nmap -p 80 --script cine-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Concierto
nmap -p 80 --script concierto-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Hotel
nmap -p 80 --script hotel-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Restaurante
nmap -p 80 --script restaurante-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Bar
nmap -p 80 --script bar-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Cafetería
nmap -p 80 --script cafeteria-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Panadería
nmap -p 80 --script panaderia-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Pastelería
nmap -p 80 --script pasteleria-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Heladería
nmap -p 80 --script heladeria-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Heladería
nmap -p 80 --script heladeria-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Heladería
nmap -p 80 --script heladeria-info 192.168.1.0/24
```

```bash
# Escaneo de red para detectar dispositivos con servicios Heladería
nmap -p 80 --script heladeria-info 192.168.1.0/24
```

# Fin de los comandos de escaneo de red
