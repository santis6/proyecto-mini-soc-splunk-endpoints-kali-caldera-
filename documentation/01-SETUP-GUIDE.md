# 🚀 Setup Guide - Mini SOC Lab


# 📋 ¿Qué es este laboratorio?
---

Un entorno SOC completo con 5 máquinas virtuales que simula un centro de operaciones de seguridad real. Perfecto para portafolio y aprendizaje práctico.

# 🛠️ Prerrequisitos Necesarios
-----------------------------

### Hardware (Mínimo):

-   RAM: 16GB (usa 2GB por VM + 6GB para host)

-   Disco: 150GB SSD recomendado

-   CPU: 4 núcleos con virtualización habilitado

### Software:

1.  VirtualBox 7.+ - Descarga desde [virtualbox.org](https://www.virtualbox.org/)

2.  Git - Para clonar repositorios

3.  7-Zip - Para extraer archivos

### Conocimientos:

-   Básico de línea de comandos

-   Conceptos básicos de redes

-   Saber instalar un sistema operativo

## 🌐 Paso 1: Configurar la Red
----------------------------

### Por qué esta configuración:

-   Host-Only: Aislamiento total, sniffing desde host, sin riesgo de fuga

-   NAT: Solo para máquinas que necesitan internet (Splunk, Kali)

### Cómo configurar:

#### 1\. Crear red Host-Only:


```
VirtualBox → Archivo → Herramientas → Administrador de red
→ Crear → IP: 192.168.56.1 → Máscara: 255.255.255.0
```

#### 2\. Asignar configuración por VM:
```
| VM | Adaptador 1 | Adaptador 2 | IP |
| --- | --- | --- | --- |
| Splunk Server | NAT | Host-Only | 192.168.56.40 |
| Windows Client | Host-Only | - | 192.168.56.20 |
| Ubuntu Client | Host-Only | - | 192.168.56.30 |
| Kali Linux | NAT | Host-Only | 192.168.56.10 |
| Caldera Server | Host-Only | - | 192.168.56.50 |
```
#### En el caso de Ubuntu Server, Ubuntu Desktop y Kali crear el siguiente archivo `.yaml` para configurar manualmente la red.
#### En el caso de Windows, configurar la red manual desde -> Configuracion -> Red e Internet -> Ethernet -> Asignación de IP -> Manual

```
sudo nano /etc/netplan/00-installer-config.yaml
```
```
network:
  version: 2
  ethernets:
    enp0s3: # Reemplaza 'enp0s3' con el nombre de tu interfaz
      dhcp4: false
      addresses:
      - 192.168.56.#/24 # Aquí va la ip, recuerden revisar la tabla de configuración para ver que IP asignar a cada endpoint
      routes:
        - to: default
          via: 192.168.56.1 # Puerta de Enlace o Gateway
      nameservers:
          addresses: # Servidores DNS de Google
           - 8.8.8.8
           - 8.8.4.4
```
> NOTA: Corroborar que en el caso de Ubuntu Server y Kali (Que requieren de internet) agregar la interfaz de NAT y interfaz de Host-Only Adapter
> Mayormente la interfaz NAT es `enp0s3` y la Host-Only Adapter  `enp0s8`
#### Posteriormente aplicamos
```
sudo netplan apply
```
#### Reiniciamos la VM y corroboramos si el cambio fue aplicado correctamente:
```
ip addr
```
<img width="1359" height="718" alt="network setup" src="https://github.com/user-attachments/assets/2e6d9798-6574-4be1-92fd-b6968f15161d" />`


## 🖥️ Paso 2: Instalar las Máquinas Virtuales
-------------------------------------------

### Splunk Server (El corazón del SOC)

-   SO: Ubuntu Server 22.04 LTS

-   Config: 4GB RAM, 40GB disco, 2 CPUs

-   Usuario: `$USER$` / `$PASSWORD$`

-   IP estática: `192.168.56.40/24`

-   Importante: Durante instalación, configura red manual con gateway `192.168.56.1`

-   NOTA: Reemplazar $USER$ por su usuario de preferencia al igual que $PASSWORD$ con su contraseña.

#### Post-instalación esencial:


```
sudo apt update && sudo apt upgrade -y
sudo apt install -y net-tools curl wget git
```

### Windows 11 Client (Endpoint)

-   Config: 2GB RAM, 30GB disco

-   IP: `192.168.56.20/24`

-   Usuario: `$USER$` / `$PASSWORD$`

-   Red: Configurar IP estática en Configuración → Red

### Ubuntu Desktop Client

-   Config: 2GB RAM, 25GB disco

-   IP: `192.168.56.30/24`

-   Mismo proceso que Splunk pero con interfaz gráfica

### Kali Linux (Red Team)

-   Descarga: Imagen de VirtualBox desde [kali.org](https://www.kali.org/get-kali/)

-   Config: 2GB RAM, 20GB disco

-   Credenciales: `$USER$` / `$PASSWORD$`

-   Red: Configurar IP estática en `/etc/network/interfaces`

### Caldera Server

-   SO: Ubuntu Server 22.04 (igual que Splunk)

-   Config: 2GB RAM, 20GB disco

-   IP: `192.168.56.50/24`

<img width="529" height="427" alt="5 vms creadas" src="https://github.com/user-attachments/assets/f1c39013-e47d-4d1f-a434-31a1706e398b" />



# ⚡ Paso 3: Instalar y Configurar Splunk
--------------------------------------

## Instalación (en Splunk Server):


#### Descargar e instalar
> Buscar la última versión desde el siguiente link y copiarlo -> [Splunk Enterprise](https://www.splunk.com/en_us/download/splunk-enterprise.html?locale=en_us)
```
cd /tmp
```
```
wget -O splunk-9.1.1.deb "URL_DESCARGA_SPLUNK"
```
```
sudo dpkg -i splunk-9.1.1.deb
```

#### Configurar inicio automático
```
sudo /opt/splunk/bin/splunk start --accept-license
```
```
sudo /opt/splunk/bin/splunk enable boot-start
```
#### Acceso Inicial:

1.  Navegador: `https://192.168.56.40:8000`

2.  Login: `$USER$` / `$PASSSWORD$`

3.  Cambia contraseña: Elige una

#### Configuración Básica:

1.  Data Inputs → TCP → Puerto `9997`

2.  Data Inputs → Files & Directories → `/var/log/*`

3.  Guarda y aplica


<img width="1359" height="767" alt="splunk primer vistazo" src="https://github.com/user-attachments/assets/f08b9f12-a699-4496-b8cc-502a20e2e773" />



# 🔄 Paso 4: Configurar Forwarders
--------------------------------

### En Windows Client:



#### 1. Descargar [Universal Forwarder](https://www.splunk.com/en_us/download/universal-forwarder.html?locale=en_us)
#### 2. Instalar con: 
```
msiexec.exe /i splunkforwarder.msi AGREETOLICENSE=Yes /quiet
```
#### 3. Configurar:
```
& "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" add forward-server 192.168.56.40:9997
& "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" add monitor "C:\Windows\System32\winevt\Logs"
```

### En Ubuntu Client:



#### Descargar [Universal Forwarder](https://www.splunk.com/en_us/download/universal-forwarder.html?locale=en_us)
#### Instalar forwarder
```
sudo dpkg -i splunkforwarder.deb
```
```
sudo /opt/splunkforwarder/bin/splunk start --accept-license
```
#### Configurar
```
sudo /opt/splunkforwarder/bin/splunk add forward-server 192.168.56.40:9997
```
```
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/
```
<img width="1359" height="640" alt="forwarder ubuntu" src="https://github.com/user-attachments/assets/b53d86fd-b7f3-4ac7-8477-2175b2f0f2ad" />


<img width="1359" height="644" alt="forwarder windows" src="https://github.com/user-attachments/assets/39dd7955-0957-4202-8dfa-330de1dffb21" />



🔥 Paso 5: Instalar MITRE Caldera
---------------------------------

### En Caldera Server:

bash

# Instalar dependencias
sudo apt update
sudo apt install -y docker.io docker-compose git

# Clonar y ejecutar Caldera
git clone https://github.com/mitre/caldera.git --recursive
cd caldera
docker-compose up -d

### Acceso:

-   URL: `http://192.168.56.50:8888`

-   Usuario: `admin`

-   Contraseña: `admin` (cambia inmediatamente)

### Configurar Agentes:

1.  En Caldera: Campaigns → Agents

2.  Generate Agent para Windows y Linux

3.  Descargar y ejecutar en los endpoints

📸 Toma captura de: Dashboard de Caldera con agentes conectados

✅ Paso 6: Validar el Laboratorio
--------------------------------

### Prueba 1: Conectividad

bash

# Desde Kali, prueba todas las IPs:
ping 192.168.56.20  # Windows
ping 192.168.56.30  # Ubuntu
ping 192.168.56.40  # Splunk
ping 192.168.56.50  # Caldera

### Prueba 2: Logs en Splunk

splunk

# En Splunk Search, ejecuta:
index=* | stats count by host
# Deberías ver win-client y linux-client

### Prueba 3: Simulación Simple

bash

# Desde Kali, ejecuta un escaneo básico
nmap -sS 192.168.56.20

# Verifica en Splunk si aparece la actividad

⚠️ Problemas Comunes y Soluciones
---------------------------------

### 1\. "VT-x/AMD-V no habilitado"

-   Solución: Entrar a BIOS/UEFI → Habilitar Virtualization Technology

### 2\. VMs no se comunican

powershell

# En Windows VMs:
netsh advfirewall set allprofiles state off

# En Ubuntu VMs:
sudo ufw disable

### 3\. Splunk no recibe logs

bash

# Verificar que el puerto esté abierto
sudo netstat -tulpn | grep 9997
# Debería mostrar LISTEN

### 4\. Caldera no inicia

bash

# Verificar logs de Docker
cd caldera
docker-compose logs

🎯 Lista de Verificación Final
------------------------------

-   5 VMs creadas con configuraciones correctas

-   IPs estáticas asignadas (192.168.56.20-50)

-   Splunk accesible en [https://192.168.56.40:8000](https://192.168.56.40:8000/)

-   Forwarders enviando logs a Splunk

-   Caldera accesible en [http://192.168.56.50:8888](http://192.168.56.50:8888/)

-   Agentes de Caldera conectados

-   Ping funciona entre todas las máquinas

-   Capturas de pantalla guardadas

📁 Estructura de Capturas Recomendada
-------------------------------------

text

documentation/images/
├── virtualbox/
│   ├── network-config.png
│   └── all-vms-created.png
├── splunk/
│   ├── first-login.png
│   └── receiving-logs.png
└── caldera/
    └── agents-active.png

🚀 ¿Y ahora qué?
----------------

Tu Mini SOC está listo. Ahora puedes:

1.  Practicar con escenarios pre-configurados

2.  Crear dashboards personalizados en Splunk

3.  Simular ataques reales con Caldera

4.  Documentar tu aprendizaje para tu portafolio

Recuerda: Este laboratorio es solo para educación. Nunca lo conectes a internet real ni uses datos reales.

* * * * *

¿Listo para continuar? El siguiente paso es configurar alertas y dashboards en Splunk para comenzar a monitorear.
