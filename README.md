# 📦 ESPHome Professional Packages
**Paquetes modulares optimizados para ESP32/ESP8266 en Home Assistant**

[![ESPHome](https://esphome.io/_images/logo-text.png)](https://esphome.io)

Colección de **configuraciones probadas y estables** que solucionan problemas comunes:
- ✅ Sin bucles de reinicio por pérdida de WiFi/API
- ✅ Punto de acceso solo manual (AP_timeout: 0s) 
- ✅ Monitoreo completo en Home Assistant
- ✅ Provisioning BLE + Portal cautivo
- ✅ LED integrado como indicador de estado

## 🚀 Instalación Rápida

### 🛠️ Solución de Problemas Comunes
- **Problema**: El dispositivo no se conecta a WiFi.  
  **Solución**: Verificar que `wifi_ssid` y `wifi_password` estén correctamente definidos en `secrets.yaml`.  
- **Problema**: Reinicios frecuentes.  
  **Solución**: Asegurar que `ap_ssid` y `ap_password` sean únicos y no conflictuén con otros dispositivos.

### 1. **Preparar secrets.yaml**
```yaml
wifi_ssid: "MiWiFi"
wifi_password: "mipass"
ap_ssid: "ESP-Config"
ap_password: "12345678"
admin: "admin123"
2. ESP32-C3 / ESP32 (Recomendado)
text
substitutions:
  device_name: "sensor-salon"
  friendly_name: "Salón"

esphome:
  name: ${device_name}

packages:
  - !include 0configuracion_base.yaml
  - !include lib-wifi.yaml  
  - !include lib-esp32_version.yaml

# Aquí tus sensores
sensor:
  - platform: dht
    pin: GPIO4
    model: DHT22
    temperature:
      name: "Temperatura"
    humidity:
      name: "Humedad"
3. ESP8266 (NodeMCU, Wemos, etc.)
text
packages:
  - !include 0configuracion_base.yaml
  - !include lib-wifi.yaml
  - !include lib-esp8266_version.yaml  # Cambia esta línea
4. Compilar → Flashear → ¡Listo!
📁 Estructura de Paquetes
Archivo	✅ Incluye	🎯 Para
0configuracion_base.yaml	Logger, API estable, OTA, captive portal	Todos
lib-wifi.yaml	WiFi robusto + monitoreo + AP manual	Todos
lib-esp32_version.yaml	ESP-IDF optimizado, temp interna, LED	ESP32/C3
lib-esp8266_version.yaml	Framework Arduino básico	ESP8266
lib-consumer.yaml	BLE Improv + Webserver	ESP32
🔍 Entidades en Home Assistant
Sensores Principales
text
🌡️ Seal WiFi (RSSI)
⏱️ Uptime 
🔢 Intentos sin WiFi/API
🌡️ Temperatura Interna (ESP32)
📶 IP/SSID/MAC
Binary Sensors
text
🔴 Modo sin conexión
📡 Modo AP activo
✅ API conectada
Estados
text
📱 Estado conexión: "Conectado" | "No API" | "Modo AP" | "Sin conexión"
Eventos HA (para automatizaciones)
text
avisoestadoconexion: ["estadonoapi", "estadomodoap"]
avisoestadoesp32: ["temperaturaelevada", "conexiondebil"]
Botones
text
🔄 Reiniciar | Modo Seguro | Factory Reset
📡 Desconectar/Reconectar WiFi
💤 Deep Sleep (ESP32)
🎛️ Indicador LED Integrado (ESP32)
Estado LED	🚦 Significado
Apagado	✅ Todo OK
Fijo	📡 Modo AP activo
Parpadea lento	🔥 Temp > 60°C
Parpadea rápido	📴 RSSI < -80dBm
⚙️ Personalización
Cambiar umbrales (WiFi)
text
# En lib-wifi.yaml
umbralcontadores: 20    # Intentos máximo sin conexión
intervalochequeo: 15s   # Frecuencia chequeo
LED integrado
text
# En lib-esp32_version.yaml
led_integra_pin: GPIO2
🎯 Casos de Uso
text
✅ Sensores inalámbricos estables
✅ Dispositivos críticos (24h)
✅ Flotas grandes de dispositivos
✅ Diagnóstico remoto
✅ Primer boot sin cables (BLE)
📖 Flujo Completo
text
1. Copia → Pega → Compila
2. Flashea (OTA/USB/BLE)
3. Se conecta automáticamente
4. Monitorea todo en HA
5. AP manual si algo falla
🤝 Contribuir
Fork este repositorio

Crea mi-paquete.yaml

Documéntalo aquí

Pull Request

💬 Soporte
ESPHome Discord

Home Assistant Community

Issues en GitHub

⭐ Star si te ahorra tiempo!
¡Copia → Pega → Compila → Listo! 🚀