# ESPHome Libraries & Packages

Colección de **paquetes reutilizables** para ESPHome. Configuraciones probadas y optimizadas para ESP32, ESP32-C3 y ESP8266.

## 📁 Estructura del repositorio

├── configuración-base.yaml # Configuración esencial (API, OTA, logger)
├── configuración-portal.yaml # Portal cautivo + WiFi básico
├── configuración-wifi.yaml # WiFi robusto + AP manual
├── esp32-wifi.yaml # WiFi específico ESP32
├── lib_esp32-version.yaml # Framework ESP-IDF para ESP32/C3
├── lib_esp8266-version.yaml # Framework Arduino para ESP8266
└── README.md

text

## 🚀 Uso rápido

### 1. ESP32-C3 Super Mini (recomendado)
```yaml
esphome:
  name: ${device_name}

packages:
  - !include configuración-base.yaml
  - !include configuración-wifi.yaml  
  - !include lib_esp32-version.yaml
2. ESP8266 (NodeMCU, Wemos, etc.)
text
esphome:
  name: ${device_name}

packages:
  - !include configuración-base.yaml
  - !include configuración-wifi.yaml
  - !include lib_esp8266-version.yaml
📋 Contenido de cada paquete
Archivo	✅ Incluye	🎯 Para
configuración-base.yaml	Logger, API (reboot_timeout: 0s), OTA, captive_portal	Todos
configuración-wifi.yaml	WiFi estable, AP manual (ap_timeout: 0s), reboot_timeout: 0s	Todos
lib_esp32-version.yaml	esp32: board: esp32-c3-devkitm-1, ESP-IDF optimizado	ESP32/C3
lib_esp8266-version.yaml	Framework Arduino, flash_mode: dio	ESP8266
🔧 Secrets necesarios
secrets.yaml:

text
wifi_ssid: "MiWiFi"
wifi_password: "mipass"
device_name: "sensor-salon"
friendly_name: "Salón"
✨ Características incluidas
✅ WiFi robusto	reboot_timeout: 0s, AP solo manual
✅ API estable	reboot_timeout: 0s, sin reinicios locos
✅ OTA seguro	Contraseña dinámica opcional
✅ Sin bucles	No se reinicia solo por perder WiFi/HA
✅ Optimizado	Config específico por chip
💡 Ejemplo completo ESP32-C3
text
substitutions:
  device_name: sensor-salon
  friendly_name: Salón

esphome:
  name: ${device_name}

packages:
  - !include configuración-base.yaml
  - !include configuración-wifi.yaml
  - !include lib_esp32-version.yaml

# Aquí tus sensores, switches, etc.
sensor:
  - platform: dht
    pin: GPIO4
    model: DHT22
    temperature:
      name: "Temperatura"
    humidity:
      name: "Humedad"
🆕 Crear tu propio paquete
Crea mi-paquete.yaml

Añádelo a packages:

Documenta en este README

🤝 Contribuir
Fork el repositorio

Crea tu paquete lib_mi-funcion.yaml

Pull Request con descripción

📞 Soporte
ESPHome Discord

Home Assistant Community

Issues en GitHub

⭐ Star si te ahorra tiempo!
¡Copia → Pega → Compila → Listo! 🚀