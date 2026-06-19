
# Primeros pasos

Bienvenido a la aplicación Meshtastic de Apple, su complemento para iOS, iPadOS y macOS para la comunicación por radio malla fuera de la red.

## Lo que necesitas

- Un dispositivo de radio Meshtastic LoRa compatible (ver[Meshtastic.org](https://meshtastic.org/docs/hardware/))
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

### Configuración del primer lanzamiento

La primera vez que abres la aplicación, una breve configuración guiada te guía a través de los permisos que Meshtastic utiliza, en orden. Cada pantalla explica para qué es el permiso, y puedes cambiar cualquiera de estos más adelante en Configuración de iOS. Conceder un permiso o tocar **Continuar** avanza al siguiente paso.

**1. Bluetooth**: se requiere para descubrir y conectarse a su radio.

![Bluetooth permission screen](../assets/screenshots/onboarding_bluetooth.png)

**2. Red local**: permite a la aplicación encontrar radios conectadas a través de WiFi o Ethernet.

![Local Network permission screen](../assets/screenshots/onboarding_localNetwork.png)

**3. Notificaciones**: cambie las alertas que desea antes de otorgar permiso: mensajes entrantes, nuevos nodos, batería baja y alertas críticas que evitan el modo silencioso y No molestar.

![Notifications permission screen](../assets/screenshots/onboarding_notifications.png)

**4. Ubicación** - opcional; permite la posición de suministro de su teléfono a la malla y mantener el mapa actualizado en segundo plano. Este paso se omite si ya ha otorgado el acceso a la ubicación.

![Location permission screen](../assets/screenshots/onboarding_location.png)

**5. Siri y accesos directos** - opcional; habilita comandos de voz y mensajería CarPlay.

![Siri and Shortcuts permission screen](../assets/screenshots/onboarding_siri.png)

## Paso 3: Establezca su nombre y una identificación corta

1. Vaya a **Configuración → Usuario**.
2. Introduzca un **Nombre largo** (su nombre para mostrar, hasta 39 caracteres).
3. Introduzca un **Nombre corto** (hasta 4 caracteres o un emoji, que se muestra en el círculo del nodo).
4. Pulsa **Guardar**.

Su nombre se transmite a los nodos cercanos automáticamente.

## Paso 4: Explora la malla

Una vez conectada, la aplicación muestra los nodos cercanos en la pestaña **Nodos**. Toque cualquier nodo para ver detalles como la intensidad de la señal, la última hora escuchada y la posición.

Envíe su primer mensaje tocando la pestaña **Mensajes** y seleccionando el canal principal.

> **Tip — Navegación rápida**
> Toque el icono de la pestaña actualmente activo para volver a la vista de nivel superior desde cualquier lugar de esa pestaña.

## Paso 5: Compruebe su configuración

Visite **Configuración → LoRa** para verificar que su código de región coincida con su ubicación. Usar la región equivocada es ilegal e impedirá la comunicación con otros nodos en su área.

## Siguientes pasos

-[Conexión del dispositivo Bluetooth](bluetooth.md)— Gestionar múltiples radios
-[Mensajes y canales](messages.md)— Enviar transmisiones y mensajes directos
-[Lista de nodos](nodes.md)— Entender los indicadores de estado del nodo
-[Mapa y puntos de referencia](map.md)- Ver la malla en un mapa

