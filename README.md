# Sitio web — Axolotl Discovery


## Axolotl for Education

Las herramientas educativas se leen de `assets/educacion.json`. Mientras esté vacío (`[]`),
la sección muestra un aviso de "próximamente". Para agregar una herramienta, añade un objeto:

```json
[
  {
    "titulo": "Nombre de la herramienta",
    "nivel": "Bachillerato · Licenciatura",
    "descripcion": "Qué hace y qué aprende el estudiante con ella.",
    "etiquetas": ["Docking", "Interactivo"],
    "enlace": "https://enlace-a-la-herramienta",
    "codigo": "https://github.com/Axolotl-Discovery/repo",
    "imagen": "./assets/educacion/captura.png"
  }
]
```

Solo `titulo` y `descripcion` son obligatorios; los demás campos se pueden omitir.

Para una app que se descarga (en vez de abrirse en el navegador) agrega:
`"descarga": true` (el enlace se abre en la misma pestaña), `"boton": "Descargar para Windows"`
(texto del botón; por defecto "Abrir herramienta") y `"nota": "Windows 10/11 · 160 MB"` (texto pequeño bajo el botón).
Para que el enlace siempre baje la última versión usa
`https://github.com/<org>/<repo>/releases/latest/download/<archivo>`.
