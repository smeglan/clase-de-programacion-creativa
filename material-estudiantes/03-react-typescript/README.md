# 3. Modelado de datos, estructuras y React

El módulo 02 enseñó cómo expresar pasos con variables, condiciones, ciclos y funciones. Ahora damos un salto importante: aprender a **modelar** aquello sobre lo que trabaja la aplicación y a mostrar ese modelo mediante React.

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

Cada elemento necesita una `key` estable. El `id` del modelo de datos suele ser una buena opción.

## Preguntas opcionales de profundización

No son entregas adicionales del taller. Úsalas si terminas antes o quieres explorar el tema con más profundidad.

1. Modela una biblioteca: libro, lector y préstamo.
2. Implementa un historial de edición con pila y una función `deshacer`.
3. Implementa una cola FIFO para turnos de soporte.
4. Construye una cola de prioridad y explica por qué no es una cola normal.
5. Representa un tablero con una matriz y muéstralo en React.
6. Construye una tiendita que filtre productos y agregue elementos al carrito sin mutar el estado.

Continúa con el [Taller de modelado y React: Fila creativa](taller-modelado-react.md). Es un único proyecto visual de máximo cuatro horas: modela turnos como objetos, los organiza en una cola FIFO y los muestra con React.
