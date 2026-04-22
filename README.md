# Instalación y Configuración de Mosquitto MQTT en Raspberry Pi con Publisher ESP32

Este proyecto demuestra la implementación de una arquitectura de mensajería **IoT** utilizando el protocolo **MQTT**. Se configuró una **Raspberry Pi 3B+** como Broker (servidor) y un **ESP32** como Publisher (cliente) para la transmisión de datos en tiempo real.

## 🚀 Requisitos de Hardware
- **Broker:** Raspberry Pi 3B+ (con fuente de poder de 5V @ 3A).
- **Publisher:** ESP32 DevKit V1.
- **Red:** Conexión local Wi-Fi (2.4 GHz).

## 🛠️ 1. Configuración del Broker (Raspberry Pi)

### Instalación de Mosquitto
En la terminal de la Raspberry Pi, se ejecutaron los siguientes comandos para actualizar el sistema e instalar el broker:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install mosquitto mosquitto-clients -y
sudo systemctl enable mosquitto
