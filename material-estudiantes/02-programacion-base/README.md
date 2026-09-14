# 2. Fundamentos de TypeScript y lógica de programación

Este módulo enseña las herramientas mínimas para leer, escribir y explicar programas pequeños. La meta no es repetir sintaxis: es poder transformar un problema sencillo en pasos claros, código verificable y pruebas propias.

Los objetos, el modelado de datos, las estructuras como pila o cola, y su conexión con React se trabajan en el [módulo 03](../03-react-typescript/README.md). Aquí construimos las bases que los hacen comprensibles.

## TypeScript: JavaScript con contratos

TypeScript se escribe de forma muy parecida a JavaScript, pero permite declarar qué tipo de dato esperamos. Eso ayuda a detectar errores antes de ejecutar el programa y hace que las funciones sean más fáciles de entender.

```ts
const nombre: string = "Ada";
const edad: number = 20;
const inscrito: boolean = true;
```

TypeScript desaparece al construir la aplicación: el navegador ejecuta JavaScript. Los tipos son una ayuda para quien escribe, lee y mantiene el código.

## Variables: recordar información con nombres claros

Una variable guarda un valor que el programa necesita usar.

```ts
const nombreCurso = "Programación Creativa";
let puntos = 0;
```

- Usa `const` cuando no vas a reasignar la variable.
- Usa `let` cuando el valor debe cambiar.
- Evita `var`; pertenece a una forma antigua de declarar variables y puede causar confusiones de alcance.

El nombre debe expresar la intención. `totalCarrito` comunica más que `x`.

## Tipos básicos

| Tipo | Representa | Ejemplo |
|---|---|---|
| `string` | texto | `"Hola"` |
| `number` | números enteros o decimales | `12`, `3.5` |
| `boolean` | verdadero o falso | `true` |
| `undefined` | ausencia de valor asignado | `undefined` |
| `null` | ausencia intencional de valor | `null` |

TypeScript puede inferir muchos tipos:

```ts
const mensaje = "Hola"; // TypeScript infiere string
const limite = 10; // TypeScript infiere number
```

Declara el tipo explícitamente cuando aclara un contrato, en especial en parámetros, retornos, datos que llegan de fuera o valores que pueden adoptar varias formas.

```ts
let resultado: number | null = null;
```

Esto significa: `resultado` tendrá un número o todavía no tendrá un resultado.

## Operadores: transformar y comparar valores

```ts
const suma = 3 + 4;
const esMayor = suma > 5;
const tieneAcceso = esMayor && true;
```

- Aritméticos: `+`, `-`, `*`, `/`, `%`.
- Comparación: `>`, `<`, `>=`, `<=`, `===`, `!==`.
- Lógicos: `&&` (y), `||` (o), `!` (no).

Usa `===` y `!==` para comparar valores. Evita `==`, porque convierte tipos de forma implícita y puede producir resultados inesperados.

## Condiciones: elegir entre caminos

Una condición sirve cuando el programa debe responder de manera distinta según un estado.

```ts
function mensajeDeAcceso(edad: number): string {
  if (edad >= 18) {
    return "Puede entrar";
  }

  return "Debe esperar";
}
```

Antes de escribir un `if`, formula la pregunta en lenguaje natural: “¿la edad es al menos 18?”. Si no puedes formularla, probablemente todavía no tienes clara la regla.

### Condiciones combinadas

```ts
function puedeComprar(disponible: boolean, dinero: number, precio: number): boolean {
  return disponible && dinero >= precio;
}
```

Una buena condición es corta, expresa una regla y puede probarse con casos normales, límite e inválidos.

## Ciclos: repetir con una razón

Un ciclo visita o repite algo. Antes de usarlo, define:

1. qué se repite;
2. cuándo termina;
3. qué cambia en cada repetición;
4. qué resultado se espera.

```ts
function tablaDel(numero: number): string[] {
  const lineas: string[] = [];

  for (let multiplicador = 1; multiplicador <= 10; multiplicador++) {
    lineas.push(`${numero} × ${multiplicador} = ${numero * multiplicador}`);
  }

  return lineas;
}
```

### Ciclos más usados

```ts
for (let indice = 0; indice < 3; indice++) {
  console.log(indice);
}

for (const palabra of ["react", "typescript", "vite"]) {
  console.log(palabra);
}
```

`for...of` es útil cuando importa el valor. El `for` con índice es útil cuando también importa la posición. `while` se reserva para situaciones donde no conocemos de antemano cuántas repeticiones habrá; hay que cuidar que su condición cambie para no crear un ciclo infinito.

## Funciones: nombrar una solución reutilizable

Una función recibe datos, realiza una tarea concreta y devuelve un resultado. Es una forma de explicar el programa en piezas.

```ts
function convertirCelsiusAFahrenheit(celsius: number): number {
  return (celsius * 9) / 5 + 32;
}
```

En este ejemplo:

- `convertirCelsiusAFahrenheit` dice qué hace;
- `celsius: number` describe la entrada;
- `: number` describe la salida;
- `return` entrega el resultado.

### Una función, una responsabilidad

Prefiere funciones pequeñas y con nombres directos:

```ts
function esPar(numero: number): boolean {
  return numero % 2 === 0;
}

function etiquetaParidad(numero: number): string {
  return esPar(numero) ? "par" : "impar";
}
```

Evita una función llamada `hacerTodo`, que lee datos, calcula valores, modifica una interfaz y además publica resultados. Cuando una función tiene demasiadas responsabilidades, es difícil probarla y corregirla.

## Arreglos básicos: repetir valores del mismo tipo

Un arreglo permite guardar una secuencia de valores. Por ahora lo usaremos para practicar recorridos, acumulación y transformación simple. El modelado profundo de arreglos de objetos y las estructuras de datos aparece en el módulo 03.

```ts
const precios: number[] = [10, 20, 30];
const etiquetas: string[] = ["nuevo", "oferta"];
```

```ts
function sumarLista(numeros: number[]): number {
  let total = 0;

  for (const numero of numeros) {
    total += numero;
  }

  return total;
}
```

## Probar antes de confiar

Cada reto debe tener al menos tres pruebas:

```ts
console.log(convertirCelsiusAFahrenheit(0)); // 32
console.log(convertirCelsiusAFahrenheit(100)); // 212
console.log(convertirCelsiusAFahrenheit(-40)); // -40
```

- Caso normal: lo que esperamos que ocurra normalmente.
- Caso límite: valor mínimo, máximo o borde de una regla.
- Caso inválido: dato que no debería aceptarse o que requiere un mensaje claro.

## Trucos y buenas prácticas de supervivencia

Estas reglas no reemplazan pensar, pero evitan muchos errores antes de que aparezcan.

### 1. Usa cláusulas de guarda para evitar el infierno de `if`

Una cláusula de guarda resuelve primero los casos que no pueden continuar. Así el caso principal queda al final, con menos sangría y más claridad.

```ts
function calcularPrecioFinal(precio: number, descuento: number): number {
  if (precio < 0) {
    throw new Error("El precio no puede ser negativo");
  }

  if (descuento < 0 || descuento > 100) {
    throw new Error("El descuento debe estar entre 0 y 100");
  }

  return precio * (1 - descuento / 100);
}
```

Evita este patrón cuando solo añade niveles de anidación:

```ts
if (precio >= 0) {
  if (descuento >= 0 && descuento <= 100) {
    return precio * (1 - descuento / 100);
  }
}
```

La regla práctica es: si un caso invalida la operación, sácalo primero con `return` o `throw`.

### 2. Pon nombre a las condiciones importantes

Una condición larga es más fácil de leer y probar cuando tiene nombre.

```ts
const tieneSaldoSuficiente = dinero >= precio;
const productoEstaDisponible = disponible && existencias > 0;
const puedeComprar = tieneSaldoSuficiente && productoEstaDisponible;

if (!puedeComprar) {
  return "No se puede completar la compra";
}
```

El nombre debe responder una pregunta. Si cuesta nombrar la condición, puede que la regla de negocio todavía no esté clara.

### 3. Antes de escribir un `while`, escribe su salida

Un `while` necesita una condición que pueda dejar de cumplirse. Antes de programarlo, responde:

1. ¿Cuál es la condición inicial?
2. ¿Qué variable cambia en cada vuelta?
3. ¿Qué valor hace que el ciclo termine?
4. ¿Hay un límite de seguridad?

```ts
let intentos = 0;
const maximoIntentos = 3;

while (intentos < maximoIntentos) {
  intentos += 1;
  console.log(`Intento ${intentos}`);
}
```

El error clásico es olvidar cambiar `intentos`. Entonces la condición nunca cambia y el ciclo puede ejecutarse para siempre.

### 4. Prefiere `for` cuando conoces la cantidad de repeticiones

```ts
for (let intento = 1; intento <= 3; intento++) {
  console.log(`Intento ${intento}`);
}
```

Usa `for` o `for...of` si ya sabes cuántos elementos o intentos debes recorrer. Reserva `while` para situaciones donde la cantidad depende de una condición que cambia durante el proceso, como leer hasta encontrar una respuesta válida.

### 5. No uses `while (true)` sin una salida visible

Puede ser válido en casos puntuales, pero para fundamentos suele ocultar la condición real de finalización.

```ts
// Menos claro para quien está aprendiendo
while (true) {
  if (energia <= 0) break;
  energia -= 1;
}

// La condición de salida es visible desde el inicio
while (energia > 0) {
  energia -= 1;
}
```

### 6. No modifiques una lista mientras la recorres sin un plan

Eliminar elementos dentro de un ciclo puede saltarse valores o cambiar índices inesperadamente. Para filtrar, crea una nueva lista.

```ts
const numeros = [1, 2, 3, 4, 5];
const impares = numeros.filter((numero) => numero % 2 !== 0);
```

Más adelante, en React, esta práctica será todavía más importante: el estado debe actualizarse creando una nueva versión, no mutando la anterior.

### 7. Evita `any`: si no sabes el tipo, dilo con honestidad

`any` desactiva gran parte de la ayuda de TypeScript. Prefiere un tipo específico, una unión, `unknown` o un modelo que puedas validar.

```ts
function mostrarMensaje(valor: string | number): string {
  return String(valor);
}
```

Usa `unknown` para datos externos que todavía no has comprobado. Antes de usarlos, valida qué contienen.

### 8. Evita números y textos mágicos

Un valor importante merece un nombre.

```ts
const IVA = 0.19;
const LIMITE_DE_INTENTOS = 3;

const totalConIva = subtotal * (1 + IVA);
```

Así puedes cambiar una regla en un solo lugar y el código explica por qué existe ese valor.

### 9. Una función pequeña se prueba mejor

Si una función calcula, muestra alertas, modifica datos y además hace peticiones, divídela. Las funciones con una responsabilidad son más fáciles de entender, reutilizar y depurar.

### 10. Lee el error completo antes de cambiar código

Cuando aparezca un error:

1. lee la primera línea y localiza el archivo y la línea;
2. identifica qué valor esperaba el programa y qué recibió;
3. reduce el problema a un ejemplo pequeño;
4. prueba una corrección;
5. escribe qué aprendiste en la bitácora.

No arregles un error pegando una respuesta de IA sin verificarla. Comprueba siempre que la solución cambie la causa y no solo esconda el síntoma.

### Lista de comprobación antes de entregar un reto

- [ ] Los nombres de variables y funciones expresan su intención.
- [ ] Los casos inválidos se revisan primero con cláusulas de guarda cuando corresponde.
- [ ] Cada `while` cambia una variable que puede hacerlo terminar.
- [ ] Probé un caso normal, uno límite y uno inválido.
- [ ] No usé `any` para silenciar un error.
- [ ] No dejé valores mágicos sin nombre.
- [ ] Puedo explicar el código sin leerlo línea por línea.

## Recorrido del taller

El [Taller de fundamentos](taller-fundamentos.md) sigue este orden:

| Nivel | Qué afianza | Retos |
|---|---|---|
| 0 | variables, tipos y condiciones simples | 0-3 |
| 1 | ciclos, acumuladores y decisiones combinadas | 4-8 |
| 2 | funciones, arreglos y lógica de negocio | 9-13 |
| siguiente módulo | abstracción, objetos y estructura FIFO | taller del módulo 03: proyecto Fila creativa |
| siguiente módulo | convertir modelo y lógica en interfaz React | taller del módulo 03: proyecto Fila creativa |

## Siguiente paso

Cuando puedas explicar y resolver los retos 0-13, continúa con [Modelado de datos, estructuras y React](../03-react-typescript/README.md). Allí aprenderás a representar entidades con objetos, elegir estructuras como pila o cola y mostrar esos datos en una interfaz.
