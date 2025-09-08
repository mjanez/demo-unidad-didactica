# Guía básica de LiaScript

Este documento explica cómo editar y usar LiaScript localmente y en producción, los básicos de Markdown en LiaScript, y cómo exportar a SCORM y PDF con el exporter.

## Índice
- [Introducción](#introducción)
- [LiaScript DevServer (desarrollo local)](#liascript-devserver-desarrollo-local)
- [Renderizar documentación LiaScript (navegador)](#renderizar-documentación-liascript-navegador)
   - [LiveEditor](#liveeditor)
- [Exportar la documentación](#exportar-la-documentación)
   - [Exportar a SCORM](#exportar-a-scorm)
   - [Exportar a PDF](#exportar-a-pdf)
- [Cheatsheet LiaScript: referencia rápida y ejemplos avanzados](#cheatsheet-liascript-referencia-rápida-y-ejemplos-avanzados)

---

## Introducción

[LiaScript](https://github.com/LiaScript/LiaScript) es un dialecto de Markdown para crear cursos interactivos y material educativo, un *open educational resource* (OER/REA, [Recurso Educativo Abierto](https://github.com/LiaScript/LiaScript)). Los documentos contienen metadatos en un bloque HTML al principio (comentario `<!-- ... -->`) y pueden importar macros, scripts, CSS y otros recursos.

Esta guía da pasos mínimos y ejemplos para trabajar con [LiaScript](https://liascript.github.io/course/?https://raw.githubusercontent.com/liaScript/docs/master/README.md) en tu máquina y publicar o probar el contenido.

---

## LiaScript DevServer (desarrollo local)

Requisitos previos:

- [Node.js](https://nodejs.org/) (recomendado: la versión LTS moderna). Instálalo desde https://nodejs.org/
- [npm](https://www.npmjs.com/) (viene con Node.js)

Instalación del `devserver` globalmente (en una terminal Bash en Windows o WSL):

```sh
npm install -g @liascript/devserver
```

Levantar el servidor desde la carpeta del proyecto (por ejemplo, desde la raíz del repositorio `demo-unidad-didactica`):

```sh
# Clona el repositorio
git clone https://github.com/mjanez/demo-unidad-didactica.git

# en el directorio del proyecto
liascript-devserver

# o para abrir el navegador automáticamente en la vista del archivo actual
liascript-devserver --open
# usar una carpeta concreta como raíz
liascript-devserver -i unidades -o
# puerto alternativo
liascript-devserver -p 3001 -o
# live reload (recarga automática al guardar)
liascript-devserver --input unidades --live -o
```

>[!TIP]
> Para modificar el CSS en local, puedes modificar en las opciones de configuracion (inicio del Markdown) `link: https://cdn.jsdelivr.net/gh/mjanez/demo-unidad-didactica@main/assets/css/dge-liascript.css` por una hoja CSS situada en el mismo directorio que el document, ej `./dge-liascript.css`


Salida esperada (ejemplo):

```sh
 _     _       ____            _       _
| |   (_) __ _/ ___|  ___ _ __(_)_ __ | |_
| |   | |/ _` \___ \ / __| '__| | '_ \| __|
| |___| | (_| |___) | (__| |  | | |_) | |_
|_____|_|\__,_|____/ \___|_|  |_| .__/ \__|
                                |_|

✨ watching for changes on: "unidades"
📡 starting server
   - local:           http://localhost:3000
   - on your network: http://192.168.68.102:3000
✨ hit Ctrl-c to close the server
```


---

## Renderizar documentación LiaScript (navegador)

1. Ábrela en tu navegador `https://liascript.github.io/course/?https://github.com/<usuario>/<repo>/<archivo>.md`:

   https://liascript.github.io/course/?https://github.com/mjanez/demo-unidad-didactica/blob/main/unidades/06.md

### LiveEditor
1. Abre el [LiveEditor](https://liascript.github.io/liveeditor/)
2. Modifica el contenido en el editor y clicka en parse para ver el resultado.

---

## Exportar la documentación
1. Instala desde npm

```sh
npm install -g --verbose @liascript/exporter
```

* `-h` `--help`: muestra la ayuda 
* `-i` `--input` : archivo usado como entrada.
* `-p` `--path`: ruta a empaquetar, si no se establece, se utiliza la ruta del archivo de entrada.
* `-o` `--output`: nombre del archivo de salida (el predeterminado es `output`), la extensión es definida por el formato.
* `-s` `--style`: estilos adicionales que se pasan a la exportación, se pueden utilizar para correcciones, como `"height: 100vh; width: 100%; border: 2px;"`
* `-f` `--format`: `scorm1.2`, `scorm2004`, `json`, `fullJson`, `web`, `ims`, `pdf`, `android`, `linkedData` (el predeterminado es `json`)
* `-v` `--version`: muestra la versión actual
* `-k` `--key`: clave de voz responsiva

### Exportar a SCORM
Para generar SCORM usando `liascript-exporter`:

2. Generar SCORM

```sh
liascript-exporter -i unidades/06.md -o curso-scorm.zip --format scorm1.2
```

Opciones útiles:

* `--scorm-organization <nombre>`: establece el título de la organización
* `--scorm-masteryScore <valor>`: establece el `masteryScore` de SCORM (un valor entre 0 y 100), el valor predeterminado es 0
* `--scorm-typicalDuration <duración>`: establece la duración de SCORM, el valor predeterminado es `PT0H5M0S`
* `--scorm-iframe`: utiliza un iframe, cuando un parámetro de inicio de SCORM no funciona
* `--scorm-embed`: incrusta el Markdown en el código JS, úsalo en Moodle 4 para manejar restricciones con la carga dinámica.

>[!NOTE]
> - El exporter empaqueta los activos y genera el manifest SCORM. Revisa el ZIP antes de subirlo a un LMS.
> - Si deseas una versión más completa, ejecuta `liascript-exporter --help`.

### Exportar a PDF
Para generar PDF usando `liascript-exporter`:

1. Instala [Google Chrome](https://www.google.com/chrome/) o usa [Brave](https://brave.com/) o [Edge](https://www.microsoft.com/edge), si ya los tienes.

** Usando Google Chrome: **
```sh
liascript-exporter --format pdf -i https://raw.githubusercontent.com/mjanez/demo-unidad-didactica/refs/heads/main/unidades/06.md
```

** Usando un `path` personalizado:
```sh
PUPPETEER_EXECUTABLE_PATH="C:/Program Files/BraveSoftware/Brave-Browser-Beta/Application/brave.exe" liascript-exporter --format pdf -i https://raw.githubusercontent.com/mjanez/demo-unidad-didactica/refs/heads/main/unidades/06.md
```

Opciones útiles:

* `--pdf-stylesheet`: Inyecta CSS personalizado en el PDF para cambiar el tema.
* `--pdf-theme`: Temas LiaScript: `default`, `turquoise`, `blue`, `red`, `yellow`
* `--pdf-timeout`: Establece un horizonte de tiempo adicional para esperar hasta que se complete.
* `--pdf-preview`: Abre el navegador de vista previa (predeterminado falso), la impresión no es posible.


#### Modo vista previa
Puedes usar el modo `--pdf-preview` para inspeccionar el curso antes de imprimir, incluso con entradas HTTPS:

```sh
liascript-exporter --format pdf --pdf-preview -i unidades/06.md
```

#### Personalización de estilos y temas  
Puedes modificar la apariencia del PDF usando CSS personalizado con `--pdf-stylesheet` o elegir un tema predefinido con `--pdf-theme` (`default`, `turquoise`, `blue`, `red`, `yellow`):

```sh
# Usar un tema específico
liascript-exporter --format pdf --pdf-theme red -i unidades/06.md

# Usar CSS personalizado
liascript-exporter --format pdf --pdf-stylesheet custom.css -i unidades/06.md
```

Ejemplo de `custom.css` para definir colores y fuentes globales:

```css
:root {
   --color-highlight: 2, 255, 0;
   --color-background: 122, 122, 122;
   --color-border: 0, 0, 0;
   --color-text: 0, 0, 255;
   --
```

#### Usar proyectos para agrupar cursos y personalizar la portada

LiaScript permite crear **proyectos** para agrupar varios cursos y generar una página de resumen personalizada. Un proyecto se define mediante un archivo YAML que describe el título, logo, descripción, metadatos y la colección de cursos.

**Ejemplo básico de archivo de proyecto (`curriculum.yml`):**

```yml
title: >
   <span style="background-color: #006ab3; color: white; padding: 5px;">
      Colección de cursos OER
   </span>
comment: >
   Esta página agrupa todos los cursos de la unidad didáctica.
logo: https://example.org/logo.png

collection:
   - url: https://raw.githubusercontent.com/mjanez/demo-unidad-didactica/main/unidades/01.md
   - url: https://raw.githubusercontent.com/mjanez/demo-unidad-didactica/main/unidades/02.md
   - url: https://raw.githubusercontent.com/mjanez/demo-unidad-didactica/main/unidades/06.md
      title: Catálogo de metadatos
      comment: Curso sobre DCAT y metadatos
      tags:
         - Metadatos
         - DCAT
```

Puedes añadir subcolecciones, contenido HTML entre cursos, personalizar logos, comentarios y etiquetas para navegación.

**Cómo generar la página de proyecto:**

```sh
liascript-exporter -i curriculum.yml --format project
```

**Generar PDF para cada curso automáticamente:**

```sh
liascript-exporter -i curriculum.yml --format project --project-generate-pdf --pdf-format A4
```

También puedes pasar argumentos específicos a cada curso usando la clave `arguments` en el YAML.

**Ventajas de los proyectos:**
- Agrupa y organiza cursos en una sola página.
- Personaliza portada, metadatos y navegación.
- Permite generar PDFs y SCORM para cada curso desde la configuración del proyecto.
- Añade contenido HTML entre cursos para secciones o información adicional.

Consulta la [documentación oficial](https://github.com/LiaScript/LiaScript/wiki/Projects) para más opciones avanzadas.


---

## Cheatsheet LiaScript: referencia rápida y ejemplos avanzados

### Encabezados

```md
# Título principal (nivel 1)
## Sección (nivel 2)
### Subsección (nivel 3)
#### Título nivel 4
##### Título nivel 5
###### Título nivel 6
```

### Énfasis y formato de texto

```md
*texto* o _texto_        → cursiva
**texto** o __texto__   → negrita
***texto*** o ___texto___ → cursiva y negrita
~tachado~               → tachado
~~subrayado~~           → subrayado (según tema)
~~~tachado y subrayado~~~
^superíndice^           → ^ejemplo^
`código`                → código inline
```

### Párrafos y citas

```md
Un párrafo se separa por una línea en blanco.

> Este bloque es una cita destacada.
```

### Listas

```md
* Elemento 1
   - Sub-elemento
   + Otro sub-elemento
* Elemento 2

1. Paso uno
2. Paso dos
```

### Separadores

```md
---
```

### Enlaces, imágenes y multimedia

```md
[Texto](https://ejemplo.org)           → enlace
![Alt](https://ejemplo.org/img.png)    → imagen externa
![Alt](/img/local.png)                 → imagen local
?[Audio](https://ejemplo.org/audio.mp3)→ audio externo
!?[Video](https://youtube.com/...)     → video externo
```

### Fórmulas matemáticas (KaTeX)

```md
$ E = mc^2 $                → fórmula inline

$$
    \sum_{i=1}^n x_i
$$                          → bloque de fórmula
```

### Animaciones y efectos

```md
{{1}} Este bloque aparece en el paso 1.
{{2-3}} Este bloque aparece en el paso 2 y desaparece en el 3.
{2}{Texto animado}          → micro-animación
```

### Voz y sprachausgabe

```md
--{{1}}-- Este texto se lee en el paso 1.
--{{2 Spanish Female}}-- Este texto se lee en español.
<!-- --{{1}}-- Solo se lee, no se muestra en modo libro. -->
```

#### Lista de voces soportadas

Pregunta de texto:
```md
[[respuesta]]
```

Opción única:
```md
[( )] Incorrecta
[(X)] Correcta
```

Selección múltiple:
```md
[[ ]] Incorrecta
[[X]] Correcta
```

Selección tipo matriz:
```md
[[Encabezado 1] [Encabezado 2]]
[ [X] [ ] ]
[ ( ) (X) ]
```

### Tablas y visualizaciones

```md
| Columna A | Columna B |
| --------- | --------- |
| Valor 1   | Valor 2   |
```

Tablas pueden visualizarse como gráficos, mapas, radar, etc. usando atributos en comentarios HTML:

```md
<!-- data-type="barchart" -->
| Animal | Peso (kg) | Edad (años) |
| ------ | ---------:| -----------:|
| Ratón  |      0.03 |           2 |
| Humano |        68 |          70 |
```


### Otros ajustes
* __`data-type`__: Puedes usar `data-type="map|boxplot|barchart|..."` para sobrescribir la representación identificada automáticamente con la que prefieras. Los nombres pueden tomarse de los títulos anteriores; no importa si usas mayúsculas o minúsculas. De este modo también es posible usar tipos que actualmente no se infieren automáticamente, como Sankey o BoxPlot.

   Si no quieres mostrar las tablas como diagramas, también puedes usar `data-type="None"` y solo se mostrará la tabla.

* __`data-show`__: Añade este atributo o ponlo a true (`data-show="true"`) si quieres visualizar los datos inmediatamente, sin necesidad de hacer clic en el botón de alternancia. Aun así, los usuarios podrán cambiar a la representación en tabla.

* __`data-transpose`__: En el sentido matemático, aplica este atributo o ponlo a true (`data-transpose="true"`) si quieres intercambiar filas y columnas. Una ventaja es que, por ejemplo, puedes usar PieChart y dejar que la tabla crezca verticalmente en lugar de crear una tabla horizontal enorme.

* __`data-title`__: Normalmente la primera celda define el título del diagrama, pero si quieres títulos más grandes y no escribir encabezados de tabla gigantes, usa este atributo: `data-title="Usa el título que quieras..."`

* __`data-xlabel`__: Como arriba, puedes definir las cadenas para las etiquetas; en este caso, la etiqueta del eje x.

* __`data-ylabel`__: O la etiqueta del eje y.

* __`data-src`__: Actualmente este atributo se usa para referirse a tus datos GeoJSON cuando usas la representación `data-type="Map"`, pero esto podría cambiar en el futuro para cargar y visualizar datos directamente, por ejemplo CSV.

   Si usas archivos GeoJSON de sitios externos como:

   https://code.highcharts.com/mapdata/

   puede ser útil usar un proxy CORS (por ejemplo cors-anywhere) si los datos no se visualizan por restricciones CORS:

   `data-src="https://cors-anywhere.herokuapp.com/https://code.highcharts.com/mapdata/custom/europe.geo.json"`



### Código y ejecución

```md
```js
console.log("Hola LiaScript");
```
<script>@input</script>
```

Se pueden agrupar varios segmentos de código directamente y asignarles un nombre. El signo `+` o `-` que precede al título define si el archivo aparece expandido o contraído en la primera visualización. En el script se hace referencia a los distintos segmentos de código mediante `@input(0)` y `@input(1)`

```` md
``` js     -EvalScript.js
let who = data.first_name + " " + data.last_name;

if(data.online) {
  who + " is online"; }
else {
  who + " is NOT online"; }
```
``` json    +Data.json
{
  "first_name" :  "Sammy",
  "last_name"  :  "Shark",
  "online"     :  true
}
```
<script>
  // insert the JSON dataset into the local variable data
  let data = @input(1);

  // eval the script that uses this dataset
  eval(`@input(0)`);
</script>
````

### Macros y metadatos

Define macros en el bloque HTML inicial:

```md
<!--
author: Nombre del autor
@macro: <b style="color: @0">@1</b>
-->
@macro(rojo,Texto en rojo)
```

### HTML embebido

```md
<h2 style="color:blue">Título en HTML</h2>
```

### ASCII-Art y diagramas

```ascii
+-----+     +-----+
| A   | --> | B   |
+-----+     +-----+
```

### Comentarios y estilos

```md
<!-- style="color:red;" -->
Texto en rojo.
```

### Importar recursos

```md
<!--
script: https://ejemplo.org/script.js
link: https://ejemplo.org/estilos.css
import: https://ejemplo.org/plantilla.md
-->
```

### Información del archivo

``` md
<!--
version:  0.0.1

author:   Equipo gestor de la plataforma datos.gob.es

email:    soporte@datos.gob.es

comment:  De qué trata el curso o la unidad,
          En varias líneas ...

logo:     https://cdn.jsdelivr.net/gh/mjanez/demo-unidad-didactica@assets/img/logo_dge.svg

language: es|en|de|...

narrator: Spanish Female|English Female|Brazilian Portuguese Female|...

mode:     Presentation|Slides|Textbook
dark:     false

date:     09/09/2020

@onload
alert("algún JavaScript se ejecutará al inicio....")
@end

attribute: Erste Danksagung ....
attribute: Zweite Danksagung mit Lizenz[MIT](https://opensource.org/licenses/MIT)

translation: Deutsch  translations/German.md
translation: Français translations/French.md
translation: Русский  translations/Russian.md
-->
```

---

Consulta la [Cheatsheet oficial (alemán)](https://github.com/LiaScript/CheetSheet/tree/main) para más ejemplos y opciones avanzadas.
