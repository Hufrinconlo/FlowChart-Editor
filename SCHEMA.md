# Formato de archivo de proyecto (.json)

Este es el formato que genera "Guardar proyecto" en el editor (`saveProject()` /
`quickSaveProject()` en [EDITOR.html](EDITOR.html)). Es JSON plano — se puede editar
a mano o con una IA sin necesidad de leer el resto del programa.

## Estructura raíz

```json
{
  "nodes": [ /* array de figuras — ver abajo */ ],
  "conns": [ /* array de conexiones — ver abajo */ ],
  "nid": 3,
  "cid": 2,
  "v": 1,
  "ts": 1755000000000,
  "projMeta": { "desc": "", "owner": "", "version": "", "notes": "" }
}
```

- `nid` / `cid`: contadores usados para generar el próximo id (`"n"+nid`, `"c"+cid`).
  Si añades nodos/conexiones a mano, sube estos contadores para que coincidan con
  el id numérico más alto que uses (evita colisiones al seguir editando en la app).
- `v`: número de versión del formato. Déjalo en `1`.
- `ts`: timestamp de guardado (`Date.now()`), informativo, no se usa para lógica.
- `projMeta`: metadatos opcionales del proyecto (todos los campos son texto libre,
  se puede omitir el objeto entero o dejar campos vacíos).

## Nodo (figura)

```json
{
  "id": "n1",
  "shape": "rect",
  "color": "blue",
  "x": 120,
  "y": 80,
  "w": 160,
  "h": 60,
  "title": "Recibir pedido",
  "sub": "",
  "info": "",
  "consideraciones": "",
  "normativa": [],
  "durVal": 0,
  "durUnit": "min",
  "fontFamily": "",
  "fontSize": 0
}
```

| Campo | Tipo | Notas |
|---|---|---|
| `id` | string | único, formato `"n"+número` |
| `shape` | string | ver **Shapes válidos** abajo |
| `color` | string | ver **Colores válidos** abajo |
| `x`, `y` | number | esquina superior izquierda, en px, sistema de coordenadas del canvas |
| `w`, `h` | number | ancho/alto en px |
| `title` | string | texto principal dentro de la figura |
| `sub` | string | subtítulo/texto secundario, opcional |
| `info` | string | texto largo mostrado en el panel de detalle / sidebar exportado, opcional |
| `consideraciones` | string | texto largo adicional, opcional |
| `normativa` | string[] | lista de referencias normativas (líneas de texto), opcional |
| `durVal` | number | valor de duración, `0` = sin badge de duración |
| `durUnit` | string | `"min"`, `"h"` o `"d"` (minutos/horas/días) |
| `fontFamily` | string | CSS font-family; `""` = usa la fuente por defecto (`'DM Sans', sans-serif`) |
| `fontSize` | number | `0` = tamaño automático |
| `customColor` | object | opcional, solo cuando `color:"custom"` — ver **Color personalizado** abajo |

### Shapes válidos

`rect`, `diamond`, `circle`, `cylinder`, `parallelogram`, `hexagon` (figuras normales),
`anno-text`, `anno-rect`, `anno-ellipse` (anotaciones de fondo, sin puertos de conexión).

### Colores válidos

`blue`, `cyan`, `teal`, `green`, `emerald`, `yellow`, `amber`, `orange`, `red`, `rose`,
`pink`, `purple`, `violet`, `indigo`, `gray`, `slate`, `dark`
(paletas fijas definidas en `PAL`), o `custom` para un color arbitrario (ver abajo).

### Color personalizado

Cuando `color` es `"custom"`, el nodo debe incluir además un objeto `customColor` con
los 4 tonos derivados (el editor los genera solos a partir de un único hex elegido por
el usuario, aclarándolo hacia blanco para el relleno):

```json
"color": "custom",
"customColor": { "fill": "#d9e8f5", "stroke": "#2f6fb0", "light": "#eef5fb", "text": "#111111" }
```

| Campo | Notas |
|---|---|
| `fill` | color de relleno de la figura (versión clara del color elegido) |
| `stroke` | el color elegido por el usuario, tal cual — define el borde |
| `light` | versión aún más clara, usada en el resalte superior de la figura "base de datos" |
| `text` | color del texto dentro de la figura (`#111111` en la práctica siempre, ya que `fill` siempre queda lo bastante claro) |

Si editas esto a mano, basta con fijar `stroke` al hex deseado y aclarar los otros dos
hacia blanco (mezclando ~72% para `fill`, ~86% para `light`).

## Conexión (flecha entre nodos)

```json
{
  "id": "c1",
  "from": "n1",
  "fromPort": "right",
  "to": "n2",
  "toPort": "left",
  "style": "straight",
  "dash": "solid",
  "arrowEnd": "arrow",
  "color": "#64748b",
  "weight": 2,
  "label": ""
}
```

| Campo | Tipo | Notas |
|---|---|---|
| `id` | string | único, formato `"c"+número` |
| `from`, `to` | string | ids de los nodos origen/destino |
| `fromPort`, `toPort` | string | `"top"`, `"right"`, `"bottom"`, `"left"` |
| `style` | string | `"straight"`, `"curved"`, `"ortho"` (`"step"` es un valor legacy equivalente a `"ortho"`) |
| `dash` | string | `"solid"`, `"dashed"`, `"dotted"`, `"dashdot"` |
| `arrowEnd` | string | `"arrow"`, `"filled"`, `"none"`, `"circle"`, `"diamond-end"`, `"both"` |
| `color` | string | color hex de la línea, ej. `"#64748b"` |
| `weight` | number | grosor de línea en px |
| `label` | string | texto sobre la línea, opcional (se autocompleta "Sí"/"No" al salir de un `diamond`) |
| `midOffsetX` / `midOffsetY` | number | opcional, solo para `style:"ortho"`; desplazamiento manual del punto medio de la ruta. Omitir = ruta automática |

## Ejemplo mínimo (dos nodos conectados)

```json
{
  "nodes": [
    { "id":"n1","shape":"circle","color":"green","x":80,"y":80,"w":160,"h":50,
      "title":"Inicio","sub":"","info":"","consideraciones":"","normativa":[],
      "durVal":0,"durUnit":"min","fontFamily":"","fontSize":0 },
    { "id":"n2","shape":"rect","color":"blue","x":80,"y":200,"w":160,"h":60,
      "title":"Procesar","sub":"","info":"","consideraciones":"","normativa":[],
      "durVal":0,"durUnit":"min","fontFamily":"","fontSize":0 }
  ],
  "conns": [
    { "id":"c1","from":"n1","fromPort":"bottom","to":"n2","toPort":"top",
      "style":"straight","dash":"solid","arrowEnd":"arrow","color":"#64748b",
      "weight":2,"label":"" }
  ],
  "nid": 2,
  "cid": 1,
  "v": 1,
  "ts": 0
}
```

Para editar un flujograma con una IA: pásale este archivo junto con el `.json`
exportado del proyecto, y puede modificar nodos/conexiones directamente sin
necesitar leer [EDITOR.html](EDITOR.html).
