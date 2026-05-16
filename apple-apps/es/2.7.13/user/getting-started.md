
# Primeros pasos

Bienvenido a la aplicación Meshtastic de Apple, su compañero de iOS, iPadOS, macOS, watchOS y visionOS para la comunicación por radio en malla fuera de la red.

## Lo que necesitas

- Un dispositivo de radio Meshtastic LoRa compatible (ver [meshtastic.org](https://meshtastic.org/docs/hardware/))
- IPhone, iPad o Mac con la aplicación Meshtastic instalada
- Bluetooth habilitado en su dispositivo

## Paso 1: Enciende tu radio

Enciende tu radio Meshtastic. La mayoría de los dispositivos muestran una pantalla de bienvenida y comienzan a transmitir en la malla inmediatamente.

## Paso 2: Abre la aplicación y conéctate

1. Abre la aplicación Meshtastic.
2. Toque **Conectar** (el icono de la antena en la barra de pestañas).
3. Su radio debería aparecer en la lista de dispositivos cercanos en unos segundos.
4. Toque el nombre del dispositivo para conectarse.

El indicador de conexión se vuelve verde cuando la radio está emparejada y se comunica.

> **Consejo:** Si su dispositivo no aparece, asegúrese de que el Bluetooth esté habilitado en la configuración de iOS y de que la radio esté dentro del alcance (aproximadamente 10 metros para el emparejamiento inicial).

## Paso 3: Establezca su nombre y una identificación corta

1. Vaya a **Configuración → Usuario**.
2. Introduzca un **Nombre largo** (su nombre para mostrar, hasta 39 caracteres).
3. Introduzca un **Nombre corto** (hasta 4 caracteres o un emoji, que se muestra en el círculo del nodo).
4. Pulsa **Guardar**.

Su nombre se transmite a los nodos cercanos automáticamente.

## Paso 4: Explora la malla

Una vez conectada, la aplicación muestra los nodos cercanos en la pestaña **Nodos**. Toque cualquier nodo para ver detalles como la intensidad de la señal, la última hora escuchada y la posición.

Envíe su primer mensaje tocando la pestaña **Mensajes** y seleccionando el canal principal.

## Paso 5: Compruebe su configuración

Visite **Configuración → LoRa** para verificar que su código de región coincida con su ubicación. Usar la región equivocada es ilegal e impedirá la comunicación con otros nodos en su área.

## Acerca de Meshtastic

![About Meshtastic](../assets/screenshots/aboutMeshtastic.png)

## Siguientes pasos

- [Conexión del dispositivo Bluetooth](bluetooth.md) - administrar múltiples radios
- [Mensajes y canales](messages.md) — envía transmisiones y mensajes directos
- [Lista de nodos](nodes.md) - entender los indicadores de estado de los nodos
- [Mapa y Waypoints](map.md) — ver la malla en un mapa

