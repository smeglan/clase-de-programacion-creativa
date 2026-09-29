# Clase 3: apoyos y extensiones

## Apoyo puntual

### Empezar con dos instancias

```ts
class Turno {
  constructor(
    public nombre: string,
    public motivo: string,
  ) {}

  descripcion(): string {
    return `${this.nombre}: ${this.motivo}`;
  }
}

const uno = new Turno("Lina", "Pregunta sobre React");
const dos = new Turno("Tomás", "Error de instalación");

console.log(uno.descripcion());
console.log(dos.descripcion());
```

Dibujar una caja por cada instancia y escribir en ella sus propios valores. El método usa `this` para leer la caja que lo recibió.

### Criterio de logro del apoyo

El estudiante crea dos instancias, predice la salida de `descripcion()` y explica qué operación ofrece la clase.

## Extensión opcional

1. Añadir un precio privado a `Producto` y una operación validada para cambiarlo.
2. Modelar `CarritoItem` como una composición de un producto y una cantidad.
3. Escribir una interfaz `Describible` y hacer que dos clases implementen el contrato.
4. Implementar el mismo ejemplo con un `type` y funciones, y comparar claridad y costo.

### Criterio de logro de la extensión

La solución funciona con varios casos y el estudiante explica qué problema resuelve y qué alternativa descartó.
