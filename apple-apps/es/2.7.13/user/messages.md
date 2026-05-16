
# Mensajes y canales

Meshtastic utiliza un sistema de canales para transmisiones grupales y mensajes directos para conversaciones privadas individuales.

## Canal

### Historial de mensajes

Las conversaciones de canal cargan los **50 mensajes** más recientes de forma predeterminada. Desplácese hasta la parte superior y toque **Cargar más** para buscar el siguiente lote. Esto mantiene la aplicación receptiva en los canales con miles de mensajes.

### Índice de canal

| Representación | Significado |
|--------|---------|
| **0** (círculo primario) | Canal principal: los paquetes de transmisión se envían aquí. Los datos de ubicación se transmiten desde el primer canal en el que están habilitados (firmware 2.7+). |
| **1–7** | Canales secundarios: grupos de mensajería separados, cada uno protegido por su propia clave. |

### Configuración del canal

![Channel form](../assets/screenshots/channelForm_primary.png)

El formulario de canal le permite configurar el nombre del canal, la clave de cifrado, el rol, el uso compartido de posiciones y la configuración del enlace ascendente/descendente MQTT.

### Seguridad del canal

| Icono | Significado |
|------|---------|
| ![Securely Encrypted](../assets/screenshots/lockClosed.png) | **Encriptado de forma segura**: el canal utiliza una clave AES de 128 o 256 bits. |
| ![Not Securely Encrypted](../assets/screenshots/lockOpen.png) | **No está cifrado de forma segura** - el canal no utiliza ninguna clave o una clave conocida de 1 byte, pero no se utiliza para datos de ubicación precisos. |
| ![Insecure with Location](../assets/screenshots/lockOpenRed.png) | **Inseguro con la ubicación**: el canal no está cifrado de forma segura y se utiliza para datos de ubicación precisos. |
| ![Insecure with MQTT](../assets/screenshots/lockOpenMqtt.png) | **Inseguro con MQTT** - los datos de ubicación no cifrados de forma segura y precisos se están vinculando a Internet a través de MQTT. |

---

> **Tip — Compartir canales**
> Un código QR contiene la configuración de LoRa y los canales necesarios para que las radios se comuniquen. Utilice **Reemplazar canales** para sobrescribir o **Añadir canales** para añadir a los canales existentes.

> **Tip — Gestionar canales**
> El canal principal maneja el tráfico de transmisión. Agregue canales secundarios para grupos de mensajería separados, cada uno asegurado por su propia clave.

> **Tip — Administración habilitada**
> Seleccione un nodo en el menú desplegable para administrar dispositivos conectados o remotos.

---

## Mensajes directos

### Contactos

| Elemento | Significado |
|---------|---------|
| ![Favorites](../assets/screenshots/favorite.png) | **Favoritos**: los contactos favoritos y los nodos con mensajes recientes aparecen en la parte superior de la lista de contactos. |
| ![Long press](../assets/screenshots/longPress.png) | **Acciones de presión larga**: presione prolongadamente para marcar como favorito o silenciar el contacto, o eliminar una conversación. |

### Cifrado

![Encryption legend](../assets/screenshots/lockLegend.png)

| Icono | Significado |
|------|---------|
| ![Shared Key](../assets/screenshots/lockOpen.png) | **Clave compartida**: los mensajes directos están utilizando la clave compartida para el canal. |
| ![Public Key Encryption](../assets/screenshots/lockClosed.png) | **Cifrado de clave pública** - los mensajes directos utilizan la infraestructura de clave pública para el cifrado. Requiere firmware 2.5 o posterior. |
| ![PKI Mismatch](../assets/screenshots/keySlash.png) | **Desajuste de clave pública** - la clave pública más reciente para este nodo no coincide con la clave grabada anteriormente. Verifique con quién está enviando mensajes comparando claves públicas en persona o por teléfono. |

---

### Reacciones de tapback

Presione largamente cualquier mensaje y toque **Tapback** para enviar una reacción emoji.

![Tapback input](../assets/screenshots/tapbackInput.png)

---

> **Tip — Mensajes**
> Enviar transmisiones de canales y mensajes directos. Presione largamente cualquier mensaje para acciones como copiar, responder, tapback y detalles de entrega.

---

## Estado del mensaje

![Message status reference](../assets/screenshots/ackErrors.png)

| Color | Significado |
|--------|---------|
| Gris | Entrega exitosa. |
| Burbuja naranja | **Reconocido por otro nodo** — el mensaje fue retransmitido, pero no confirmado por el destinatario final. |

Los siguientes errores pueden aparecer en una burbuja de mensaje (rojo a menos que se indique lo contrario):

| Estatus | Descripción |
|--------|-------------|
| No hay ruta | No se encontró ninguna ruta hacia el nodo de destino. |
| Tengo NAK | El nodo de destino rechazó explícitamente el mensaje. |
| Tiempo de espera | El mensaje se agotó a la espera de confirmación. |
| Sin interfaz | La interfaz de radio no está disponible. |
| Retransmitir máximo | Máximos intentos de retransmisión alcanzados sin éxito. |
| Sin canal | El canal especificado no existe en el destino. |
| Demasiado grande | El paquete supera el tamaño máximo permitido. |
| Sin respuesta | No se recibió respuesta del destino. |
| PKI falló | Falló el cifrado/descifrado de la infraestructura de clave pública. |
| Mala solicitud | Paquete malformado rechazado por el destino. |
| No autorizado | El nodo de destino rechazó la solicitud debido a los permisos. |

> Gris indica entrega exitosa. El naranja indica un error que se puede volver a intentar. El rojo indica un fallo permanente que no tendrá éxito al volver a intentarlo.

---

## Apariencia del enlace

Enlaces en burbujas de mensajes, incluyendo URL, enlaces de canales Meshtastic y rebajas`[text](url)`Enlaces: están diseñados con un subrayado y el color del enlace de los estándares de diseño (Azul 400). Esto hace que los enlaces sean visualmente distintos del texto de mensaje normal tanto en modo claro como oscuro. Al tocar un enlace se abre en el navegador, o para las URL de canales/contacto de Meshtastic, se abre el controlador apropiado en la aplicación.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/messageText_link_dark.png">
  <img src="../assets/screenshots/messageText_link.png" alt="Message bubble with styled link">
</picture>

---

## Formato de mensajes (iOS 18+)

En iOS 18 y posteriores, los botones de formato aparecen en la barra de herramientas compacta debajo del campo de redacción después de haber escrito al menos 3 caracteres. Los botones de formato comparten la fila de la barra de herramientas con la campana de alerta, el pin de posición y el contador de bytes, todos representados como iconos compactos. La barra de herramientas se desplaza horizontalmente si excede el ancho de la pantalla.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/composeArea_formatting_dark.png">
  <img src="../assets/screenshots/composeArea_formatting.png" alt="Compose area with formatting toolbar and live preview">
</picture>

### Estilos compatibles

| Pulsador | Estilo | Sintaxis de rebaja |
|--------|-------|-----------------|
| Símbolo SF en negrita | Atrevido | `**texto**` |
| Símbolo SF en cursiva | Cursiva | `*texto*` |
| Símbolo de SF tachado | Tachado | `~~texto~~` |
| Símbolo de código SF | Codificación | `` `texto` `` |
| Enlace al símbolo SF | Enlace | `[texto](url)` |

### Cómo formatear texto

1. **Seleccione texto y toque un botón**: seleccione una palabra o frase en el campo de composición y, a continuación, toque un botón de formato. Los delimitadores de rebajas apropiados se insertan alrededor de la selección. Cualquier delimitador de rebaja existente dentro de la selección se elimina primero para evitar la superposición de la sintaxis. El espacio en blanco en los bordes de la selección se mueve fuera de los delimitadores para que Markdown se represente correctamente.
2. **Primero toque un botón, luego escriba** - con el cursor colocado (sin selección), toque un botón de formato. Se insertan delimitadores y el cursor se coloca entre ellos para que pueda escribir texto formateado inmediatamente.
3. **Desactivar**: seleccione el texto que ya está envuelto con delimitadores y toque el mismo botón de formato para eliminar los delimitadores.

### Vista previa en vivo

Cuando el campo de composición contiene la sintaxis de rebaja, aparece una burbuja de vista previa sobre el campo de redacción que muestra cómo se verá el mensaje cuando se envíe. La vista previa se actualiza en tiempo real a medida que escribes. Cuando no hay rebajas, la vista previa está oculta.

El formato de Markdown también se representa en las vistas previas de la lista de mensajes del canal y del usuario, para que pueda ver el texto formateado de un vistazo.

| Ejemplo | Descripción |
|---------|-------------|
| ![Bold preview](../assets/screenshots/messagePreview_bold.png) | Vista previa que muestra el formato **negrita** aplicado al texto. |
| ![Mixed preview](../assets/screenshots/messagePreview_mixed.png) | Vista previa que muestra **negrita**, *cursiva*, ~~strikethrough~~ y formato `code` combinados. |

### Cambio de estilo

Cuando selecciona texto que ya contiene delimitadores de rebaja y aplica un estilo diferente, los delimitadores existentes se eliminan y se reemplazan por el nuevo estilo. Por ejemplo, seleccionando`**bold**`Y tocar Strikethrough produce`~~bold~~`.

Después de aplicar un estilo, la selección se expande para incluir los delimitadores (por ejemplo, seleccionando`dolphin`Y al tocar selecciones en negrita`**dolphin**`), Lo que facilita el apagado o el cambio a un estilo diferente inmediatamente.

### Seguridad de selección

Si su selección se superpone parcialmente a los delimitadores existentes, la selección se expande automáticamente para incluir la ejecución del delimitador completo antes del formato. Cualquier carácter delimitador huérfano (no emparejado) que quede en otra parte del texto se limpia automáticamente. Esto evita las rebajas convertidas como`th***~~~~~~e~~`.

### Usuarios de iOS 17

La barra de herramientas de formato solo está disponible en iOS 18 y versiones posteriores. Los usuarios de iOS 17.x ven el campo de composición estándar sin cambios en su experiencia.

### Mac Catalyst

En Mac Catalyst, presionar **Enter** envía el mensaje. Presione **Shift+Enter** para insertar un salto de línea. El botón de la paleta de caracteres permanece disponible junto con los botones de formato.

> **Tip — Límite de mensajes**
> Los mensajes están limitados a 200 bytes. Los delimitadores de Markdown cuentan para este límite (por ejemplo,`**bold**`Utiliza 4 bytes adicionales para el`**`Pares). El contador de bytes en la barra de herramientas muestra el espacio restante.

