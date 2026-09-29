# Guion docente: explicación de la clase 3

## Idea central

Primero decidimos qué necesita saber el programa. Una clase puede reunir datos y operaciones que pertenecen a una misma entidad, y puede proteger reglas que no deben romperse. La clase es una herramienta; abstraer correctamente es la decisión principal.

```text
propósito → datos pertinentes → modelo → instancias → operaciones y reglas
```

## Guion de explicación

### Abrir con una situación

Puedes decir:

> Supongamos que queremos mostrar productos de una tienda escolar. Un producto real puede tener proveedor, peso, código de barras, precio y muchas cosas más. ¿Cuáles necesita guardar esta aplicación para mostrar un catálogo? Primero fijemos el propósito; después elegimos los datos.

Anota las propuestas del grupo y pregunta qué campo hace falta para una operación o decisión concreta. Recalca que lo que se omite no es falso: no se necesita para el propósito actual.

### De variables a un objeto

```ts
const nombreProducto = "Cuaderno";
const precioProducto = 12000;
const disponibleProducto = true;
```

Pregunta cómo se repetirían esos nombres para diez productos. Luego agrupa un producto en un objeto:

```ts
type ProductoDatos = {
  nombre: string;
  precio: number;
  disponible: boolean;
};

const cuaderno: ProductoDatos = {
  nombre: "Cuaderno",
  precio: 12000,
  disponible: true,
};
```

Aclara: el `type` describe una forma; `cuaderno` es un valor objeto que tiene esa forma. Esta solución es válida. No necesitamos una clase solo para juntar campos.

### Del objeto a la clase

Pregunta qué pasaría si varios productos pudieran aplicar descuentos y tuviéramos que repetir la misma validación. Presenta:

```ts
class Producto {
  private precio: number;

  constructor(
    public nombre: string,
    precioInicial: number,
  ) {
    if (precioInicial < 0) {
      throw new Error("El precio no puede ser negativo");
    }

    this.precio = precioInicial;
  }

  aplicarDescuento(porcentaje: number): void {
    if (porcentaje < 0 || porcentaje > 100) {
      throw new Error("El descuento debe estar entre 0 y 100");
    }

    this.precio *= 1 - porcentaje / 100;
  }

  precioActual(): number {
    return this.precio;
  }
}

const cuaderno = new Producto("Cuaderno", 12000);
const marcador = new Producto("Marcador", 6000);

cuaderno.aplicarDescuento(10);
console.log(cuaderno.precioActual()); // 10800
console.log(marcador.precioActual()); // 6000
```

Desglosa las palabras en contexto:

- `class Producto`: una definición que describe qué datos y operaciones tendrán sus instancias.
- `new Producto(...)`: crea un objeto nuevo siguiendo esa definición.
- `constructor`: inicializa la instancia.
- `this`: la instancia actual; cada objeto tiene su propio `precio` y `nombre`.
- `private`: limita el acceso directo al precio desde fuera de la clase.
- método: operación disponible en cada instancia.

`public nombre: string` es una forma corta de declarar e inicializar una propiedad desde el constructor. Si confunde al grupo, escribe la propiedad y la asignación `this.nombre = nombre` de forma explícita.

### Encapsulamiento y regla

Pregunta qué podría salir mal si cualquier parte del programa pudiese escribir `precio = -50`. Mostrar que la clase controla el cambio mediante una operación validada. No prometas que `private` valida datos externos por sí solo: la regla funciona porque el constructor y el método ejecutan la comprobación.

### Clase o tipo

Compara dos situaciones:

- Un catálogo de React que solo necesita mostrar productos: `type Producto` más funciones como `filtrarProductos` puede ser suficiente.
- Un objeto con estado y reglas que deben conservarse juntos: una clase puede ofrecer operaciones controladas.

Di:

> No vamos a elegir la forma que tenga más sintaxis. Vamos a elegir la que deje más clara la regla y resulte más fácil de usar y probar.

### Composición e interfaces

Presenta sin profundizar demasiado:

- **Composición, “tiene un”:** un carrito tiene artículos; una orden tiene una dirección de entrega.
- **Herencia, “es un”:** una clase `Gato` podría ser un `Animal`, si de verdad comparte las reglas pertinentes.
- **Interface:** define un contrato de operaciones, por ejemplo `Describible` con el método `descripcion()`.

Sugiere empezar preguntando “¿tiene un?” antes de crear una herencia. La guía del estudiante desarrolla estos conceptos y también explica cuándo no hace falta una clase.

## Preguntas para comprobar comprensión

1. ¿Qué campos necesita el catálogo y qué propósito justifica cada uno?
2. ¿Cuál es la diferencia entre `Producto` y `cuaderno`?
3. ¿A qué objeto se refiere `this` cuando llamamos `cuaderno.aplicarDescuento(10)`?
4. ¿Qué ocurriría si intentamos aplicar un descuento de 150 %?
5. ¿Cuándo usarías `type` y funciones en lugar de una clase?
6. ¿Un carrito “es un” producto o “tiene” productos?

## Errores de explicación que conviene evitar

- No presentar clase y objeto como sinónimos.
- No decir que todo dato relacionado tiene que ser una clase.
- No definir `this` como “la clase”; es la instancia actual.
- No confundir `private` con validación automática en ejecución.
- No presentar herencia como solución por defecto.

## Puente a la siguiente clase

> Ya modelamos datos y operaciones. En la próxima clase veremos cómo se estructura una página web con HTML y cómo se presenta con CSS. Después conectaremos esa estructura con React, usando los modelos en interfaces interactivas.
