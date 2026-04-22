Markdown
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
Configuración de Acceso Externo
Para permitir que el ESP32 se conecte desde fuera de la Raspberry, se editó el archivo de configuración en /etc/mosquitto/conf.d/external.conf:

Plaintext
listener 1883
allow_anonymous true
Se reinició el servicio para aplicar los cambios:

Bash
sudo systemctl restart mosquitto
📡 2. Configuración del Publisher (ESP32)
El firmware se desarrolló en Arduino IDE utilizando la librería PubSubClient. El dispositivo publica un mensaje cada 5 segundos al tópico escom/test.

Código Fuente (Firmware)
C++
#include <WiFi.h>
#include <PubSubClient.h>

// Configuración de red y broker
const char* ssid = "TU_SSID_WIFI";
const char* password = "TU_PASSWORD_WIFI";
const char* mqtt_server = "192.168.1.72"; // IP de la Raspberry Pi

WiFiClient espClient;
PubSubClient client(espClient);

void setup() {
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqtt_server, 1883);
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nWiFi conectado");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32_MarioFabela")) {
      Serial.println("¡Conectado!");
    } else {
      Serial.print("Falló, rc=");
      Serial.print(client.state());
      delay(5000);
    }
  }
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  static unsigned long lastMsg = 0;
  unsigned long now = millis();
  if (now - lastMsg > 5000) {
    lastMsg = now;
    client.publish("escom/test", "Hola desde el ESP32 del inge");
    Serial.println("Mensaje enviado a la Raspberry");
  }
}
📊 3. Evidencias de Funcionamiento
Las capturas de pantalla adjuntas en el repositorio muestran:

Conexión exitosa: Log del Monitor Serie del ESP32 confirmando la conexión al Wi-Fi y al Broker.

Recepción de datos: Terminal de la Raspberry Pi ejecutando mosquitto_sub -h localhost -t "escom/test" -v y mostrando los mensajes recibidos en tiempo real.

Desarrollado por: Mario Fabela

Institución: Escuela Superior de Cómputo (ESCOM - IPN)

Materia: Sistemas en Red / Internet de las Cosas (IoT)
