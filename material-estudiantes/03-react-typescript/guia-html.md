# HTML de cero a una base sólida

Esta guía te lleva desde un archivo vacío hasta una página bien estructurada, semántica y accesible. No necesitas saber HTML antes de empezar. Al terminar, tendrás una base para diseñar con CSS y para entender el JSX que usarás en React.

> “De cero a una base sólida” es una meta realista: nadie aprende todas las etiquetas de memoria. Lo valioso es saber elegir elementos por su significado, organizarlos y consultar la documentación cuando haga falta.

## Ruta de aprendizaje

1. Crear y abrir una página.
2. Entender etiquetas, elementos, atributos y anidación.
3. Organizar texto, enlaces, listas e imágenes.
4. Dar estructura semántica a la página.
5. Crear tablas de datos y formularios accesibles.
6. Revisar, validar y completar un proyecto.

## 1. Qué es HTML

HTML significa *HyperText Markup Language*. Es un lenguaje de marcado: utiliza elementos para dar estructura y significado al contenido de una página. Por ejemplo, un navegador puede distinguir un encabezado, una lista, un enlace y un formulario.

HTML no es el lenguaje que define los colores y la distribución visual; eso le corresponde principalmente a CSS. Tampoco realiza por sí solo la lógica general de una aplicación; para eso se usa JavaScript. En una página web:

| Tecnología | Pregunta que responde | Ejemplo |
|---|---|---|
| HTML | ¿Qué contenido hay y qué significa? | Es un título, una navegación o un formulario. |
| CSS | ¿Cómo se ve y cómo se distribuye? | Fondo violeta, columnas, espacio. |
| JavaScript | ¿Qué sucede ante una acción o un cambio? | Abrir un menú o actualizar un contador. |

### Por qué vale la pena aprender HTML

HTML describe qué es cada parte de una página: un encabezado, una navegación, un artículo, un botón o un formulario. Esa estructura permite que el navegador presente el contenido de manera coherente y que las tecnologías de asistencia identifiquen mejor su propósito.

Aprender HTML también hace más claro el paso a React. JSX se parece a HTML y conserva muchas de sus etiquetas; React crea elementos que el navegador representa como HTML. Vite organiza el proyecto y facilita el desarrollo, pero no reemplaza HTML. Una buena base te ayuda a escribir JSX con sentido, elegir elementos adecuados y detectar problemas de estructura y accesibilidad.

En resumen: HTML aporta estructura y significado; CSS controla la presentación; JavaScript añade comportamiento. Las herramientas modernas cambian cómo organizas y construyes una interfaz, pero sigues necesitando decidir qué contenido representa cada elemento.

## 2. Prepara tu primera página

Crea una carpeta llamada `mi-pagina` y dentro crea `index.html`. Usa un editor de texto y guarda el archivo con la extensión `.html`.

Escribe lo siguiente:

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Mi primera página</title>
  </head>
  <body>
    <h1>Hola, web</h1>
    <p>Esta página está escrita en HTML.</p>
  </body>
</html>
```

Guarda el archivo y ábrelo con doble clic. El navegador mostrará la página. Cuando cambies el archivo, guarda y actualiza la pestaña.

### Qué hace el esqueleto

- `<!doctype html>` indica que el documento usa HTML moderno.
- `<html lang="es">` contiene la página e indica que su idioma principal es español.
- `<head>` contiene información del documento y recursos, que normalmente no aparecen como contenido principal.
- `<meta charset="UTF-8" />` permite mostrar correctamente caracteres como ñ y tildes.
- `<meta name="viewport" ... />` adapta el área de la página al ancho del dispositivo.
- `<title>` define el título que aparece en la pestaña del navegador.
- `<body>` contiene el contenido de la página.

## 3. Etiquetas, elementos y atributos

Un elemento común tiene etiqueta de apertura, contenido y etiqueta de cierre:

```html
<p>Estoy aprendiendo.</p>
```

En este caso:

- `<p>` es la etiqueta de apertura;
- `Estoy aprendiendo.` es el contenido;
- `</p>` cierra el elemento;
- las tres partes forman el elemento completo.

Los **atributos** agregan información o configuración. Se escriben en la etiqueta de apertura:

```html
<html lang="es">
<a href="contacto.html">Contacto</a>
<img src="fotos/perfil.jpg" alt="Retrato de Alex en el jardín" />
```

Un atributo normalmente tiene nombre y valor. En el ejemplo, `href` contiene el destino del enlace; `src` indica la ubicación de la imagen; `alt` ofrece una alternativa textual.

### Elementos vacíos

Algunos elementos no contienen texto ni llevan etiqueta de cierre. Se llaman elementos vacíos. Entre ellos están:

```html
<img src="logo.svg" alt="Nombre del sitio" />
<br />
<hr />
<input type="text" />
```

No agregues cierres inventados como `</img>`. Para separar párrafos o secciones, usa elementos apropiados en vez de muchos `<br />`.

### Anidar en el orden correcto

Los elementos pueden contener otros elementos, pero deben cerrarse en el orden inverso a su apertura:

```html
<p>Aprendo <strong>HTML paso a paso</strong>.</p>
```

Incorrecto:

```html
<p>Aprendo <strong>HTML paso a paso.</p></strong>
```

Piensa en cajas: no cierres la caja exterior antes de cerrar la interior.

### Caracteres con significado especial

Los símbolos `<` y `>` forman parte de las etiquetas. Si quieres mostrarlos como texto, escribe entidades HTML:

```html
<p>En HTML, &lt;p&gt; crea un párrafo.</p>
<p>Un café cuesta 5&nbsp;000 pesos.</p>
```

Las entidades frecuentes son `&lt;` para `<`, `&gt;` para `>`, `&amp;` para `&` y `&quot;` para comillas. En archivos guardados como UTF-8 puedes escribir normalmente tildes y ñ.

## 4. Organiza texto con significado

### Encabezados

Usa encabezados para expresar jerarquía, no solo para cambiar el tamaño del texto:

```html
<h1>Mi portafolio</h1>
<h2>Proyectos</h2>
<h3>Calculadora creativa</h3>
<h2>Sobre mí</h2>
```

`<h1>` es el encabezado principal de la página. `<h2>` introduce secciones principales y `<h3>` una subsección dentro de una sección. Mantén un orden lógico: no elijas `<h4>` solo porque se ve más pequeño; el tamaño se cambia con CSS.

### Párrafos y énfasis

```html
<p>HTML ayuda a describir el contenido de una página.</p>
<p><strong>Importante:</strong> guarda tus cambios antes de actualizar.</p>
<p>La palabra <em>semántica</em> se refiere al significado.</p>
```

- `<strong>` señala importancia.
- `<em>` da énfasis en el texto.
- `<b>` y `<i>` llaman la atención visualmente sin aportar el mismo significado; usa primero las opciones semánticas cuando correspondan.
- `<small>` puede marcar notas o texto secundario.
- `<code>` marca fragmentos de código.

No uses una etiqueta por su apariencia predeterminada. Esa apariencia puede cambiar con CSS y variar entre navegadores.

### Citas y abreviaturas

```html
<blockquote cite="https://example.com/">
  <p>Una cita extensa va en un bloque independiente.</p>
</blockquote>

<p>La <abbr title="World Wide Web">Web</abbr> conecta documentos.</p>
```

Usa `<blockquote>` para una cita en bloque, `<q>` para una cita breve dentro de una oración y `<abbr>` para una abreviatura que quieras explicar.

## 5. Enlaces y rutas

Un enlace permite navegar. Su texto debe explicar el destino:

```html
<a href="contacto.html">Ir a la página de contacto</a>
<a href="https://developer.mozilla.org/">Consultar la guía de MDN</a>
<a href="#proyectos">Saltar a mis proyectos</a>
```

Un enlace con `href="#proyectos"` apunta a un elemento que tenga `id="proyectos"`:

```html
<section id="proyectos">
  <h2>Mis proyectos</h2>
</section>
```

### Rutas relativas

Si tus archivos están organizados así:

```text
mi-pagina/
├── index.html
├── contacto.html
└── imagenes/
    └── perfil.jpg
```

Desde `index.html`, las rutas son `contacto.html` y `imagenes/perfil.jpg`. El nombre debe coincidir exactamente, incluidas las extensiones. Es una buena práctica usar nombres sencillos, sin espacios ni tildes, para archivos y carpetas.

Evita usar “haz clic aquí” como único texto del enlace: al leer una lista de enlaces fuera de su contexto, no se entiende a dónde llevan.

## 6. Listas

Usa listas cuando los elementos formen un conjunto o una secuencia.

### Lista sin orden

```html
<ul>
  <li>Diseño</li>
  <li>Programación</li>
  <li>Experimentación</li>
</ul>
```

`<ul>` es una lista sin orden numerado; cada elemento va en `<li>`.

### Lista ordenada

```html
<ol>
  <li>Escribe el HTML.</li>
  <li>Guarda el archivo.</li>
  <li>Ábrelo en el navegador.</li>
</ol>
```

Usa `<ol>` cuando el orden importe, como en instrucciones o pasos.

### Lista de descripciones

```html
<dl>
  <dt>HTML</dt>
  <dd>Lenguaje de marcado que estructura el contenido.</dd>
  <dt>CSS</dt>
  <dd>Lenguaje de estilos que define la presentación.</dd>
</dl>
```

`<dl>` agrupa términos y sus descripciones.

## 7. Imágenes y contenido multimedia

### Imágenes

Guarda los recursos en una carpeta y usa una ruta relativa:

```html
<img
  src="imagenes/afiche-exposicion.jpg"
  alt="Afiche azul de la exposición de arte estudiantil"
  width="800"
  height="600"
/>
```

El texto de `alt` comunica el propósito o contenido relevante de la imagen. No empieces con “imagen de”; describe lo que la persona necesita saber. Si la imagen es puramente decorativa y no aporta información, usa `alt=""`.

Para acompañarla con una leyenda:

```html
<figure>
  <img src="imagenes/maqueta.jpg" alt="Maqueta de una casa con techo verde" />
  <figcaption>Prototipo del proyecto de vivienda sostenible.</figcaption>
</figure>
```

`width` y `height` pueden ayudar al navegador a reservar el espacio de la imagen. Para que la imagen se adapte visualmente al ancho, después aprenderás CSS.

### Audio y video

Si el recurso debe reproducirse en la página, ofrece controles nativos:

```html
<video controls width="640" poster="imagenes/portada-video.jpg">
  <source src="media/demo.mp4" type="video/mp4" />
  Tu navegador no puede reproducir este video.
</video>
```

Los controles permiten pausar y cambiar el volumen. Para video con diálogo o narración, procura ofrecer subtítulos. No uses reproducción automática con sonido.

## 8. Estructura semántica de una página

Las etiquetas semánticas describen el papel de cada región. Una estructura frecuente es:

```html
<body>
  <header>
    <a href="index.html">Mi portafolio</a>
    <nav aria-label="Principal">
      <ul>
        <li><a href="#proyectos">Proyectos</a></li>
        <li><a href="#sobre-mi">Sobre mí</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <section id="proyectos" aria-labelledby="titulo-proyectos">
      <h1 id="titulo-proyectos">Proyectos</h1>
      <article>
        <h2>Mi primera página</h2>
        <p>Una presentación creada con HTML.</p>
      </article>
    </section>

    <section id="sobre-mi">
      <h2>Sobre mí</h2>
      <p>Me gusta crear y aprender.</p>
    </section>
  </main>

  <footer>
    <p>© 2026 Mi portafolio</p>
  </footer>
</body>
```

- `<header>` presenta una página o una sección.
- `<nav>` agrupa enlaces de navegación importantes.
- `<main>` contiene el contenido principal de esa página.
- `<section>` agrupa contenido temático y normalmente lleva un encabezado.
- `<article>` representa una pieza que se puede entender por sí sola, como una publicación o una tarjeta de proyecto.
- `<aside>` contiene material relacionado, pero secundario.
- `<footer>` contiene información de cierre de una página o sección.
- `<div>` y `<span>` son contenedores genéricos: úsalos cuando no haya una etiqueta más específica que represente el contenido.

No todas las páginas necesitan todas estas etiquetas. Escoge las que expliquen realmente la organización del contenido.

### Sección o artículo: ¿cuál elijo?

Una `section` reúne contenido de un mismo tema dentro de la página. Un `article` representa una unidad independiente que podría compartirse o reutilizarse por sí sola. Si solo necesitas agrupar elementos para aplicarles CSS y no existe un significado más específico, un `div` puede ser adecuado.

## 9. Tablas para datos tabulares

Usa una tabla cuando las filas y columnas expresen relaciones entre datos, no para construir el diseño visual de la página.

```html
<table>
  <caption>Horario semanal del laboratorio</caption>
  <thead>
    <tr>
      <th scope="col">Día</th>
      <th scope="col">Actividad</th>
      <th scope="col">Hora</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Lunes</th>
      <td>Diseño</td>
      <td>09:00</td>
    </tr>
    <tr>
      <th scope="row">Miércoles</th>
      <td>Prototipado</td>
      <td>11:00</td>
    </tr>
  </tbody>
</table>
```

- `<table>` crea la tabla.
- `<caption>` explica qué información contiene.
- `<thead>` y `<tbody>` separan encabezados y contenido.
- `<tr>` crea una fila.
- `<th>` es una celda de encabezado; `scope` relaciona el encabezado con su columna o fila.
- `<td>` contiene un dato.

## 10. Formularios: pedir datos correctamente

Los formularios permiten que una persona ingrese información. Cada campo necesita una etiqueta visible asociada.

```html
<form action="#" method="get">
  <div>
    <label for="nombre">Nombre</label>
    <input id="nombre" name="nombre" type="text" autocomplete="name" required />
  </div>

  <div>
    <label for="correo">Correo</label>
    <input id="correo" name="correo" type="email" autocomplete="email" required />
  </div>

  <div>
    <label for="mensaje">Mensaje</label>
    <textarea id="mensaje" name="mensaje" rows="4"></textarea>
  </div>

  <button type="submit">Enviar</button>
</form>
```

- `<form>` agrupa controles para enviar datos.
- `<label for="correo">` se asocia al control que tiene `id="correo"`.
- `id` identifica un elemento dentro del documento; no debe repetirse.
- `name` es la clave que se usa al enviar el valor del campo.
- `type="email"` pide un formato de correo y activa ayudas del navegador.
- `required` indica que el campo debe llenarse.
- `<textarea>` es un campo de texto de varias líneas.
- `<button type="submit">` envía el formulario.

Para opciones:

```html
<label for="tema">Tema del mensaje</label>
<select id="tema" name="tema">
  <option value="idea">Tengo una idea</option>
  <option value="ayuda">Necesito ayuda</option>
</select>
```

Para agrupar opciones relacionadas usa `<fieldset>` y `<legend>`. Para casillas y botones de opción, cada control necesita una etiqueta.

La validación HTML mejora la experiencia, pero no protege por sí sola un servidor: los datos también deben validarse donde se reciben. En un archivo local el formulario no tiene un servicio que guarde la información; para eso se necesita JavaScript y/o un servidor.

### Botones

Dentro de un formulario, escribe el tipo del botón explícitamente:

- `type="submit"` envía el formulario;
- `type="reset"` restablece los controles (úsalo solo cuando tenga sentido);
- `type="button"` es un botón genérico que normalmente se conecta a JavaScript.

Para navegar a otra dirección, usa un enlace `<a>`. Para ejecutar una acción, usa `<button>`.

## 11. Atributos comunes

| Atributo | Uso |
|---|---|
| `lang` | Idioma del contenido. |
| `href` | Destino de un enlace. |
| `src` | Ubicación de una imagen o recurso. |
| `alt` | Alternativa textual de una imagen. |
| `id` | Identificador único dentro de la página. |
| `class` | Nombre o nombres para agrupar elementos y darles estilos. |
| `title` | Información adicional; no lo uses como reemplazo de una etiqueta o explicación visible. |
| `name` | Nombre del dato de un campo al enviar un formulario. |
| `value` | Valor inicial o valor asociado a un control. |
| `disabled` | Desactiva un control mientras se cumpla una condición de la interfaz. |
| `required` | Marca un campo como obligatorio en la validación del navegador. |

Algunos atributos son **booleanos**: su presencia significa verdadero. Por ejemplo, `required` o `disabled` se escriben sin `="true"`:

```html
<input type="text" required />
```

## 12. Accesibilidad desde el HTML

Una buena estructura ayuda a que más personas puedan recorrer y comprender la página, también con teclado o lector de pantalla.

- Elige la etiqueta adecuada para el propósito: enlace para navegar y botón para ejecutar una acción.
- Mantén los encabezados en un orden que refleje la organización de la página.
- Escribe texto de enlace descriptivo.
- Añade `alt` a imágenes informativas; usa `alt=""` para las decorativas.
- Asocia cada campo del formulario con un `<label>`.
- Conserva controles nativos y asegúrate de que se puedan usar con teclado.
- No comuniques un estado solamente por color; acompaña el color con texto o estructura.
- Usa el atributo `lang` correcto en `<html>`.

Usar ARIA no arregla un HTML mal elegido. Primero intenta expresar el contenido con elementos HTML nativos; agrega ARIA solo cuando haga falta describir una interacción que la semántica nativa no cubre.

## 13. Metadata, comentarios y páginas completas

En `head` puedes incluir metadatos que describen el documento:

```html
<meta name="description" content="Portafolio de proyectos de programación creativa." />
```

El título y la descripción deben ser específicos de la página. La vista previa en buscadores depende de varios factores y no se garantiza solo por incluir estos datos.

Los comentarios sirven para notas del código; el navegador no los muestra:

```html
<!-- Navegación principal del sitio -->
<nav aria-label="Principal">...</nav>
```

No guardes contraseñas, claves o datos privados en el HTML: cualquier persona puede ver el contenido que se entrega al navegador.

## 14. Cosas que suelen confundirse

### `id` y `class`

- Usa `id` para un identificador único, por ejemplo para enlazar a una sección o asociar una etiqueta a un control.
- Usa `class` para marcar uno o varios elementos que comparten una función o estilo.
- No asignes el mismo `id` a varios elementos.

### Enlace y botón

- `<a href="...">` lleva a un destino.
- `<button>` activa una acción.

### Encabezado y texto grande

`<h1>` a `<h6>` indican niveles de sección; no son controles de tamaño. CSS determina el tamaño visual.

### Espacio y salto de línea

No uses muchas etiquetas `<br />` para separar bloques. Separa ideas en párrafos y usa CSS para ajustar el espacio entre elementos.

### Tabla y diseño

Una tabla presenta relaciones de datos por filas y columnas. Para organizar el diseño visual, se usan CSS Grid, Flexbox u otras herramientas de CSS, no tablas HTML.

## 15. Valida y depura tu HTML

El navegador intenta mostrar documentos incluso si tienen errores, así que “se ve” no siempre significa “está bien estructurado”. Revisa:

1. que todas las etiquetas estén bien escritas;
2. que cierres los elementos y los anides correctamente;
3. que cada atributo esté en la etiqueta adecuada;
4. que las rutas a imágenes, páginas y recursos existan;
5. que los `id` no se repitan;
6. que los campos tengan etiquetas y las imágenes informativas tengan alternativa;
7. que los encabezados y regiones tengan sentido al leerlos sin estilos.

Puedes pegar o subir una copia de tu archivo al [Nu HTML Checker](https://validator.w3.org/nu/). Lee los mensajes uno por uno y corrige primero los errores. El validador detecta problemas de sintaxis y estructura, pero no decide si el contenido es claro, accesible o útil.

## 16. Proyecto final: una página de evento

Crea una página para anunciar una actividad inventada (exposición, torneo, club o concierto). Debe incluir:

- título de la página y descripción en `head`;
- `header`, navegación, `main` y `footer`;
- un solo encabezado principal, más encabezados para cada sección;
- una introducción, fecha y ubicación;
- una lista de actividades o artistas;
- una imagen con `alt` adecuado;
- una tabla si hay un horario con filas y columnas relacionadas;
- un formulario de inscripción con campos etiquetados;
- enlaces que describan su destino.

Primero haz que la página tenga sentido sin CSS. Después puedes añadir estilos con la [guía práctica de HTML y CSS](../02-programacion-base/guia-html-css.md).

### Revisión por otra persona

Pídele a un compañero que use solo el teclado y que responda:

1. ¿Puedes recorrer los enlaces y campos en un orden entendible?
2. ¿Las etiquetas de formulario anuncian qué dato se espera?
3. ¿Los títulos forman un esquema claro?
4. ¿Se entiende el propósito de las imágenes y enlaces?
5. ¿El archivo pasa el validador sin errores?

## 17. Chuleta rápida

```html
<header>Marca o encabezado de página</header>
<nav aria-label="Principal"><a href="index.html">Inicio</a></nav>
<main>
  <h1>Título principal</h1>
  <section>
    <h2>Una sección</h2>
    <p>Un párrafo con <strong>énfasis importante</strong>.</p>
    <ul><li>Un elemento</li><li>Otro elemento</li></ul>
    <a href="contacto.html">Ir a contacto</a>
  </section>
</main>
<footer>Información de cierre</footer>
```

## Vocabulario

| Término | Significado |
|---|---|
| Etiqueta | Marca de apertura o cierre escrita entre `<` y `>`. |
| Elemento | Etiqueta, contenido y cierre (cuando corresponde). |
| Atributo | Configuración o información adicional de un elemento. |
| Anidación | Organización de elementos dentro de otros elementos. |
| Semántica | Significado o propósito del elemento en la página. |
| Metadata | Información sobre el documento, usualmente dentro de `head`. |
| Ruta | Dirección que apunta a otro archivo o recurso. |
| Validación | Revisión de la estructura y sintaxis según las reglas de HTML. |
| HTML semántico | Uso de elementos cuyo nombre expresa el papel del contenido. |

## Qué aprender después

- **CSS:** colores, tipografía, diseño adaptable, Flexbox y Grid.
- **JavaScript:** interacción, eventos y manipulación de datos.
- **React y JSX:** componentes que describen interfaces dinámicas. Tu HTML sigue siendo una base útil; JSX tiene diferencias que estudiarás en el [módulo de React con Vite](README.md#3-de-html-a-react-con-vite).

## Referencias recomendadas

- [Aprender HTML - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content)
- [HTML semántico - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Structuring_documents)
- [Formularios - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/HTML_forms)
- [Accesibilidad con HTML - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/HTML)
- [Nu HTML Checker](https://validator.w3.org/nu/)
