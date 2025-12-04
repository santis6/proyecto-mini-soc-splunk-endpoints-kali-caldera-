# 🛡️ Mini SOC Lab - Laboratorio de Ciberseguridad
---
## 📌 Índice

-    [🎯 Objetivo del Proyecto](#-objetivo-del-proyecto)

-    [🌲 Arquitectura del Laboratorio](#-arquitectura-del-laboratorio)

-    [🧩 Componentes del Laboratorio](#-componentes-del-laboratorio)

-    [🚀 Flujo de Funcionamiento](#-flujo-de-funcionamiento)

-    [📦 Requisitos](#-requisitos)

-    [📚 Documentación Incluida](#-documentación-incluida)

* * * * *

 ## 🎯 Objetivo del Proyecto 
------------------------

> Mini SOC Lab es un entorno de aprendizaje práctico que simula un Security Operations Center (SOC) completo. Diseñado para desarrolladores de portafolio y estudiantes de ciberseguridad, permite:

-   Practicar monitoreo y análisis de seguridad en un entorno controlado

-   Simular ataques reales usando técnicas MITRE ATT&CK

-   Desarrollar habilidades de detección y respuesta a incidentes

-   Demostrar competencias técnicas para entrevistas de trabajo

-   Crear un proyecto impresionante para GitHub

## Características clave:

-   ✅ SIEM Splunk Enterprise (gratis hasta 500MB/día)

-   ✅ Plataforma de simulación MITRE Caldera

-   ✅ Endpoints Windows 11 + Ubuntu 22.04

-   ✅ Kali Linux para pruebas manuales

-   ✅ Configuración 100% automatizada

-   ✅ Dashboards profesionales pre-configurados

* * * * *

## 🌲 Arquitectura del Laboratorio
-------------------------------

## Diagrama de Red


                    [ Kali Linux ]       [ MITRE Caldera ]
                    (192.168.56.10)      (192.168.56.50)
                            |                    |
                            |--- Ataques ------->|
                            |                    |
                  [ Windows 11 ]          [ Ubuntu 22.04 ]
                  (192.168.56.20)        (192.168.56.30)
                            |                    |
                            |-------- Logs ----->|
                                          |
                                    [ Splunk SIEM ]
                                    (192.168.56.40)

# Especificaciones Técnicas

| Componente | Sistema Operativo | RAM | Disco | IP | Rol |
| --- | --- | --- | --- | --- | --- |
| Splunk-Server | Ubuntu Server 22.04 | 4GB | 40GB | 192.168.56.40 | SIEM Central |
| Windows-Client | Windows 11 Pro | 2GB | 30GB | 192.168.56.20 | Endpoint Windows |
| Linux-Client | Ubuntu Desktop 22.04 | 2GB | 25GB | 192.168.56.30 | Endpoint Linux |
| Caldera-Server | Ubuntu Server 22.04 | 2GB | 20GB | 192.168.56.50 | Simulación APT |
| Kali-Attacker | Kali Linux 2023.4 | 2GB | 20GB | 192.168.56.10 | Ataques manuales |

Red: 192.168.56.0/24 (VirtualBox Internal Network)

* * * * *

# 🧩 Componentes del Laboratorio
------------------------------

## 1\. Splunk Enterprise (SIEM)

-   Versión: 9.0+ (Licencia gratuita de 500MB/día)

-   Funcionalidades:

    -   Colección de logs desde Windows y Linux

    -   Correlación de eventos en tiempo real

    -   Dashboards pre-configurados

    -   Alertas automatizadas

    -   Búsquedas SPL para threat hunting

## 2\. MITRE Caldera (Red Team)

-   Versión: 4.0+

-   Capacidades:

    -   Simulación automatizada de adversarios APT

    -   Campañas pre-configuradas (APT29, FIN6, etc.)

    -   Alineación con MITRE ATT&CK Matrix

    -   Reportes automáticos de actividad

## 3\. Endpoints de Prueba

-   Windows 11 Pro:

    -   Splunk Universal Forwarder instalado

    -   Logs de Windows Event Viewer

    -   PowerShell logging habilitado

    -   Simulación de estación de trabajo corporativa

-   Ubuntu 22.04 Desktop:

    -   Splunk Universal Forwarder configurado

    -   Logs de syslog y auth.log

    -   Servicios comunes (SSH, Apache, etc.)

    -   Scripts de automatización

## 4\. Kali Linux (Red Team Manual) 

-   Versión: 2023.4

-   Herramientas incluidas:

    -   Metasploit Framework

    -   Nmap, Wireshark, Burp Suite

    -   John the Ripper, Hashcat

    -   Setoolkit para phishing

    -   Scripts de automatización de ataques

* * * * *

🚀 Flujo de Funcionamiento
--------------------------

### Fase 1: Despliegue (30 minutos)


1\. Clonar repositorio → 2. Configurar red VirtualBox → 3. Ejecutar scripts de instalación

## Fase 2: Configuración (20 minutos)


1\. Splunk configura inputs → 2. Endpoints instalan forwarders → 3. Caldera despliega agentes

## Fase 3: Simulación (Variable)

```
┌─────────────────┐    ┌─────────────────┐     ┌─────────────────┐
│   Kali Linux    │    │  MITRE Caldera  │     │    Manuales     │
│  (Ataques       │───▶│  (Campañas      │───▶│    (Scripts     │
│   manuales)     │    │   automatizadas)│     │  personalizados)│
└─────────────────┘    └─────────────────┘     └─────────────────┘
         │                       │                       │
         └──────────┬────────────┴────────────┬──────────┘
                    │                         │
              ┌─────▼─────┐             ┌─────▼─────┐
              │ Windows 11│             │ Ubuntu 22 │
              │  (Logs)   │             │   (Logs)  │
              └─────┬─────┘             └─────┬─────┘
                    └────────────┬────────────┘
                                 │
                          ┌──────▼──────┐
                          │  Splunk SIEM │
                          │ (Análisis y  │
                          │  Detección)  │
                          └──────────────┘
```
## Fase 4: Análisis y Respuesta


1\. Alertas en Splunk → 2. Investigación con SPL → 3. Triaje de incidentes → 4. Reporte final

## Escenarios Pre-configurados:

1.  Ataque de Ransomware: Simulación de cifrado y rescate

2.  Movimiento Lateral: Compromiso de múltiples sistemas

3.  Exfiltración de Datos: Transferencia de información sensible

4.  Persistencia: Instalación de backdoors y servicios maliciosos

* * * * *

# 📦 Requisitos
---

## Requisitos Mínimos de Hardware

| Componente | Mínimo | Recomendado |
| --- | --- | --- |
| RAM | 16 GB | 32 GB |
| Almacenamiento | 150 GB libre | 250 GB SSD |
| CPU | 4 núcleos con VT-x/AMD-V | 8 núcleos |
| Sistema Operativo | Windows 10/11, Ubuntu 20.04+, macOS | Linux |

## Software Requerido

1.  VirtualBox 6.1+ o VMware Workstation Player

2.  Git para clonar el repositorio

3.  PowerShell 5.1+ (Windows) o Bash (Linux/macOS)

4.  Conexión a Internet para descargar ISOs

## Conocimientos Previos Recomendados

-   Básico de redes TCP/IP

-   Conceptos de sistemas operativos

-   Familiaridad con línea de comandos

-   Motivación para aprender 😊

* * * * *

# 📚 Documentación Incluida
-------------------------

## Guías Paso a Paso

| Documento | Descripción | Tiempo Estimado |
| --- | --- | --- |
| 01-SETUP-GUIDE.md | Instalación completa desde cero | 45 minutos |
| 02-SPLUNK-CONFIG.md | Configuración avanzada de Splunk | 30 minutos |
| 03-CALDERA-OPERATIONS.md | Uso de MITRE Caldera | 25 minutos |
| 04-ATTACK-SCENARIOS.md | 10 escenarios de ataque paso a paso | Variable |
| 05-INCIDENT-RESPONSE.md | Playbooks de respuesta a incidentes | 20 minutos |

## Scripts de Automatización

```
deployment/
├── scripts/
│   ├── 01-setup-network.sh/ps1    # Configura red VirtualBox
│   ├── 02-deploy-splunk.sh        # Instala y configura Splunk
│   ├── 03-configure-endpoints.sh  # Configura Windows/Ubuntu
│   ├── 04-deploy-caldera.sh       # Instala MITRE Caldera
│   └── 05-run-attacks.sh          # Ejecuta simulaciones automáticas
└── configs/
    ├── splunk/                    # Configuraciones de Splunk
    ├── caldera/                   # Campañas de Caldera
    └── endpoints/                 # Scripts para endpoints
```
## Dashboards Pre-configurados

1.  Security Overview Dashboard: Vista general del SOC

2.  Endpoint Monitoring: Estado de sistemas Windows/Linux

3.  Threat Hunting Console: Búsquedas proactivas

4.  Incident Response Board: Seguimiento de casos

5.  MITRE ATT&CK Matrix: Mapeo de técnicas detectadas

## Recursos Adicionales

-   Cheatsheets: Comandos SPL, PowerShell, Linux

-   Plantillas: Reportes de incidentes, documentación

-   Ejemplos de Logs: Para práctica y pruebas

-   Enlaces: Recursos externos recomendados

* * * * *

# ⚡ Inicio Rápido
---------------

## 1. Clonar el repositorio
```
git clone https://github.com/tu-usuario/mini-soc-lab.git
cd mini-soc-lab
```
## 2. Ejecutar despliegue automático (Linux/macOS)
```
chmod +x deploy.sh
sudo ./deploy.sh
```
## 3. O despliegue manual (Windows)

```
powershell -ExecutionPolicy Bypass -File deploy.ps1
```
Acceso post-instalación:

-   🔗 Splunk: [https://192.168.56.40:8000](https://192.168.56.40:8000/) (admin/changeme)

-   🔗 Caldera: [http://192.168.56.50:8888](http://192.168.56.50:8888/) (admin/admin)

-   🔗 Kali: SSH a 192.168.56.10 (kali/kali)

* * * * *

⚠️ Importante
-------------

Este laboratorio es solo para fines educativos y de práctica en entornos aislados. Nunca lo despliegues en redes productivas o conectadas a Internet.

* * * * *

📄 Licencia
-----------

MIT License - Ver [LICENSE](https://license/) para más detalles.

* * * * *

⭐ ¿Te gusta el proyecto? Dale una estrella en GitHub y compártelo con otros estudiantes de ciberseguridad. ¡Juntos aprendemos más! 🚀
