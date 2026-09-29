# Guion docente: explicación de la clase 4

## Idea central

Una página web tiene contenido estructurado con HTML, presentación con CSS y comportamiento que se puede programar con JavaScript. Hoy el grupo verá directamente cómo un botón del documento puede responder a una acción, sin un framework ni una instalación.

Hilo conductor:

```text
HTML organiza el contenido → CSS define su apariencia → JavaScript responde a eventos y actualiza la página
```

## Guion de la sesión

### 0–5 min: abrir la clase

Puedes decir:

> Hoy vamos a crear una página web sencilla que se abre directamente en el navegador. No necesitamos instalar un framework para aprender a describir el contenido y cambiar cómo se ve. Más adelante usaremos este mismo tipo de marcado dentro de React, con JSX.

Pide crear o abrir la carpeta `primera-pagina` en el editor.

### 5–12 min: separar estructura y apariencia

Muestra una tarjeta de perfil y pregunta qué partes tiene: título, descripción, datos, enlace. Explica:

> HTML describe qué contenido hay y cómo se organiza. CSS decide cómo se presenta: colores, tamaños, espacios y distribución. El navegador interpreta los dos archivos y dibuja la página.

Evita decir que HTML “es el diseño” o que CSS “hace que funcione el botón”. JavaScript puede añadir comportamiento: por ejemplo, escuchar un clic y actualizar un mensaje.

### 12–25 min: documento mínimo

Escribe y lee el esqueleto del archivo:

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Mi primera página</title>
    <link rel="stylesheet" href="estilos.css" />
  </head>
  <body>
    <main>
      <h1>Mi espacio creativo</h1>
      <p>Estoy aprendiendo a crear páginas web.</p>
    </main>
  </body>
</html>
```

Explica solo lo necesario: `head` guarda información del documento y enlaces a recursos; `body` contiene lo que vemos; las etiquetas delimitan elementos y deben cerrarse y anidarse correctamente; `href` indica dónde está la hoja CSS.

### 25–35 min: etiquetas con significado

Compara un montón de `<div>` con etiquetas que dicen qué representan:

```html
<header>Nombre del sitio</header>
<main>
  <section>
    <h2>Sobre mí</h2>
    <p>Una descripción breve.</p>
  </section>
</main>
<footer>Contacto</footer>
```

Di:

> La etiqueta no se elige por el tamaño o el color que queremos. La elegimos por el papel del contenido. Después CSS se encarga de cómo se ve.

No hace falta memorizar todas las etiquetas semánticas hoy; usen las que ayuden a describir la página.

### 35–40 min: abrir el archivo

Guardar `index.html` y abrirlo con doble clic. Mostrar que la barra de direcciones puede empezar con `file:///`. Añadir un nuevo párrafo, guardar y actualizar el navegador.

Aclara que este modo alcanza para archivos locales estáticos. Para este ejemplo pequeño usaremos un archivo JavaScript clásico sin módulos y la etiqueta `script` con `defer`, que funciona al abrir el HTML localmente. Cuando usemos módulos, paquetes y React, necesitaremos un servidor de desarrollo como Vite.

### 40–52 min: construir una tarjeta

Añadir un enlace que parezca botón y una lista:

```html
<article class="tarjeta">
  <p class="etiqueta">Proyecto</p>
  <h2>Mi calculadora</h2>
  <p>Una herramienta pequeña para practicar operaciones.</p>
  <ul>
    <li>Suma</li>
    <li>Resta</li>
    <li>Interfaz clara</li>
  </ul>
  <a href="#contacto">Ver más</a>
</article>
```

Pregunta qué es el contenido y qué nombre aparece dentro de `class`. Explica que `class` permite seleccionar elementos desde CSS; en HTML es un atributo, aquí no significa una clase de POO.

### 52–70 min: conectar y leer CSS

Crear `estilos.css`:

```css
body {
  margin: 0;
  padding: 2rem;
  font-family: Arial, sans-serif;
  background: #f3f4f6;
  color: #18212f;
}

.tarjeta {
  max-width: 32rem;
  margin: 2rem auto;
  padding: 1.5rem;
  border-radius: 1rem;
  background: white;
  box-shadow: 0 0.5rem 1.5rem #18212f22;
}

.etiqueta {
  color: #5b3cc4;
  font-weight: bold;
}
```

Explica la forma de una regla: selector, llaves, propiedad y valor. Cambiar juntos el color de fondo, el espacio interior y el ancho. Guardar y actualizar después de cada cambio.

### 70–90 min: JavaScript plano: del clic al cambio en la página

Añade dentro de `body`:

```html
<button id="boton-saludo" type="button">Saludar</button>
<p id="mensaje">Todavía no hay saludo.</p>
``

En `head`, enlaza el archivo con `<script src="app.js" defer></script>`. Crea `app.js` junto al HTML y escribe:

```js
const button = document.querySelector("#boton-saludo");
const message = document.querySelector("#mensaje");

button.addEventListener("click", () => {
  message.textContent = "¡Mi página ya responde a una acción!";
});
``

Guardar y pulsar el botón. Lee en voz alta: `const` guarda las referencias, `querySelector` encuentra los elementos, `addEventListener` escucha el clic y la función flecha describe qué hacer. `textContent` cambia el texto. Muestra la consola del navegador si alguien obtiene un error.

No enseñar módulos o `type="module"` aquí: las importaciones locales pueden fallar con `file://`. Esta página sencilla se ejecuta sin servidor.

### 90–100 min: comprobar comprensión

Preguntas rápidas:

- Si quiero agregar una sección, ¿en cuál archivo trabajo?
- Si quiero cambiar el fondo o el espacio, ¿en cuál archivo trabajo?
- ¿Para qué sirve `class="tarjeta"`?
- ¿Por qué elegimos `<article>` en vez de usar `<div>` para todo?

### 100–120 min: ejercicio guiado

El grupo replica la tarjeta desde cero siguiendo el orden de la [guía del estudiante](../../material-estudiantes/02-programacion-base/guia-html-css.md). Detente después de HTML para que observen la estructura sin estilos; enlaza el CSS al final.

### 120–160 min: laboratorio de diseño

Cada estudiante elige tarjeta personal, tarjeta de proyecto o portada pequeña del portafolio. Debe tener un título principal, descripción, una lista, un enlace y estilos propios. El docente acompaña con el [checklist](checklist-docente.md) y ofrece los [apoyos](apoyos-y-extensiones.md).

### 160–173 min: revisión en parejas

La pareja identifica una etiqueta semántica, una regla CSS y la interacción JavaScript. Luego propone una mejora de legibilidad o jerarquía visual. Abrir el archivo local del compañero para comprobar el resultado.

### 173–180 min: cierre y puente a los siguientes temas

Puedes decir:

> En la clase anterior modelamos datos y reglas con POO. Hoy organizamos contenido con HTML, damos estilo con CSS y añadimos una interacción con JavaScript. Luego llevaremos esas ideas a componentes React usando JSX y Vite.

## Errores de explicación que conviene evitar

- No decir que “ya nadie usa HTML”: JSX se parece a HTML, pero el navegador finalmente muestra elementos web con estructura HTML.
- No confundir Vite con un motor de interfaz: Vite es una herramienta de desarrollo y construcción; React es la biblioteca que organiza interfaces con componentes.
- No enseñar la sintaxis de JSX como idéntica a HTML: `className`, etiquetas cerradas y expresiones JavaScript tienen reglas propias.
- No usar `type="module"` o imports en archivos `file://`; para enseñar el evento del botón, un script clásico con `defer` funciona. Reservar módulos para el proyecto con Vite.
- No seleccionar etiquetas por su apariencia predeterminada; la semántica y los estilos son decisiones distintas.

## Referencias docentes

- [Guía estudiantil de JavaScript con HTML](../../material-estudiantes/03-react-typescript/guia-javascript-html.md)
- [Guía de eventos - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Events)
- [Manipular documentos - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/DOM_scripting)

- [Sintaxis básica de HTML - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax)
- [Estructura semántica - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Structuring_documents)
- [Descripción de interfaces y JSX - React](https://react.dev/learn/describing-the-ui)
- [Guía de inicio de Vite](https://vite.dev/guide/)


