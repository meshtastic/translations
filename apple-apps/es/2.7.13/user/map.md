
# Mapa y puntos de referencia

La pestaña Mapa muestra todos los nodos que han compartido una posición, superpuestos a una capa base de Apple Maps.

## Pasadores de nodo

Cada nodo que ha informado de una posición GPS aparece como un pin circular de color en el mapa. La **línea continua verde** muestra un nodo conectado directamente; **líneas discontinuas naranjas** muestran nodos alcanzados a través de la malla. Una estrella púrpura marca un punto de referencia. Toque un pin para ver el nombre del nodo, la última hora escuchada, la información de la señal y un acceso directo para enviar un mensaje directo.

Los pines se actualizan automáticamente cuando se recibe un nuevo paquete de posición de la malla.

## Filtrando nodos en el mapa

Toque el **botón de filtro** (icono de embudo,`line.3.horizontal.decrease.circle`) En la barra de herramientas inferior derecha para abrir la hoja de filtros de mapas. Cuando cualquier filtro está activo, el icono aparece **lleno** para indicar que el filtrado está en vigor.

| Filtro | Descripción |
|--------|-------------|
| Vía LoRa | Mostrar solo los nodos escuchados directamente a través de la radio LoRa |
| Vía MQTT | Mostrar solo nodos puenteados a través de MQTT |
| En línea | Mostrar solo los nodos escuchados en las últimas 2 horas |
| Cifrado | Mostrar solo nodos que utilizan el cifrado PKI |
| Favoritos | Mostrar solo los nodos que has marcado como favoritos |
| Distancia | Límite a nodos dentro de un radio elegido de su ubicación actual |
| Salta Lejos | Deslizador de **Todos** a **7** — restringe por recuento de saltos (0 = solo directo) |
| Roles | Filtrar por uno o más roles de dispositivo (por ejemplo, enrutador, cliente, repetidor) |

> **Tip — Comprobación del rango de LoRa**
> Habilite el filtro **Via LoRa** y desactive **Via MQTT** para ver solo los nodos a los que se puede acceder directamente a través de la radio, lo que es útil para evaluar si un enlace LoRa directo es factible.

## Capas de mapa

Toque el icono de capa (arriba a la derecha) para cambiar entre:

| Capa | Descripción |
|-------|-------------|
| Parámetro | Apple Maps híbrido de calle/satélite predeterminado |
| Satélite | Imágenes aéreas |
| Superposiciones GeoJSON | Capas de mapas personalizadas cargadas desde`.geojson`Archivos en el almacenamiento de archivos de la aplicación |

## Puntos de referencia

Los waypoints se denominan puntos de interés que puedes compartir a través de la malla.

### Creando un punto de referencia

1. Presione largamente en cualquier lugar del mapa.
2. Introduzca un nombre, una descripción opcional y un icono de candado (para limitar la edición al creador).
3. Toque **Guardar**: el punto de ruta se transmite a todos los nodos del canal principal.

### Edición de un Waypoint

Toque un pin de punto de referencia existente y, a continuación, toque **Editar**. Cambia la transmisión a la malla inmediatamente.

### Eliminar un Waypoint

Toque el punto de referencia y, a continuación, toque **Eliminar**. La eliminación se transmite a todos los nodos.

## Sendero de nodos

Cuando un nodo ha reportado múltiples posiciones a lo largo del tiempo, una línea de seguimiento conecta las posiciones históricas en el mapa, mostrando la ruta del nodo.

## Tu ubicación

Su posición GPS actual aparece como un punto azul (indicador de ubicación estándar de iOS). Habilite la transmisión de posición en **Configuración → Posición** para compartir su ubicación con la malla.

