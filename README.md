# WakeOnLan-proyecto
Wake on LAN para maquinas del servidor para el proyecto

## 1. Objetivo

Desarrollar un script en Bash que permita **encender automáticamente servidores Linux mediante Wake-on-LAN** cuando se detecte que:

- El servidor está apagado.
- Un servicio crítico no responde.
- Existe una ventana programada de mantenimiento o respaldo.

### Problema que resuelve

En infraestructuras DevOps o entornos de laboratorio:

- Los servidores pueden permanecer apagados para ahorrar energía.
- Puede ser necesario encenderlos remotamente sin acceso físico.
- Algunos servicios críticos pueden requerir reinicio remoto si la máquina no responde.

El script automatiza el proceso de verificación y encendido sin intervención humana.

---

## 2. Alcance

El script se ejecutará:

- En un **Host Linux administrador** (máquina de control).
- Desde una VM de Vagrant o servidor de monitoreo.
- En la misma red LAN que los servidores objetivo.

Actuará enviando paquetes mágicos WoL hacia servidores previamente configurados.

### Requisitos

- Wake-on-LAN habilitado en BIOS/UEFI del servidor.
- Soporte WoL habilitado en la interfaz de red (`ethtool`).
- Dirección MAC conocida del servidor.
- Herramienta `wakeonlan` o `etherwake` instalada.

---

## 3. Descripción General del Funcionamiento

El script:

1. Verifica si el servidor responde a ping.
2. Si no responde:
   - Envía paquete WoL.
3. Espera un tiempo determinado.
4. Vuelve a verificar conectividad.
5. Registra el resultado en un archivo log.
6. Maneja errores y reintentos automáticos.

---

## 4. Variables Configurables

```bash
SERVER_NAME="servidor_web"
SERVER_IP="192.168.1.50"
SERVER_MAC="00:1A:2B:3C:4D:5E"
PING_COUNT=3
WAIT_TIME=60
MAX_RETRIES=3
LOG_FILE="/var/log/wol_automation.log"
# Justificación Técnica de Herramientas – Script Wake-on-LAN para Servidores Linux

---

## 2. Uso de `wakeonlan` o `etherwake`

Estas herramientas se utilizan para enviar el **Magic Packet** necesario para activar Wake-on-LAN.

### ¿Por qué `wakeonlan`?

- Es simple y portable.
- No requiere especificar interfaz manualmente.
- Fácil de integrar en scripts.

Ejemplo:

```bash
wakeonlan 00:1A:2B:3C:4D:5E
