
# ¿Qué te cuentas?

Cambios arquitectónicos y de procedimiento recientes de aproximadamente los últimos 12 meses. El más nuevo en la parte superior.

<!-- DEV_WHATS_NEW_START -->
<!-- Añadir nuevas entradas en la parte superior. Formato:
**Mes AAAAY** —[Página o área](relative/path.md)- Una frase sobre lo que cambió arquitectónica o procedimentalmente.
Muestra aproximadamente los últimos 12 meses de cambios; archivos de más de un año eliminándolos.
-->

**Mayo de 2026** —[Enlaces profundos](deep-links.md)— Añadido`audio`Y`neighborInfo`Enlaces profundos para las nuevas pantallas de configuración del módulo.

**Mayo de 2026** —[Arquitectura](architecture.md)— Audio, pantallas de configuración del módulo de información del vecino; campos de umbral del contador Pax; selector de orientación de la brújula;`IntervalConfiguration.neighborInfo`Caso de enumeración para el selector de intervalos de actualización.

**Mayo de 2026** —[Arquitectura](architecture.md)— Canal de traducción de documentos (`009`): Traducción a nivel de rebaja con alimentación CDN de la comunidad, almacenamiento en caché basado en manifiestos y contribución automática de vuelta a`meshtastic/translations`Recompra.

**Mayo de 2026** —[Arquitectura](architecture.md)— Traducción automática de documentos (`008`): Integración del marco de traducción de Apple en el dispositivo para documentos en la aplicación, con caché basada en archivos en Soporte de aplicaciones.

**Mayo de 2026** —[Arquitectura](architecture.md)— Barra de herramientas de formato de mensajes (`004`): Barra de herramientas de marcado SwiftUI pura usando`TextSelection`(iOS 18+), almacenamiento de rebajas sin procesar en el existente`messagePayload`Campo - sin cambios de esquema.

**Mayo de 2026** —[SwiftDatos](swiftdata.md)— Estrategia de guardado documentada (autoguardación desactivada, guardados desactivados),`@Attribute(.unique)`Índices y límites de datos para posiciones/telemetría/mensajes. Fijo rano`QueryCoreData`/`UpdateCoreData`Referencias.

**Mayo de 2026** —[CarPlay](carplay.md)— Límites de captura documentados y predicados en las consultas de datos de CarPlay.

**Mayo de 2026** —[Enlaces profundos](deep-links.md)— Añadido`coreDataBrowser`Enlace profundo para el navegador de la base de datos SwiftData.

**Mayo de 2026** —[Pruebas](testing.md)— Convenciones de prueba de instantáneas establecidas: vistas multiestado consolidadas en imágenes combinadas individuales (pares claros + oscuros), uso`assertViewSnapshot`Ayudante con explícito`width`/`height`Y`transparent: true`Para instantáneas de iconos.

**Mayo de 2026** —[Arquitectura](architecture.md)— Sistema de documentación en la aplicación añadido (`003-app-docs-markdown`): Fuente de rebaja bajo`docs/user/`Y`docs/developer/`Se convierte a HTML por`scripts/build-docs.sh`Y empaquetado en`Meshtastic/Resources/docs/`.

**Abril de 2026** —[Transporte](transport.md)— Extensiones de transporte y ciclo de vida de conexión documentados de AccessoryManager.

**Mar 2026** —[SwiftDatos](swiftdata.md)— Guía inicial para desarrolladores de SwiftData: configuración de ModelContainer,`@Query`Uso,`MeshPackets`Actor, migraciones de esquemas.
<!-- DEV_WHATS_NEW_END -->

