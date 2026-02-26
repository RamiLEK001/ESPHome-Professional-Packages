# ESPHome Professional Packages 🚀

Bienvenido a la Wiki de **ESPHome Professional Packages**, un ecosistema modular diseñado para dotar a tus placas ESP32 y ESP8266 de capacidades de grado empresarial: telemetría agresiva, un motor de alarmas en C++ con notificaciones nativas y supervivencia de red híbrida (Improv, Fallback AP).

**Filosofía**: Copia → Pega → Compila → Disfruta = 100% de operatividad en 3 minutos.

---

## 📁 Preparación de Carpetas en Home Assistant

Para que ESPHome pueda leer librerías externas mediante `!include`, es **obligatorio** ubicarlas en el sistema de archivos de Home Assistant primero.

1. Usa tu add-on preferido para explorar archivos (Samba Share, File Editor, Studio Code Server, etc.).
2. Navega a la ruta principal de ESPHome, típicamente: `/homeassistant/esphome` (o `/config/esphome`). *Aquí es donde ESPHome guarda todos los `.yaml` de tus dispositivos.*
3. Crea una nueva carpeta allí adentro. En este tutorial la llamaremos **`lib`**. La ruta final te quedará como `/homeassistant/esphome/lib/`.
4. Sube a esa carpeta `lib/` nuestras dos librerías maestras:
   * `lib-estados_alarmas.yaml`
   * `lib-wifi_conectividad.yaml`
5. **Importante**: ¡No subas el archivo `configuracion_base.yaml` como un archivo suelto a esa carpeta! Ese archivo será nuestra plantilla maestra para copiar y pegar en el propio Dashboard.

---

## ⚡ Guía de Instalación Rápida (Quick Start)

Este proyecto utiliza una arquitectura de *Módulo Principal* a través de la plantilla `configuracion_base.yaml`. Este código actuará como el núcleo de configuración física de tu placa.

1. **Inicia un nuevo dispositivo** en tu Dashboard de ESPHome (Ej: `http://homeassistant.local:8123`).
2. Sigue el asistente para conectarlo al WiFi y dale a instalar.
3. Cuando termine y te muestre el archivo `.yaml` vacío, dale a **Edit**.
4. Borra todo y **copia y pega** el contenido íntegro del archivo `configuracion_base.yaml` de este repositorio. *(La sección final de "packages:" ya viene configurada para apuntar mágicamente a la carpeta `lib/`)*.
5. **Descomenta tu Placa Física**: En el archivo base verás la sección *"CONFIGURACIÓN DE PLACA"*. Quita las `#` de la placa que estés usando (ESP32 estándar, ESP32-C3 o ESP8266). Esto inyecta sus perfiles físicos de RAM y CPU para la telemetría automática.
6. Dale a **"Install"**. Automáticamente buscará los paquetes en la carpeta `lib/` de Home Assistant ¡y flasheará tu dispositivo completamente blindado!

---

## 🧠 Arquitectura de Módulos (Packages)

Todo el código pesado e indescifrable está escondido en librerías mantenibles para que tú solo veas un archivo limpio:

1. **`configuracion_base.yaml`**: Selección de Hardware, secretos OTA e inclusión de librerías. Gestiona las **Substitutions globales** directamente en el perfil de la placa (Versión del proyecto, nombre amigable de inyección y límites de hardware como RAM/CPU).
2. **`lib-wifi_conectividad.yaml`**: El Motor de Red "Cero Secretos". Unifica y abstrae las utilidades vitales de conexión sin pre-configurar SSIDs:
   * **Modo AP Fallback**: Si el WiFi se cae, tras 90 segundos la placa levanta su propia red temporal de configuración.
   * **Improv Inalámbrico (BLE)**: Permite integrar la placa a Home Assistant a través de Bluetooth sin pre-configurar SSIDs.
   * **Web Server y Captive Portal**: Interfaz Web local de emergencia si te quedas sin acceso al router.
   * **Watchdogs API**: Reincio duro del ESP si pierde el contacto con Home Assistant durante más de 5 minutos, salvándolo de bloqueos catastróficos.
3. **`lib-estados_alarmas.yaml`**: El Cerebro Inspector. Implementa un motor en C++ que chequea a fondo los componentes físicos de hardware, latencias y conexiones cada 29 segundos inyectando los diagnósticos como Notificaciones PUSH.

---

## 🏷️ La Regla de Oro: Cómo Nombrar tu Dispositivo

En la parte superior de `configuracion_base.yaml` encontrarás el núcleo de identidad:

```yaml
esphome:
  name: "poner-nombre-de-dispositivo" # <--- ESTO SÍ DEBEN CAMBIARLO 
  friendly_name: "Sin Configurar"     # <--- ESTO NO LO DEBEN TOCAR
```

**ATENCIÓN**:
1. ✅ **`name:`** (El nombre Físico). Cambia `poner-nombre-de-dispositivo` a un nombre en minúsculas y sin espacios (EJ: `sensor-puerta`). Así es como el chip se registrará internamente en la tabla DHCP de tu Router.
2. ❌ **`friendly_name:`** (El nombre Amigable). **¡Jamás toques esta línea en el YAML!** 

La arquitectura asume que ESPHome inyectará constantemente las palabras "Sin Configurar" en los +30 sensores de la placa y en sus notificaciones de alarma. Esto evita que cruces telemetrías sueltas e idénticas si tienes 5 placas esparcidas por tu casa.

**¿Cómo le pongo nombre amigable (Ej: "Sensor Salón")?**
¡Fácil, limpio y sin tener que recompilar código jamás!
1. Flashea la placa y agrégala a la app de Home Assistant tal y como está ("Sin Configurar").
2. En Home Assistant, ve a **Ajustes -> Dispositivos** y busca tu placa de ESPHome recién añadida.
3. Haz clic en el engranaje superior de configuración ⚙️.
4. Escribe el nuevo nombre ahí y dale a "Renombrar Entidades". Home Assistant automáticamente cambiará absolutamente todo (De "Sin Configurar Batería" a "Sensor Salón Batería") en décimas de segundo.

---

## 📊 Telemetría y Capacidades Disponibles

Tus sensores en la UI se poblarán instantáneamente de docenas de variables agrupadas en "Diagnósticos".

### Monitoreo de Red
* Señal de WiFi precisa (convertida matemáticamente de dBm a Porcentaje amigable 0-100%).
* Interruptores visuales booleanos para depuración del árbol (API conectada, AP encendido, WiFi vivo).
* Mapeo de identidad (MAC, IP del Chip, BSSID de la Antena que está dando cobertura a la placa).
* Botones PUSH configurables para Forzar la Desconexión/Reconexión controlada sin quitar la alimentación, y Botones rápidos de Deep Sleep o Safe Mode de la BIOS de la ESP.

### Inteligencia de Hardware (C++)
* Free RAM + Memory Fragmentation (Útil para cazar memory leaks en tu propio código a lo largo de semanas).
* Uptime en formato legible humano ("4d 13h 25m"), no Segundos brutos irreconocibles.
* *Loop Time* (Latencia de la máquina virtual), capaz de chivarte si agregas componentes lentos que asfixian el dispositivo.
* Frecuencia del reloj CPU al límite máximo de la placa y Temperatura Interna de la circuitería del silicio ESP32 en Celcius.
* Traductor humano para `reset_reason` ("Por qué se había apagado la ESP la última vez").

---

## 🚨 Motor de Alarmas Táctico

Incluye un analizador de *Thresholds* que inyecta información al sistema unificado de HA (ej: `ha_notify_service: "notify.notify"`) y categoriza en 4 Gravedades el estado del chip ESPHome **cada 29 segundos**.

* **0. Nada**: Operativa al 100%. Todo en control.
* **1. Leve**: Información y telemetría no-urgente (Ej: "Reboot Sugerido al sobrepasar la semana entera encendido para alargar la vida útil" o "Señal WiFi Débil (<50%)").
* **2. Moderado**: Alerta Push a la app *solo si es propenso a convertirse en daño catastrófico próximo* (Ej: "Falta de Memoria (<50kb)" o "Reinicio por Error Desconocido").
* **3. Grave**: La peor situación. El Motor notifica y registra la avería en HA (Ej: "Temp Crítica del silicio (>80°) | Hardware Fríendose", o "API y Red caídas").

---

## ⚠️ Advertencia Técnica para Chips ESP8266

Este proyecto empuja los límites extrayendo métricas del hardware avanzado y módulos BLE/Improv solo equipados en arquitecturas modernas (ESP32). ¡Puedes flashear un pobre ESP8266! Funcionará perfectamente gracias a las rutinas C++ blindadas, **PERO**:

> [!CAUTION]  
> Para ESP8266: Debes ingresar a `lib-estados_alarmas.yaml` de forma **obligatoria**, buscar los bloques `internal_temperature`, `min_free` y `psram`, **y borrarlos** o te lanzará Errores de Compilación de que 'no existen'. Están etiquetados con grandes exclamaciones para que los encuentres en 3 segundos.

---

## ❓ FAQ y Resolución de Problemas (Troubleshooting)

### ¿Cómo funciona el arranque si no hay WiFi grabado? (Flujo de Conexión)
El sistema está diseñado para ser seguro y no hacer "Boot-Loops":
1. **Primer Encendido (Fábrica)**: La placa arranca con el módem WiFi físicamente apagado para ahorrar energía (`enable_on_boot: false`). Instantáneamente activa el **Improv BLE** para que puedas detectarlo desde el Bluetooth de tu móvil y pasarle las credenciales.
2. **Pérdida de Router en Casa**: Si tienes el WiFi guardado pero el router se apaga, la placa intenta conectarse. Tras 3 minutos sin éxito, el motor interno nativo (`ap_timeout`) levanta su propio Punto de Acceso Abierto (Fallback AP) para que entres a la interfaz de recuperación y puedas añadir red.
3. **Pérdida de Home Assistant (API)**: Si hay WiFi, pero Home Assistant está caído, la placa esperará 5 minutos en ese estado ciego. Pasados los 5 minutos, la propia ESP apretará internamente el botón de reinicio en busca de una recuperación limpia.

### Mis sensores exponen `psram` y no me compila
El sensor `psram:` en `lib-estados_alarmas.yaml` rastrea la memoria externa Pseudo-Static RAM. **Si tu ESP32 no es una versión avanzada (ej: WROVER) o es un ESP8266**, careces de este chip. 
*Solución*: El bloque de `psram:` viene **comentado por defecto** en el YAML base de alarmas para evitar errores universales. Solo debes descomentarlo si activaste `psram:` explícitamente en el hardware de tu `configuracion_base.yaml`.

### ¿Para qué es cada botón en Home Assistant?
El ecosistema inyecta varios botones útiles. Entiéndelos antes de usarlos:

* **Reconectar WiFi**: Obliga al módem interno a apagarse dos segundos y volverse a encender. Útil si la placa se ha quedado enlazada a un repetidor WiFi lejano y quieres obligarla a buscar el más cercano.
* **Apagar (deep sleep)**: Apaga totalmente la Placa/CPU durante 24h. *Atención*: Solo revivirá si físicamente usas un cable para puentear su pin de Reset manual.
* **Reiniciar dispositivo**: Un reinicio caliente normal y elegante (el mismo que usa la placa cuando pierde la API para auto-salvarse).
* **Reiniciar en modo seguro**: Inicia la ESP obviando todo tu código personalizado (sensores complejos) para evitar Boot-Loops. Ideal si has programado algo mal y necesitas que la ESP32 aguante encendida para poder flashearle un OTA arreglado.
* **Restablecer a fábrica**: El "Botón Nuclear". Borra de forma total e irrecuperable de las entrañas de silicio el WiFi, el Improv, y todo ajuste residual de Home Assistant. ¡La placa actuará como recién comprada lista para ser re-adoptada de cero!

*(Nota: Históricamente ESPHome carece de un método eficiente para forzar el Modo AP bajo demanda apagando activamente el WiFi interno del Framework. Por ello, el botón "Restablecer a fábrica" cumple el mismo cometido arquitectónico (Borrar perfil y levantar portal Improv y Red Abierta) siendo 100% nativo).*

### ¿Por qué se enciende el LED físico de la placa? (Status Indicator)
El LED integrado (luz azul) no es solo un adorno, funciona como un monitor analógico de emergencia para que sepas el estado de la placa con solo mirarla. El LED tiene dos funciones vitales y exclusivas:

1. **Modo Emparejamiento (Improv BLE)**:
   * **Parpadeo lento (1 seg)**: La placa está virgen o no encuentra conexión WiFi y ha levantado su Bluetooth temporal. Está en "Modo Escucha" esperando que abras Home Assistant en tu móvil para pasarle la clave WiFi.
   * **Parpadeo rápido**: Home Assistant la ha detectado y le está inyectando las credenciales de red de tu casa.
   * **Apagado**: Emparejamiento exitoso. La placa ya se conectó a tu router y cerró el Bluetooth.
   
2. **Motor de Alarmas (Fallo de Hardware/Red)**:
   * Si tras estar funcionando de forma normal el LED azul **se enciende fijo y no se apaga**, significa que el procesador C++ interno ha detectado una **Alarma Moderada o Grave** (Nivel 2 o 3).
   * Ejemplos: El servidor de Home Assistant lleva caído 5 minutos, la placa no encuentra el WiFi de tu casa y entró en Modo AP, la temperatura del componente electrónico ha subido a niveles peligrosos (>80°C) o se ha agotado el 95% de la RAM impidiéndole operar con seguridad. ¡Abre tu App de Home Assistant inmediatamente y lee los diagnósticos de Sensor de Texto para ver de qué se queja!