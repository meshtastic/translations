
# Lista de nodos

La pestaña Nodos muestra todos los dispositivos que su radio ha escuchado en la malla. Toque cualquier nodo para obtener más detalles.

## Estado del nodo

| Elemento | Significado |
|---------|---------|
| ![Node circle](../assets/screenshots/circleTextDefault.png) | **Nombre corto y nombre largo** - cada nodo tiene un nombre corto (hasta 4 bytes) que se muestra en el círculo de color y un nombre largo que se muestra al lado. El color del círculo se deriva del número de nodo. El nombre corto puede ser un emoji o iniciales. |
| ![Online](../assets/screenshots/nodeOnline.png) | **En línea** - el nodo se ha escuchado recientemente y se considera en línea. |
| ![Idle / Sleeping](../assets/screenshots/nodeIdle.png) | **Inactivo / Durmiendo** - el nodo no ha sido escuchado recientemente y puede estar inactivo o fuera de alcance. |
| ![Hops Away](../assets/screenshots/hopsAway.png) | **Hops Away** - el número de nodos intermedios que retransmiten mensajes entre usted y este nodo. No saltar significa comunicación directa. |

## Cifrado

| Icono | Significado |
|------|---------|
| ![Shared Key](../assets/screenshots/lockOpen.png) | **Clave compartida**: los mensajes directos están utilizando la clave compartida para el canal. |
| ![Public Key Encryption](../assets/screenshots/lockClosed.png) | **Cifrado de clave pública** - los mensajes directos utilizan la infraestructura de clave pública. Requiere firmware 2.5+. |
| ![PKI Mismatch](../assets/screenshots/keySlash.png) | **Desajuste de clave pública** - la clave pública no coincide con la clave registrada anteriormente. Comprueba que el contacto está fuera de banda. |

## Funciones del dispositivo

Cada nodo está configurado con un rol que determina cómo se comporta en la malla. Los roles se muestran en la vista de detalle del nodo.

| Icono | Papel | Descripción |
|------|------|-------------|
| ![](../assets/screenshots/roleClient.png) | Cliente | Dispositivo estándar del usuario final. Envía y recibe mensajes, comparte posición. |
| ![](../assets/screenshots/roleClientMute.png) | Cliente Silenciar | Me gusta el cliente, pero no reenvía paquetes de otros dispositivos. Reduce el tráfico de malla cerca de áreas congestionadas. |
| ![](../assets/screenshots/roleClientHidden.png) | Cliente oculto | Solo se transmite según sea necesario para el sigilo o el ahorro de energía. |
| ![](../assets/screenshots/roleClientBase.png) | Base de clientes | Nodo de techo que distribuye mensajes ampliamente desde nodos de cliente silencioso cercano. |
| ![](../assets/screenshots/roleRouter.png) | Rúter | Nodo de infraestructura dedicado: siempre retransmite los paquetes una vez. Mejor posicionado en ubicaciones estratégicas para maximizar la cobertura. |
| ![](../assets/screenshots/roleRouterLate.png) | Enrutador Tarde | Como Router, pero retransmite una vez después de todos los demás nodos. Más adecuado para despliegues en la azotea. |
| ![](../assets/screenshots/roleTracker.png) | Rastreador | Transmite paquetes de posición GPS como prioridad. Optimizado para informes de ubicación frecuentes. |
| ![](../assets/screenshots/roleSensor.png) | Sensor | Transmite paquetes de telemetría como prioridad. Optimizado para los datos del sensor. |
| ![](../assets/screenshots/roleTak.png) | TAK | Optimizado para la comunicación del sistema ATAK. Reduce las transmisiones de rutina. |
| ![](../assets/screenshots/roleTakTracker.png) | Rastreador TAK | Habilita las transmisiones automáticas de TAK PLI. Reduce las transmisiones de rutina. |
| ![](../assets/screenshots/roleLostAndFound.png) | Objetos perdidos | Transmite la ubicación como un mensaje al canal predeterminado para ayudar con la recuperación del dispositivo. |

[Elegir el rol de dispositivo adecuado →](https://meshtastic.org/blog/choosing-the-right-device-role/)

## Ejemplos completos de filas de nodos

La fila completa del nodo muestra el avatar del círculo, el nivel de batería, el estado de cifrado, la hora de última escucha, el rol del dispositivo, la intensidad de la señal y los indicadores de registro a la vez.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/standard_directConnected_dark.png" />
  <img src="../assets/screenshots/standard_directConnected.png" alt="Directly connected node, favorite, with signal meter" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/standard_multiHop_dark.png" />
  <img src="../assets/screenshots/standard_multiHop.png" alt="Multi-hop node 4 hops away" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/standard_mqtt_dark.png" />
  <img src="../assets/screenshots/standard_mqtt.png" alt="MQTT-bridged node" />
</picture>

## Ejemplos de filas de nodos compactos

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_directConnected_allInfo_dark.png" />
  <img src="../assets/screenshots/compact_directConnected_allInfo.png" alt="Directly connected node with all telemetry info" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_multiHop_dark.png" />
  <img src="../assets/screenshots/compact_multiHop.png" alt="Multi-hop node 7 hops away" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_withPosition_dark.png" />
  <img src="../assets/screenshots/compact_withPosition.png" alt="Node with position, 1 hop" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_pkiMismatch_dark.png" />
  <img src="../assets/screenshots/compact_pkiMismatch.png" alt="PKI key mismatch node" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_mqtt_dark.png" />
  <img src="../assets/screenshots/compact_mqtt.png" alt="MQTT-bridged node" />
</picture>

## Iconos adicionales

Toque un nodo y desplácese hasta la sección Registros para obtener métricas detalladas:

| Registro | Descripción |
|-----|-------------|
| ![Distance & Bearing](../assets/screenshots/logDistance.png) | Dirección y distancia al nodo según el GPS. Requiere que ambos dispositivos compartan la ubicación. |
| ![Channel badge](../assets/screenshots/channelBadge.png) | El círculo numerado muestra qué canal utiliza el nodo. Solo se muestra para canales secundarios (no canal primario 0). |
| ![Device Metrics](../assets/screenshots/logDeviceMetrics.png) | Nivel de la batería, voltaje, utilización del canal y tiempo de aire reportado por el nodo. |
| ![Positions](../assets/screenshots/logPositions.png) | Datos de posición GPS, incluyendo latitud, longitud y altitud. |
| ![Environment](../assets/screenshots/logEnvironment.png) | Datos del sensor: temperatura, humedad, presión barométrica. |
| ![Detection Sensor](../assets/screenshots/logDetectionSensor.png) | Alertas de movimiento o apertura/cierre de la puerta desde el nodo. |
| ![Trace Routes](../assets/screenshots/logTraceRoutes.png) | Rutas de seguimiento grabadas que muestran los saltos que un mensaje tomó a través de la malla. |

## Vista detallada del nodo

Toque cualquier nodo para ver la vista detallada completa con información de hardware, métricas de señal, sensores de entorno y navegación de registro:

![Node Detail](../assets/screenshots/nodeDetail.png)

[Documentos de configuración del dispositivo →](https://meshtastic.org/docs/configuration/radio/device/)

