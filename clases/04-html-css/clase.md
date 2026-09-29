# Clase 4: Primera página interactiva con HTML, CSS y JavaScript

## Datos generales

- Duración presencial: 3 horas.
- Trabajo no presencial asociado: 9 horas.
- Unidad: Interfaces web y estructura de documentos.
- Resultado de aprendizaje: el estudiante estructura y presenta información con HTML y CSS, y añade una interacción sencilla con JavaScript moderno en el navegador.
- Meta de la clase: crear una página personal o tarjeta de proyecto con `index.html`, `estilos.css` y `app.js`, que abra en el navegador sin instalar herramientas ni crear un servidor.
- Prerrequisitos: crear y guardar archivos con un editor; reconocer carpetas y abrirlas en el navegador.

## Objetivos de la sesión

Al terminar, cada estudiante podrá:

1. distinguir el papel de HTML (estructura y significado) y CSS (apariencia y distribución);
2. reconocer etiquetas, contenido, atributos y anidación;
3. construir un documento básico con etiquetas semánticas;
4. enlazar una hoja de estilos y modificar colores, tipografía, espaciado y diseño;
5. guardar y abrir `index.html` directamente en un navegador;
6. conectar un archivo JavaScript con `defer` y responder a un evento de clic;
7. explicar cómo HTML, CSS y JavaScript aportan estructura, presentación y comportamiento.

## Preparación docente

- Preparar una carpeta llamada `primera-pagina` con archivos `index.html`, `estilos.css` y `app.js`.
- Probar que `index.html` abre correctamente al hacer doble clic.
- Preparar una referencia visual sencilla, como una tarjeta de perfil o de proyecto, que pueda construirse durante la clase.
- Probar un editor y navegador disponibles en el aula.
- No hace falta instalar Node, npm, extensiones ni Vite para el ejercicio central. Se usará JavaScript clásico con sintaxis moderna (`const`, función flecha, `querySelector` y eventos), enlazado con `defer`; no usar módulos ni imports en `file://`.
- Revisar la [práctica de HTML y CSS](../../material-estudiantes/02-programacion-base/guia-html-css.md) y la [guía de JavaScript con HTML](../../material-estudiantes/03-react-typescript/guia-javascript-html.md).

## Núcleo común - 180 minutos

### 1. Activación: mirar la página como estructura - 15 minutos

Mostrar una página gráfica sencilla y preguntar qué partes reconocen: título, descripción, imagen, lista, botón. Acordar que primero se construirá la estructura y luego su apariencia.

### 2. Explicación y demostración - 35 minutos

Explicar el esqueleto HTML, las etiquetas de contenido, atributos, anidación y las etiquetas semánticas más frecuentes (`header`, `main`, `section`, `article`, `footer`). Mostrar que CSS selecciona elementos y aplica estilos. Usar el [guion docente](guion-explicacion-docente.md).

Relacionar JSX con HTML solo al final de la explicación: React usa una sintaxis parecida para describir la interfaz; no iniciar todavía una aplicación React.

### 3. Práctica guiada - 40 minutos

Todo el grupo construye una tarjeta sencilla con `index.html` y `estilos.css`:

1. escribir el esqueleto del documento;
2. añadir título, párrafo, lista y enlace o botón;
3. guardar y abrir `index.html` en el navegador;
4. enlazar `estilos.css` y comprobar que cambie el fondo;
5. ajustar ancho, márgenes, tipografía, color y borde de la tarjeta;
6. añadir al HTML `<script src="app.js" defer></script>` y guardar `app.js` junto a `index.html`;
7. seleccionar botón y párrafo con `document.querySelector`, escuchar `click` y cambiar `textContent` con `const` y una función flecha.

Ejemplo para programar en conjunto:

```html
<button id="boton-saludo" type="button">Saludar</button>
<p id="mensaje">Todavía no hay saludo.</p>
```

```js
const button = document.querySelector("#boton-saludo");
const message = document.querySelector("#mensaje");

button.addEventListener("click", () => {
  message.textContent = "¡Mi página ya responde a una acción!";
});
```

Guardar cambios y actualizar el navegador para ver el resultado.

### 4. Laboratorio: una página propia - 70 minutos

Cada estudiante elige crear una tarjeta personal, una tarjeta de proyecto o una mini portada para su portafolio. Debe tener:

- un título principal;
- una descripción breve;
- una sección con al menos tres elementos (intereses, herramientas o características);
- un enlace o botón con texto claro; al menos un botón responde a una acción con JavaScript;
- estilos que organicen el contenido y aseguren contraste legible.

Durante el laboratorio, pedir que identifiquen en su código qué etiqueta representa cada parte, qué regla CSS controla su aspecto y qué evento JavaScript modifica la página.

### 5. Recorrido entre pares y cierre - 20 minutos

En parejas, abrir la página de otra persona y describir su estructura antes de comentar el diseño. Cierre escrito:

> HTML se ocupa de ___; CSS se ocupa de ___; JavaScript se ocupa de ___. Para ver mis cambios, ___.

Anticipar la ruta: POO se trabajó en la clase anterior. Después de aprender JavaScript con HTML, el siguiente paso será convertir la estructura en JSX y levantar una aplicación React con Vite.

## Criterios de logro

- `index.html` abre en el navegador y muestra el contenido esperado.
- Los elementos están anidados correctamente y tienen texto o atributos pertinentes.
- El HTML distingue las partes principales del documento.
- `app.js` carga correctamente y responde al evento de clic.
- `estilos.css` está enlazado y produce cambios visibles.
- La página tiene jerarquía visual, espaciado y contraste legibles.
- El estudiante explica la diferencia entre estructura, apariencia y comportamiento; describe qué evento escucha su programa y qué elemento modifica.

## Refuerzo sugerido

Si alguien se bloquea, usar el esqueleto de la guía del estudiante y completar primero solo título y párrafo. Confirmar que se vean en el navegador antes de añadir CSS. Después agregar un solo estilo por vez.

```html
<button id="boton-saludo" type="button">Saludar</button>
<p id="mensaje">Todavía no hay saludo.</p>
```

Crear `app.js` junto a `index.html` y escribir:

```js
const button = document.querySelector("#boton-saludo");
const message = document.querySelector("#mensaje");

button.addEventListener("click", () => {
  message.textContent = "¡Mi página ya responde a una acción!";
});
```

Explicar que `defer` permite ejecutar el archivo después de leer el HTML, que `querySelector` busca un elemento y que `addEventListener` escucha un evento. El JavaScript cambia el texto sin recargar la página.

## Extensión opcional

- Añadir una imagen con `alt` descriptivo.
- Hacer que la tarjeta se adapte a una pantalla angosta con una media query.
- Añadir un estado visual de `:hover` al enlace o botón y revisar que siga siendo legible.

## Trabajo no presencial - 9 horas

1. Mejorar la página con contenido original y una imagen con texto alternativo.
2. Probarla en una pantalla angosta y ajustar el CSS.
3. En la bitácora, anotar etiquetas, atributos y reglas CSS; explicar también qué elemento busca su JavaScript, qué evento escucha y qué cambia al activarse.
4. Traer una captura o el archivo para revisión en la próxima clase.

## Materiales

- Editor de texto y navegador; no hace falta servidor local para este ejemplo sin módulos.
- Carpeta `primera-pagina` con `index.html`, `estilos.css` y `app.js`.
- [Guía para estudiantes](../../material-estudiantes/02-programacion-base/guia-html-css.md), [guion docente](guion-explicacion-docente.md), [checklist](checklist-docente.md) y [apoyos y extensiones](apoyos-y-extensiones.md).
- [Guía de JavaScript con HTML](../../material-estudiantes/03-react-typescript/guia-javascript-html.md).

## Bloqueos previsibles y respuestas

- **No se ve el cambio:** guardar el archivo, actualizar el navegador y comprobar que se abrió el `index.html` de la carpeta correcta.
- **No aparecen los estilos:** verificar que `estilos.css` esté junto al HTML y que el `href` coincida con el nombre exacto.
- **El navegador muestra etiquetas como texto:** buscar un `<` o `>` que falte y revisar cómo están anidados los elementos.
- **La página se ve distinta a la de otra persona:** recordar que el navegador aplica estilos predeterminados y comprobar que ambos abrieron el archivo correcto.
- **Se pregunta por qué no se usa React:** explicar que estamos aprendiendo la estructura que React describirá después con JSX; la página estática permite ver esa estructura directamente.


