# Sitio web — Axolotl Discovery

Sitio estático de una sola página (`index.html`), sin dependencias externas ni
proceso de build. Pensado para publicarse gratis en GitHub Pages.

## Estructura

```
Sitio Web/
├── index.html          ← todo el sitio (HTML + CSS + JS en un solo archivo)
├── assets/
│   ├── logo-full.png    ← logo con el nombre, para uso general
│   ├── logo-icono.png   ← solo el ícono, usado en el sitio (header, footer)
│   ├── proyectos.json   ← contenido del portafolio
│   ├── lib/3Dmol-min.js ← visor 3D de moléculas (3Dmol.js 2.5.5, licencia BSD)
│   └── proyectos/       ← archivos de cada proyecto (PDB, imágenes)
│       └── AXD-GAL-26-01/  ← 6 complejos MOR + ligando
└── README.md
```

## Cómo publicarlo en GitHub Pages

1. Crea un repositorio nuevo en GitHub (puede ser público o privado si tienes
   plan de pago; GitHub Pages gratuito requiere que sea público).
2. Sube el contenido de esta carpeta (`index.html`, la carpeta `assets/`) a
   la raíz del repositorio — por ejemplo, arrastrando los archivos en la
   página web de GitHub, o con:
   ```
   git init
   git add index.html assets README.md
   git commit -m "Sitio de Axolotl Discovery"
   git branch -M main
   git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
   git push -u origin main
   ```
3. En GitHub, ve a **Settings → Pages** del repositorio.
4. En "Build and deployment", selecciona **Deploy from a branch**, rama
   `main` y carpeta `/ (root)`. Guarda.
5. En un par de minutos tu sitio estará disponible en:
   `https://TU-USUARIO.github.io/TU-REPO/`

Si quieres un dominio propio (por ejemplo axolotldiscovery.com), en la misma
sección de Pages puedes configurar un "Custom domain".

## Sobre el formulario de cotización

El formulario de la sección "Cotiza tu proyecto" NO tiene backend (GitHub
Pages solo sirve archivos estáticos, no puede recibir formularios
directamente). Ahora mismo funciona así: al dar clic en "Solicitar
cotización", se arma automáticamente un correo dirigido a
`fcoarmandosd@ieee.org` con todos los datos capturados, y se abre el
programa de correo del visitante (Gmail, Outlook, etc.) para que solo dé
"Enviar".

Esto funciona sin necesidad de configurar nada, pero depende de que el
visitante tenga un cliente de correo configurado en su navegador/dispositivo
(en computadoras suele funcionar bien; en celular a veces abre la app de
correo directamente).

### Alternativa: recibir las solicitudes sin depender del correo del visitante

Si más adelante quieres que las cotizaciones lleguen directo a tu bandeja sin
pasarle esa responsabilidad al visitante, puedes usar un servicio gratuito
como [Formspree](https://formspree.io) (o similar):

1. Crea una cuenta gratuita en Formspree y un formulario nuevo — te dará una
   URL tipo `https://formspree.io/f/xxxxxxx`.
2. En `index.html`, cambia la etiqueta `<form class="quote" id="quoteForm">`
   por:
   ```html
   <form class="quote" id="quoteForm" action="https://formspree.io/f/xxxxxxx" method="POST">
   ```
3. Borra (o deja, no estorba) el bloque de JavaScript que arma el `mailto`,
   ya que Formspree se encarga del envío directamente.

Dímelo si quieres que te ayude a hacer ese cambio cuando tengas la cuenta
creada.

## Cómo agregar un proyecto al portafolio

La sección "Proyectos" del sitio lee su contenido del archivo
`assets/proyectos.json`. Para agregar un proyecto nuevo, no hace falta
tocar el HTML — solo edita ese archivo con cualquier editor de texto
(Bloc de notas, VS Code, etc.) y agrega un objeto nuevo a la lista.

Estructura de cada proyecto:

```json
{
  "titulo": "Nombre corto del proyecto",
  "cliente": "Nombre del cliente (opcional, déjalo vacío \"\" si prefieres no mostrarlo)",
  "resumen": "Una o dos frases describiendo el proyecto y lo que se hizo.",
  "etiquetas": ["Docking multi-diana", "AlphaFold2", "3 dianas"],
  "imagen": "./assets/proyectos/nombre-imagen.png",
  "enlace": "https://github.com/Axolotl-Discovery/nombre-del-repo"
}
```

Todos los campos son opcionales excepto `titulo` y `resumen` — si no tienes
imagen o enlace, simplemente omite esas líneas (o dales el valor `""`).

Ejemplo con dos proyectos en el archivo:

```json
[
  {
    "titulo": "Botrytis cinerea — biocontrol",
    "cliente": "",
    "resumen": "Docking de tres compuestos naturales contra tres dianas de Botrytis cinerea, con controles positivos frente a fungicidas comerciales.",
    "etiquetas": ["Multi-diana", "Controles positivos", "AutoDock Vina"]
  },
  {
    "titulo": "Otro proyecto",
    "cliente": "Nombre del cliente",
    "resumen": "Descripción breve.",
    "etiquetas": ["Alebrije", "AlphaFold2"]
  }
]
```

### Visor 3D de estructuras (.pdb)

Si un proyecto tiene el campo `estructuras`, su tarjeta se muestra a todo lo
ancho con un visor 3D interactivo (3Dmol.js) en lugar de imagen. Cada
estructura es un botón para cambiar de complejo:

```json
"estructuras": [
  { "nombre": "Fentanilo · sitio", "archivo": "./assets/proyectos/AXD-GAL-26-01/MOR_fentanilo_site2.pdb" },
  { "nombre": "NF1 · sitio", "archivo": "./assets/proyectos/AXD-GAL-26-01/MOR_NF1_site2.pdb", "nota": "Análogo para-NH₂" }
]
```

- Copia los `.pdb` a `assets/proyectos/<código del proyecto>/`.
- El ligando se detecta solo (todo lo que esté como `HETATM`); se dibuja en
  rosa, con los residuos a ≤4.5 Å en gris.
- La afinidad y el tipo de corrida se leen solos de los `REMARK` del PDB
  (`REMARK 2 Corrida: …` y `REMARK 3 Energia de afinidad: -9.4 kcal/mol`),
  así que no hace falta escribirlos en el JSON. `nota` es opcional.
- Quita del PDB aguas, iones u otros HETATM que no sean el ligando, o
  también se marcarán como ligando.
- Ten en cuenta que todo lo que subas aquí queda público: confirma con el
  cliente antes de publicar estructuras o compuestos suyos.

Si agregas imágenes, colócalas dentro de `assets/proyectos/` (crea esa
carpeta) y referencia la ruta relativa en el campo `imagen`, tal como en el
ejemplo de arriba.

### Importante: probar localmente antes de subir a GitHub

Como esta sección carga el archivo `proyectos.json` con JavaScript
(`fetch`), si abres `index.html` directamente haciendo doble clic
(protocolo `file://`) el navegador puede bloquear esa carga por seguridad y
la sección se verá vacía aunque el JSON tenga datos. Esto NO pasa una vez
publicado en GitHub Pages (ahí funciona perfecto), pero si quieres
previsualizarlo en tu computadora antes de subirlo, levanta un servidor
local simple desde esta carpeta:

```
python -m http.server 8000
```

y abre `http://localhost:8000` en el navegador.

**Forma más fácil (Windows):** da doble clic en `Ver sitio en local.bat`. Abre
una ventana negra (déjala abierta mientras revisas) y el sitio en tu navegador
en `http://localhost:8000`. No necesita Python; usa PowerShell, que ya viene
con Windows. Para detenerlo, cierra esa ventana. Los archivos
`Ver sitio en local.bat` y `servidor-local.ps1` son solo para tu computadora;
puedes subirlos o no a GitHub, no afectan al sitio publicado.
