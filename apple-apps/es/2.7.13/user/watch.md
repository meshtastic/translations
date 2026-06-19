
# Aplicación Apple Watch

La aplicación Meshtastic Apple Watch es un complemento de la aplicación para iPhone que pone dos características en su muñeca: una **brújula Foxhunt** para la búsqueda de direcciones de radio y un **panel de conectividad telefónica** para confirmar que su reloj está sincronizado.

Los datos del nodo se envían al reloj automáticamente cada vez que la aplicación del iPhone está dentro del alcance a través de WatchConnectivity. No se requiere conexión Bluetooth a su radio Meshtastic en el propio reloj.

## Requisitos

| Requisito | Información interna |
|-------------|---------|
| Apple Watch | watchOS emparejado con el iPhone |
| Aplicación para iPhone | Aplicación Meshtastic para iPhone abierta y conectada a una radio |
| Lugar | Servicios de ubicación de reloj habilitados para la búsqueda de direcciones |
| Cercanía | Reloj y iPhone dentro del rango normal de Bluetooth/Wi-Fi entre sí |

## Pestañas

La aplicación Watch utiliza un diseño de página vertical. Desliza el dedo hacia arriba o hacia abajo para cambiar entre las dos pestañas.

### Caza de zorros

La pestaña Foxhunt enumera los nodos de malla que están dentro de **½ milla (≈ 800 m)** de su ubicación actual del reloj y tienen una posición GPS conocida. Los nodos marcados como objetivos de caza de zorros de la aplicación para iPhone siempre aparecen en la parte superior de la lista, independientemente de la distancia.

Cada fila muestra:

| Elemento | Significado |
|---------|---------|
| Círculo de colores | Nombre corto del nodo, color derivado del número del nodo |
| Nombre | Nombre largo del nodo |
| Distancia | Distancia desde su ubicación actual, codificada por colores por proximidad |
| Flecha | Mini flecha de rodamiento apuntando hacia el nodo, gira con su rumbo |

Toque cualquier fila para abrir la **Brújula Foxhunt** para ese nodo.

#### Brújula de caza de zorros

La brújula apunta hacia el nodo seleccionado usando el sensor de rumbo de su reloj. Está diseñado para la búsqueda de direcciones por radio (cazazoro) - caminar hasta que la flecha apunte hacia adelante y la distancia sea cero.

| Elemento | Significado |
|---------|---------|
| Dial giratorio | Las direcciones cardinales (N/NE/E...) giran con su rumbo físico |
| Triángulo naranja | Indicador norte fijo en la parte superior del anillo |
| Flecha de color | Flecha de rodamiento apuntando hacia el nodo objetivo |
| Cono de dirección | Cuña translúcida que resalta la dirección del objetivo |
| Círculo central | La dirección actual en grados, rumbo al objetivo y distancia |
| Círculo de nodos | Insignia de nombre corto del nodo objetivo |

**Codificación de color de distancia:**

| Color | Distancia |
|--------|----------|
| Rojo | Lejos (> 2⁄3 de ½ milla) |
| Amarillo | Rango medio (1⁄3 - 2⁄3 de ½ milla) |
| Verde | Cerrar (< 1⁄3 de ½ milla) |

**Retroalimentación háptica:** El reloj toca tu muñeca cuando estás mirando a menos de 10° del rodamiento del nodo objetivo, útil cuando no puedes mirar la pantalla.

### Teléfono

La pestaña Teléfono muestra el estado de conectividad entre su reloj y la aplicación de iPhone asociada.

| Estado | Significado |
|-------|---------|
| Teléfono conectado (verde) | La aplicación del iPhone es accesible; se muestra el número de nodos |
| Teléfono no disponible | El reloj está fuera de alcance o la aplicación del iPhone no funciona |

Toque **Actualizar** para solicitar una lista de nodos actualizada desde la aplicación del iPhone. Si el teléfono no está disponible temporalmente, el reloj vuelve a los datos del nodo recibidos más recientemente.

## Establecer objetivos de caza de zorros

Desde la aplicación para iPhone, marque un nodo como un objetivo de caza de zorros desde su vista de detalles. Los nodos marcados se empujan al reloj y se fijan en la parte superior de la lista de caza de zorros, independientemente de la distancia, útil cuando sabes qué nodo estás cazando antes de estar dentro de un rango de ½ milla.

