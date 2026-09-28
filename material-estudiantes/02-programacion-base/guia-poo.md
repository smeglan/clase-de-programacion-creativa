# Guía 7: Programación orientada a objetos: clases y abstracción

## Objetivos

Al terminar esta guía deberías poder:

- Explicar con tus palabras qué es la **abstracción** y por qué es la idea central de la programación orientada a objetos.
- Decidir qué datos de un problema vale la pena modelar y cuáles conviene dejar fuera, en lugar de copiarlo todo.
- Escribir una clase en TypeScript con propiedades, constructor, métodos y `this`, e instanciarla con `new`.
- Aplicar **encapsulamiento** con `private`, getters y setters que validan las reglas del problema.
- Distinguir **composición** ("tiene un") de **herencia** ("es un") y elegir la correcta en un caso concreto.
- Usar una `interface` como contrato para escribir código polimórfico y explicar qué cambia para quien llama.
- Decidir si un problema se resuelve mejor con una clase o con `type` y funciones puras, que es lo habitual en React.
- Refactorizar un programa real: pasar de variables sueltas a un modelo compuesto, paso a paso.

> **Cómo probar los ejemplos**
>
> Son clases de **TypeScript**, como las de las guías 5 y 6. Guarda el código en un archivo `.ts`, por ejemplo `poo.ts`, y ejecútalo con:
>
> ```bash
> npx tsx poo.ts
> ```
>
> Si el entorno no permite `tsx`, quita las anotaciones de tipo y ejecuta con `node poo.js`. La lógica es la misma: las clases existen también en JavaScript; lo único que TypeScript agrega es la ayuda de los tipos.

## 1. El problema: "tengo datos que siempre viajan juntos"

En la [Guía 6](guia-ordenamientos.md) escribiste funciones que reciben números sueltos, como `bubbleSort([5, 2, 9, 1, 7])`. Todo funcionaba bien porque los datos no tenían nombre propio.

Ahora el problema cambia. Estás armando la tiendita del curso y necesitas guardar información de productos: nombre, precio, categoría, disponibilidad y cuántos hay en bodega.

La forma más rápida de resolverlo, y la que casi todo el mundo escribe primero, es usar variables sueltas:

```ts
let nombreProducto = "Cuaderno";
let precioProducto = 12000;
let categoriaProducto = "papeleria";
let disponibleProducto = true;
let cantidadProducto = 4;
```

Funciona... hasta que llega el segundo producto. Ahora tienes `nombreProducto2`, `precioProducto2`, `categoriaProducto2`, y con veinte productos tienes cien variables. La pregunta se vuelve incómoda:

> *¿Qué variable era la del marcador? ¿Y la cantidad de cuál producto era 4?*

El problema real no es la cantidad de variables: es que **los datos que describen a un producto quedaron sueltos y el programa ya no sabe qué datos van juntos**.

## 2. Abstracción: el eje de esta guía

Si el problema es "los datos no están juntos", la respuesta suena obvia: "los junto en un objeto". Pero la palabra que describe lo que acabas de hacer no es "juntarlos", es **abstraer**. Vale la pena entenderla bien, porque es la idea más útil de toda la programación orientada a objetos, mucho más que las palabras `class` o `extends`.

### 2.1 Qué significa abstraer

*Abstraer* viene de "arrastrar fuera". Es **dejar fuera los detalles que no importan para el problema que estás resolviendo**.

En la vida real lo haces todo el tiempo sin darte cuenta:

- Un mapa de la ciudad no tiene las coordenadas exactas de cada esquina: tiene nombres de calles, barrios y colores. Ese detalle existe, pero no importa para tu tarea: llegar de A a B.
- Un plano de una casa muestra dónde va cada mueble, no la fórmula exacta del cemento.
- Una nota del professor en el cuaderno no registra la ortografía de cada palabra, solo qué hay que corregir.

Un buen modelo es como un mapa: **sirve para el propósito**. No es una copia incompleta por descuido; es una copia hecha a la medida del propósito.

### 2.2 El producto real y el producto del programa

Miremos un producto de una tienda real. Podría tener:

```text
código de barras, proveedor, peso en gramos, altura, ancho, fecha de vencimiento,
fotografía, material, país de origen, IVA, descuento mayorista, garantía, voltaje...
```

Ahora pregúntate qué necesita saber **esta** aplicación para cumplir **su** propósito: mostrar un catálogo, filtrar por categoría, agregar al carrito y calcular un total. Con eso basta:

```ts
type Producto = {
  id: string;
  nombre: string;
  precio: number;
  categoria: "papeleria" | "comida" | "tecnologia";
  disponible: boolean;
};
```

Fuera quedaron el código de barras, el peso y la garantía. No es que sean inútiles: es que **esta aplicación no los necesita todavía**. Y esa es la diferencia entre abstraer y adivinar:

> La abstracción no pregunta "¿cómo es un producto real?", pregunta **"¿qué necesita saber esta aplicación para cumplir su propósito?"**.

Si el propósito cambia, el modelo cambia. Una app de inventario sí necesitaría `stock`; una app de ventas, nada de eso. Este mismo `Producto` es el punto de partida del [módulo 03](../03-react-typescript/README.md).

### 2.3 Tres niveles de abstracción

Cuando abstraes, en realidad estás decidiendo tres cosas a la vez. Vale la pena separarlas, porque cada una se resuelve con una herramienta distinta.

**Nivel 1. Abstracción de datos: qué guardo y qué no**

Es el nivel del `type Producto` de arriba. Guardas lo que el programa usa, con nombres que dicen qué son (`precio`, no `p`). Si dos datos siempre se usan juntos y describen lo mismo, van en el mismo objeto.

**Nivel 2. Abstracción de comportamiento: qué puede hacerse con esos datos**

Un producto no solo *tiene* precio: también puede *calcular* su costo con IVA o *aplicar* un descuento. Las operaciones del negocio son parte del modelo, y por eso conviene que vivan junto a los datos y no repartidas por distintos archivos:

```ts
const IVA = 0.19;

type Producto = {
  nombre: string;
  precio: number;
};

function costoConIVA(producto: Producto): number {
  return producto.precio * (1 + IVA);
}

function aplicarCupon(producto: Producto, porcentaje: number): Producto {
  if (porcentaje < 0 || porcentaje > 100) {
    throw new Error("El cupón debe estar entre 0 y 100");
  }

  return { nombre: producto.nombre, precio: producto.precio * (1 - porcentaje / 100) };
}

const cuaderno: Producto = { nombre: "Cuaderno", precio: 12000 };

console.log(costoConIVA(cuaderno));                              // 14280
console.log(aplicarCupon(cuaderno, 10).precio);                  // 10800
```

Fíjate en dos cosas: el valor `IVA` tiene nombre (nada de números mágicos, como pide el módulo 02) y `aplicarCupon` **devuelve un producto nuevo** en vez de modificar el que recibió. Esa es la misma idea que verás en React más adelante: crear una versión nueva en lugar de mutar la anterior.

**Nivel 3. Abstracción de interfaz: qué NO dejas ver**

Esta es la que casi todo el mundo pasa por alto, y es la más valiosa. La interfaz de un objeto es el conjunto de preguntas que se le puede hacer. Un `Carrito` debería exponer `agregar`, `quitar` y `total`; no debería exponer cómo guarda por dentro los artículos.

La regla práctica, y la que resume toda esta sección:

> **El código que usa un objeto no debería saber cómo están guardados sus datos por dentro.**

Si para calcular el total del carrito necesitas conocer la lista interna de artículos, la abstracción está mal hecha. En la sección 4 lo arreglamos con `private`, getters y setters.

### 2.4 Cuándo vale la pena abstraer (y cuándo no)

Abstraer tiene un costo: escribes más líneas que las mínimas necesarias. Vale la pena cuando se cumple alguna de estas señales:

| Señal | Por qué indica que hay algo que abstraer |
|---|---|
| **Cohesión**: hay datos que siempre se usan juntos y siempre describen lo mismo | Un objeto agrupa lo que pertenece a una sola entidad |
| **Reglas de negocio**: hay condiciones que no se pueden violar (el precio no es negativo) | El modelo puede impedirlas, en vez de confiar en que todos se acuerden |
| **Repetición (regla de tres)**: la misma lógica copiada por tercera vez | Ya no es casualidad: hay una entidad o una operación escondida ahí |
| **Misma forma en lugares distintos**: un producto y una tarea se parecen en estructura | La forma común es un buen contrato (una `interface`) |

Y una advertencia igual de importante:

> **No abstraes por anticipar el futuro.** No guardes `altura`, `peso` y `color` "por si acaso" en un producto que solo se usa para vender en línea. Abstraer de más produce modelos rígidos, campos que nadie llena y preguntas que ya no sabes responder.

La estrategia que funciona: **primero escribir el programa de forma simple, y abstraer cuando la repetición o las reglas te digan que hay algo que modelar**. Esa es la diferencia entre abstraer y sobre-ingenierizar.

## 3. De objeto a clase: la receta repetida

Hasta ahora agrupamos datos con un `type` y un objeto literal. Eso es un objeto. Pero cuando varios objetos de la misma clase comparten exactamente la misma forma **y las mismas operaciones**, ya no estás escribiendo una receta nueva cada vez: estás describiendo una plantilla. Esa plantilla es una **clase**.

```ts
type Producto = {
  id: string;
  nombre: string;
  precio: number;
  categoria: "papeleria" | "comida" | "tecnologia";
};

const cuaderno: Producto = { id: "p-01", nombre: "Cuaderno", precio: 12000, categoria: "papeleria" };
const marcador: Producto = { id: "p-02", nombre: "Marcador", precio: 6000, categoria: "papeleria" };
```

Si además de guardar el precio quieres calcular el costo con IVA, cada objeto tendría que repetir ese cálculo, y si mañana el IVA cambia hay que cambiarlo en todos lados. La clase te permite **escribir la operación una sola vez**:

```ts
class Producto {
  private readonly id: string;
  private readonly nombre: string;
  private precio: number;
  private readonly categoria: "papeleria" | "comida" | "tecnologia";

  constructor(
    id: string,
    nombre: string,
    precio: number,
    categoria: "papeleria" | "comida" | "tecnologia",
  ) {
    this.id = id;
    this.nombre = nombre;
    this.precio = precio;
    this.categoria = categoria;
  }

  descripcion(): string {
    return `${this.nombre} (${this.categoria})`;
  }

  aplicarDescuento(porcentaje: number): void {
    if (porcentaje < 0 || porcentaje > 100) {
      throw new Error("El descuento debe estar entre 0 y 100");
    }

    this.precio = this.precio * (1 - porcentaje / 100);
  }

  costoConIVA(iva: number): number {
    return this.precio * (1 + iva);
  }
}

const cuaderno = new Producto("p-01", "Cuaderno", 12000, "papeleria");
const marcador = new Producto("p-02", "Marcador", 6000, "papeleria");

cuaderno.aplicarDescuento(10);

console.log(cuaderno.descripcion());      // Cuaderno (papeleria)
console.log(cuaderno.costoConIVA(0.19));   // 12852
console.log(marcador.costoConIVA(0.19));   // 7140
```

### Las piezas de una clase

| Pieza | Qué es | En el ejemplo |
|---|---|---|
| **Propiedad** | Un dato que el objeto guarda | `nombre`, `precio` |
| **Constructor** | La receta que se ejecuta al crear el objeto | `constructor(id, nombre, precio, categoria)` |
| **`this`** | "el objeto que está ejecutando este código" | `this.precio = precio` asigna al objeto actual, no a una variable suelta |
| **Método** | Una operación que usa los datos del objeto | `descripcion()`, `aplicarDescuento()` |
| **Instancia** | Cada objeto creado con `new` | `cuaderno` y `marcador` son dos instancias distintas |

Tres detalles de `this` que conviene entender de una:

1. `this` es lo que permite que el mismo método sirva para muchos objetos. `descripcion()` no sabe si la llamaste sobre el cuaderno o sobre el marcador: usa `this.nombre` y listo.

2. `this` **pierde su referencia** si guardas el método suelto en una variable:

   ```ts
   const describir = cuaderno.descripcion;
   describir(); // TypeError: this is undefined
   ```

   Se arregla pasando el objeto explícitamente con `cuaderno.descripcion.call(cuaderno)`, o más simple: no separar el método de su objeto.

3. Los métodos se llaman **con paréntesis**. `cuaderno.descripcion` es la función; `cuaderno.descripcion()` es la llamada. Es el mismo error que en la [Guía 4](guia-listas.md) con `compra.push` sin `()`.

Y un recordatorio de la [Guía 4](guia-listas.md) que aquí cobra más sentido: dos instancias nunca son iguales, aunque tengan los mismos datos, porque `===` compara **referencias** (¿es *el mismo* objeto?), no contenido.

```ts
const a = new Producto("p-01", "Cuaderno", 12000, "papeleria");
const b = new Producto("p-01", "Cuaderno", 12000, "papeleria");

console.log(a === b); // false: son dos objetos distintos
```

## 4. Encapsulamiento: los datos se protegen, no se tocan

Hasta ahora las propiedades son públicas: cualquiera podría escribir `producto.precio = -500` y crear un producto con precio negativo. Eso no es un error de sintaxis, es un error de **diseño**: dejamos la puerta abierta para que alguien rompa una regla del negocio.

El **encapsulamiento** consiste en exponer la forma de leer y modificar un dato de manera controlada, en lugar de dejar la puerta abierta.

```ts
class Producto {
  readonly id: string;
  private nombre: string;
  private precio: number;

  constructor(id: string, nombre: string, precio: number) {
    if (precio < 0) {
      throw new Error("El precio no puede ser negativo");
    }

    this.id = id;
    this.nombre = nombre;
    this.precio = precio;
  }

  get nombreProducto(): string {
    return this.nombre;
  }

  get valor(): number {
    return this.precio;
  }

  set nuevoPrecio(valor: number) {
    if (valor < 0) {
      throw new Error("El precio no puede ser negativo");
    }

    this.precio = valor;
  }

  conDescuento(porcentaje: number): number {
    if (porcentaje < 0 || porcentaje > 100) {
      throw new Error("El descuento debe estar entre 0 y 100");
    }

    return this.precio * (1 - porcentaje / 100);
  }
}

const cuaderno = new Producto("p-01", "Cuaderno", 12000);

console.log(cuaderno.nombreProducto); // Cuaderno
console.log(cuaderno.valor);          // 12000
console.log(cuaderno.conDescuento(10)); // 10800

cuaderno.nuevoPrecio = 15000;
console.log(cuaderno.valor);          // 15000

cuaderno.nuevoPrecio = -5;           // Error: El precio no puede ser negativo
```

Tres ideas detrás de ese código:

1. **`private`** esconde el dato. Desde fuera no existe `cuaderno.precio`: existen las preguntas que tú decidiste exponer. Si mañana renombras el dato interno a `precioEnCentavos`, nadie más se rompe. Eso es exactamente la abstracción de interfaz de la sección 2.3.
2. **Un setter sin validar no encapsula nada.** `set nuevoPrecio(valor) { this.precio = valor; }` es un `public` disfrazado. La validación es lo que convierte un dato cualquiera en un dato confiable.
3. **`readonly`** marca lo que no cambia después de crear el objeto: el `id` de un producto, el código de un turno, la fecha de un movimiento. Es documentación que además te revisa el editor.

Un detalle de diseño que verás seguido en código real: los getters también pueden **calcular**. `valor` podría devolver `this.precio * (1 + 0.19)` y quien llama ni se entera: esa decisión queda dentro del modelo.

## 5. Composición: "tiene un" (la relación que más se usa)

Dos objetos casi siempre se relacionan **conteniendo** uno al otro, no heredando de él. Un carrito **tiene** productos. Una tarea **tiene** etiquetas. Un turno **tiene** una persona.

```ts
class Producto {
  constructor(
    readonly id: string,
    readonly nombre: string,
    private precio: number,
  ) {}

  get valor(): number {
    return this.precio;
  }
}

type Articulo = {
  producto: Producto;
  cantidad: number;
};

class Carrito {
  private readonly articulos: Articulo[] = [];

  agregar(producto: Producto, cantidad: number): void {
    if (cantidad <= 0) {
      throw new Error("La cantidad debe ser mayor que 0");
    }

    const existente = this.articulos.find((a) => a.producto.id === producto.id);

    if (existente) {
      existente.cantidad += cantidad;
      return;
    }

    this.articulos.push({ producto, cantidad });
  }

  quitar(producto: Producto): void {
    const indice = this.articulos.findIndex((a) => a.producto.id === producto.id);

    if (indice === -1) {
      throw new Error("Ese producto no está en el carrito");
    }

    this.articulos.splice(indice, 1);
  }

  total(): number {
    let suma = 0;

    for (const articulo of this.articulos) {
      suma += articulo.producto.valor * articulo.cantidad;
    }

    return suma;
  }

  cantidadDeArticulos(): number {
    return this.articulos.length;
  }
}

const cuaderno = new Producto("p-01", "Cuaderno", 12000);
const marcador = new Producto("p-02", "Marcador", 6000);

const carrito = new Carrito();
carrito.agregar(cuaderno, 2);
carrito.agregar(marcador, 1);
carrito.agregar(cuaderno, 1); // se suma al que ya estaba

console.log(carrito.total());               // 42000
console.log(carrito.cantidadDeArticulos()); // 2
```

Tres cosas que mirar aquí:

- `agregar` recibe un `Producto` **ya hecho** (lo crea quien llama) y lo guarda. Al que usa el carrito no le importa cómo se construyó el producto: por eso `total()` solo necesita `valor`.
- `total()` es una pregunta clara sobre el carrito, y su respuesta **no depende de la representación interna**. Mañana guardas los artículos en un `Map` en vez de en un arreglo y `total()` sigue igual: eso es la abstracción funcionando.
- `agregar(cuaderno, 1)` no crea un artículo nuevo: busca si ya existe y acumula. Esa decisión (qué pasa con los repetidos) está **dentro del carrito**, no en el código que llama. Esa es la diferencia entre un modelo útil y una lista de variables sueltas con más pasos.

## 6. Herencia: "¿es un?"

La herencia es la otra relación posible: en vez de decir "A **tiene** un B", se dice "A **es un** B". Un producto digital es un producto; un producto físico es un producto. Eso se escribe con `extends`.

```ts
class Producto {
  constructor(
    readonly id: string,
    readonly nombre: string,
    protected precio: number,
  ) {}

  descripcion(): string {
    return `${this.nombre}`;
  }

  get valor(): number {
    return this.precio;
  }
}

class ProductoDigital extends Producto {
  constructor(id: string, nombre: string, precio: number, readonly enlaceDescarga: string) {
    super(id, nombre, precio);
  }

  descripcion(): string {
    return `${this.nombre} (digital, descarga inmediata)`;
  }

  enviarDescargas(): string {
    return `Enlace enviado: ${this.enlaceDescarga}`;
  }
}

class ProductoFisico extends Producto {
  constructor(
    id: string,
    nombre: string,
    precio: number,
    readonly pesoGramos: number,
  ) {
    super(id, nombre, precio);
  }

  get envio(): number {
    return this.pesoGramos > 1000 ? 8000 : 3000;
  }
}

const guia = new ProductoDigital("p-10", "Guía de TypeScript", 9000, "https://ejemplo.com/guia");
const cuaderno = new ProductoFisico("p-01", "Cuaderno", 12000, 350);

console.log(guia.descripcion());  // Guía de TypeScript (digital, descarga inmediata)
console.log(cuaderno.descripcion()); // Cuaderno
console.log(cuaderno.envio);      // 3000
```

Dos cosas que se ven en el código y que debes poder explicar:

- **`super(...)`** llama al constructor de la clase padre. En TypeScript es obligatorio llamar a `super` antes de usar `this` en una subclase. Es la forma de decir "primero construye la parte que es un producto, y después agrega lo propio de digital".
- **`protected`** es como `private`, pero visible para las subclases. El precio no se puede tocar desde fuera del objeto, pero `ProductoDigital` sí puede leerlo para calcular su `valor`.

### La prueba del "es un"

Antes de heredar, hazte esta pregunta en voz alta:

> *¿Puedo decir, sin forzarlo, que el objeto A **es un** B?*

- "Un producto digital **es un** producto": sí, sin esfuerzo. La herencia aplica.
- "Un producto digital **tiene un** producto": suena absurdo. No heredamos.
- "Un carrito **es un** producto": no, es absurdo. Composition.
- "Una factura **es un** documento": aquí sí aplica la herencia.

Cuando la frase "es un" suena forzada, la relación correcta casi siempre es **composición**, no herencia.

### Por qué la composición es la relación más usada

| | Herencia ("es un") | Composición ("tiene un") |
|---|---|---|
| Relación | El hijo **reemplaza** al padre en casi cualquier contexto | El hijo **necesita** al padre para funcionar |
| Acoplamiento | Alto: si cambias el padre, cambias todos los hijos | Bajo: puedes cambiar el hijo sin tocar el padre |
| Pruebas | Hay que preparar un padre para probar un hijo | Puedes probarlos por separado |
| Reutilización | A veces se hereda solo por "no duplicar código" | Se reutiliza composing, no heredando |
| Regla práctica | Solo si el "es un" es natural | Por defecto, cuando no hay duda |

El error clásico es heredar por comodidad:

```ts
class Factura extends Producto {} // "una factura es un producto" → absurdo
```

Componer, en cambio, es la relación honesta: una factura **tiene** productos.

```ts
class Factura {
  constructor(
    readonly cliente: string,
    private readonly productos: { producto: Producto; cantidad: number }[],
  ) {}

  total(): number {
    let suma = 0;
    for (const articulo of this.productos) {
      suma += articulo.producto.valor * articulo.cantidad;
    }
    return suma;
  }
}
```

`Factura` no es un producto: es algo que **contiene** productos. Si la clase `Factura` hubiera que heredar de `Producto`, aparecerían preguntas absurdas: ¿de qué categoría es una factura? ¿Cuánto pesa? ¿Se puede agregar una factura a un carrito? Cada pregunta absurda es la señal de que la relación equivocada es la herencia.

## 7. Polimorfismo: mismo nombre, distinto comportamiento

El polimorfismo responde a una pregunta muy concreta: ¿qué pasa cuando dos cosas distintas responden a la **misma pregunta** de manera distinta?

En la tiendita, dos módulos de venta pueden pedir el pago de formas diferentes: un producto digital se manda por correo, uno físico se entrega en mano. Si el código que procesa la venta debe conocer los detalles de cada caso, se vuelve un `if` interminable. El polimorfismo deja que cada objeto responda por sí mismo.

```ts
interface MetodoDePago {
  readonly nombre: string;
  cobrar(monto: number): string;
}

class Tarjeta implements MetodoDePago {
  readonly nombre = "tarjeta";

  cobrar(monto: number): string {
    if (monto <= 0) {
      throw new Error("El monto a cobrar debe ser mayor que 0");
    }
    return `Cobro con tarjeta por $${monto}`;
  }
}

class Efectivo implements MetodoDePago {
  constructor(private readonly recibido: number) {}

  get nombre(): string {
    return "efectivo";
  }

  cobrar(monto: number): string {
    if (this.recibido < monto) {
      throw new Error("No alcanza el efectivo recibido");
    }
    return `Cobro en efectivo por $${monto}, cambio $${this.recibido - monto}`;
  }
}

class Transferencia implements MetodoDePago {
  readonly nombre = "transferencia";

  cobrar(monto: number): string {
    return `Transferencia por $${monto} en proceso`;
  }
}

function procesarPago(metodo: MetodoDePago, monto: number): string {
  return metodo.cobrar(monto);
}

const metodo = new Tarjeta();

console.log(procesarPago(metodo, 42000));            // Cobro con tarjeta por $42000
console.log(procesarPago(new Efectivo(50000), 42000)); // Cobro en efectivo por $42000, cambio $8000
console.log(procesarPago(new Transferencia(), 42000)); // Transferencia por $42000 en proceso
```

Lo interesante está en `procesarPago`: **no sabe** si recibe una tarjeta, efectivo o una transferencia. Solo pide `cobrar(monto)`. Cada clase implementa el contrato a su manera, y el código que llama funciona con cualquiera de ellas. Eso es polimorfismo, y es la razón de ser de las `interface`.

`implements` no crea una jerarquía: es una promesa de que la clase tendrá esos métodos. `interface MetodoDePago` podría implementarla una clase de otro lenguaje o un objeto creado con `implements`-like typing; en la práctica, sirve para que el editor te avise si te falta un método.

### La alternativa: un tipo unión y un `switch`

El mismo problema se puede resolver sin clases, y en TypeScript es una opción muy válida:

```ts
type MetodoDePago =
  | { tipo: "tarjeta"; numero: string }
  | { tipo: "efectivo"; recibido: number }
  | { tipo: "transferencia"; banco: string };

function procesarPago(metodo: MetodoDePago, monto: number): string {
  switch (metodo.tipo) {
    case "tarjeta":
      return `Cobro con tarjeta terminada en ${metodo.numero.slice(-4)} por $${monto}`;
    case "efectivo":
      if (metodo.recibido < monto) {
        throw new Error("No alcanza el efectivo recibido");
      }
      return `Cobro en efectivo por $${monto}, cambio $${metodo.recibido - monto}`;
    case "transferencia":
      return `Transferencia a ${metodo.banco} por $${monto} en proceso`;
  }
}
```

Los dos enfoques resuelven el mismo problema y ambos permiten agregar una forma de pago nueva sin tocar la función que procesa el cobro. La diferencia es de estilo, no de poder:

| Enfoque | Cuándo se ve cómodo | Costo |
|---|---|---|
| `interface` + clases | Cuando cada variante tiene comportamiento propio (varios métodos distintos) | Un archivo por clase, más ceremonia |
| Tipo unión + `switch` | Cuando las variantes son solo datos y el comportamiento se decide en un solo lugar | El `switch` crece si las variantes se multiplican |

Este es el mismo tipo de decisión que ya tomaste en el módulo 02 con `number | null`: **describir las formas que puede tomar un dato**. Acá la diferencia es que además hay comportamiento.

## 8. ¿Clase o funciones y tipos?

Sería deshonesto terminar esta guía sin decir algo que puede sorprender: **en TypeScript y en React no siempre se usan clases**, y a menudo es mejor no hacerlo. Esto no es una contradicción: la abstracción que aprendiste en la sección 2 se puede lograr con `type` y funciones.

La misma tiendita, sin una sola clase:

```ts
type Producto = {
  readonly id: string;
  readonly nombre: string;
  readonly precio: number;
};

type Carrito = {
  readonly articulos: { readonly producto: Producto; readonly cantidad: number }[];
};

function crearCarrito(): Carrito {
  return { articulos: [] };
}

function agregar(carrito: Carrito, producto: Producto, cantidad: number): Carrito {
  if (cantidad <= 0) {
    throw new Error("La cantidad debe ser mayor que 0");
  }

  const existe = carrito.articulos.some((a) => a.producto.id === producto.id);
  const articulos = existe
    ? carrito.articulos.map((a) =>
        a.producto.id === producto.id ? { producto, cantidad: a.cantidad + cantidad } : a,
      )
    : [...carrito.articulos, { producto, cantidad }];

  return { articulos };
}

function total(carrito: Carrito): number {
  let suma = 0;

  for (const articulo of carrito.articulos) {
    suma += articulo.producto.precio * articulo.cantidad;
  }

  return suma;
}

let carrito = crearCarrito();
carrito = agregar(carrito, { id: "p-01", nombre: "Cuaderno", precio: 12000 }, 2);

console.log(total(carrito)); // 24000
```

Fíjate en las diferencias y en lo que se mantiene:

- **Encapsulamiento por otra vía**: en vez de `private`, las funciones reciben los datos y devuelven datos nuevos. Los datos son inmutables (`readonly`), así que nadie los puede romper por accidente. Eso es el mismo objetivo de la sección 4, alcanzado con `readonly` en lugar de `private` y `set`.
- **Nada de `new`**: no hay construcción, solo funciones que transforman valores.
- **La abstracción sigue intacta**: el carrito sigue ocultando cómo guarda sus artículos, y `total()` sigue funcionando si mañana cambia la representación interna.

| Situación | Recomendación |
|---|---|
| Datos que se pasan, se guardan y se transforman | `type` + funciones puras. Es la forma por defecto en este curso |
| Muchas instancias que comparten un comportamiento complejo y mutable | Una clase con `private` y métodos |
| Entidades del dominio con reglas estrictas (un pago, un turno, una cuenta) | Clase si necesitas encapsulamiento real; `type` + función de fábrica si te basta |
| Un objeto que React va a guardar en `useState` | Modelo de datos (`type`) + funciones. React prefiere datos inmutables |
| Librerías y frameworks que ya te dan clases | Aprende a leerlas, aunque no escribas las tuyas |

Un detalle técnico que merece la pena: cuando TypeScript compila una clase con `private`, en el JavaScript final esas propiedades no son privadas de verdad; el `private` se revisa al compilar. Para encapsulamiento real en JavaScript existen los campos privados nativos, los que empiezan con `#`:

```ts
class Contador {
  #valor: number;

  constructor(valor: number) {
    this.#valor = valor;
  }

  get valor(): number {
    return this.#valor;
  }

  sumar(cantidad: number): void {
    this.#valor += cantidad;
  }
}

const contador = new Contador(0);
contador.sumar(5);
console.log(contador.valor); // 5
```

Con `#valor` nadie puede tocar el dato desde fuera: ni siquiera usando tricks. Es el nivel más estricto de encapsulamiento en JavaScript.

## 9. Ejemplo completo: de variables sueltas a un carrito compuesto

Ahora juntamos todo con un recorrido completo. Es la forma recomendada de entender un refactor: **de abajo hacia arriba**, un paso a la vez, comprobando que cada versión funciona.

### Paso 0: variables sueltas (funciona, pero no escala)

```ts
let nombreProducto = "Cuaderno";
let precioProducto = 12000;
let cantidadProducto = 2;
let nombreProducto2 = "Marcador";
let precioProducto2 = 6000;
let cantidadProducto2 = 1;

console.log(precioProducto * cantidadProducto + precioProducto2 * cantidadProducto2); // 30000
```

Funciona, y es honestamente el punto de partida correcto. Los nombres describen cosas (`precioProducto2` es el segundo producto). Pero no hay forma de responder "¿cuántos productos hay?" sin contar variables a mano.

### Paso 1: agrupar en objetos (abstracción de datos)

```ts
type Producto = {
  readonly id: string;
  readonly nombre: string;
  readonly precio: number;
};

type Carrito = {
  readonly productos: { producto: Producto; cantidad: number }[];
};

const carrito: Carrito = {
  productos: [
    { producto: { id: "p-01", nombre: "Cuaderno", precio: 12000 }, cantidad: 2 },
    { producto: { id: "p-02", nombre: "Marcador", precio: 6000 }, cantidad: 1 },
  ],
};

let suma = 0;
for (const articulo of carrito.productos) {
  suma += articulo.producto.precio * articulo.cantidad;
}

console.log(suma); // 30000
console.log(carrito.productos.length); // 2
```

Cada producto ya es una unidad coherente: los datos que lo describen viajan juntos. `length` responde la pregunta que antes no tenía respuesta. Y `readonly` ya evita que alguien modifique el carrito sin querer.

### Paso 2: poner las reglas dentro del modelo (comportamiento y encapsulamiento)

```ts
type Producto = {
  readonly id: string;
  readonly nombre: string;
  readonly precio: number;
};

type Carrito = {
  readonly productos: { readonly producto: Producto; readonly cantidad: number }[];
};

function crearProducto(id: string, nombre: string, precio: number): Producto {
  if (precio < 0) {
    throw new Error("El precio no puede ser negativo");
  }
  return { id, nombre, precio };
}

function agregarAlCarrito(
  carrito: Carrito,
  producto: Producto,
  cantidad: number,
): Carrito {
  if (cantidad <= 0) {
    throw new Error("La cantidad debe ser mayor que 0");
  }

  const existe = carrito.productos.some((a) => a.producto.id === producto.id);
  const productos = existe
    ? carrito.productos.map((a) =>
        a.producto.id === producto.id ? { producto, cantidad: a.cantidad + cantidad } : a,
      )
    : [...carrito.productos, { producto, cantidad }];

  return { productos };
}

function total(carrito: Carrito): number {
  let suma = 0;
  for (const articulo of carrito.productos) {
    suma += articulo.producto.precio * articulo.cantidad;
  }
  return suma;
}

const cuaderno = crearProducto("p-01", "Cuaderno", 12000);

let carrito: Carrito = { productos: [] };
carrito = agregarAlCarrito(carrito, cuaderno, 2);
carrito = agregarAlCarrito(carrito, crearProducto("p-02", "Marcador", 6000), 1);
carrito = agregarAlCarrito(carrito, cuaderno, 1);

console.log(carrito.productos.length); // 2
console.log(total(carrito));            // 42000
```

### Paso 3: si prefieres clases, el mismo modelo con `private`

```ts
class Producto {
  constructor(
    readonly id: string,
    readonly nombre: string,
    private precio: number,
  ) {
    if (precio < 0) {
      throw new Error("El precio no puede ser negativo");
    }
  }

  get valor(): number {
    return this.precio;
  }

  cambiarPrecio(valor: number): void {
    if (valor < 0) {
      throw new Error("El precio no puede ser negativo");
    }
    this.precio = valor;
  }
}

class Carrito {
  private readonly productos: { producto: Producto; cantidad: number }[] = [];

  agregar(producto: Producto, cantidad: number): void {
    if (cantidad <= 0) {
      throw new Error("La cantidad debe ser mayor que 0");
    }
    const existente = this.productos.find((a) => a.producto.id === producto.id);
    if (existente) {
      existente.cantidad += cantidad;
      return;
    }
    this.productos.push({ producto, cantidad });
  }

  total(): number {
    let suma = 0;
    for (const articulo of this.productos) {
      suma += articulo.producto.valor * articulo.cantidad;
    }
    return suma;
  }

  cantidadDeArticulos(): number {
    return this.productos.length;
  }
}

const carrito = new Carrito();
carrito.agregar(new Producto("p-01", "Cuaderno", 12000), 2);
carrito.agregar(new Producto("p-02", "Marcador", 6000), 1);

console.log(carrito.cantidadDeArticulos()); // 2
console.log(carrito.total());                // 30000
```

**Los tres pasos dan el mismo resultado en las preguntas que importan.** Esa es la lección del refactor: cambiar la representación interna no debería cambiar lo que el programa sabe responder. Y fíjate en la diferencia real entre el paso 2 y el 3: en el paso 2 cada función recibe el carrito y devuelve uno nuevo (ideal para React); en el paso 3 el carrito se modifica por dentro y el que llama no necesita enterarse. Son dos estilos con el mismo nivel de abstracción.

## 10. ¿Cómo pienso este problema?

Cuando te pidan "hazlo con objetos" o "modela esto", el camino es siempre el mismo. Son las preguntas que hacen buena abstracción, en orden:

1. **¿De qué entidades habla el problema?** No de qué variables escribí, sino de qué cosas existen: un producto, un carrito, un turno, una persona. Cada entidad es un candidato a objeto.
2. **¿Qué necesita saber el programa de cada entidad?** Solo lo que usarás. Ese es el filtro de la sección 2.2: lo que no está en la lista, no va en el modelo.
3. **¿Qué reglas no puede romper?** Las que lanzarían un error: precios negativos, cantidades en cero, nombres vacíos. Esas reglas van dentro del modelo, en el constructor o en el setter.
4. **¿Quién necesita cada dato?** Si solo lo usa el propio objeto, es `private`. Si lo necesita el que llama, necesita un getter. Si lo necesitan todos, es público.
5. **¿Composición o herencia?** Pruébalo con la frase: "¿A **es un** B?" Si suena forzado, componer.
6. **¿Repitió algo por tercera vez?** Ahí hay una entidad o una operación escondida. Sácala ahora.
7. **¿Clase o funciones?** Si el dato se guarda, se pasa y se transforma (caso típico en React), `type` + funciones puras. Si necesitas encapsulamiento real y muchas instancias, clase.

Si respondes esas siete preguntas, el modelo casi se escribe solo. Y si al escribirlas te sobra un campo, es señal de que estás abstrayendo de más.

## 11. Errores frecuentes

### Error 1: olvidar el `new`

```ts
const cuaderno = Producto("p-01", "Cuaderno", 12000, "papeleria"); // TypeError
```

**Error:** una clase es una plantilla, no una función común. Hay que instanciarla con `new`: `new Producto(...)`. Si olvidas el `new`, el error dice algo como "Class constructor cannot be invoked without 'new'".

### Error 2: llamar un método sin paréntesis

```ts
console.log(cuaderno.descripcion); // imprime la función, no el resultado
```

**Error:** `descripcion` es el método; `descripcion()` es la llamada. Es el mismo error de la [Guía 4](guia-listas.md) con `compra.push` sin `()`, y el síntoma es parecido: ves código en lugar de un resultado.

### Error 3: perder el `this`

```ts
const describir = cuaderno.descripcion;
describir(); // TypeError: this is undefined
```

**Error:** el método quedó desligado del objeto, y sin objeto no hay `this`. O se llama como método (`cuaderno.descripcion()`), o se pasa el objeto con `call`. Al mapear una función a un arreglo, siempre pon el objeto: `lista.map((p) => p.descripcion())` funciona; `lista.map(Producto.descripcion)` falla.

### Error 4: `private` que no protege nada

```ts
set nuevoPrecio(valor: number) {
  this.precio = valor; // setter sin validar
}
```

**Error:** parece encapsulamiento, pero es un `public` con pasos extra. Si el setter no valida ni transforma, la protección es de mentira. Y ojo: en JavaScript, un `private` de TypeScript desaparece al compilar; para protección real existen los campos `#privados`.

### Error 5: heredar por comodidad

```ts
class Factura extends Producto {} // "una factura es un producto" → absurdo
```

**Error:** se hereda para no repetir código, no porque la relación tenga sentido. Cuando aparecen preguntas absurdas ("¿de qué categoría es una factura?"), la relación correcta es composición. Componer cuesta unas líneas más y evita acoplar la subclase a la clase base.

### Error 6: la clase que lo hace todo

```ts
class Tienda {
  agregarProducto() {}
  calcularTotal() {}
  guardarEnDisco() {}
  enviarCorreo() {}
  mostrarEnPantalla() {}
}
```

**Error:** una clase con cinco responsabilidades es difícil de probar y de cambiar: para probar el total necesitas el disco y el correo. Es el mismo error que la función `hacerTodo` del módulo 02, y la regla es la misma: una responsabilidad por objeto. `Tienda` llama a `Carrito` para el total y a `Notificador` para el correo.

### Error 7: mutar datos que pertenecen a React

```tsx
// En React esto no dispara el redibujado: mutaste el estado
carrito.productos.push(nuevoProducto);

// Así funciona: creas una versión nueva
setCarrito({ productos: [...carrito.productos, nuevoProducto] });
```

**Error:** aunque un objeto tenga métodos, si lo guardas en el estado de React y lo mutas por dentro, React no se entera del cambio. La encapsulación ayuda (por eso los métodos de clase que mutan se usan con cuidado en React), pero la regla del curso sigue siendo: **crea una versión nueva, no mutes la anterior**. Volveremos a esto en el [módulo 03](../03-react-typescript/README.md).

## 12. Resolver antes de seguir

> Para cada ejercicio entrega: el código, una salida de ejemplo y una explicación de la estrategia. Si usas clases, la explicación debe incluir qué datos escondiste y con qué regla los protegiste.

### Nivel 0 - perderle el miedo

1. Escribe una clase `Tarea` con `titulo` y `completada`, y un método `completar()` que marque la tarea como completada. Instancia dos tareas y llama al método en una sola.
2. Escribe una clase `Contador` con un número privado, un getter `valor` y un método `incrementar()`. Muestra que desde fuera no puedes escribir `contador.numero`.

### Nivel 1 - aplicar lo esencial

3. Escribe una clase `Rectangulo` con `ancho` y `alto` privados, y métodos `area()` y `perimetro()`. Los constructores con ancho o alto en cero o negativos deben lanzar un error.
4. Agrega a `Rectangulo` un método `esCuadrado(): boolean` y compruébalo con tres instancias: un cuadrado, un rectángulo y un error.
5. Convierte la clase `Producto` de la sección 4 para que use `#precio` en lugar de `private precio`. ¿Qué diferencia notas al intentar escribir `producto.precio = 0` desde fuera?

### Nivel 2 - composición y contratos

6. Escribe una clase `Playlist` que **tenga** una lista de canciones (composición, no herencia) con métodos `agregar`, `eliminar` y `duracionTotal`. Cada canción es un objeto `{ titulo, segundos }`.
7. Escribe una clase `Cuentas` con un `Map` de saldo por usuario, y los métodos `depositar`, `retirar` y `saldo`. Investiga qué pasaría si en vez de un `Map` guardaras un arreglo de objetos, y qué ventaja tiene cada opción para preguntarle a un cliente.
8. Define una `interface` `Notificable` con un método `enviar(destinatario: string): string` e implementa tres clases: `Correo`, `MensajeTexto` y `NotificacionPush`. Después escribe una función que envíe las tres sin saber cuál es cuál. ¿Qué tendrías que cambiar si agregas una cuarta forma?

### Nivel 3 - profundización

9. Escribe una versión con clases y otra con `type` y funciones puras del mismo carrito de la sección 8. Compara: cuántas líneas, cuál es más fácil de probar, cuál se parece más a lo que hace falta en React.
10. Investiga qué es el patrón **Strategy** (un objeto que sabe hacer una cosa, como un método de pago) y reescribe el ejemplo de `MetodoDePago` sin herencia y sin `implements`, solo con funciones. ¿Cuál de los tres te parece más claro para explicar en voz alta?
11. Escribe tu propia abstracción para algo real: una biblioteca, un curso, un juego. Escribe primero la lista de "lo que un X real tiene" y luego "lo que **esta** aplicación necesita", y justifica cada campo que decidiste dejar fuera.

## 13. Vocabulario

| Término | Definición corta |
| --- | --- |
| Abstracción | Modelar solo lo que importa para el propósito del programa, dejando fuera el resto. |
| Modelo | La representación de una entidad (sus datos y su comportamiento) tal como la entiende el programa. |
| Clase | Plantilla que describe datos y operaciones; se convierte en objetos al instanciarla. |
| Instancia / objeto | Un objeto concreto creado a partir de una clase con `new`. |
| `this` | Referencia al objeto que está ejecutando el método. |
| Método | Función que pertenece a un objeto y usa sus datos. |
| Encapsulamiento | Exponer el dato mediante lecturas y cambios controlados, en vez de dejarlo abierto. |
| `private` / `protected` / `public` | Alcance de una propiedad: solo la clase, la clase y sus subclases, o cualquiera. |
| `readonly` | Propiedad que se puede leer pero no reasignar. |
| Getter / setter | Forma controlada de leer o escribir una propiedad. |
| Constructor | Código que se ejecuta al crear un objeto con `new`. |
| Composición | Relación "tiene un": un objeto contiene a otro(s). Es la relación más usada. |
| Herencia (`extends`) | Relación "es un": una clase reutiliza y especializa a otra. |
| `super` | Llamada al constructor de la clase padre desde una subclase. |
| Polimorfismo | Varias clases que responden a la misma llamada (mismo contrato) de forma distinta. |
| `interface` / `implements` | Contrato que define qué operaciones debe tener una clase, sin obligar a heredar. |
| Cohesión | Que los datos de un objeto pertenezcan a la misma entidad. |
| Bajo acoplamiento | Que cambiar una parte del programa obligue a cambiar pocas otras. |

## 14. Chuleta rápida

```ts
// Clase con constructor, propiedades privadas y método
class Producto {
  readonly id: string;
  private precio: number;

  constructor(id: string, readonly nombre: string, precio: number) {
    if (precio < 0) throw new Error("El precio no puede ser negativo");
    this.id = id;
    this.precio = precio;
  }

  get valor(): number {
    return this.precio;
  }

  cambiarPrecio(valor: number): void {
    if (valor < 0) throw new Error("El precio no puede ser negativo");
    this.precio = valor;
  }
}

const cuaderno = new Producto("p-01", "Cuaderno", 12000);
console.log(cuaderno.valor); // 12000

// Composición: "tiene un"
class Carrito {
  private readonly productos: { producto: Producto; cantidad: number }[] = [];

  agregar(producto: Producto, cantidad: number): void {
    this.productos.push({ producto, cantidad });
  }

  total(): number {
    return this.productos.reduce(
      (suma, a) => suma + a.producto.valor * a.cantidad,
      0,
    );
  }
}

// Herencia: "es un" (solo si la frase suena natural)
class ProductoDigital extends Producto {
  constructor(id: string, nombre: string, precio: number, readonly enlace: string) {
    super(id, nombre, precio);
  }
}

// Polimorfismo: contrato + implements
interface MetodoDePago {
  readonly nombre: string;
  cobrar(monto: number): string;
}

class Tarjeta implements MetodoDePago {
  readonly nombre = "tarjeta";
  cobrar(monto: number): string {
    return `Cobro con tarjeta por $${monto}`;
  }
}

// Alternativa sin clases: tipo unión + switch
type Canal = "correo" | "sms";

function avisar(canal: Canal, mensaje: string): string {
  return canal === "correo" ? `Correo: ${mensaje}` : `SMS: ${mensaje}`;
}

// Abstracción: el modelo y su contrato
type ProductoTO = {
  id: string;
  nombre: string;
  precio: number;
  categoria: "papeleria" | "comida" | "tecnologia";
  disponible: boolean;
};
```

## 15. Herramientas para profundizar

- **MDN Web Docs, "Using classes in JavaScript"** (`developer.mozilla.org`): la referencia oficial sobre clases, `this`, herencia y campos privados `#`. Está en español y es la fuente más confiable para detalles del lenguaje.
- **TypeScript Handbook, sección "Object Types" y "Classes"**: explica cómo los tipos se combinan con las clases y qué cambia en el JavaScript que se genera. Los capítulos "Handbook / Classes" son cortos y muy claros.
- **TypeScript Playground** (`typescriptlang.org/play`): puedes pegar el código de esta guía y ver el error exacto cuando algo no compila, sin instalar nada. Útil para entender qué garantiza `private` y qué no.
- **El código del curso**: la calculadora, el catálogo y la tiendita son el laboratorio real. Cada vez que modeles una entidad ahí, pregúntate lo mismo que en la sección 2: ¿esto es lo que la aplicación necesita, o lo que el objeto real tiene?

## 16. Lo que viene después

Esta guía te dio el vocabulario de la programación orientada a objetos (clase, instancia, encapsulamiento, composición, herencia, polimorfismo) y, sobre todo, la idea que las sostiene: **abstraer es elegir qué merece existir en el programa**. Con eso entiendes el código cuando crece hacia un proyecto real.

Lo que sigue, en el [módulo 03](../03-react-typescript/README.md), es la otra mitad de la historia: modelar entidades con `type` e `interface` (el camino que este curso prefiere), elegir estructuras de datos como pila, cola, matriz o diccionario según el comportamiento que necesites, y mostrar todo eso en pantalla con React. Después viene el [taller de modelado y React: Fila creativa](../03-react-typescript/taller-modelado-react.md), donde modelas turnos como objetos y los organizas en una cola FIFO.

Antes de seguir, comprueba que puedes responder estas cuatro preguntas sin mirar el código:

1. ¿Qué es abstraer y por qué no es lo mismo que "copiarlo todo"?
2. ¿Qué diferencia hay entre `private precio` y `#precio`?
3. ¿Por qué se dice que la composición es la relación que más se usa?
4. ¿Cuándo conviene un tipo unión en vez de una clase?

Si las cuatro salen con tus propias palabras, la guía hizo su trabajo. Si alguna no, vuelve a la sección 2 (abstracción) o a la 6 (herencia vs. composición) y vuelve a intentarlo.

