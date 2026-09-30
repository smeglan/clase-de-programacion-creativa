# 3. De HTML a React con Vite

En el [módulo 02](../02-programacion-base/README.md) aprendiste la lógica de programación y completaste la [guía obligatoria de POO](../02-programacion-base/guia-poo.md). También creaste una [primera página con HTML y CSS](../02-programacion-base/guia-html-css.md).

Ahora vas a llevar una página estática a un proyecto con React. El recorrido será:

```text
index.html abierto directamente → proyecto Vite → componente React con JSX y CSS
```

## Antes de continuar: comprende HTML

Lee primero la [guía independiente de HTML: de cero a una base sólida](guia-html.md). HTML sigue siendo importante aunque construyas interfaces con React: describe la estructura y el significado del contenido que el navegador representa. React usa JSX, una sintaxis parecida a HTML, y termina creando elementos HTML. Conocer HTML te permite elegir etiquetas semánticas, hacer interfaces más accesibles y entender qué escribes cuando pases a JSX.

Después de la guía, repasa la [práctica de HTML y CSS](../02-programacion-base/guia-html-css.md), y luego estudia [JavaScript con HTML](guia-javascript-html.md) para crear interacciones en el navegador antes de continuar con Vite y React.

## Antes de programar: la guía de React

Este README te muestra el camino en orden corto. La [Guía de React: de la página estática a una interfaz que responde](guia-react.md) es la versión extendida: aplica la misma estructura con la profundidad de la [Guía 7 de POO](../02-programacion-base/guia-poo.md), con el problema, los conceptos uno por uno, un ejemplo completo refactorizado paso a paso, errores frecuentes, ejercicios por niveles, glosario y chuleta.

Cuando ya puedas construir y hacer responder una página estática, es el momento de leerla: explica en detalle lo que este README solo menciona.

- por qué la página estática se queda corta y qué cambia cuando la verdad de los datos pasa del DOM al estado;
- las cuatro diferencias entre HTML y JSX, con la tabla de traducción;
- por qué un componente empieza con mayúscula y qué error aparece si no;
- por qué una variable `let` no redibuja la pantalla y `useState` sí;
- la diferencia entre input controlado y no controlado;
- por qué la `key` de una lista debe ser el `id` del dato y no el índice;
- por qué en React se crea una versión nueva de un arreglo en vez de modificarlo;
- y las dos reglas de los Hooks, con el error que producen.

## 1. Antes de empezar

Comprueba que Node.js y npm estén instalados:

```bash
node --version
npm --version
```

Necesitas entender carpetas, guardar archivos y usar la terminal. Si todavía no tienes este entorno listo, consulta la [guía de herramientas](../01-herramientas/README.md). Según la [guía actual de Vite](https://vite.dev/guide/), se requiere Node 20.19+ o 22.12+; revisa esa página si aparece una advertencia de versión porque el requisito puede cambiar.

La página HTML y CSS anterior se puede abrir con doble clic porque es estática. En este recorrido, React se ejecutará dentro de un proyecto con dependencias y un servidor de desarrollo; Vite se encarga de iniciar ese entorno y preparar la aplicación.

> React es la biblioteca para construir la interfaz; Vite es la herramienta de desarrollo y construcción del proyecto. No son dos nombres para lo mismo.

## 2. Crea un proyecto React con Vite

Abre una terminal en la carpeta donde guardas tus prácticas y ejecuta:

```bash
npm create vite@latest mi-pagina-react -- --template react-ts
cd mi-pagina-react
npm install
npm run dev
```

Vite mostrará una dirección local, normalmente `http://localhost:5173/`. Ábrela en el navegador. Para detener el servidor de desarrollo, vuelve a la terminal y presiona `Ctrl + C`.

Qué hace cada comando:

- `npm create vite@latest ...` crea un proyecto inicial usando la plantilla React con TypeScript.
- `cd mi-pagina-react` entra a la carpeta creada.
- `npm install` descarga las dependencias declaradas por el proyecto.
- `npm run dev` inicia el servidor local para trabajar y ver cambios.

Si prefieres crear el proyecto con las preguntas interactivas de Vite, ejecuta `npm create vite@latest`, elige un nombre, React y TypeScript.

## 3. Reconoce las piezas del proyecto

La plantilla contiene más archivos que la página estática, pero cada uno tiene una función:

```text
mi-pagina-react/
├── index.html
├── package.json
└── src/
    ├── main.tsx
    ├── App.tsx
    ├── App.css
    └── index.css
```

- `index.html` es el documento inicial que carga el navegador. Incluye el contenedor donde React dibuja la interfaz.
- `src/main.tsx` conecta React con ese contenedor.
- `src/App.tsx` contiene el componente principal de la página. Aquí escribirás JSX.
- `src/App.css` y `src/index.css` contienen estilos CSS.
- `package.json` registra dependencias y comandos del proyecto.

En esta práctica cambia principalmente `App.tsx` y los archivos CSS. No borres el contenedor raíz de `index.html` ni el código de montaje de `main.tsx`.

## 4. Pasa tu HTML a JSX

En la guía estática pudiste escribir:

```html
<article class="tarjeta">
  <h1>Mi proyecto</h1>
  <p>Una descripción breve.</p>
</article>
```

En `src/App.tsx`, ese contenido se coloca dentro de un componente:

```tsx
function App() {
  return (
    <main>
      <article className="tarjeta">
        <h1>Mi proyecto</h1>
        <p>Una descripción breve.</p>
      </article>
    </main>
  );
}

export default App;
```

JSX se parece a HTML, pero tiene algunas reglas propias:

- Usa `className` en lugar de `class` para asignar una clase CSS.
- Cierra las etiquetas, incluso las que no llevan contenido: `<img />`, `<input />`.
- Devuelve un elemento raíz. Si necesitas varios elementos hermanos, envuélvelos en `<main>`, un `<div>` o un fragmento `<>...</>`.
- Usa llaves `{}` para mostrar un valor o una expresión de JavaScript, por ejemplo `<h1>Hola, {nombre}</h1>`.
- Los atributos se escriben en camelCase en algunos casos: `onClick`, `htmlFor`.

Las etiquetas semánticas que aprendiste (`main`, `article`, `section`, `h1`, `p`, `ul`) siguen siendo útiles en JSX. React las convierte en elementos que el navegador representa como HTML.

## 5. Conserva los estilos CSS

Puedes reaprovechar la mayoría de las reglas CSS de tu página anterior. Importa la hoja al inicio de `App.tsx`:

```tsx
import "./App.css";
```

Y usa la clase en JSX:

```tsx
<article className="tarjeta">...</article>
```

Una clase CSS escrita como `.tarjeta` se conserva igual. El atributo cambia de `class` a `className` porque `class` ya tiene otro significado en JavaScript.

## 6. Convierte contenido repetido en datos

Una página estática repite a mano cada tarjeta. En React puedes guardar la información en un arreglo y describir una tarjeta como un componente:

```tsx
import "./App.css";

type Proyecto = {
  id: string;
  nombre: string;
  descripcion: string;
};

  const proyectos: Proyecto[] = [
  { id: "p-1", nombre: "Calculadora", descripcion: "Operaciones básicas." },
  { id: "p-2", nombre: "Catálogo", descripcion: "Productos por categoría." },
];

function TarjetaProyecto({ proyecto }: { proyecto: Proyecto }) {
  return (
    <article className="tarjeta">
      <h2>{proyecto.nombre}</h2>
      <p>{proyecto.descripcion}</p>
    </article>
  );
}

function App() {
  return (
    <main>
      <h1>Mis proyectos</h1>
      <section className="galeria">
        {proyectos.map((proyecto) => (
          <TarjetaProyecto key={proyecto.id} proyecto={proyecto} />
        ))}
      </section>
    </main>
  );
}

export default App;
```

- `Proyecto` describe los datos permitidos.
- `proyectos` contiene dos objetos de ese tipo.
- `TarjetaProyecto` recibe un proyecto por `props` y lo muestra.
- `map` crea una tarjeta por cada dato.
- `key` le da a React una identidad estable para cada elemento de la lista.

Este ejemplo une lo que ya viste: HTML semántico, CSS, objetos, arreglos, funciones y tipos.

## 7. Cómo trabajar con Vite

Deja abierta la terminal con `npm run dev`. Cuando guardas cambios en los archivos, Vite actualiza la página. Si cierras la terminal, el servidor se detiene; vuelve a iniciarlo desde la carpeta del proyecto con `npm run dev`.

Para generar una versión lista para publicar, más adelante usarás:

```bash
npm run build
```

La salida de producción queda en `dist/`. En este curso prepararás la publicación después de aprender el flujo de Git y GitHub.

## 8. Errores frecuentes

- **`npm` o `node` no se reconoce:** confirma la instalación y abre una terminal nueva.
- **`Missing script: dev`:** comprueba que la terminal está dentro de la carpeta que contiene `package.json`.
- **No se ve un cambio:** revisa la terminal por errores y guarda el archivo correcto.
- **Aparece un error por `class`:** cambia el atributo JSX a `className`.
- **Error de cierre o de elementos hermanos:** cierra todas las etiquetas y asegúrate de que el `return` tenga un solo elemento raíz.
- **Error con el CSS:** confirma que `App.css` está en `src` y que la ruta de `import` coincide.
- **No inicia por la versión de Node:** instala una versión compatible indicada por Vite y vuelve a abrir la terminal.

Estos son los errores de entorno. Los errores de React tienen su propia lista, con diez casos explicados uno por uno, en la [sección 15 de la guía de React](guia-react.md#15-errores-frecuentes): `class` en vez de `className`, elementos hermanos sin contenedor, `key` con índice, estado mutado, estado leído después de cambiarlo, un `useState` dentro de una condición, un componente en minúscula, estado derivado guardado como estado, el componente que lo hace todo y `style` escrito como texto.

## 9. Orden recomendado para aprender este módulo

1. Confirma que la página estática de HTML abre en el navegador.
2. Lee la [Guía de React](guia-react.md) completa: es la que explica el porqué de los pasos siguientes.
3. Crea el proyecto React con Vite y explora los archivos principales.
4. Copia la estructura de la página estática a JSX y reutiliza el CSS.
5. Define un modelo de datos con `type` o `interface`.
6. Representa listas y estructuras de datos con componentes.
7. Añade eventos y estado para permitir cambios en la interfaz.
8. Completa el [Taller Fila creativa](taller-modelado-react.md).

Los conceptos de abstracción, objetos, arreglos y estructuras que siguen en esta página te ayudarán a tomar decisiones antes de escribir componentes más grandes.

## Abstracción: construir un modelo útil

Una aplicación no copia el mundo real completo. Selecciona los detalles necesarios para resolver un problema y deja fuera los que no importan todavía.

Imagina una tiendita. En la vida real un producto puede tener proveedor, código de barras, peso, fecha de vencimiento, fotografía, tamaño y decenas de atributos. Para filtrar y vender en nuestro proyecto inicial solo podríamos necesitar esto:

```ts
type Producto = {
  id: string;
  nombre: string;
  precio: number;
  categoria: "papeleria" | "comida" | "tecnologia";
  disponible: boolean;
};
```

La abstracción no pregunta “¿cómo es el producto real?”, sino “¿qué necesita saber esta aplicación para cumplir su propósito?”. Si el problema cambia, el modelo también puede cambiar.

## De la situación a la interfaz

```text
Situación real → modelo de datos → operaciones → interfaz

Productos de una tienda
    → Producto y Producto[]
    → buscar, filtrar, agregar al carrito
    → tarjetas, botones y total en React
```

Primero definimos los datos y las operaciones. Después React los representa visualmente. Esta separación evita que toda la lógica quede atrapada dentro de botones y componentes.

## Objetos: datos que pertenecen a la misma entidad

Un objeto reúne características relacionadas de una misma cosa o concepto.

```ts
type Tarea = {
  id: string;
  titulo: string;
  completada: boolean;
  prioridad: 1 | 2 | 3;
};

const tarea: Tarea = {
  id: "t-01",
  titulo: "Publicar la calculadora",
  completada: false,
  prioridad: 1,
};
```

Usa un objeto cuando varios valores responden juntos a la pregunta “¿qué describe esto?”. Un producto tiene datos propios; una tarea tiene otros. No agrupes datos solo porque están cerca en el código.

## Tipos, interfaces y objetos reales

`type` e `interface` describen un contrato durante el desarrollo; el objeto es el valor real que existe mientras se ejecuta el programa.

```ts
interface CarritoItem {
  producto: Producto;
  cantidad: number;
}

const item: CarritoItem = {
  producto: {
    id: "p-01",
    nombre: "Cuaderno",
    precio: 12,
    categoria: "papeleria",
    disponible: true,
  },
  cantidad: 2,
};
```

En este curso puedes usar `type` o `interface` para modelar datos. Lo importante es que el modelo sea claro, no memorizar una supuesta regla absoluta entre ambas formas.

## Arreglos de objetos: una colección con significado

```ts
const productos: Producto[] = [
  { id: "p-01", nombre: "Cuaderno", precio: 12, categoria: "papeleria", disponible: true },
  { id: "p-02", nombre: "Marcador", precio: 6, categoria: "papeleria", disponible: false },
];
```

Un arreglo indica que tenemos una secuencia. Las operaciones expresan preguntas diferentes:

```ts
const disponibles = productos.filter((producto) => producto.disponible);
const nombres = productos.map((producto) => producto.nombre);
const marcador = productos.find((producto) => producto.id === "p-02");
const hayAgotados = productos.some((producto) => !producto.disponible);
```

- `filter`: ¿cuáles cumplen una regla?
- `map`: ¿cómo transformo cada elemento?
- `find`: ¿cuál es el primer elemento que busco?
- `some`: ¿existe al menos un elemento con esta condición?

## Arreglo, estructura abstracta y comportamiento

Un arreglo no es automáticamente una pila o una cola. Puede implementar distintos comportamientos según las operaciones que permitamos.

| Estructura | Regla | Imagen mental | Ejemplo |
|---|---|---|---|
| Lista | se recorre o consulta en orden | lista de compras | catálogo |
| Pila - LIFO | último en entrar, primero en salir | platos apilados | deshacer |
| Cola - FIFO | primero en entrar, primero en salir | fila de atención | turnos |
| Deque | entrada y salida por ambos extremos | fila con dos puertas | buffer o historial |
| Cola de prioridad | sale primero quien tiene mayor prioridad | urgencias | tareas críticas |
| Matriz | datos por filas y columnas | tablero | mapa o juego |
| Diccionario/Mapa | acceso por clave | agenda | producto por `id` |
| Conjunto/Set | no repite valores | álbum | etiquetas |

> Usaremos las siglas internacionales **FIFO** (*First In, First Out*) y **LIFO** (*Last In, First Out*). En español también se ven PEPS y UEPS. Una pila corresponde al comportamiento LIFO.

### Pila - LIFO

```ts
const historial: string[] = [];

historial.push("cambiar color");
historial.push("agregar botón");

const accionParaDeshacer = historial.pop(); // "agregar botón"
```

Una pila sirve cuando el último paso debe revertirse primero: deshacer, navegación atrás o exploración de caminos.

### Cola - FIFO

```ts
type Turno = { nombre: string; motivo: string };

const turnos: Turno[] = [];

turnos.push({ nombre: "Lina", motivo: "Pregunta sobre React" });
turnos.push({ nombre: "Tomás", motivo: "Error de instalación" });

const siguiente = turnos.shift(); // Lina
```

En una demostración pequeña `push` y `shift` son suficientes. En sistemas grandes, retirar el primer elemento de un arreglo puede ser costoso; por eso se usan índices de inicio o estructuras especializadas.

### Cola de prioridad

```ts
type TareaUrgente = {
  titulo: string;
  prioridad: 1 | 2 | 3;
};

const tareas: TareaUrgente[] = [
  { titulo: "Responder correo", prioridad: 2 },
  { titulo: "Corregir error crítico", prioridad: 1 },
  { titulo: "Ordenar archivos", prioridad: 3 },
];

const siguienteTarea = [...tareas].sort((a, b) => a.prioridad - b.prioridad)[0];
```

Una cola de prioridad no es FIFO: la regla de salida es la importancia. El ejemplo ordena una copia porque es sencillo de leer; sistemas grandes suelen usar un *heap*.

### Matriz, diccionario y conjunto

```ts
const tablero: string[][] = [
  ["🌱", "🌱", "🪨"],
  ["🌱", "🚶", "🌱"],
  ["💧", "🌱", "🏁"],
];

const productosPorId: Record<string, Producto> = {
  "p-01": productos[0],
};

const etiquetas = new Set<string>(["react", "typescript"]);
etiquetas.add("algoritmos");
```

- Una matriz expresa que importa la posición: fila y columna.
- Un diccionario responde “dame el dato con esta clave”.
- Un conjunto responde “¿este valor ya fue incluido?”.

## Elegir la estructura correcta

1. ¿Necesito deshacer lo último? Pila.
2. ¿Necesito atender en orden de llegada? Cola.
3. ¿Importa más la urgencia que el orden? Cola de prioridad.
4. ¿Busco por identificador o nombre? Diccionario o `Map`.
5. ¿No quiero repetidos? `Set`.
6. ¿Importa una coordenada? Matriz.
7. ¿Solo quiero recorrer, filtrar o mostrar una secuencia? Arreglo/lista.

## React: mostrar el modelo y permitir cambios

Un componente representa una parte de la interfaz. Sus props reciben datos; el estado guarda datos que cambian por la interacción.

```tsx
type SaludoProps = {
  nombre: string;
};

function Saludo({ nombre }: SaludoProps) {
  return <h1>Hola, {nombre}</h1>;
}
```

```tsx
import { useState } from "react";

function Contador() {
  const [cantidad, setCantidad] = useState(0);

  return (
    <button onClick={() => setCantidad(cantidad + 1)}>
      Veces: {cantidad}
    </button>
  );
}
```

### Estado con arreglos y objetos

En React no mutamos directamente el estado. Creamos una nueva versión para que React pueda detectar el cambio.

```tsx
const [carrito, setCarrito] = useState<CarritoItem[]>([]);

function agregarAlCarrito(producto: Producto) {
  setCarrito((actual) => [...actual, { producto, cantidad: 1 }]);
}
```

El estado `carrito` representa datos. `agregarAlCarrito` representa una operación del dominio. El componente muestra el resultado. Mantener estas capas separadas hace que la aplicación sea más fácil de probar y modificar.

### Renderizar listas

```tsx
function Catalogo({ productos }: { productos: Producto[] }) {
  return (
    <ul>
      {productos.map((producto) => (
        <li key={producto.id}>
          {producto.nombre}: ${producto.precio}
        </li>
      ))}
    </ul>
  );
}
```

Cada elemento necesita una `key` estable. El `id` del modelo de datos suele ser una buena opción, y el [error 3 de la guía de React](guia-react.md#error-3-olvidar-la-key-o-usar-el-índice) muestra por qué usar el índice produce textos descuadrados al reordenar o borrar.

La sección [React: mostrar el modelo y permitir cambios](#react-mostrar-el-modelo-y-permitir-cambios) resume el estado y las listas. Si alguna de esas piezas te queda dando vueltas, la [guía de React](guia-react.md) las explica una por una: el [estado](guia-react.md#5-estado-lo-que-cambia-y-hay-que-redibujar), los [eventos y formularios controlados](guia-react.md#6-eventos-y-formularios-controlados), las [listas y la `key`](guia-react.md#7-listas-map-y-key), la [inmutabilidad](guia-react.md#11-inmutabilidad-crear-una-versión-nueva) y las [reglas de los Hooks](guia-react.md#12-reglas-de-los-hooks).

## Preguntas opcionales de profundización

No son entregas adicionales del taller. Úsalas si terminas antes o quieres explorar el tema con más profundidad.

1. Modela una biblioteca: libro, lector y préstamo.
2. Implementa un historial de edición con pila y una función `deshacer`.
3. Implementa una cola FIFO para turnos de soporte.
4. Construye una cola de prioridad y explica por qué no es una cola normal.
5. Representa un tablero con una matriz y muéstralo en React.
6. Construye una tiendita que filtre productos y agregue elementos al carrito sin mutar el estado. La [sección 13 de la guía de React](guia-react.md#13-ejemplo-completo-la-tiendita) resuelve exactamente esta: es el mismo recorrido, con el código completo y la explicación de por qué cada actualización crea una versión nueva.

Continúa con el [Taller de modelado y React: Fila creativa](taller-modelado-react.md). Es un único proyecto visual de máximo cuatro horas: modela turnos como objetos, los organiza en una cola FIFO y los muestra con React.
