# Guía de React: de la página estática a una interfaz que responde

Esta guía toma la página que construiste en la [guía de HTML](guia-html.md) y la [práctica de HTML y CSS](../02-programacion-base/guia-html-css.md), le añade la interacción que aprendiste en [JavaScript con HTML](guia-javascript-html.md), y la convierte en un proyecto con componentes. No introduce una biblioteca nueva desde cero: **reordena el HTML que ya sabes escribir** para que el programa pueda volver a dibujarlo cuando los datos cambian.

El objetivo no es aprender la API entera de React. Es entender cuatro ideas y saber explicarlas:

- una función que devuelve interfaz es un **componente**;
- los datos que un componente recibe se llaman **props**;
- los datos que un componente cambia se llaman **estado**;
- cambiar el estado produce una versión nueva de la interfaz, nunca una modificación de la anterior.

Si puedes decir esas cuatro frases con tus propias palabras y entiendes por qué son cuatro frases y no una, la guía hizo su trabajo.

> **Cómo probar los ejemplos**
>
> Los ejemplos viven en un proyecto Vite con la plantilla `react-ts`, como el que describe el [README del módulo](README.md#2-crea-un-proyecto-react-con-vite). Si todavía no lo tienes:
>
> ```bash
> npm create vite@latest mi-pagina-react -- --template react-ts
> cd mi-pagina-react
> npm install
> npm run dev
> ```
>
> El código de cada sección va en `src/App.tsx` (o en un archivo dentro de `src/`, y lo importas desde `App.tsx`). Cada bloque es un componente completo, así que puedes pegar uno, verlo en `http://localhost:5173/`, guardarlo y pegar el siguiente. No hace falta instalar nada más: React ya viene en las dependencias del proyecto.
>
> Un detalle que sorprende la primera vez: Vite activa **Strict Mode**, un modo de desarrollo que ejecuta dos veces algunas funciones para ayudarte a detectar impurezas. Si ves que una de tus funciones se registra dos veces en la consola, no es un bug tuyo: es el modo de desarrollo de Vite. La guía lo usa como ejemplo en la [sección 5](#5-estado-lo-que-cambia-y-hay-que-redibujar).

## Objetivos

Al terminar esta guía deberías poder:

- Explicar por qué una página estática se queda corta y qué problema concreto resuelve React.
- Traducir una etiqueta de HTML a JSX y decir en voz alta qué cambió: `className`, cierre de etiquetas vacías, un solo elemento raíz, llaves `{}` y atributos en `camelCase`.
- Escribir un componente como una función que devuelve JSX, con nombre en mayúscula, y explicar por qué la mayúscula importa.
- Pasar datos a un componente con `props`, declararlos con un `type` y leerlos por destructuración.
- Crear estado con `useState`, actualizarlo y explicar por qué asignar a la variable del estado no redibuja nada.
- Actualizar el estado con la forma de función `(valorAnterior) => nuevoValor` y saber cuándo es la opción correcta.
- Escribir un formulario controlado con `onChange` y `value`, y explicar qué pasa si falta uno de los dos.
- Convertir contenido repetido en datos y renderizarlo con `map` y una `key` estable.
- Mostrar una cosa u otra según una condición con un operador ternario o `&&`, y evitar las trampas de cada uno.
- Componer componentes unos dentro de otros, aplicando la misma preferencia por composición que aprendiste en la [Guía 7 de POO](../02-programacion-base/guia-poo.md#6-herencia-es-un).
- Decidir en qué componente vive un estado, siguiendo la regla del ancestro común más cercano.
- Explicar por qué en React se crea una versión nueva de un arreglo o un objeto en vez de modificar el que ya existe.
- Nombrar las dos reglas de los Hooks y reconocer el error que aparece al romperlas.
- Refactorizar una página estática a React paso a paso, comprobando que cada versión funciona antes de pasar a la siguiente.
- Leer un error de React y decir, en una frase, qué regla se rompió.

## 1. El problema: la página estática no alcanza

La [guía de HTML](guia-html.md#8-estructura-semántica-de-una-página) te dejó una página bien estructurada: `header`, `main`, `section`, `article`, `footer`, con un `h1` por sección, listas que significan algo y un formulario con sus etiquetas. Esa página abre con doble clic y se ve bien. Es el punto de partida correcto, y conviene no perderlo: **cada etiqueta que aprendiste ahí sigue siendo la etiqueta correcta en React**.

El problema aparece en cuanto la interfaz tiene que **cambiar**.

### 1.1 La repetición no escala

Imagina la tiendita del curso con veinte productos. La forma estática es la que escribiste en la [práctica de HTML y CSS](../02-programacion-base/guia-html-css.md): copiar, pegar y cambiar el nombre.

```html
<!-- La forma estática, con cuatro productos -->
<article class="tarjeta">
  <h2>Cuaderno</h2>
  <p>$12000</p>
</article>

<article class="tarjeta">
  <h2>Marcador</h2>
  <p>$6000</p>
</article>

<!-- ... y así hasta el producto veinte -->
```

Cuatro productos son veinte líneas. Veinte productos son cien líneas de HTML que ya no vas a leer nunca. Y el problema no es el tamaño: es que la información está **escrita dentro de la presentación**. No hay forma de responder "¿cuántos productos tienen el precio menor a 10000?" salvo contando a mano. La respuesta a esa pregunta no existe, porque en el HTML los datos no están guardados en ningún lado: están pintados.

En la [Guía 4 de listas](../02-programacion-base/guia-listas.md) viste que la forma de responder preguntas sobre una colección es guardarla en un arreglo. Y en la [Guía 7 de POO](../02-programacion-base/guia-poo.md#22-el-producto-real-y-el-producto-del-programa) viste que lo que hace falta guardar depende del propósito: este catálogo necesita nombre, precio, categoría y disponibilidad, nada más.

Entonces el orden correcto de las cosas es este, y conviene tenerlo claro antes de escribir la primera línea de React:

```text
primero el modelo de datos → después las operaciones → al final la pantalla
```

React no invierte ese orden. React **representa** un modelo de datos. Si inviertes el orden y empiezas por la pantalla, terminas con la misma página estática de antes, solo que con `div` en lugar de `article`.

### 1.2 La interacción manual se rompe

En [JavaScript con HTML](guia-javascript-html.md) resolviste el siguiente problema: el botón tiene que cambiar algo en la página. La solución fue `querySelector` y `addEventListener`.

```js
const boton = document.querySelector("#agregar");
const lista = document.querySelector("#lista");

boton.addEventListener("click", () => {
  const elemento = document.createElement("li");
  elemento.textContent = nombre.value;
  lista.appendChild(elemento);
});
```

Funciona, y es importante entender por qué funciona. JavaScript tiene un modelo mental simple: **el documento existe, yo lo busco y lo modifico**. `querySelector` devuelve el elemento, `appendChild` lo inserta, `textContent` le cambia el texto. La página es una estructura mutable y tú la estás editando pieza por pieza.

Ese modelo tiene un costo invisible mientras la página es pequeña. En cuanto aparecen dos listas, un contador y un botón que depende de cuántos elementos hay, el estado de la página está repartido: en el texto de un `<li>`, en el contenido de un `<p>`, en el `disabled` de un botón. Para saber si la lista está vacía tienes que **contar los elementos del DOM**. Para saber qué botón debería estar deshabilitado tienes que contarlos otra vez. Y si borras un elemento con `remove()`, el texto de la lista y el contador quedan desincronizados: nada te avisa.

El problema se puede resumir en una frase:

> **Con JavaScript plano, la verdad sobre los datos vive en el DOM. React invierte eso: la verdad vive en el estado, y el DOM es solo su representación.**

Ese es el cambio de fondo. Todo lo demás —componentes, `props`, `map`, `key`— son formas de organizar un estado para poder redibujarlo con confianza.

### 1.3 Qué resuelve React y qué no

Conviene ser exacto aquí, porque el malentendido más común es pensar que React reemplaza HTML, CSS o JavaScript.

| Tecnología | Pregunta que responde | ¿Cambia con React? |
|---|---|---|
| HTML | ¿Qué contenido hay y qué significa? | No. Las etiquetas son las mismas, dentro de JSX |
| CSS | ¿Cómo se ve y cómo se distribuye? | No. Tus clases y reglas siguen igual |
| JavaScript | ¿Qué lógica se ejecuta? | Sí, pero se organiza en componentes |
| React | ¿Cuándo hay que volver a dibujar y con qué datos? | Es lo nuevo |

React **no** es un reemplazo de HTML ni de CSS ni de un lenguaje de programación. Es la manera de declarar: *"estos son mis datos, y quiero que esta pantalla se dibuje a partir de ellos"*. Vite, por su parte, es otra cosa: prepara el proyecto, instala dependencias y levanta el servidor de desarrollo. Se explica en el [README del módulo](README.md#2-crea-un-proyecto-react-con-vite).

### 1.4 El recorrido de esta guía

Cada sección es un paso del mismo problema, no un tema suelto:

```text
página estática → JSX → componente → props → estado → eventos
                → listas → condiciones → composición → inmutabilidad
                →Hooks → tiendita completa
```

Si algo no queda claro, casi siempre la causa es que se saltó un paso. La sección 13 vuelve a recorrer el camino completo con el código completo.

## 2. JSX: el mismo HTML, con JavaScript adentro

JSX es una forma de escribir HTML dentro de un archivo de JavaScript o TypeScript. La extensión `.tsx` existe justamente para que el editor sepa que ese archivo mezcla las dos cosas.

La idea completa cabe en un ejemplo: la sección de productos que en la [guía de HTML](guia-html.md#8-estructura-semántica-de-una-página) escribiste con etiquetas puras, en JSX.

```tsx
<article className="tarjeta">
  <h2>Cuaderno</h2>
  <p>$12000</p>
</article>
```

Se parece a HTML. Y debe parecerse: escribir JSX que se parece a HTML es una decisión de diseño de React, hecha para que puedas reutilizar lo que ya sabes y para que el resultado sea legible. Pero hay cuatro diferencias, y tropezar con ellas es el primer obstáculo de todo el módulo.

### 2.1 `class` se escribe `className`

```tsx
<div className="tarjeta">...</div>
```

**Error:** `class` es una palabra reservada de JavaScript, así que no puede usarse como nombre de propiedad. React usa `className`, y el resultado en el navegador sigue siendo `class="tarjeta"` a secas. Tus reglas CSS `.tarjeta` no cambian en nada.

El mismo problema aparece con `for` en las etiquetas de los formularios: se escribe `htmlFor`.

```tsx
<label htmlFor="nombre">Nombre</label>
<input id="nombre" />
```

### 2.2 Las etiquetas sin contenido se cierran

En HTML, `<img src="cuaderno.png">` y `<input type="text">` se pueden dejar abiertas. En JSX, **todas** se cierran, incluidas las que no llevan contenido.

```tsx
<img src="cuaderno.png" alt="Cuaderno de espiral" />
<input type="text" value={nombre} onChange={cambiar} />
<br />
```

Fíjate también en que el valor de `alt` y de `href` va entre comillas, como en HTML. Los atributos de tipo texto van así; los que son expresiones o funciones van entre llaves.

### 2.3 Un componente devuelve un solo elemento

Este es el que más confunde al principio. En HTML puedes tener cinco `<li>` hermanos porque están dentro de un `<ul>`. En JSX, una función que devuelve JSX tiene que devolver **un solo elemento**:

```tsx
// Así no compila: hay tres elementos hermanos
function Lista() {
  return (
    <li>Cuaderno</li>
    <li>Marcador</li>
    <li>Goma</li>
  );
}
```

Hay tres salidas posibles, y lachoice correcta depende de la pregunta que estés preguntando:

| Necesito | Solución | Ejemplo |
|---|---|---|
| Un contenedor con sentido en el documento | Una etiqueta semántica | `<ul>...</ul>`, `<section>...</section>` |
| Un contenedor neutro, sin significado | `div` | `<div className="fila">...</div>` |
| Varios elementos sin contenedor real | Un fragmento `<>...</>` | cuando el padre ya agrupa todo |

```tsx
function Lista() {
  return (
    <ul>
      <li>Cuaderno</li>
      <li>Marcador</li>
    </ul>
  );
}

function Vacio() {
  return (
    <>
      <strong>No hay productos</strong>
      <p>Agrega uno para empezar.</p>
    </>
  );
}
```

**Elige la primera opción siempre que exista.** Como viste en la [sección 8 de la guía de HTML](guia-html.md#8-estructura-semántica-de-una-página), la etiqueta dice qué significa el contenido. Envolver todo en `<div>` para evitarte pensar es exactamente el error que esa guía te pidió no cometer.

### 2.4 Las llaves abren la puerta a JavaScript

Dentro de JSX, `{}` significa "aquí va una expresión de JavaScript". Es la diferencia más útil de las cuatro.

```tsx
type Producto = {
  nombre: string;
  precio: number;
  categoria: "papeleria" | "comida" | "tecnologia";
};

function Tarjeta({ producto }: { producto: Producto }) {
  const precioConIva = producto.precio * 1.19;

  return (
    <article className="tarjeta">
      <h2>{producto.nombre}</h2>
      <p>${producto.precio}</p>
      <p>Con IVA: ${precioConIva.toFixed(0)}</p>
      <span className="etiqueta">{producto.categoria}</span>
    </article>
  );
}
```

Dentro de las llaves puede ir: una variable, una operación, una llamada a función, un operador ternario, un `&&`. Todo lo que sea una expresión que **devuelva un valor**.

Y lo que no puede ser una llave es una sentencia. Por eso `precioConIva.toFixed(0)` funciona, con los paréntesis, y `if (producto.precio > 0)` no: un `if` es una sentencia, y las llaves de JSX quieren un valor.

### 2.5 Los atributos con valor de JavaScript también llevan llaves

```tsx
<input
  type="text"
  value={nombre}                                  // expresión
  onChange={(evento) => setNombre(evento.target.value)}  // función
  aria-label="Nombre del producto"                 // texto
/>
```

La regla para distinguirlos es simple: si el valor es texto fijo, comillas; si es un dato o una función, llaves. En JSX, un `class="tarjeta"` con comillas en lugar de llaves es un error, no un detalle estilístico.

### 2.6 La tabla de traducción

| En HTML | En JSX | Por qué |
|---|---|---|
| `class="tarjeta"` | `className="tarjeta"` | `class` es palabra reservada en JavaScript |
| `for="nombre"` | `htmlFor="nombre"` | misma razón |
| `<img src="a.png">` | `<img src="a.png" />` | toda etiqueta se cierra |
| `<input type="text">` | `<input type="text" />` | toda etiqueta se cierra |
| `<h1>Hola</h1>` y `<p>...</p>` juntos | envueltos en un contenedor, o `<>...</>` | un componente devuelve un elemento |
| `<p>Hola, Ada</p>` | `<p>Hola, {nombre}</p>` | las llaves insertan un valor |
| `onclick="agregar()"` | `onClick={agregar}` | los atributos de evento van en `camelCase` y con llaves |
| `style="color: red"` | `style={{ color: "red" }}` | el estilo es un objeto, no texto |
| `tabindex="0"` | `tabIndex={0}` | los atributos que no son de HTML van en `camelCase` |

La última fila merece una aclaración: React convierte los nombres de atributos de JSX a HTML por ti. Si escribes `tabindex`, React te avisa; lo correcto es `tabIndex={0}`. Y `className` es la excepción que no se parece a su nombre real, precisamente porque `class` no se podía usar.

## 3. Componentes: funciones que devuelven interfaz

Un componente es una función que devuelve JSX. Eso es todo. No necesita heredar de nada, ni llamar a `super`, ni usar `React.Component`.

```tsx
function TarjetaProducto() {
  return (
    <article className="tarjeta">
      <h2>Cuaderno</h2>
      <p>$12000</p>
    </article>
  );
}
```

### 3.1 Por qué la mayúscula importa

Hay una regla en JSX que parece arbitraria y no lo es: **un componente empieza con mayúscula**.

```tsx
<TarjetaProducto />   {/* React entiende: "este es un componente" */}
<tarjetaProducto />   {/* React entiende: "esto es una etiqueta HTML" */}
```

La razón es técnica y simple. En HTML, las etiquetas se escriben en minúsculas porque forman parte del lenguaje del documento. JSX usa las minúsculas para las etiquetas del documento y reserva las mayúsculas para los valores de JavaScript. Con una sola convención, el compilador sabe qué es una cosa y qué es la otra, sin ambigüedad.

El mismo criterio aplica al archivo:

| Nombre | Cómo lo interpreta React | Consecuencia |
|---|---|---|
| `Tarjeta.tsx` | un componente | correcto |
| `tarjeta.tsx` | un módulo con valores | el componente no se reconoce |

Si te sale el error *"Element type is invalid"*, *"tags are neither `<tag>` nor `<>...</>`"* o *"Unexpected token"*, revisa primero las mayúsculas de los nombres.

### 3.2 Un componente, una responsabilidad

Esto ya lo dijo la [Guía 7 de POO](../02-programacion-base/guia-poo.md#error-6-la-clase-que-lo-hace-todo) y aquí se cumple igual. Un componente que lee un formulario, filtra, calcula un total, guarda en disco y pinta es un componente que no se puede probar ni entender.

```tsx
// Difícil de probar, imposible de reutilizar
function App() {
  const [productos, setProductos] = useState<Producto[]>([]);
  const [busqueda, setBusqueda] = useState("");

  const filtrados = productos.filter((p) => p.nombre.includes(busqueda));
  const total = filtrados.reduce((suma, p) => suma + p.precio, 0);

  return (
    <main>
      <input value={busqueda} onChange={(e) => setBusqueda(e.target.value)} />
      {/* ... y además guarda en localStorage, manda correos, arma el PDF ... */}
    </main>
  );
}
```

La misma lógica repartida en cuatro piezas se lee, se prueba y se reutiliza:

```tsx
function useCatalogo(iniciales: Producto[]) {
  const [productos, setProductos] = useState<Producto[]>(iniciales);
  const [busqueda, setBusqueda] = useState("");

  const filtrados = productos.filter((producto) =>
    producto.nombre.toLowerCase().includes(busqueda.toLowerCase()),
  );

  return { productos, setProductos, busqueda, setBusqueda, filtrados };
}
```

No hace falta que se llamen "hooks personalizados" todavía. Lo importante es la forma: **la lógica de datos vive fuera de la pantalla, y la pantalla solo muestra**. Esa separación es la que hace que un proyecto con 40 componentes siga siendo legible, y es la misma idea que en el [README del módulo](README.md#de-la-situación-a-la-interfaz) aparece como `Situación real → modelo de datos → operaciones → interfaz`.

### 3.3 Un componente se usa con etiquetas

Un componente se invoca con notación de etiqueta, y eso abre una decisión que conviene nombrar: **etiqueta con contenido o etiqueta con `children`**.

```tsx
function Tarjeta({ titulo, children }: { titulo: string; children: React.ReactNode }) {
  return (
    <article className="tarjeta">
      <h2>{titulo}</h2>
      {children}
    </article>
  );
}

// Con contenido escrito dentro
<Tarjeta titulo="Resumen">
  <p>El total del carrito es de $42000.</p>
</Tarjeta>

// Con contenido generado por código
<Tarjeta titulo="Resumen">
  {productos.map((producto) => (
    <p key={producto.id}>{producto.nombre}</p>
  ))}
</Tarjeta>
```

`children` es una prop como cualquier otra, con la diferencia de que React la crea por ti a partir de lo que escribiste entre las etiquetas. La [sección 9](#9-composición-en-vez-de-herencia) vuelve a esto, porque es la relación más importante de React.

### 3.4 Componentes que solo muestran y componentes que recuerdan

Otra división útil:

| Tipo | Qué hace | Ejemplo |
|---|---|---|
| **De presentación** | recibe datos por `props` y los dibuja. No tiene estado | `TarjetaProducto` |
| **Con estado** | guarda datos que cambian por interacción | `FormularioProducto` |

Preferencia por este curso: empieza con componentes de presentación. Si un componente necesita estado, muchas veces es señal de que el estado debe subir (sección 10) y no de que el componente debe recordarlo todo.

## 4. Props: los datos que viajan hacia abajo

Las `props` son los datos que un componente recibe de quien lo usa. Se declaran en el `type` del componente, y por eso son la forma en que React hace cumplir los contratos que aprendiste en la [Guía 7](../02-programacion-base/guia-poo.md#7-polimorfismo-mismo-nombre-distinto-comportamiento): si le pasas el dato equivocado, el editor te avisa antes de ejecutar.

```tsx
type Producto = {
  id: string;
  nombre: string;
  precio: number;
  categoria: "papeleria" | "comida" | "tecnologia";
  disponible: boolean;
};

type TarjetaProps = {
  producto: Producto;
  destacada?: boolean;
};

function Tarjeta({ producto, destacada = false }: TarjetaProps) {
  return (
    <article className={destacada ? "tarjeta tarjeta--destacada" : "tarjeta"}>
      <h2>{producto.nombre}</h2>
      <p>${producto.precio}</p>
      {!producto.disponible && <p>Agotado</p>}
    </article>
  );
}
```

Cuatro cosas que pasan aquí:

1. **El `type` de las props se declara aparte**, con nombre propio (`TarjetaProps`). Es lo que permite que el componente sea legible y que el error apunte al lugar correcto.
2. **Se desestructura en la firma**: `{ producto, destacada = false }`. Con muchos componentes, desestructurar ahorra escribir `props.` en cada línea.
3. **Una prop opcional se marca con `?`** y su valor por defecto se define en la desestructuración. Así el componente tiene un valor garantizado aunque nadie lo pase.
4. **Las props son de solo lectura**. Un componente que recibe `producto` y hace `producto.precio = 0` está rompiendo una regla: el dato es de quien lo pasó. La sección 11 vuelve a esto.

### 4.1 Props frente a variables de módulo

Una confusión frecuente: ¿por qué pasar el dato por `props` en vez de dejarlo en una constante en el mismo archivo?

```tsx
// Esto funciona, pero el componente no se puede reutilizar
const PRODUCTO: Producto = {
  id: "p-01",
  nombre: "Cuaderno",
  precio: 12000,
  categoria: "papeleria",
  disponible: true,
};

function Tarjeta() {
  return <h2>{PRODUCTO.nombre}</h2>;
}

// Esto se puede usar veinte veces, con veinte datos distintos
function TarjetaConProp({ producto }: { producto: Producto }) {
  return <h2>{producto.nombre}</h2>;
}
```

La diferencia es **de dónde viene el dato**, y por tanto quién lo controla:

| | Constante de módulo | Prop |
|---|---|---|
| Dónde vive el dato | en el archivo, fija para todos los usos | en quien lo usa |
| ¿Quién decide el valor? | el componente | el que lo renderiza |
| ¿Se puede reutilizar con otros datos? | no | sí |
| ¿Se puede probar con un caso concreto? | hay que editar el archivo | se lo pasas al probarlo |

Cuando leas código de otra persona y veas un componente que importa un dato y lo pinta, pregúntate si eso debería ser una prop. Casi siempre sí.

### 4.2 Pasar funciones también

Las props no son solo datos: pueden ser funciones. Es la forma de que un hijo le avise a su padre que algo pasó, sin que el padre pierda el control de los datos.

```tsx
type ItemProps = {
  producto: Producto;
  onQuitar: (id: string) => void;
};

function Item({ producto, onQuitar }: ItemProps) {
  return (
    <li>
      {producto.nombre} — ${producto.precio}
      <button type="button" onClick={() => onQuitar(producto.id)}>
        Quitar
      </button>
    </li>
  );
}
```

`Item` no sabe qué pasa cuando quitan algo. Solo avisa. Quien lo usa decide qué hacer. Esa es la diferencia entre un componente que se puede probar en aislamiento y uno acoplado a toda la aplicación.

## 5. Estado: lo que cambia y hay que redibujar

Si las `props` son lo que un componente **recibe**, el estado es lo que un componente **recuerda** entre un redibujado y el siguiente. La diferencia parece sutil y no lo es: las `props` las pone alguien más, y el estado lo pone el componente.

```tsx
import { useState } from "react";

type ContadorProps = {
  inicial: number;
};

function Contador({ inicial }: ContadorProps) {
  const [cantidad, setCantidad] = useState(inicial);

  return (
    <div>
      <p>Cantidad: {cantidad}</p>
      <button type="button" onClick={() => setCantidad(cantidad + 1)}>
        Sumar uno
      </button>
      <button type="button" onClick={() => setCantidad(inicial)}>
        Reiniciar
      </button>
    </div>
  );
}
```

`useState(inicial)` devuelve un par: el valor actual y la función que lo cambia. Se separan con la sintaxis de desestructuración de arreglos, y por convención el primero se llama con un sustantivo y el segundo con `set` más ese mismo nombre.

### 5.1 Por qué no basta con una variable normal

Esta es la parte que más cuesta aceptar, porque `let` parece exactamente lo mismo. Compara:

```tsx
function Contador() {
  let cantidad = 0;

  return (
    <div>
      <p>Cantidad: {cantidad}</p>
      <button type="button" onClick={() => cantidad++}>
        Sumar uno
      </button>
    </div>
  );
}
```

El botón "funciona": la variable aumenta. Y la pantalla no cambia nunca. La razón es precisa y vale la pena memorizarla:

> Cuando el estado cambia, React vuelve a ejecutar el componente para **calcular otra vez** el JSX. Las variables que se declaran dentro del cuerpo del componente se crean de cero en cada ejecución, y por eso siempre vuelven a su valor inicial.

Un diagrama mental ayuda más que cualquier explicación:

```text
pulsas "Sumar uno"
      │
      ▼
useState guarda 0 + 1 = 1   ← el estado vive FUERA del componente
      │
      ▼
React vuelve a ejecutar Contador()
      │
      ▼
se crea una cantidad local = 0  (sección 5.2)
la línea del useState lee 1     ← el estado no se recrea, se recupera
      │
      ▼
el JSX se calcula con 1 → la pantalla muestra 1
```

El estado no está "adentro" del componente: está **en la lista de React**, y el componente va a buscarla cada vez que se ejecuta. Por eso sobrevive y por eso una variable local no.

### 5.2 Tres reglas que explican casi todos los errores de estado

**Regla 1. El estado es de solo lectura.** Esta línea no hace nada:

```tsx
function Contador() {
  const [cantidad, setCantidad] = useState(0);

  cantidad = cantidad + 1; // no cambia nada: es una constante reasignada
}
```

En realidad ni siquiera compila: `cantidad` es una `const`. Pero el error equivalente sí aparece cuando el estado es un objeto o un arreglo y lo que se muta es su contenido (sección 11).

**Regla 2. El estado solo se cambia desde la función que lo creó.** `setCantidad` es la única vía. No se guarda en una variable global, no se pasa a otro archivo, no se llama desde fuera.

**Regla 3. El valor se recalcula, no se recuerda.** Al escribir `setCantidad(cantidad + 1)`, el `cantidad` que lees es el de **esta** ejecución del componente. Si dos clics ocurren antes de que React vuelva a ejecutar el componente, ambos leen el mismo valor viejo. Por eso existe la forma de función:

```tsx
// Puede perder un incremento si dos clics pasan rápido
setCantidad(cantidad + 1);

// Recibe el valor real más reciente: no puede equivocarse
setCantidad((valorActual) => valorActual + 1);
```

La versión con función es la correcta por defecto cuando el nuevo valor **depende** del anterior. Cuando el valor es independiente (`setSaludo("Hola")`), cualquiera de las dos formas sirve.

### 5.3 Varios estados, y cuándo agrupar

Un componente puede llamar a `useState` las veces que necesite:

```tsx
const [nombre, setNombre] = useState("");
const [precio, setPrecio] = useState(0);
const [disponible, setDisponible] = useState(true);
```

React los guarda en el mismo orden en cada ejecución, y por eso sabe cuál es cuál. Esa es exactamente la razón por la que existen las [reglas de los Hooks](#12-reglas-de-los-hooks).

El orden importa, entonces conviene razonar sobre el estado como una unidad. Dos errores típicos:

- **Estado que describe lo mismo, repartido en varios lugares.** Guardar `nombre` en un sitio y `nombreError` en otro obliga a mantenerlos sincronizados a mano. Mejor un solo estado que describa el formulario entero (sección 6.4).
- **Estado derivado, guardado como estado.** Esto no debería ser estado, debería ser un cálculo:

```tsx
// Redundante: `total` siempre coincide con los productos
const [productos, setProductos] = useState<Producto[]>([]);
const [total, setTotal] = useState(0);
```

```tsx
// Correcto: `total` se calcula cada vez, nunca puede quedar desfasado
const [productos, setProductos] = useState<Producto[]>([]);
const total = productos.reduce((suma, p) => suma + p.precio, 0);
```

> **La pregunta que decide:** ¿este dato se puede **calcular** a partir de otro que ya tengo, o solo aparece cuando alguien hace algo? Si se calcula, no es estado.

### 5.4 El estado es local al componente

Dos componentes que usan `useState` con el mismo nombre inicial no comparten nada:

```tsx
function Contador() {
  const [cantidad, setCantidad] = useState(0);
  return <button onClick={() => setCantidad(cantidad + 1)}>{cantidad}</button>;
}

function OtroContador() {
  const [cantidad, setCantidad] = useState(0);
  return <button onClick={() => setCantidad(cantidad + 1)}>{cantidad}</button>;
}
```

Cada uno tiene su propio estado, y el nombre `cantidad` está repetido a propósito porque cada `useState` crea una variable distinta. Si los dos componentes se llamaran igual en el mismo archivo, eso sí sería un error: dos declaraciones no pueden compartir identificador, así que el nombre del componente es parte de lo que hace único su estado. Para compartir datos entre componentes, la vía es `props` (sección 10).

Y un detalle que merece atención, porque no es evidente: **el estado sobrevive a los redibujados, no al desmontaje.** Si un componente desaparece de la pantalla y vuelve a aparecer, su estado empieza de nuevo en el valor inicial. En el [taller Fila creativa](taller-modelado-react.md) eso no importa porque el estado vive en el componente raíz; pero explica por qué un formulario puede "perder" lo escrito al cambiar de pestaña o al navegar a otra vista. Volveremos a esto en la sección 11.

## 6. Eventos y formularios controlados

Un evento es algo que hace la persona o el navegador: hacer clic, escribir, enviar. En JSX se escriben como props que empiezan por `on` y llevan una función.

```tsx
<button type="button" onClick={agregar}>Agregar</button>
```

Fíjate en dos detalles que vienen de la [guía de JavaScript con HTML](guia-javascript-html.md): el `type="button"` (es lo correcto en un botón que **no** envía un formulario, y evita sorpresas) y que en JSX **no hay paréntesis** cuando la función es la prop entera.

```tsx
onClick={agregar}                                  // React le pasa el evento
onClick={() => agregar()}                          // tú la llamas, sin argumentos
onClick={() => agregar(producto.id)}               // tú la llamas, con argumentos
onClick={(evento) => setX(evento.target.value)}    // necesitas el evento
```

El patrón es: sin paréntesis si la función es la prop entera, porque React la invocará y le pasará el evento. Con paréntesis si la llamas tú, ya sea porque necesitas argumentos propios o porque quieres ignorar el evento.

### 6.1 `onChange` en inputs

En el navegador plano usaste `addEventListener("input", ...)` o `"change"`. En React, todo eso es `onChange`, y ocurre en cada tecla, no al salir del campo.

```tsx
function Campo({ etiqueta, valor, onCambio }: {
  etiqueta: string;
  valor: string;
  onCambio: (valor: string) => void;
}) {
  return (
    <label>
      {etiqueta}
      <input type="text" value={valor} onChange={(evento) => onCambio(evento.target.value)} />
    </label>
  );
}
```

Ese `evento.target.value` es la propiedad `value` del elemento del DOM que cambió, que ya usaste en la [guía de JavaScript con HTML](guia-javascript-html.md). En TypeScript el editor te lo sugiere: no hace falta escribir el tipo del evento.

### 6.2 Input controlado: `value` y `onChange` van juntos

Si escribes `value` sin `onChange`, React avisa:

```text
Warning: You provided a `value` prop to a form field without an `onChange` handler.
This will render a read-only field. If the field should be mutable use `defaultValue`.
```

Qué está pasando: pusiste el valor inicial y React lo dejó puesto. Como no hay forma de cambiarlo, el campo se comporta como solo lectura, igual que el `readonly` de un input en HTML puro.

Las dos salidas válidas son:

```tsx
// Controlado: React manda en el valor. Es el que usa este curso.
<input value={nombre} onChange={(e) => setNombre(e.target.value)} />

// No controlado: el campo guarda su propio valor y React no interviene
<input defaultValue={nombreInicial} />
```

**Quédate con el controlado.** Es el que permite mostrar el valor en otro lado de la pantalla, deshabilitar el botón mientras el campo está vacío o **resetearlo** después de enviar, que es justo lo que hace el [taller Fila creativa](taller-modelado-react.md).

### 6.3 El formulario y `preventDefault`

La [guía de JavaScript con HTML](guia-javascript-html.md#5-haz-una-versión-con-formulario) ya resolvió esto: si no llamas a `event.preventDefault()`, el navegador recarga la página y pierdes todo el estado. En React el mismo método sirve:

```tsx
function Formulario() {
  const [nombre, setNombre] = useState("");
  const [motivo, setMotivo] = useState("");

  function enviar(evento: React.FormEvent<HTMLFormElement>) {
    evento.preventDefault();

    const nombreLimpio = nombre.trim();
    const motivoLimpio = motivo.trim();

    if (!nombreLimpio || !motivoLimpio) {
      return;
    }

    // ... agregar el turno
    setNombre("");
    setMotivo("");
  }

  return (
    <form onSubmit={enviar}>
      <label>
        Nombre
        <input value={nombre} onChange={(e) => setNombre(e.target.value)} />
      </label>
      <label>
        Motivo
        <input value={motivo} onChange={(e) => setMotivo(e.target.value)} />
      </label>
      <button type="submit">Agregar turno</button>
    </form>
  );
}
```

Tres cosas para copiar: la **cláusula de guarda** con `trim()` y `return` temprano (la regla que ya usabas en el módulo 02), el tipo del evento en TypeScript, y el **reseteo manual** de los campos. En un input controlado, vaciar el estado vacía el campo: no hace falta buscar el elemento en el DOM.

### 6.4 El estado del formulario: un objeto o varios valores

Un formulario de tres campos se puede escribir de dos formas:

```tsx
// Forma A: un estado por campo
const [nombre, setNombre] = useState("");
const [motivo, setMotivo] = useState("");
const [prioridad, setPrioridad] = useState(1);
```

```tsx
type FormularioTurno = {
  nombre: string;
  motivo: string;
  prioridad: 1 | 2 | 3;
};

// Forma B: un estado que describe el formulario completo
const [formulario, setFormulario] = useState<FormularioTurno>({
  nombre: "",
  motivo: "",
  prioridad: 1,
});

function actualizar(campo: keyof FormularioTurno, valor: string) {
  setFormulario((actual) => ({ ...actual, [campo]: valor }));
}
```

La forma B tiene una ventaja que ya usaste en la [Guía 7 de POO](../02-programacion-base/guia-poo.md#paso-2-poner-las-reglas-dentro-del-modelo-comportamiento-y-encapsulamiento): **el formulario es una sola entidad con sus reglas**, y todas sus propiedades se actualizan juntas. Si aparece un campo nuevo, la forma A te obliga a añadir un `useState` y acordarte de vaciarlo también; la forma B solo necesita tocar el `type`.

Usa la forma B cuando los campos viajan juntos (un formulario, una entidad), y la forma A cuando son contadores independientes. El criterio es el mismo de siempre: **cohesión**.

## 7. Listas: `map` y `key`

Volvemos al problema de la sección 1.1. Los datos están en un arreglo y la pantalla debe mostrar uno por cada elemento. Para eso está `map`, que ya conoces del [módulo 02](../02-programacion-base/guia-listas.md).

```tsx
const productos: Producto[] = [
  { id: "p-01", nombre: "Cuaderno", precio: 12000, categoria: "papeleria", disponible: true },
  { id: "p-02", nombre: "Marcador", precio: 6000, categoria: "papeleria", disponible: false },
];

function Catalogo({ productos }: { productos: Producto[] }) {
  return (
    <ul>
      {productos.map((producto) => (
        <Tarjeta key={producto.id} producto={producto} />
      ))}
    </ul>
  );
}
```

Dentro de las llaves del `map` hay un elemento completo, con sus llaves y su `key`. Tres reglas:

1. **`map` devuelve elementos, no datos.** Cada callback devuelve un `<Tarjeta />`, no un `Producto`.
2. **La lista necesita un contenedor con sentido.** `<ul>` para una lista de cosas, `<ol>` si el orden importa, `<table>` si cada fila tiene varias columnas comparables, `<div className="grilla">` si es una cuadrícula de tarjetas.
3. **Cada elemento de la lista lleva `key`.**

### 7.1 Qué es `key` y por qué la necesita React

`key` le dice a React **quién es quién** en esa lista. Sin eso, React solo ve una secuencia de elementos y asume que el primero sigue siendo el primero, el segundo sigue siendo el segundo, y así.

Cuando eso falla, aparecen los errores más confusos de React. El caso clásico es una lista de tareas con casilla de marcar:

```tsx
// No uses el índice como key
{tareas.map((tarea, indice) => (
  <li key={indice}>
    <input type="checkbox" checked={tarea.completada} />
    {tarea.nombre}
  </li>
))}
```

Al marcar la primera, React cree que la posición 0 cambió de contenido, así que **reutiliza** los elementos que ya existían y solo les cambia el texto y el `checked`. El resultado: las casillas se desplazan una fila y los textos se descuadran. No parece un error de `checked`; parece un error de tu lógica.

La `key` correcta es el identificador del dato:

```tsx
{tareas.map((tarea) => (
  <li key={tarea.id}>
    <input type="checkbox" checked={tarea.completada} />
    {tarea.nombre}
  </li>
))}
```

Con la `key` correcta, React sabe que la tarea "t-02" se movió y reutiliza ese mismo elemento. Es el mismo `id` que usaste como clave en el diccionario de la sección de estructuras del [README del módulo](README.md#matriz-diccionario-y-conjunto), y cumple el mismo papel: **identidad estable, no posición**.

La regla práctica, y la causa de la mayoría de los errores:

> **La `key` viene del dato (`id`), nunca de la posición (`index`).**

Solo el índice es aceptable cuando la lista nunca se reordena, no agrega ni quita elementos, y no tiene ningún estado propio. Ese "nunca" aparece en casi todos los proyectos reales, así que por defecto usa el `id`.

### 7.2 Filtrar y mostrar con el mismo `map`

Un mismo `map` puede listas distintas según el estado, y por eso la `key` importa todavía más:

```tsx
const [busqueda, setBusqueda] = useState("");

const visibles = productos.filter((producto) =>
  producto.nombre.toLowerCase().includes(busqueda.toLowerCase()),
);

return (
  <main>
    <input value={busqueda} onChange={(e) => setBusqueda(e.target.value)} />
    {visibles.length === 0 ? (
      <p>No hay productos que coincidan con "{busqueda}".</p>
    ) : (
      <ul>
        {visibles.map((producto) => (
          <Tarjeta key={producto.id} producto={producto} />
        ))}
      </ul>
    )}
  </main>
);
```

Nótese que el `filter` y el `map` son funciones puras: reciben datos y devuelven datos nuevos. No modifican nada. Es la misma disciplina del [módulo 02](README.md#6-convierte-contenido-repetido-en-datos), y por eso este código es trivial de probar: llama a la función con un arreglo y compara el resultado.

## 8. Renderizado condicional

Un componente devuelve JSX, y el JSX puede depender de datos. Hay tres formas de escribir la decisión, y cada una tiene una trampa.

### 8.1 Operador ternario: cuando hay dos opciones

```tsx
{turnos.length > 0 ? (
  <ol>
    {turnos.map((turno) => (
      <li key={turno.id}>{turno.nombre}</li>
    ))}
  </ol>
) : (
  <p>No hay personas en espera.</p>
)}
```

Es la forma más explícita y la más recomendada cuando las dos ramas tienen contenido real. La alternativa cuando una rama es solo texto:

```tsx
<p>{turnos.length > 0 ? "Hay gente esperando" : "Fila vacía"}</p>
```

### 8.2 Operador `&&`: cuando una opción es no mostrar nada

```tsx
{producto.disponible && <p>Disponible</p>}
{carrito.length > 0 && <Badge cantidad={carrito.length} />}
```

Es más corto, pero tiene una trampa que sorprende a todo el mundo la primera vez:

```tsx
function Contador({ total }: { total: number }) {
  return <p>{total > 0 && "Hay productos"}</p>;
}
```

Si `total` es `0`, JavaScript devuelve **el segundo operando**, es decir `0`. Y `0` es un número, y React dibuja los números. El resultado en pantalla es un `0` suelto donde no debía haber nada.

La causa es que `&&` en JavaScript no devuelve un booleano, sino el primer operando falsy. La solución es forzar la condición a booleano:

```tsx
<p>{total > 0 && "Hay productos"}</p>            {/* mal: muestra un 0 */}
<p>{total > 0 ? "Hay productos" : null}</p>       {/* bien */}
<p>{total > 0 ? "Hay productos" : ""}</p>         {/* bien */}
```

Vale la pena saber **por qué** pasa, porque la corrección no es obvia si no entiendes que `0` es un valor dibujable. En la [Guía 1](../02-programacion-base/guia-if.md#ejemplo-12-valores-verdaderos-o-falsos-truthy-y-falsy) viste los valores *truthy* y *falsy* de JavaScript: `0`, `""` y `undefined` son falsy pero **no son booleanos**, y React dibuja cualquier valor que no sea `null`, `undefined` o un booleano.

### 8.3 `style` es un objeto, no texto

```tsx
<p style={{ color: "crimson" }}>Agotado</p>
<p style={{ backgroundColor: color, padding: "0.5rem" }}>...</p>
```

Dos comillas dobles por fuera y un objeto por dentro. El error habitual es escribir `style="color: red"` (texto), que React rechaza. Las propiedades usan el mismo nombre de CSS pero en `camelCase`: `background-color` se escribe `backgroundColor`.

Para algo que cambia según el estado, dos condiciones y un objeto suelen quedar mejor que una clase:

```tsx
<span className="etiqueta">{producto.disponible ? "Disponible" : "Agotado"}</span>
```

Las clases CSS son la opción recomendada en este curso, y ya tienes un archivo de estilos y clases que funcionan.

### 8.4 Lo que nunca se hace en el render

Un error de principiante que produce interfaces raras: llamar dentro del JSX una función que calcula cosas, o guardar en el estado un valor derivado.

```tsx
{productos.map((p) => <li key={p.id}>{p.precio * 1.19.toFixed(0)}</li>)}
```

Un cálculo corto está bien. Lo que no se hace es una petición de red, un `console.log` con efectos, ni escribir en el estado:

```tsx
// Nunca: renderiza, y además escribe en el estado
function Catalogo() {
  const total = calcularTotal(productos);
  setTotal(total); // bucle infinito de renderizados
}
```

**Regla:** el render **calcula**, no **produce efectos**. Si algo debe ocurrir por fuera de la pantalla (una petición, guardar en `localStorage`, una animación), eso es trabajo de un `useEffect`, que es un tema posterior a esta guía. Anótalo como pregunta y sigue.

## 9. Composición en vez de herencia

La [Guía 7 de POO](../02-programacion-base/guia-poo.md#6-herencia-es-un) estableció una regla que resulta aún más cierta en React: **antes de preguntar "¿es un?", pregúntate si puedes meter uno dentro del otro**. En React, casi siempre sí se puede, y la respuesta es componer.

### 9.1 La prueba del "es un" en React

Piensa en un catálogo con tarjetas de productos y un botón "agregar al carrito". ¿El botón "es un" producto? Suena absurdo. ¿Es "una" tarjeta? También. Lo que pasa es que **la tarjeta tiene** un botón.

```tsx
function Tarjeta({ producto, onAgregar }: { producto: Producto; onAgregar: () => void }) {
  return (
    <article className="tarjeta">
      <h2>{producto.nombre}</h2>
      <p>${producto.precio}</p>
      <BotonSutil onClick={onAgregar}>Agregar al carrito</BotonSutil>
    </article>
  );
}
```

La tarjeta **tiene** un botón. Se compone. Y la consecuencia práctica es enorme: `BotonSutil` no sabe nada de productos, así que se puede usar en el formulario, en un modal o en el pie de página.

### 9.2 `children`: la composición hecha explícita

`children` es lo que escribiste entre las etiquetas de apertura y cierre. Ya lo viste en la [sección 3.3](#33-un-componente-se-usa-con-etiquetas); aquí está el motivo de fondo.

```tsx
type PanelProps = {
  titulo: string;
  children: React.ReactNode;
};

function Panel({ titulo, children }: PanelProps) {
  return (
    <section className="panel">
      <h2>{titulo}</h2>
      {children}
    </section>
  );
}
```

`Panel` no sabe qué hay dentro. Puede contener texto, un formulario, una lista o los tres. Su trabajo es dar la estructura y dejar el hueco. Es la misma abstracción de interfaz de la [sección 2.3 de la guía de POO](../02-programacion-base/guia-poo.md#23-tres-niveles-de-abstracción): el código que usa el panel no necesita saber qué contiene, y el panel no necesita saber qué usará.

Y si la composición se repite, un componente con varias "ranuras" explícitas se lee incluso mejor:

```tsx
type DialogoProps = {
  titulo: string;
  cuerpo: React.ReactNode;
  pie: React.ReactNode;
};

function Dialogo({ titulo, cuerpo, pie }: DialogoProps) {
  return (
    <article className="dialogo">
      <h2>{titulo}</h2>
      <div className="dialogo__cuerpo">{cuerpo}</div>
      <footer className="dialogo__pie">{pie}</footer>
    </article>
  );
}

<Dialogo
  titulo="Confirmar"
  cuerpo={<p>¿Quieres eliminar esta tarea?</p>}
  pie={
    <>
      <button type="button">Cancelar</button>
      <button type="button">Eliminar</button>
    </>
  }
/>
```

`React.ReactNode` es el tipo que acepta "cualquier cosa que React pueda dibujar": texto, un elemento, un arreglo de elementos, `null`. No necesitas memorizar sus variantes; solo saber que no es `string`.

### 9.3 La tabla de decisión

| Situación | En vez de heredar, haz |
|---|---|
| Un elemento dentro de otro | meterlo como `children` |
| El mismo estilo en varios botones | un componente `Boton` con `variante` |
| Un formulario que cambia según quién lo usa | un componente con `children` que recibe los campos |
| Un comportamiento común de tres componentes | un **hook personalizado** (`useContador`) |
| Compartir datos entre ramas | subir el estado (sección 10) |

La última fila es la más importante, y la que más se malinterpreta: **compartir datos no se hace con herencia, se hace subiendo el estado**. Es lo que sigue.

## 10. Dónde vive el estado

Dos componentes que necesitan el mismo dato no pueden "heredarse" el estado. La única forma es que el componente que lo tiene esté **por encima** de los dos, y se lo pase por `props`. Eso se llama **elevar el estado** (*lifting state up*) y es la decisión estructural más importante de React.

El caso canónico: un campo de búsqueda arriba, el catálogo abajo, y los dos necesitan el mismo texto.

```tsx
// Mal: cada uno tiene su propia copia, y nadie los sincroniza
function Catalogo() {
  const [busqueda, setBusqueda] = useState("");
  return <input value={busqueda} onChange={(e) => setBusqueda(e.target.value)} />;
}

function Filtros() {
  const [busqueda, setBusqueda] = useState("");
  return <input value={busqueda} onChange={(e) => setBusqueda(e.target.value)} />;
}
```

```tsx
// Bien: el estado vive en App, y los dos lo reciben
function App() {
  const [busqueda, setBusqueda] = useState("");

  return (
    <main>
      <label>
        Buscar
        <input value={busqueda} onChange={(e) => setBusqueda(e.target.value)} />
      </label>
      <Filtros busqueda={busqueda} />
      <Catalogo productos={productos} busqueda={busqueda} />
    </main>
  );
}
```

La regla para decidir dónde va un estado es siempre la misma, y no requiere ninguna otra:

> **El estado vive en el ancestro común más cercano de todos los componentes que lo necesitan. Ni más arriba, ni más abajo.**

- ¿Solo un componente lo necesita? Va **dentro** de ese componente.
- ¿Dos componentes hermanos lo necesitan? Sube al ancestro común más cercano.
- ¿Toda la aplicación lo necesita? Sube al componente raíz.

Subir más de lo necesario tiene un costo concreto: cada cambio de ese estado vuelve a ejecutar a todos los descendientes que lo reciben, aunque no lo necesiten. Ese sobre-rendimiento es visible cuando la lista crece, y es la razón por la que este curso no usa un almacén global de estado.

## 11. Inmutabilidad: crear una versión nueva

Esta sección es la que más de un curso de React posterga, y es la que más errores causa cuando se salta. Es también la que conecta directamente con el [Error 7 de la guía de POO](../02-programacion-base/guia-poo.md#error-7-mutar-datos-que-pertenecen-a-react).

### 11.1 El problema con los arreglos y los objetos mutados

Cuando guardas un arreglo en el estado, React **no guarda el arreglo, guarda la referencia** a él. Compara la referencia nueva con la anterior para decidir si redibujar.

```tsx
const [productos, setProductos] = useState<Producto[]>([]);

function agregar(producto: Producto) {
  productos.push(producto);      // muta el arreglo que React tiene anotado
  setProductos(productos);       // le pasa la MISMA referencia
}

function quitar(id: string) {
  const restantes = productos.filter((p) => p.id !== id);
  setProductos(restantes);       // referencia nueva: sí funciona
}
```

En `agregar`, la segunda línea no produce ningún cambio visible. `productos` sigue siendo **el mismo objeto** de antes, así que React recibe la misma referencia, no ve nada nuevo y no vuelve a dibujar. El elemento **sí** se agregó al arreglo —el código de abajo lo encuentra— pero la pantalla no cambió. Ese es el síntoma clásico: *el dato está ahí, la pantalla no se entera*.

En `quitar` se crea un arreglo nuevo con `filter`, la referencia es distinta y React redibuja. Todo bien.

> **Regla del curso: nunca modifiques lo que está en el estado. Crea una versión nueva y pon esa en el estado.**

### 11.2 Las tres formas de crear la versión nueva

| Quiero… | Con `mutar` (no) | Creando versión nueva |
|---|---|---|
| Agregar al final | `lista.push(x)` | `[...lista, x]` |
| Quitar un elemento | `lista.splice(i, 1)` | `lista.filter((x) => x !== y)` |
| Quitar el primero | `lista.shift()` | `lista.slice(1)` |
| Cambiar un objeto | `objeto.precio = 0` | `{ ...objeto, precio: 0 }` |
| Cambiar un campo de un objeto dentro de un arreglo | `lista[0].precio = 0` | `lista.map((x) => x.id === id ? { ...x, precio: 0 } : x)` |
| Ordenar | `lista.sort(...)` | `[...lista].sort(...)` |

El `...` es el operador de propagación: copia los elementos existentes. La segunda forma tiene un detalle que conviene no pasar por alto: `lista.sort()` **ordena el arreglo original y devuelve el mismo objeto**, así que sin el `[...]` tendrías el problema de siempre.

El `slice(1)` de la tercera fila es exactamente el que usa el [taller Fila creativa](taller-modelado-react.md#paso-4-atiende-al-siguiente-turno) para atender al primero de la fila: crea una lista nueva desde el segundo elemento. Cumplir FIFO sin mutar el estado es posible, y es la demostración más limpia de por qué la regla existe.

### 11.3 Por qué React funciona así

No es arbitrario, y entenderlo evita tres clases de errores:

- **Rendimiento.** React compara referencias, no compara los mil elementos de un arreglo. Si dos renderizados producen el mismo `children` por referencia, React reutiliza los hijos y no los vuelve a construir. La regla le da esa oportunidad gratis.
- **Predecibilidad.** Si un componente recibiera un objeto que otro acaba de mutar, no podría saber si esos datos son suyos o si cambiarán bajo sus pies. Con datos inmutables, un valor recibido significa "esto no va a cambiar mientras lo tengo".
- **Historial.** React guarda los valores anteriores del estado para poder volver atrás. Un arreglo mutado no deja rastro del cambio; uno nuevo sí. (Lo verás cuando llegue el botón "deshacer" de la extensión A del taller, que es una pila LIFO construida de versiones nuevas.)

Con `readonly` de TypeScript, el compilador te ayuda a no meter el pie:

```ts
type Producto = {
  readonly id: string;
  readonly nombre: string;
  readonly precio: number;
};

type Carrito = {
  readonly items: readonly Producto[];
};
```

Con `readonly`, `carrito.items.push(...)` es un error de compilación, no un bug silencioso. Es la misma herramienta que usaste en el [paso 2 de la guía de POO](../02-programacion-base/guia-poo.md#paso-2-poner-las-reglas-dentro-del-modelo-comportamiento-y-encapsulamiento) para hacer que el carrito fuera confiable sin `private`.

### 11.4 Regla de la versión completa

Para no perderte, la versión completa de la regla, que vale para todo el código que escribas en React:

1. Nunca asignar al estado directamente.
2. Nunca llamar a `push`, `splice`, `shift`, `pop`, `sort`, `reverse` sobre algo que esté en el estado.
3. Crear la versión nueva con `...`, `map`, `filter`, `slice` o un objeto literal con `...`.
4. Pasar esa versión nueva a la función que cambia el estado.
5. Si es posible, usar la forma de función `(actual) => nuevo` para no depender del valor viejo.

## 12. Reglas de los Hooks

Un Hook es una función que empieza por `use` y que le permite a un componente usar capacidades de React: estado, y más adelante otras cosas. `useState` es el primero.

Las reglas son dos, y se pueden decir completas:

1. **Llama a los Hooks solo en el nivel más alto** de un componente funcional o de un hook personalizado. Nunca dentro de una condición, un ciclo, una función anidada o un bloque `try`.
2. **Llama a los Hooks solo mientras React está ejecutando un componente** o un hook personalizado. Nunca desde una función normal, un manejador de evento o un `setTimeout`.

```tsx
// Correcto: en el nivel más alto, siempre y en el mismo orden
function Contador({ inicial }: { inicial: number }) {
  const [cantidad, setCantidad] = useState(inicial);
  const [historico, setHistorico] = useState<number[]>([]);

  function agregar() {
    setHistorico((actual) => [...actual, cantidad]);
    setCantidad((valor) => valor + 1);
  }

  return <button onClick={agregar}>{cantidad}</button>;
}
```

```tsx
// Incorrecto: el Hook depende de una condición
function Contador({ mostrar }: { mostrar: boolean }) {
  if (mostrar) {
    const [cantidad, setCantidad] = useState(0); // rompe la regla 1
  }
  return <p>...</p>;
}
```

```tsx
// Incorrecto: el Hook está después de un return temprano
function Contador({ visible }: { visible: boolean }) {
  if (!visible) {
    return null;
  }

  const [cantidad, setCantidad] = useState(0); // rompe la regla 1
  return <p>{cantidad}</p>;
}
```

### 12.1 Por qué existen

React guarda los Hooks de un componente **en una lista, en orden de llamada**. Cuando el componente se vuelve a ejecutar, React recorre esa lista en el mismo orden y te devuelve el estado de cada uno, posición por posición.

Si en la tercera ejecución la primera llamada a `useState` desaparece porque una condición cambió, todo se desplaza: lo que React cree que era "el segundo estado" ahora es el primero, y el estado de tu contador se pega al valor de otro Hook. El síntoma clásico es un componente con un estado que cambia de valor solo, o un error que dice algo como *"Rendered fewer hooks than expected"*.

Por eso el orden tiene que ser estable, y por eso no se pueden meter Hooks en condiciones. No es una convención de estilo: es un requisito de cómo funciona el almacenamiento.

### 12.2 Dónde va un `return` temprano

Si un componente **no necesita estado**, un `return` temprano no tiene ningún problema: no hay Hooks cuyo orden pueda cambiar.

```tsx
function Vacio({ visible }: { visible: boolean }) {
  if (!visible) {
    return <p>No hay contenido.</p>;
  }

  return <p>Listo.</p>;
}
```

El problema aparece cuando el componente **sí** necesita estado. Ahí la salida temprana no se puede quedar arriba, porque los Hooks de más abajo dejarían de ejecutarse en algunas ejecuciones. La solución es separar las dos responsabilidades en dos componentes:

```tsx
// El componente exterior decide qué se muestra y no usa estado
function Panel({ children }: { children: React.ReactNode }) {
  if (!children) {
    return <p>No hay contenido.</p>;
  }

  return <Contenido>{children}</Contenido>;
}

// El interior tiene los Hooks y siempre se ejecuta igual
function Contenido({ children }: { children: React.ReactNode }) {
  const [abierto, setAbierto] = useState(false);

  return (
    <section>
      <button onClick={() => setAbierto((valor) => !valor)}>
        {abierto ? "Ocultar" : "Mostrar"}
      </button>
      {abierto && children}
    </section>
  );
}
```

El patrón mental es: **la decisión de qué renderizar va en el componente de arriba; el estado, en el de abajo**. Y no es un detalle menor: es la diferencia entre un componente que funciona y uno que se rompe con un dato que no esperabas.

## 13. Ejemplo completo: la tiendita

Ahora juntamos todo. Esta sección es la que conviene leer despacio, porque es la forma recomendada de entender un refactor: **de abajo hacia arriba, un paso a la vez, comprobando que cada versión funciona**.

Cada paso es una versión ejecutable. No avances al siguiente hasta ver la anterior funcionando en `http://localhost:5173/`.

### Paso 0: la página estática (funciona, pero no responde)

```tsx
function App() {
  return (
    <main>
      <h1>Tienda creativa</h1>
      <section>
        <h2>Productos</h2>
        <article className="tarjeta">
          <h3>Cuaderno</h3>
          <p>$12000</p>
          <button type="button">Agregar</button>
        </article>
        <article className="tarjeta">
          <h3>Marcador</h3>
          <p>$6000</p>
          <button type="button">Agregar</button>
        </article>
      </section>
      <aside>
        <h2>Carrito</h2>
        <p>Vacío</p>
      </aside>
    </main>
  );
}
```

Es la página de la [guía de HTML](guia-html.md) con dos productos. Se ve bien, usa etiquetas con sentido y abre en el navegador. El botón no hace nada, y sin estado no puede hacerlo: no hay dónde recordar qué se agregó.

### Paso 1: el modelo de datos

Antes de cualquier componente, los datos. Es el `Producto` de la [sección 2.2 de la guía de POO](../02-programacion-base/guia-poo.md#22-el-producto-real-y-el-producto-del-programa), sin los campos que esta aplicación no necesita:

```tsx
type Producto = {
  id: string;
  nombre: string;
  precio: number;
  categoria: "papeleria" | "comida" | "tecnologia";
  disponible: boolean;
};

type ItemCarrito = {
  producto: Producto;
  cantidad: number;
};

const PRODUCTOS_INICIALES: Producto[] = [
  { id: "p-01", nombre: "Cuaderno", precio: 12000, categoria: "papeleria", disponible: true },
  { id: "p-02", nombre: "Marcador", precio: 6000, categoria: "papeleria", disponible: true },
  { id: "p-03", nombre: "Galletas", precio: 3500, categoria: "comida", disponible: true },
  { id: "p-04", nombre: "Borrador", precio: 2500, categoria: "papeleria", disponible: false },
];
```

Una función pura para el total, sin componente ni estado:

```tsx
function total(carrito: ItemCarrito[]): number {
  return carrito.reduce((suma, item) => suma + item.producto.precio * item.cantidad, 0);
}
```

Esta función es la que hace válida la separación `modelo → operaciones → interfaz`. Se puede probar sin React, sin navegador y sin pantalla.

### Paso 2: un componente por dato, y `map` para la lista

```tsx
type TarjetaProps = {
  producto: Producto;
  onAgregar: (producto: Producto) => void;
};

function Tarjeta({ producto, onAgregar }: TarjetaProps) {
  return (
    <article className="tarjeta">
      <h3>{producto.nombre}</h3>
      <p>${producto.precio}</p>
      <button
        type="button"
        disabled={!producto.disponible}
        onClick={() => onAgregar(producto)}
      >
        {producto.disponible ? "Agregar" : "Agotado"}
      </button>
    </article>
  );
}

function Catalogo({ productos, onAgregar }: { productos: Producto[]; onAgregar: (p: Producto) => void }) {
  return (
    <section>
      <h2>Productos</h2>
      <ul>
        {productos.map((producto) => (
          <li key={producto.id}>
            <Tarjeta producto={producto} onAgregar={onAgregar} />
          </li>
        ))}
      </ul>
    </section>
  );
}
```

Tres mejoras sobre el paso 0, y ninguna de ellas es estado: los datos salen de una lista, cada producto tiene un `id` que sirve de `key`, y `Tarjeta` es reutilizable y comprobable por separado. El botón todavía no agrega nada, porque `onAgregar` todavía no existe de verdad.

### Paso 3: estado para el carrito, con versiones nuevas

```tsx
import { useState } from "react";

function App() {
  const [carrito, setCarrito] = useState<ItemCarrito[]>([]);
  const [busqueda, setBusqueda] = useState("");

  function agregarAlCarrito(producto: Producto) {
    if (!producto.disponible) {
      return;
    }

    setCarrito((actual) => {
      const existente = actual.find((item) => item.producto.id === producto.id);

      const siguiente = existente
        ? actual.map((item) =>
            item.producto.id === producto.id
              ? { producto, cantidad: item.cantidad + 1 }
              : item,
          )
        : [...actual, { producto, cantidad: 1 }];

      return siguiente;
    });
  }

  const visibles = PRODUCTOS_INICIALES.filter((producto) =>
    producto.nombre.toLowerCase().includes(busqueda.toLowerCase()),
  );

  return (
    <main>
      <h1>Tienda creativa</h1>

      <label>
        Buscar
        <input value={busqueda} onChange={(e) => setBusqueda(e.target.value)} />
      </label>

      <Catalogo productos={visibles} onAgregar={agregarAlCarrito} />

      <aside>
        <h2>Carrito</h2>
        {carrito.length === 0 ? (
          <p>Vacío</p>
        ) : (
          <ul>
            {carrito.map((item) => (
              <li key={item.producto.id}>
                {item.producto.nombre} x {item.cantidad}
              </li>
            ))}
          </ul>
        )}
        <p>Total: ${total(carrito)}</p>
      </aside>
    </main>
  );
}
```

Aquí está el corazón del asunto, y son cuatro ideas en pocas líneas:

1. **El estado es un arreglo de datos**, y `total` es una función pura aparte. La pantalla no calcula: muestra.
2. **Actualizar es crear una versión nueva.** `agregarAlCarrito` nunca hace `carrito.push(...)`. Usa `map` para cambiar la cantidad de un artículo existente y `...` para agregar uno nuevo. Devuelve la versión nueva desde la función, y React la recibe.
3. **`key={item.producto.id}`** es estable porque el identificador viene del dato, no de la posición.
4. **`total` se usa, no se guarda.** No hay `const [total, setTotal] = useState(0)`, y por eso es imposible que el total mostrado no coincida con el carrito real.

### Paso 4: quitar del carrito

El paso que confirma la regla de la sección 11, porque las dos formas posibles se ven muy distintas:

```tsx
function quitarDelCarrito(id: string) {
  setCarrito((actual) => actual.filter((item) => item.producto.id !== id));
}

function restarDelCarrito(id: string) {
  setCarrito((actual) =>
    actual
      .map((item) =>
        item.producto.id === id ? { ...item, cantidad: item.cantidad - 1 } : item,
      )
      .filter((item) => item.cantidad > 0),
  );
}
```

Ninguna de las dos muta nada. `restarDelCarrito` usa dos transformaciones: `map` para bajar la cantidad y `filter` para quitar el artículo que llegó a cero. Esa es la versión legible de "si la cantidad llega a cero, el artículo desaparece", y evita tener que comprobarlo en un `if`.

**Todos los pasos dan el mismo resultado en las preguntas que importan.** Cambiar la representación no debería cambiar lo que la interfaz sabe mostrar, y esa es la diferencia entre un refactor y una reescritura.

## 14. ¿Cómo pienso este problema?

Cuando te pidan "hazlo con React" o "haz que esto responda", el camino es siempre el mismo. Son las preguntas que hacen buena interfaz, en orden:

1. **¿Qué datos necesita saber el programa?** Antes de escribir JSX, escribe los `type`. Es el filtro de la [sección 2.2 de la guía de POO](../02-programacion-base/guia-poo.md#22-el-producto-real-y-el-producto-del-programa): si no lo vas a usar, no lo guardes.
2. **¿Qué preguntas puedo hacerle a esos datos?** Escribe esas operaciones como **funciones puras**, fuera de los componentes. Si puedes probarlas sin React, ya tienes la parte difícil.
3. **¿Qué partes de la pantalla se parecen entre sí?** Cada una es un componente. Y pregunta primero por la composición: ¿uno puede ir dentro del otro?
4. **¿Qué cambia cuando la persona hace algo?** Cada respuesta a una acción es un manejador de evento, y lo que cambia es estado.
5. **¿Quién necesita ver ese cambio?** El componente más bajo que lo necesite. Si lo necesitan dos hermanos, súbelo al ancestro común más cercano (sección 10).
6. **¿Qué se puede calcular en vez de guardar?** Si sale de otro dato, calcúlalo. Menos estado significa menos casos donde dos cosas se descuadran.
7. **¿Estoy creando versiones nuevas o mutando?** Toda actualización de un arreglo u objeto dentro del estado pasa por `...`, `map`, `filter` o `slice` (sección 11).
8. **¿Cada elemento de una lista tiene `key` del dato, no del índice?** Si sí, la lista se va a comportar bien al reordenarse.

Si respondes esas ocho preguntas, el componente casi se escribe solo. Y si al hacerlo te sobra un `type` o un `useState`, es señal de que estás haciendo más de lo necesario.

## 15. Errores frecuentes

### Error 1: `class` en lugar de `className`

```tsx
<div class="tarjeta">...</div>
```

**Error:** `class` es una palabra reservada de JavaScript. React rechaza el atributo y suele mostrar algo como *"Invalid DOM property `class`. Did you mean `className`?"*. El mismo problema, con el mismo nombre, en `for` → `htmlFor`. Recuerda que la columna de la [sección 2.6](#26-la-tabla-de-traducción) es tu referencia rápida.

### Error 2: varios elementos hermanos

```tsx
function Lista() {
  return (
    <li>Cuaderno</li>
    <li>Marcador</li>
  );
}
```

**Error:** *"JSX expressions must have one parent element"* o *"Adjacent JSX elements must be wrapped in an enclosing tag"*. Un componente devuelve un solo elemento. La corrección no es envolver en `<div>` por costumbre: elige la etiqueta que signifique algo (`<ul>`, `<section>`) o usa un fragmento `<>...</>` si el padre ya agrupa todo. Es el error que más sentido tiene revisar, porque cada vez que aparece es una decisión de estructura.

### Error 3: olvidar la `key` o usar el índice

```tsx
{productos.map((producto, indice) => (
  <li key={indice}>{producto.nombre}</li>
))}
```

**Error:** React avisa *"Each child in a list should have a unique key prop"*. Con la clave puesta, el aviso desaparece, pero el problema real sigue ahí si usas el índice: al filtrar, al reordenar o al borrar, React reutiliza el elemento equivocado y los textos se descuadran de las casillas o los botones. La clave es el `id` del dato. La [sección 7.1](#71-qué-es-key-y-por-qué-la-necesita-react) tiene el caso completo.

### Error 4: modificar el estado en vez de crear una versión nueva

```tsx
function agregar(producto: Producto) {
  carrito.push({ producto, cantidad: 1 });
  setCarrito(carrito);
}
```

**Error:** el elemento se agrega al arreglo, pero la pantalla no cambia. `setCarrito` recibe la misma referencia, y React no ve nada nuevo. Es el error más silencioso de React: no hay excepción, no hay advertencia, la función se ejecuta y el resultado es que "no pasa nada". La forma correcta es `setCarrito((actual) => [...actual, { producto, cantidad: 1 }])`, como en la [sección 11](#11-inmutabilidad-crear-una-versión-nueva).

### Error 5: leer el estado viejo justo después de cambiarlo

```tsx
function agregar() {
  setCarrito((actual) => [...actual, nuevo]);
  console.log(carrito.length); // todavía el valor anterior
}
```

**Error:** la segunda línea lee la variable `carrito` de **esta** ejecución del componente, que es el valor viejo. Cambiarlo no cambia el valor local hasta que React vuelva a ejecutar el componente. Si necesitas calcular algo a partir del valor nuevo, hazlo **dentro** de la forma de función, o usa el valor que devolvió la operación pura.

### Error 6: meter un `useState` dentro de una condición o de un ciclo

```tsx
function Panel({ visible }: { visible: boolean }) {
  if (visible) {
    const [abierto, setAbierto] = useState(false);
  }
  return <p>...</p>;
}
```

**Error:** React espera el mismo número de Hooks en el mismo orden en cada ejecución. Si la cantidad o el orden cambian, los estados se desplazan de componente y aparece un error como *"Rendered fewer hooks than expected"*, o un valor que cambia solo sin que nadie lo haya tocado. La [sección 12](#12-reglas-de-los-hooks) explica por qué. Si necesitas un Hook condicional, extrae un componente y pon el estado dentro.

### Error 7: un componente con minúscula

```tsx
function tarjeta() {
  return <h2>Cuaderno</h2>;
}

function App() {
  return <tarjeta />;
}
```

**Error:** React interpreta `<tarjeta />` como una etiqueta HTML llamada `tarjeta`, no como tu componente, y falla con *"Element type is invalid"*. El nombre del **componente** y el **archivo** que lo exporta empiezan con mayúscula (`Tarjeta`, `Tarjeta.tsx`); los **nombres de las props** van en minúscula o en camelCase (`titulo`, `productoActual`), igual que los atributos de HTML. La convención no es estética: es lo que le permite a React distinguir un componente de un elemento del documento.

### Error 8: guardar en el estado lo que se puede calcular

```tsx
const [productos, setProductos] = useState<Producto[]>([]);
const [total, setTotal] = useState(0);
const [disponibles, setDisponibles] = useState<Producto[]>([]);
```

**Error:** no es un error de compilación; es un error de diseño. Son tres datos que se mantienen sincronizados a mano, y basta con olvidar uno para que la pantalla muestre cifras distintas de la realidad. Además, cada `setTotal` necesita su propio manejador de evento, y la lógica de negocio se dispersa por todos lados. Calcula: `const total = ...` y `const disponibles = productos.filter(...)`. Si se puede derivar, se deriva (sección 5.3).

### Error 9: el componente que lo hace todo

```tsx
function App() {
  const [productos, setProductos] = useState<Producto[]>([]);
  const [carrito, setCarrito] = useState<ItemCarrito[]>([]);
  const [busqueda, setBusqueda] = useState("");
  const [vista, setVista] = useState<"lista" | "cajas">("lista");
  const [cargando, setCargando] = useState(false);
  // ... y además carga datos, guarda en disco y arma el total

  return <div>{/* 300 líneas */}</div>;
}
```

**Error:** es la versión de React del `hacerTodo` del [módulo 02](../02-programacion-base/README.md#una-función-una-responsabilidad) y de la `Tienda` del [Error 6 de la guía de POO](../02-programacion-base/guia-poo.md#error-6-la-clase-que-lo-hace-todo). Un componente con seis estados y ninguna separación no se puede probar: para probar el total necesitas el formulario, la búsqueda y la vista. La salida es la misma de siempre, y funciona: `App` conserva el estado del carrito, extrae `Catalogo`, `Tarjeta`, `Formulario` y deja que cada uno tenga una responsabilidad.

### Error 10: `style` como texto

```tsx
<p style="color: crimson">Agotado</p>
```

**Error:** en JSX el estilo es un objeto de JavaScript, no una cadena de CSS: `style={{ color: "crimson" }}`, con llaves por fuera y objeto por dentro, y con los nombres de las propiedades en `camelCase`. Para lo que cambia según el estado, una clase CSS suele ser más legible (sección 8.3).

## 16. Resolver antes de seguir

> Para cada ejercicio entrega: el código, una descripción de cómo se ve funcionando y una explicación de la estrategia. Si usas estado, la explicación debe incluir qué dato es estado, qué dato es derivado y por qué no mutaste nada.

### Nivel 0 - perderle el miedo

1. Crea un componente `Saludo` que reciba una prop `nombre` y muestre `<h1>Hola, {nombre}</h1>`. Muéstralo con tres nombres distintos.
2. Crea un componente `Tarjeta` con las props `titulo` y `precio`, y un precio opcional que por defecto sea `0`. Muestra tres tarjetas.
3. Crea un `Contador` con un botón que sume uno y otro que reinicie a `0`. Después cambia el reinicio por un botón que baje de uno, y comprueba que no baja de cero.
4. Escribe la versión del `Contador` con un `let` normal en vez de `useState`. Explica con tus palabras por qué el número cambia y la pantalla no.

### Nivel 1 - aplicar lo esencial

5. Escribe un componente `ListaDeTareas` que reciba `tareas: Tarea[]` y las muestre con `ul` y `li`. Cada `Tarea` tiene `id`, `titulo` y `completada`. Muestra al menos cuatro tareas, y comprueba que la `key` es el `id`.
6. Añade un formulario con un solo campo de texto controlado. Al enviar, agrega la tarea al final y vacía el campo. No debe admitir tareas con el título vacío.
7. Convierte la lista de tareas en un formulario de la [sección 6.4](#64-el-estado-del-formulario-un-objeto-o-varios-valores): un solo estado con `titulo` y `completada`.
8. Marca y desmarca tareas con un `input type="checkbox"`. Después agrega un botón "Quitar" a cada una. Explica qué pasaría si usaras el índice como `key`.

### Nivel 2 - listas, filtro y estado

9. Escribe un `Catalogo` que reciba `productos: Producto[]` y los muestre en una grilla de tarjetas. Cada tarjeta muestra nombre, precio y si está disponible, y deshabilita el botón cuando no lo está.
10. Añade un campo de búsqueda **por encima** de `Catalogo`, no dentro de él. Filtra por nombre sin distinguir mayúsculas. Explica, en dos frases, por qué el estado no vive dentro de `Catalogo`.
11. Agrega un carrito con `useState<ItemCarrito[]>([])`. Al agregar un producto que ya está, aumenta su cantidad en vez de duplicar la línea. Todo con versiones nuevas: nada de `push` ni de mutar el arreglo.
12. Muestra el total del carrito con una **función pura** definida fuera del componente. Llámala tres veces desde la consola con datos distintos y comprueba que da lo mismo que el total que muestra la pantalla.
13. Agrega "quitar del carrito" y "restar uno", y haz que quitar el último artículo de una línea la borre de la lista. Prueba agregar, restar hasta cero y volver a agregar.

### Nivel 3 - profundización

14. Escribe un hook personalizado `useContador(inicial)` que devuelva el valor, una función para incrementar y una para reiniciar. Úsalo en tres componentes distintos y comprueba que cada uno tiene su propio estado. Explica qué parte de las reglas de los Hooks cumple y por qué.
15. Extrae un componente `Panel` con props `titulo` y `children`, y úsalo para envolver tres partes distintas de tu interfaz: una con texto, una con una lista y una con un formulario. Explica qué ventaja tiene sobre tres `<div>`.
16. Refactoriza tu proyecto del [taller de fundamentos](../02-programacion-base/taller-fundamentos.md): toma tres retos que tengas resueltos y conviértelos en componentes con estado. En cada uno, escribe primero las funciones puras en un archivo aparte y úsalas desde los componentes. Compara el número de líneas y qué parte se puede probar sin navegador.
17. Toma un proyecto tuyo de HTML plano y llévalo a React siguiendo los pasos de la sección 13. Escribe en la bitácora, paso a paso, qué cambió y qué se quedó igual. La respuesta esperada es: **la estructura y el significado se quedan igual; lo que cambia es de dónde vienen los datos y quién decide cuándo redibujar.**

## 17. Vocabulario

| Término | Definición corta |
| --- | --- |
| Componente | Función que devuelve JSX y representa una parte de la interfaz. |
| JSX | Sintaxis que permite escribir etiquetas HTML dentro de JavaScript o TypeScript. |
| Etiqueta / elemento | En JSX, la forma de invocar un componente (`<Tarjeta />`). |
| Fragmento | `<>...</>`, para devolver varios elementos sin agregar un contenedor al documento. |
| `props` | Datos que un componente recibe de quien lo usa. Se declaran con un `type` y son de solo lectura. |
| `children` | Prop que contiene lo que se escribió entre las etiquetas de un componente. |
| Estado (`state`) | Dato que un componente recuerda entre redibujados y que él mismo cambia. |
| `useState` | Hook que devuelve un par: el valor actual y la función que lo cambia. |
| Hook | Función que empieza por `use` y que da capacidades de React a un componente. |
| Renderizado | Ejecución de un componente: React calcula otra vez su JSX. |
| Redibujado | Nuevo resultado del render, con el estado actualizado. |
| Evento | Algo que hace la persona o el navegador; en JSX se conecta con `onClick`, `onChange`, `onSubmit`. |
| Input controlado | Un `input` con `value` y `onChange`, donde React manda en el valor. |
| `map` | Método que transforma un arreglo y devuelve otro, usado para renderizar listas. |
| `key` | Identidad estable de un elemento dentro de una lista; suele ser el `id` del dato. |
| Condicional en render | Ternario o `&&` para mostrar una cosa u otra según un dato. |
| Composición | Meter un componente dentro de otro con `children`, en vez de heredar. |
| Elevar el estado | Poner el estado en el ancestro común más cercano de quien lo necesita. |
| Inmutabilidad | Crear una versión nueva de un dato en vez de modificar el existente. |
| Operador de propagación | `...`, copia los elementos o propiedades de un valor. |
| `React.ReactNode` | Tipo que acepta cualquier contenido dibujable por React. |
| Strict Mode | Modo de desarrollo que ejecuta funciones dos veces para detectar impurezas. |
| Vite | Herramienta que prepara el proyecto, instala dependencias y levanta el servidor de desarrollo. |

## 18. Chuleta rápida

```tsx
import { useState } from "react";

// Modelo de datos
type Producto = {
  id: string;
  nombre: string;
  precio: number;
  disponible: boolean;
};

type ItemCarrito = {
  producto: Producto;
  cantidad: number;
};

// Función pura: se prueba sin React
function total(carrito: ItemCarrito[]): number {
  return carrito.reduce((suma, item) => suma + item.producto.precio * item.cantidad, 0);
}

// Componente de presentación
type TarjetaProps = {
  producto: Producto;
  destacada?: boolean;
};

function Tarjeta({ producto, destacada = false }: TarjetaProps) {
  return (
    <article className={destacada ? "tarjeta tarjeta--destacada" : "tarjeta"}>
      <h2>{producto.nombre}</h2>
      <p>${producto.precio}</p>
      {!producto.disponible && <p>Agotado</p>}
    </article>
  );
}

// Componente con composición
function Panel({ titulo, children }: { titulo: string; children: React.ReactNode }) {
  return (
    <section className="panel">
      <h2>{titulo}</h2>
      {children}
    </section>
  );
}

// Estado y actualización sin mutar
function App() {
  const [carrito, setCarrito] = useState<ItemCarrito[]>([]);
  const [busqueda, setBusqueda] = useState("");

  function agregar(producto: Producto) {
    setCarrito((actual) => {
      const existe = actual.some((item) => item.producto.id === producto.id);

      return existe
        ? actual.map((item) =>
            item.producto.id === producto.id
              ? { producto, cantidad: item.cantidad + 1 }
              : item,
          )
        : [...actual, { producto, cantidad: 1 }];
    });
  }

  function quitar(id: string) {
    setCarrito((actual) => actual.filter((item) => item.producto.id !== id));
  }

  function limpiar() {
    setCarrito([]); // versión nueva: un arreglo vacío
  }

  // Condicionales y listas
  return (
    <main>
      <input value={busqueda} onChange={(e) => setBusqueda(e.target.value)} />

      {carrito.length === 0 ? (
        <p>Carrito vacío</p>
      ) : (
        <ul>
          {carrito.map((item) => (
            <li key={item.producto.id}>
              {item.producto.nombre} x {item.cantidad}
              <button type="button" onClick={() => quitar(item.producto.id)}>
                Quitar
              </button>
            </li>
          ))}
        </ul>
      )}

      <p>Total: ${total(carrito)}</p>
      <button type="button" onClick={limpiar} disabled={carrito.length === 0}>
        Vaciar
      </button>
    </main>
  );
}
```

## 19. Herramientas para profundizar

- **Documentación oficial de React, en español** (`es.react.dev`): la fuente más confiable. Para esta guía son especialmente útiles las páginas *Describir la interfaz* (`es.react.dev/learn/describing-the-ui`), *Renderizado y confirmación* (`es.react.dev/learn/render-and-commit`) y *Compartir el estado entre componentes* (`es.react.dev/learn/sharing-state-between-components`), que es exactamente la sección 10 de esta guía. La referencia de *Reglas de los Hooks* (`es.react.dev/reference/rules/rules-of-hooks`) incluye los casos válidos e inválidos.
- **Referencia de `useState`** (`es.react.dev/reference/react/useState`): explica la forma de función para actualizar, la inicialización perezosa con `useState(() => valor)` y qué pasa cuando el nuevo estado es igual al anterior.
- **Vite, guía oficial** (`vite.dev/guide`): los scripts disponibles en `package.json` (`dev`, `build`, `preview`), la carpeta `public`, y los requisitos de versión de Node. Útil para el [módulo 04 de Git y GitHub](../04-git-github/README.md) y para la [publicación en Vercel](../05-publicacion/README.md).
- **MDN Web Docs, "React" en la guía de JavaScript** (`developer.mozilla.org`): el apartado de React dentro del recorrido de JavaScript conecta React con lo que ya sabes del lenguaje y es una buena revisión después de la guía de HTML.
- **El código del curso**: la calculadora, el catálogo y la tiendita son el laboratorio real. Cada componente que escribas es una oportunidad de aplicar la regla de la sección 11 y ver, en el navegador, por qué importa.

## 20. Lo que viene después

Esta guía te dio el vocabulario de React (componente, `props`, estado, JSX, Hook, renderizado) y, sobre todo, cuatro reglas que explican casi todos los errores del módulo: **el estado vive en React, no en una variable local; se cambia con la función que lo creó; se reemplaza por una versión nueva; y no se muta**.

Lo que sigue, en el [README del módulo](README.md), es la parte de modelado: elegir la estructura de datos correcta (pila, cola, matriz, diccionario) según el comportamiento que necesites, y ver las estructuras de la [sección de estructuras](README.md#arreglo-estructura-abstracta-y-comportamiento) aplicadas a la interfaz. Después viene el [taller de modelado y React: Fila creativa](taller-modelado-react.md), donde modelas turnos como objetos, los organizas en una **cola FIFO**, los agregas con un formulario controlado y los atiendes **sin mutar el estado** con `slice(1)`. Todo lo de esta guía aparece ahí, y ese es el mejor examen.

También quedan dos temas que **no** estudiaste aquí a propósito, y conviene que sepas que existen: `useEffect`, para trabajo que ocurre fuera de la pantalla (peticiones de red, guardar en `localStorage`), y los componentes con clase, que son la forma antigua y que no se usa en este curso. Cuando el taller funcione, ambos tienen su momento.

Antes de seguir, comprueba que puedes responder estas cuatro preguntas sin mirar el código:

1. ¿Por qué una variable `let` declarada en el cuerpo de un componente no redibuja la pantalla, y `useState` sí?
2. ¿Por qué `carrito.push(...)` seguido de `setCarrito(carrito)` no actualiza nada, y cuál es la versión correcta?
3. ¿Por qué la `key` de una lista debería ser el `id` del dato y no el índice?
4. ¿Qué componente debería tener el estado de un campo de búsqueda que usan un filtro y un catálogo, y por qué?

Si las cuatro salen con tus propias palabras, la guía hizo su trabajo. Si alguna no, vuelve a la [sección 5](#5-estado-lo-que-cambia-y-hay-que-redibujar) (estado), a la [sección 11](#11-inmutabilidad-crear-una-versión-nueva) (inmutabilidad) o a la [sección 7](#7-listas-map-y-key) (listas) y vuelve a intentarlo.
