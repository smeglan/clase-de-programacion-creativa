# JavaScript con HTML: de una página estática a una interacción

Esta guía enseña a conectar JavaScript con una página HTML sin React ni otros frameworks. Vas a empezar con un botón que responde a un clic y terminarás con una lista interactiva de ideas. Solo necesitas un editor de texto y un navegador.

Es el paso intermedio entre la [guía de HTML](guia-html.md) y la [Guía de React](guia-react.md): aquí ves el problema de mantener el estado a mano en el DOM, y allí ves la solución en componentes.

> HTML organiza el contenido, CSS presenta la página y JavaScript responde a acciones y cambios. Esta guía se enfoca en cómo JavaScript encuentra y actualiza elementos HTML.

## Ruta de aprendizaje

1. Preparar tres archivos que trabajan juntos.
2. Conectar JavaScript con `defer`.
3. Encontrar elementos y escuchar eventos.
4. Cambiar contenido de forma segura.
5. Representar una lista a partir de datos.
6. Crear y probar una pequeña interacción propia.

## 1. Prepara los archivos

Crea una carpeta `pagina-interactiva` con estos archivos:

```text
pagina-interactiva/
├── index.html
├── estilos.css
└── app.js
```

El navegador abre `index.html`. El HTML enlaza CSS para la apariencia y JavaScript para el comportamiento.

## 2. Construye el HTML

Escribe este contenido en `index.html`:

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Mi página interactiva</title>
    <link rel="stylesheet" href="estilos.css" />
    <script src="app.js" defer></script>
  </head>
  <body>
    <main>
      <h1>Ideas para mi proyecto</h1>
      <p id="mensaje">Todavía no has añadido una idea.</p>
      <button id="boton-idea" type="button">Proponer una idea</button>
      <ul id="lista-ideas" aria-label="Ideas propuestas"></ul>
    </main>
  </body>
</html>
```

`defer` le dice al navegador que descargue `app.js` y lo ejecute cuando haya terminado de leer el HTML. Así, el código podrá encontrar los elementos de la página. Como el script no usa módulos ni importa paquetes, esta demostración funciona al abrir el archivo localmente.

## 3. Añade apariencia

En `estilos.css`, escribe:

```css
body {
  margin: 0;
  padding: 2rem;
  color: #202536;
  background: #f1f3f8;
  font-family: system-ui, sans-serif;
}

main {
  max-width: 38rem;
  margin-inline: auto;
  padding: 2rem;
  border-radius: 1rem;
  background: white;
  box-shadow: 0 1rem 2rem #20253618;
}

button {
  padding: 0.7rem 1rem;
  border: 0;
  border-radius: 0.5rem;
  color: white;
  background: #4930ad;
  font: inherit;
  cursor: pointer;
}

button:focus-visible {
  outline: 3px solid #f0a900;
  outline-offset: 3px;
}
```

## 4. Haz que el botón responda

En `app.js`, añade:

```js
const ideas = [
  "Una galería de proyectos",
  "Un mapa de lugares favoritos",
  "Una lista de música para estudiar",
];

const message = document.querySelector("#mensaje");
const button = document.querySelector("#boton-idea");
const list = document.querySelector("#lista-ideas");
let nextIdea = 0;

button.addEventListener("click", () => {
  if (nextIdea === ideas.length) {
    message.textContent = "Ya propusiste todas las ideas.";
    return;
  }

  const item = document.createElement("li");
  item.textContent = ideas[nextIdea];
  list.append(item);

  nextIdea += 1;
  message.textContent = `Ideas propuestas: ${nextIdea}`;
});
```

Guarda los tres archivos y abre `index.html`. Haz clic varias veces y observa cómo cambia la página.

### Lee el programa de arriba abajo

- `const` guarda una referencia que no vas a reasignar; `let` se usa para `nextIdea`, porque su valor cambia.
- `document.querySelector(...)` busca en el documento el primer elemento que coincide con un selector CSS.
- `addEventListener("click", ...)` registra qué función ejecutar cuando ocurre un clic.
- `() => { ... }` es una función flecha: una forma concisa de escribir una función.
- `textContent` cambia texto; `createElement` crea un elemento; `append` lo inserta en la lista.
- `ideas.length` indica cuántos elementos hay en el arreglo. La condición evita intentar leer una idea inexistente.
- `` `Ideas propuestas: ${nextIdea}` `` es una plantilla de texto que inserta el valor de una expresión.

JavaScript puede cambiar el documento que el navegador ya mostró. No necesitas escribir etiquetas HTML dentro de una cadena para añadir texto: crear el elemento y asignarle `textContent` es más claro y seguro.

## 5. Haz una versión con formulario

Los formularios permiten que cada persona escriba sus propias ideas. En `index.html`, reemplaza el botón y el párrafo inicial por:

```html
<form id="formulario-idea">
  <label for="nueva-idea">Tu idea</label>
  <input id="nueva-idea" name="idea" required maxlength="80" />
  <button type="submit">Añadir idea</button>
</form>
<p id="mensaje" aria-live="polite">Escribe una idea para empezar.</p>
<ul id="lista-ideas" aria-label="Ideas propuestas"></ul>
```

En `app.js`, reemplaza el código anterior por:

```js
const form = document.querySelector("#formulario-idea");
const input = document.querySelector("#nueva-idea");
const message = document.querySelector("#mensaje");
const list = document.querySelector("#lista-ideas");
const ideas = [];

form.addEventListener("submit", (event) => {
  event.preventDefault();

  const idea = input.value.trim();

  if (idea === "") {
    message.textContent = "Escribe una idea antes de enviarla.";
    input.focus();
    return;
  }

  ideas.push(idea);
  const item = document.createElement("li");
  item.textContent = idea;
  list.append(item);

  message.textContent = `Tienes ${ideas.length} idea(s).`;
  form.reset();
  input.focus();
});
```

El evento `submit` ocurre al pulsar el botón o enviar el formulario desde el teclado. `preventDefault()` evita que el navegador recargue la página. `trim()` elimina espacios sobrantes al inicio y al final. El atributo `required` añade una validación básica del navegador, y el código vuelve a comprobar el valor para dar un mensaje útil.

## 6. Sintaxis moderna que ya estás usando

- **`const` y `let`:** elige `const` por defecto y `let` cuando reasignes el valor.
- **Funciones flecha:** `evento => { ... }` resulta útil para funciones breves y callbacks.
- **Plantillas de texto:** usa acentos graves y `${expresion}` para combinar texto y valores.
- **Arreglos:** `[]` guarda una colección ordenada; `push` agrega un elemento y `length` cuenta sus elementos.
- **`querySelector`:** reutiliza selectores conocidos de CSS para encontrar elementos.
- **DOM:** el Modelo de Objetos del Documento es la representación de la página que JavaScript puede consultar y modificar.

Estas son herramientas fundamentales del JavaScript actual, pero no hace falta aprender toda la sintaxis moderna en una sola sesión. Comprende cada línea y pruébala.

## 7. Reto: convierte la lista en tu proyecto

Elige una idea apropiada para tu portafolio: tareas, libros por leer, planes para el fin de semana o propuestas para un proyecto.

1. Cambia el título, las etiquetas y el texto de ayuda.
2. Añade un elemento al enviar el formulario.
3. Muestra cuántos elementos hay.
4. Evita enviar entradas vacías y conserva un límite razonable de caracteres.
5. Ajusta el CSS para que el formulario y la lista sean legibles en móvil.
6. Prueba enviar con el teclado, no solo con el ratón.

**Extensión:** agrega un botón para marcar cada elemento como completado. Usa una clase CSS, `classList.toggle("completada")` y un estilo de foco visible.

## 8. Cómo revisar errores

- **El botón no hace nada:** revisa que `app.js` esté junto a `index.html`, que el `src` coincida y que el archivo esté guardado.
- **No aparece el texto actualizado:** abre las herramientas del navegador y revisa la pestaña Console en busca de errores.
- **`querySelector` devuelve `null`:** confirma que el `id` escrito en JavaScript sea igual al de HTML y que el script incluya `defer`.
- **El formulario recarga la página:** confirma que el evento sea `submit` y que llames a `event.preventDefault()`.
- **La versión del navegador no coincide con el código:** guarda los archivos y actualiza la página abierta.

## ¿Qué aprendiste y qué sigue?

Ahora puedes describir cómo HTML, CSS y JavaScript trabajan juntos, escuchar una acción, cambiar la página y representar datos con elementos del DOM.

En la [Guía de React: de la página estática a una interfaz que responde](guia-react.md) llevas exactamente esta página a un proyecto con React. La diferencia central está explicada en su [sección 1.2](guia-react.md#12-la-interacción-manual-se-rompe): con JavaScript plano la verdad sobre los datos vive en el DOM, y por eso hay que contar elementos para saber si una lista está vacía. React invierte eso: la verdad vive en el estado y el DOM es solo su representación. Después, el [módulo 03](README.md#3-de-html-a-react-con-vite) te da el recorrido corto con Vite, y el [taller Fila creativa](taller-modelado-react.md) lo pone a prueba con un modelo, una cola FIFO y un formulario controlado.

### Para consultar

- [JavaScript en el navegador - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting)
- [Introducción a los eventos - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Events)
- [Manipular documentos - MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/DOM_scripting)
- [JavaScript Guide - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)