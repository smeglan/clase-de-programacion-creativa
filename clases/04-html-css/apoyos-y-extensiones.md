# Clase 4: apoyos y extensiones

Este material acompaña la sesión de HTML, CSS y JavaScript plano. El objetivo sigue siendo que cada estudiante cree y comprenda una página propia.

## Apoyo puntual

### 1. Confirmar archivos y carpeta

Pedir al estudiante que muestre el explorador del editor. Comprobar que `index.html` y `estilos.css` están en la misma carpeta y que el navegador abrió ese `index.html`.

### 2. Empezar con el mínimo

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Mi página</title>
  </head>
  <body>
    <main>
      <h1>Hola</h1>
      <p>Esta es mi primera página.</p>
    </main>
  </body>
</html>
```

Guardar y abrir antes de seguir. Si aparece la página, añadir una lista y luego una sección.

### 3. Aislar errores de ruta

Usar esta etiqueta en el `head`:

```html
<link rel="stylesheet" href="estilos.css" />
```

Revisar que el archivo se llame exactamente `estilos.css`, incluidas las minúsculas. Para probar si CSS carga, añadir `body { background: lightyellow; }` y actualizar.

### 4. Agregar estilos de uno en uno

Empezar con `color`, `background-color`, `padding` y `margin`. Después introducir `max-width`, `border-radius` o `box-shadow`. Preguntar qué cambió tras cada regla.

### Criterio de logro del apoyo

El estudiante abre su HTML local, muestra un título y párrafo, conecta el archivo CSS y explica al menos una regla.

## Extensión opcional

### Interacción con JavaScript moderno

Si el grupo necesita repasar el ejemplo guiado, crea `app.js` junto al HTML, añade `<script src="app.js" defer></script>` en `head` y usa `querySelector` junto con `addEventListener("click", ...)` para cambiar un mensaje con `textContent`. No se necesitan paquetes, módulos ni React.

### 1. Imagen accesible

Añadir una imagen y describir su contenido mediante el atributo `alt`. Probar que se entienda qué muestra la imagen incluso sin verla.

### 2. Adaptar a móvil

Hacer que la tarjeta use casi todo el ancho disponible en pantallas pequeñas y conserve un ancho máximo en pantallas grandes.

```css
.tarjeta {
  width: min(100%, 32rem);
  margin-inline: auto;
}
```

### 3. Estados visuales

Añadir `:hover` y `:focus-visible` al enlace y comprobar que el foco del teclado se pueda ver.

### 4. Preparar el puente hacia React

Identificar qué partes de la página podrían repetirse como componentes (por ejemplo, varias tarjetas de proyecto). Anotar qué datos cambiarían entre una tarjeta y otra; no hace falta programar React todavía.

### Criterio de logro de la extensión

La página funciona localmente y el estudiante explica el cambio visual o interactivo. Para profundizar en eventos, formularios y listas, consulta la [guía de JavaScript con HTML](../../material-estudiantes/03-react-typescript/guia-javascript-html.md).


