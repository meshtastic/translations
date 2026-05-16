
# Cómo funciona el medidor de señales Meshtastic

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/signalMeter_full_all_dark.png">
  <img src="../assets/screenshots/signalMeter_full_all.png" alt="Signal meter levels">
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/signalMeter_compact_all_dark.png">
  <img src="../assets/screenshots/signalMeter_compact_all.png" alt="Compact signal meter">
</picture>

El medidor de señal Meshtastic, a menudo visto como una serie de barras o un color de estado en la aplicación, se calcula de manera muy diferente a las "barras" en un teléfono celular tradicional o enrutador Wi-Fi.

La mayoría de los dispositivos de consumo solo miden qué tan "ruidosa" es una señal. Sin embargo, debido a que Meshtastic utiliza la tecnología **LoRa (Long Range)**, su medidor de señal utiliza una lógica que mide qué tan **clara** es la señal, en relación con la configuración específica que su malla está utilizando.

---

## 1. Las dos métricas: "Brutosidad" vs. "Claridad"

Para entender el medidor, necesita comprender las dos mediciones que toma el chip de radio LoRa cada vez que recibe un mensaje:

* **RSSI (Indicador de intensidad de señal recibida):** Este es el **volumen** de la potencia bruta que golpea su antena.
* **SNR (Relación señal-ruido):** Esta es la **claridad** de la señal en comparación con la estática de fondo.

> **Tip — La analogía:** Imagina que estás tratando de escuchar a un amigo hablándote.
> * **RSSI** es lo fuerte que es su voz.
> * **El piso de ruido** es el ruido de fondo en la habitación (aire acondicionado, otras personas hablando, tráfico).
> * **SNR** es la facilidad con la que puedes distinguir la voz de tu amigo del ruido de fondo.

Si tu amigo te grita en un concierto de rock ensordecedor, la señal es increíblemente fuerte (RSSI alto), pero aún así no puedes entenderlos porque el ruido de fondo es más fuerte (Bad SNR). Por el contrario, si tu amigo te susurra en una biblioteca muerta en silencioso, la señal es muy débil (RSSI bajo), pero puedes entenderlos perfectamente (Gran SNR).

---

## 2. La magia de LoRa: escuchar "Below the Noise Floor"

Para radios estándar (como FM o Wi-Fi), si el ruido de fondo es más fuerte que la señal (un SNR negativo), el receptor solo escucha estática.

LoRa es especial. Utiliza la modulación **"Spread Spectrum"**, que permite a la radio sacar matemáticamente una señal del aire incluso cuando está enterrada profundamente *debajo* del ruido de fondo. Es por eso que con frecuencia verá **números SNR negativos** en Meshtastic (por ejemplo, -10dB, lo que significa que la señal es 10 decibelios más débil que la estática de fondo).

Dependiendo del preajuste de Meshtastic que estés usando (por ejemplo,`LongFast`Vs.`ShortFast`), La radio tiene un **Límite SNR** específico: la cantidad máxima absoluta de ruido que puede tolerar antes de que el mensaje se pierda por completo en la estática.

---

## 3. Cómo calcula la calidad el medidor de señales

Las aplicaciones Meshtastic toman tanto RSSI como SNR y las ejecutan a través de una fórmula específica para asignar a su señal una calificación de calidad (Ninguna, Mala, Justa o Buena). Escala específicamente estos valores en función de los límites físicos del preajuste de radio que está utilizando.

Así es exactamente como la aplicación decide cuántas barras (o de qué color) mostrarte:

| Nivel | Bar | Criterios | Significado |
|-------|------|----------|---------|
| El bien | 3 | RSSI mejor que `-115 dBm` **Y** SNR por encima del límite de línea de base para su preajuste | La señal es fuerte y clara, conexión saludable. |
| Feria | 2 | Cae entre lo bueno y lo malo | La señal se está volviendo más silenciosa o más ruidosa, pero la radio entiende bien el mensaje. |
| Malo | 1 | RSSI cae a `-120 dBm` o peor, **O** SNR dentro de `5,5 dB` del punto de ruptura absoluto de su preajuste | Apenas colgando, en el borde del rango o interferencia fuerte. |
| Nona | 0 | RSSI peor que `-126 dBm` **Y** SNR ha caído `7,5 dB` por debajo del límite ideal | Transmisión completamente enterrada en estática. |

---

## 4. Lo que esto significa para ti

Debido a que el medidor de Meshtastic actúa como un **"Medidor de Claridad"**, se comporta de manera diferente a lo que la mayoría de la gente espera:

> **Tip — No se asuste por el bajo RSSI:** Podría ver un valor de RSSI aparentemente terrible como`-118 dBm`. En un teléfono móvil, tendrías cero barras. Pero si tienes un SNR de`+2 dB`, ¡Meshtastic seguirá mostrando una señal fuerte! *La biblioteca está en silencio, por lo que el susurro se escucha perfectamente. *

> **Warning — Cuidado con el ruido local:** Si conectas una antena masiva y ves un gran RSSI (por ejemplo,`-90 dBm`) Pero su medidor de señal solo muestra **1 Bar (Malo)**, tiene un problema. Significa que tiene una interferencia local, tal vez una fuente de alimentación barata, una computadora ruidosa o una torre de radio cercana, que crea tanta estática que está ahogando su malla.

