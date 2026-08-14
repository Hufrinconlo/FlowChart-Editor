# FlujoDiagram — Editor de flujo de procesos

Editor visual para crear diagramas de flujo y procesos, construido como un único archivo HTML sin dependencias externas. Guarda el proyecto en el navegador automáticamente y permite exportarlo como JSON o como HTML autónomo.

## Cómo usar

Abre `EDITOR.html` directamente en el navegador. No requiere instalación ni servidor.

## Funcionalidades

- **Figuras**: proceso, decisión, inicio/fin, base de datos, entrada/salida y preparación. Personalización de color, tamaño, título, subtítulo, descripción, duración y normativa aplicable.
- **Conexiones**: línea recta, curva y ortogonal (se adapta automáticamente según los puertos de conexión). Color, grosor, trazo y tipo de punta configurables. Las líneas ortogonales tienen un punto de ajuste arrastrable.
- **Selección múltiple**: arrastra sobre el canvas para seleccionar varias figuras con el lazo, o usa `Ctrl+A`. Las figuras seleccionadas se pueden mover o eliminar juntas.
- **Navegación**: rueda del ratón para hacer zoom (también `Ctrl +` / `Ctrl -` / `Ctrl 0`). Clic derecho + arrastrar para desplazarse por el canvas. Mini-mapa en la esquina inferior derecha.
- **Historial**: deshacer y rehacer ilimitados (`Ctrl+Z` / `Ctrl+Y`).
- **Otros**: duplicar figuras (`Ctrl+D`), copiar y pegar (`Ctrl+C` / `Ctrl+V`), editar etiquetas de conexión, modal de detalle con doble clic, guardado automático en el navegador.
- **Guardar/abrir**: descarga el diagrama como archivo `.json` (formato documentado en [SCHEMA.md](SCHEMA.md)) o gestiona varios proyectos guardados en el navegador desde el modal "Proyectos".
- **Exportar**: genera un archivo HTML autónomo listo para compartir o imprimir.

## Atajos de teclado

| Atajo | Acción |
|-------|--------|
| `Ctrl+Z` | Deshacer |
| `Ctrl+Y` | Rehacer |
| `Ctrl+A` | Seleccionar todo |
| `Ctrl+C` / `Ctrl+V` | Copiar / pegar figura |
| `Ctrl+D` | Duplicar figura |
| `Ctrl+0` | Restablecer zoom al 100 % |
| `Ctrl++` / `Ctrl+-` | Acercar / alejar |
| `Supr` / `Retroceso` | Eliminar selección |
| `Escape` | Cancelar conexión / deseleccionar |
| `Rueda del ratón` | Zoom |
| `Clic derecho + arrastrar` | Desplazar el canvas |
| `Shift+clic` | Añadir figura a la selección múltiple |

**2 do list:**