
# Traducir la aplicación

Contribuir con traducciones a la aplicación Meshtastic Apple ayuda a que el proyecto sea accesible para una audiencia más amplia. La aplicación utiliza [string catalogs](https://developer.apple.com/documentation/xcode/localizing-and-varying-text-with-a-string-catalog) en Xcode para gestionar las traducciones.

## Traducción automática de documentación

En los dispositivos que ejecutan iOS 26 o posterior, la documentación de la aplicación se traduce automáticamente al idioma de su dispositivo cuando abre la sección **Ayuda y documentos**. La canalización de traducción funciona de la siguiente manera:

1. La aplicación lee los archivos fuente de rebajas en inglés incluidos.
2. Los segmentos de texto se traducen utilizando el marco de traducción de Apple. Si el marco de traducción no es compatible con su idioma, la aplicación vuelve a FoundationModels en el dispositivo.
3. La rebaja traducida se almacena en caché localmente para que las visitas posteriores se carguen al instante.
4. El margen traducido se convierte a HTML en el dispositivo y se muestra en el visor de documentos.

Después de que todas las páginas de documentación se hayan traducido en segundo plano, la aplicación carga automáticamente los archivos de rebajas traducidos en el repositorio [meshtastic/translations](https://github.com/meshtastic/translations). Esto permite a la comunidad revisar y mejorar las traducciones generadas por máquina.

> **Tip — Usuarios en inglés** Si el idioma de su dispositivo es inglés, no se produce ninguna traducción y la documentación en inglés incluida se muestra directamente.

## Cómo contribuir con las traducciones de la interfaz de usuario

Si desea actualizar las traducciones para una configuración regional existente o agregar un nuevo idioma, siga estos pasos:

1. Bifurcar el [repositorio Meshtastic-Apple](https://github.com/meshtastic/Meshtastic-Apple/tree/main) a su cuenta de GitHub.
2. Clona el proyecto y abre`Meshtastic.xcworkspace`En Xcode.
3. Selecciona el`Localizable.xcstrings`Archivo en el navegador del proyecto.
4. Siga los [pasos para añadir o actualizar traducciones](https://developer.apple.com/documentation/xcode/localizing-and-varying-text-with-a-string-catalog) en la documentación de Apple.
5. Crea una solicitud de extracción en el proyecto con tus cambios.

Su contribución será revisada y, tras la aprobación, su traducción se incluirá en la próxima versión de la aplicación Meshtastic de Apple.

> **Tip — ¿Nuevo idioma? ** Si está agregando un idioma que aún no está presente en el proyecto, abra la configuración del proyecto Xcode, vaya a **Información → Localizaciones** y agregue la nueva configuración regional antes de editar`Localizable.xcstrings`.

¡Gracias por ayudar a ampliar el alcance de Meshtastic!

