# Tu primera página con HTML y CSS

## Qué vas a construir

Una página sencilla que puedes abrir en el navegador desde un archivo, sin instalar Vite, React ni extensiones. Usarás HTML para organizar el contenido y CSS para darle apariencia.

## Prepara la carpeta

Crea una carpeta llamada `mi-primera-pagina`. Dentro, crea dos archivos:

```text
mi-primera-pagina/
├── index.html
└── estilos.css
```

## Paso 1: escribe el contenido en HTML

Abre `index.html` en tu editor y escribe:

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
    <header>
      <p>Mi portafolio</p>
    </header>

    <main>
      <article class="tarjeta">
        <p class="etiqueta">Proyecto destacado</p>
        <h1>Una idea en construcción</h1>
        <p>
          Estoy aprendiendo a convertir ideas en experiencias interactivas.
        </p>

        <h2>Lo que estoy practicando</h2>
        <ul>
          <li>Organizar información</li>
          <li>Diseñar interfaces</li>
          <li>Probar mis ideas</li>
        </ul>

        <a href="#contacto">Ver contacto</a>
      </article>
    </main>

    <footer id="contacto">
      <p>Creado por Tu nombre</p>
    </footer>
  </body>
</html>
```

Guarda el archivo. Abre la carpeta en el explorador de archivos y haz doble clic en `index.html`. El navegador mostrará la página con estilos básicos. Cada vez que cambies el HTML, guarda y actualiza el navegador.

## Cómo leer el HTML

- Una **etiqueta** marca un elemento, como `<p>` para un párrafo.
- El **contenido** aparece entre la etiqueta de apertura y la de cierre.
- Los elementos se pueden **anidar**: por ejemplo, el título y la lista están dentro de `article`.
- Un **atributo** aporta información adicional. `lang="es"` indica el idioma; `href="..."` indica el destino del enlace.
- La **clase** `class="tarjeta"` da un nombre que CSS puede usar para encontrar ese elemento.
- Las etiquetas semánticas como `header`, `main`, `article` y `footer` describen el propósito de las secciones.

Elige cada etiqueta por el significado del contenido. No por el color o el tamaño con que aparece inicialmente.

## Paso 2: cambia la apariencia con CSS

Abre `estilos.css` y añade:

```css
body {
  margin: 0;
  padding: 2rem;
  font-family: Arial, sans-serif;
  background-color: #f1f3f8;
  color: #202536;
}

header,
footer {
  max-width: 42rem;
  margin: 0 auto;
}

.tarjeta {
  max-width: 38rem;
  margin: 2rem auto;
  padding: 2rem;
  border: 1px solid #d9deeb;
  border-radius: 1rem;
  background-color: white;
  box-shadow: 0 1rem 2rem #20253618;
}

.etiqueta {
  color: #6046c8;
  font-size: 0.85rem;
  font-weight: bold;
  text-transform: uppercase;
}

a {
  color: #4930ad;
}

a:hover,
a:focus-visible {
  color: #20105f;
}
```

Guarda los dos archivos y actualiza el navegador. Si los estilos no aparecen, revisa que `estilos.css` esté en la misma carpeta y que el nombre del archivo coincida con el valor de `href`.

## Cómo leer el CSS

```css
.tarjeta {
  padding: 2rem;
  background-color: white;
}
```

- `.tarjeta` es el **selector**: busca elementos con `class="tarjeta"`.
- Dentro de las llaves van las **declaraciones**.
- `padding` y `background-color` son **propiedades**.
- `2rem` y `white` son sus **valores**.

Prueba cambiar una propiedad por vez y observa el efecto.

## Reto: hazla tuya

1. Cambia el título, la descripción y los elementos de la lista.
2. Cambia los colores por una combinación que te guste, manteniendo texto legible.
3. Añade una segunda sección con un subtítulo y un párrafo.
4. Añade una imagen con un atributo `alt` que describa lo que aparece en ella.
5. Reduce el ancho de la ventana y ajusta el diseño si algo queda apretado.

## Si algo no funciona

- **Veo una página vacía:** confirma que abriste el `index.html` correcto y revisa que guardaste los cambios.
- **El CSS no cambia nada:** revisa el nombre `estilos.css`, la ruta de `href` y las llaves `{}`.
- **Veo una etiqueta como texto:** revisa que `<` y `>` estén escritos correctamente y que las etiquetas se cierren en el orden adecuado.
- **El enlace no abre el destino esperado:** comprueba el valor de `href`.

## Qué tiene que ver esto con React

React describe interfaces con componentes y JSX, una sintaxis que se parece a HTML. Lo que aprendiste sobre títulos, párrafos, listas, enlaces y organización del contenido sigue siendo útil. JSX tiene algunas diferencias —por ejemplo, escribe `className` en vez de `class`— que aprenderás cuando llegues a React.

Por ahora, tu página funciona directamente desde el archivo. En la [guía de JavaScript con HTML](../03-react-typescript/guia-javascript-html.md) harás que responda a acciones con JavaScript plano. Más adelante, cuando uses React y módulos separados, ejecutarás el proyecto con una herramienta de desarrollo como Vite.
