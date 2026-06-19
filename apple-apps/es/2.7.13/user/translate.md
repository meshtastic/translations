
# Traducir la aplicación

## Traducción automática de documentación

En los dispositivos con iOS 26 o posterior, la documentación de la aplicación se traduce automáticamente al idioma de su dispositivo cuando abre **Ayuda y documentos**.

**Así es como funciona:** La primera persona en usar una combinación particular de idioma + versión de aplicación traduce todas las páginas en el dispositivo y contribuye automáticamente con sus traducciones a la comunidad. Cada usuario después de ellos obtiene esas traducciones al instante, sin esperar a la traducción en el dispositivo.

1. Abres **Help & Docs** en un idioma que no sea inglés.
2. Si ya existen traducciones de la comunidad para su idioma y versión de la aplicación, se descargan al instante.
3. Si no, el marco de traducción de Apple traduce cada página en el dispositivo (~10 segundos por página).
4. Sus traducciones se cargan de forma anónima en el[Meshtastic/traducciones](https://github.com/meshtastic/translations)Repositorio.
5. El siguiente usuario en su idioma obtiene documentos traducidos al instante de la caché de la comunidad.

Sin registro, sin trabajo manual, simplemente sucede en segundo plano mientras usas la aplicación. Puede desactivar esto desactivando **Participar en las traducciones distribuidas** en la configuración de la aplicación.

> **Tip — Usuarios en inglés**
> Si el idioma de su dispositivo es el inglés, no se produce ninguna traducción y la documentación en inglés incluida se muestra directamente.

---

## Contribuir a las traducciones de la aplicación

Contribuir con las traducciones ayuda a que Meshtastic sea accesible para un público más amplio. Los hablantes nativos pueden revisar las cadenas traducidas por máquina para su idioma directamente en Xcode: busque cadenas marcadas como **Necesita revisión** y mejore cualquier cosa que suene poco natural. Si tiene un Mac con Apple Silicon, también puede ejecutar un script que utiliza Apple Intelligence en el dispositivo para generar cualquier traducción que falte para su idioma y abrir automáticamente una solicitud de extracción.

### Revisar traducciones automáticas

Todas las traducciones en el dispositivo se cargan en el[Meshtastic/traducciones](https://github.com/meshtastic/translations)Repositorio. Son un gran punto de partida, ¡pero las traducciones automáticas no son perfectas! Si eres un hablante nativo y detectas algo que podría mejorarse:

1. Navegue hasta su idioma + versión de la aplicación en[Aplicaciones de Apple/](https://github.com/meshtastic/translations/tree/main/apple-apps)
2. Editar el`.md`Archivo directamente en GitHub
3. Enviar una solicitud de extracción

Sus mejoras se servirán a todos los usuarios de ese idioma en el futuro, no se requiere codificación.

---

## Generar nuevas traducciones con un guión

### Requisitos

Antes de empezar, asegúrate de tener:

- **MacOS 26 o posterior** con Apple Silicon
- **Inteligencia de Apple habilitada** — Configuración del sistema → Inteligencia de Apple y Siri
- **[Localizador local](https://github.com/JoshuaSullivan/local-localizer)** Instalado (ver más abajo)
- **GitHub CLI** instalado —`brew install gh`Y`gh auth login`

### Instalar localizador local

```bash
git clone https://github.com/JoshuaSullivan/local-localizer.git ~/local-localizer
cd ~/local-localizer && swift build -c release
mkdir -p ~/bin && cp .build/release/local-localizer ~/bin/local-localizer
```

Asegúrate`~/bin`Está en tu PATH (agregar`export PATH="$HOME/bin:$PATH"`A su perfil de shell si es necesario).

### Añadir o completar una configuración regional

Clone el repositorio, luego ejecute el script de traducción con su código de configuración regional:

```bash
git clone https://github.com/meshtastic/Meshtastic-Apple.git
cd Meshtastic-Apple
scripts/translate-locale.sh <locale>
```

Por ejemplo:

```bash
scripts/translate-locale.sh fr          # French
scripts/translate-locale.sh de formal   # German, formal register
scripts/translate-locale.sh ja polite   # Japanese, polite register
scripts/translate-locale.sh zh-Hant-TW  # Traditional Chinese (Taiwan)
```

El guión:

1. Cuente cuántas cadenas faltan o necesitan actualizarse para la configuración regional
2. Genere un glosario que mantenga los términos de la marca Meshtastic (LoRa, MQTT, BLE, TAK, etc.) sin traducir
3. Ejecute el localizador local usando Apple Intelligence en el dispositivo, sin necesidad de Internet o clave de API
4. Marque cada cadena nueva como **Necesita revisión** para que los hablantes nativos sepan revisarlas
5. Confirmar el resultado y abrir una solicitud de extracción automáticamente

El paso de traducción se ejecuta completamente en su dispositivo y tarda entre 10 y 20 minutos en completar una configuración regional.

### Opciones de tono

| Tono | Cuándo usar |
|---|---|
| `professional` | Por defecto - claro y neutral, adecuado para la mayoría de los idiomas |
| `formal` | Recomendado para alemán (`de`), Francés (`fr`), Italiano (`it`), Español (`es`) — Selecciona la forma educada de segunda persona (Sie / vous / Lei / usted) |
| `polite` | Recomendado para japoneses (`ja`) Y coreano (`ko`) — Selecciona formas verbales educadas |
| `informal` | Registro casual |
| `neutral` | Sencillo, sin preferencia de registro |

### Revisión de cadenas traducidas

Una vez que el PR está abierto, cualquier hablante nativo puede revisar las traducciones directamente en Xcode:

1. Aire libre`Meshtastic.xcworkspace`
2. Seleccionar`Localizable.xcstrings`En el navegador de proyectos
3. Filtra por tu configuración regional y establece el filtro de estado en **Necesita revisión**
4. Lea cada cadena en contexto, edítelo si es necesario y márquelo **Revisado**
5. Empuje sus cambios a la rama de relaciones públicas

¡Gracias por ayudar a ampliar el alcance de Meshtastic!

