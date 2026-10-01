# 2. Fundamentos de TypeScript y lógica de programación

**Estás aquí:** [Inicio](../README.md) › **2. Fundamentos de TypeScript y lógica de programación**

> **¿Te se frenó una palabra?** El [glosario del curso](../glosario.md#capitulo-3) explica variables, funciones, ciclos y errores, una por una y con ejemplos.

Este módulo enseña las herramientas mínimas para leer, escribir y explicar programas pequeños. La meta no es repetir [sintaxis](../glosario.md#sintaxis): es poder transformar un problema sencillo en pasos claros, [código](../glosario.md#codigo) verificable y pruebas propias.

Los objetos, el modelado de datos, las estructuras como pila o cola, y su conexión con [React](../glosario.md#react) se trabajan en el [módulo 03](../03-react-typescript/README.md). Aquí construimos las bases que los hacen comprensibles, y en la [Guía 7](#guía-7-poo-clases-y-abstracción) podrás ver el camino intermedio: pasar de variables sueltas a un [modelo](../glosario.md#modelo) con clases y [abstracción](../glosario.md#abstraccion).

## Tu primer programa: hola mundo y qué es `console.log`

Todo curso de programación empieza con un "[hola mundo](../glosario.md#hola-mundo)", y no es superstición: es la forma más barata de comprobar que tu editor, tu [terminal](../glosario.md#terminal) y tu entorno se están hablando. Si lograste ver esas palabras, ya hay un [programa](../glosario.md#programa) corriendo en tu máquina.

En el [módulo 01](../01-herramientas/README.md#tu-primer-programa-con-node) lo escribiste y lo ejecutaste. Aquí lo desarmamos, porque esa línea contiene casi todos los conceptos que vas a ver en el módulo.

### El programa más pequeño que funciona

```js
console.log("Hola, mundo");
```

Y en la terminal:

```
Hola, mundo
```

Eso es un programa completo. No necesita estructura de control, no necesita variables, no necesita imports. Un [archivo](../glosario.md#archivo), una [instrucción](../glosario.md#instruccion), un resultado.

### Las tres piezas de `console.log`

```js
console.log("Hola, mundo");
```

En una sola línea hay tres cosas escondidas:

- **`console`** es un [objeto](../glosario.md#objeto) que ya existe: no lo creaste tú, te lo dio el entorno donde corre el programa (Node o el navegador). Cuando en la [Guía 7](#guía-7-poo-clases-y-abstracción) modelemos nuestros propios objetos, esta estructura te va a sonar familiar.
- **`log`** es un [método](../glosario.md#metodo) de ese objeto. El punto significa *"llama a este método de este objeto"*, y por eso existen `console.log`, `console.error` y `console.warn`: el mismo objeto, distintas acciones.
- **`(...)`** lleva lo que quieres mostrar. Puede ser un [valor](../glosario.md#valor) o varios separados por comas.

El orden importa cuando el mismo nombre cumple dos papeles: `console.log` empieza por un objeto, y `log(...)` es una llamada a un método. Por eso más adelante, cuando tengas tus propios objetos, `producto.descripcion()` se leerá igual: objeto, punto, método.

### Textos, números y decisiones

Lo primero que se confunde al empezar es qué lleva comillas y qué no:

```js
console.log("Hola");          // texto: comillas
console.log(42);              // número: sin comillas
console.log(true);            // decisión: true o false
console.log(2 + 3);           // una operación, no el texto "2 + 3"
console.log("2 + 3");         // el texto literal 2 + 3
```

`console.log("2 + 3")` y `console.log(2 + 3)` se parecen en la pantalla, pero no son lo mismo: el primero muestra cinco caracteres escritos a mano; el segundo calcula una suma y muestra el número. La sección [Tipos básicos](#tipos-básicos) de este módulo (números, textos y booleanos) le da nombre formal a esta diferencia.

También puedes imprimir el valor de una [variable](../glosario.md#variable). Y aquí está la trampa clásica:

```js
const edad = 20;

console.log(edad);        // 20        (el valor de la variable)
console.log("edad");      // edad      (el texto "edad")
```

### El error que vas a cometer hoy

```js
console.log(Hola);   // ReferenceError: Hola is not defined
```

Sin comillas, `Hola` no es un texto: [JavaScript](../glosario.md#javascript) lo interpreta como el **nombre de una variable**, busca esa variable, no la encuentra y te dice que no está definida. La solución casi siempre es la misma: ¿querías mostrar un texto? Entonces, comillas.

En la terminal verás el [error](../glosario.md#error) completo, con la línea donde ocurrió. No es un castigo: es el programa diciéndote exactamente qué no encontró. Por eso leer errores es parte del trabajo, y no un extra.

### Cómo se ejecuta lo que escribes

Dos formas, y las dos valen:

```bash
node hola.js          # 1. En un archivo, que es lo que usamos siempre

node                  # 2. En la terminal directa, sin guardar nada
# > "Hola, mundo"  y presionas Enter
```

Para tus prácticas de este módulo, guarda siempre el archivo: los programas de la [Guía 1](guia-if.md) y siguientes se ejecutan así, y sus comentarios `// salida esperada: ...` se comparan con lo que imprime tu [consola](../glosario.md#consola).

### Ejercicio

1. Crea `hola.js` con tu nombre, tu ciudad y una frase que te defina.
2. Ejecuta el archivo y comprueba que la terminal muestra las tres líneas.
3. Ahora quita las comillas de una de ellas y ejecuta otra vez. Lee el error completo en voz alta: ¿qué le estás pidiendo a JavaScript?
4. Agrega una línea que imprima la cantidad de caracteres de tu nombre usando `.length`, sin buscar nada: es tu turno de adivinar.
5. Usa un **argumento**: `console.log("Hola", "mundo", "!", 42)`. ¿Cuántos espacios aparecen entre los valores? ¿Por qué?

### Y ahora, a lo que vinimos

Un `console.log` no es un simple trámite: es un **punto de observación** sobre el programa. Es exactamente lo que harás durante todo el módulo 02:

- para ver qué valor tiene una variable en cada punto del código;
- para comprobar una decisión (`if`, `for`, `while`) por dónde entró el programa;
- para comparar tu resultado con el esperado;
- y, más adelante, para entender qué hace un objeto cuando todavía no hay interfaz.

Si puedes escribir "hola mundo" y explicar qué hace cada símbolo, ya tienes lo necesario para empezar.

## Guías de JavaScript: lógica antes de los tipos

Estas guías complementan el módulo con la lógica de programación en **JavaScript puro** (condiciones, ciclos y listas), sin funciones ni tipos todavía. Funcionan como base: el estudiante lee, copia, modifica y ejecuta ejemplos, y resuelve ejercicios por dificultad. Se ejecutan con Node (`node archivo.js`) o en la [consola del navegador](../glosario.md#consola-del-navegador). Para crear y guardar esos archivos de práctica se usa el [editor de texto](../glosario.md#editor); si todavía no conoces VS Code, repasa la [guía del editor](../01-herramientas/editor-vscode.md) del módulo 01.

- [Guía 1: Tomar decisiones con `if`](guia-if.md)
- [Guía 2: Repetir con `for`](guia-for.md)
- [Guía 3: Repetir con `while`](guia-while.md)
- [Guía 4: Listas (arrays)](guia-listas.md)

Después de las guías, este README presenta la misma lógica con tipos de [TypeScript](../glosario.md#typescript), y el [Taller de fundamentos](taller-fundamentos.md) la lleva a retos prácticos.

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
- Evita `var`; pertenece a una forma antigua de declarar variables y puede causar confusiones de [alcance](../glosario.md#alcance-del-proyecto).

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

Una [condición](../glosario.md#condicion) sirve cuando el programa debe responder de manera distinta según un [estado](../glosario.md#estado).

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

Un [ciclo](../glosario.md#ciclo) visita o repite algo. Antes de usarlo, define:

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

`for...of` es útil cuando importa el valor. El `for` con [índice](../glosario.md#indice) es útil cuando también importa la posición. `while` se reserva para situaciones donde no conocemos de antemano cuántas repeticiones habrá; hay que cuidar que su condición cambie para no crear un ciclo infinito.

## Funciones: nombrar una solución reutilizable

Una [función](../glosario.md#funcion) recibe datos, realiza una tarea concreta y devuelve un resultado. Es una forma de explicar el programa en piezas.

```ts
function convertirCelsiusAFahrenheit(celsius: number): number {
  return (celsius * 9) / 5 + 32;
}
```

En este ejemplo:

- `convertirCelsiusAFahrenheit` dice qué hace;
- `celsius: number` describe la entrada;
- `: number` describe la salida;
- `return` [entrega](../glosario.md#entrega) el resultado.

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

Un [arreglo](../glosario.md#arreglo) permite guardar una secuencia de valores. Por ahora lo usaremos para practicar recorridos, acumulación y transformación simple. El modelado profundo de arreglos de objetos y las estructuras de datos aparece en el módulo 03.

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

- [Caso normal](../glosario.md#caso-normal): lo que esperamos que ocurra normalmente.
- [Caso límite](../glosario.md#caso-limite): valor mínimo, máximo o borde de una regla.
- Caso inválido: dato que no debería aceptarse o que requiere un mensaje claro.

## Trucos y buenas prácticas de supervivencia

Estas reglas no reemplazan pensar, pero evitan muchos errores antes de que aparezcan.

### 1. Usa cláusulas de guarda para evitar el infierno de `if`

Una [cláusula de guarda](../glosario.md#clausula-de-guarda) resuelve primero los casos que no pueden continuar. Así el caso principal queda al final, con menos sangría y más claridad.

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

Si una función calcula, muestra alertas, modifica datos y además hace peticiones, divídela. Las funciones con una responsabilidad son más fáciles de entender, reutilizar y [depurar](../glosario.md#depurar).

### 10. Lee el error completo antes de cambiar código

Cuando aparezca un error:

1. lee la primera línea y localiza el archivo y la línea;
2. identifica qué valor esperaba el programa y qué recibió;
3. reduce el problema a un ejemplo pequeño;
4. prueba una corrección;
5. escribe qué aprendiste en la [bitácora](../glosario.md#bitacora).

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

## Guías de algoritmos: Fibonacci y ordenamientos

Cuando domines ciclos, funciones y listas con tipos, estas dos guías te llevan un paso más allá y se pueden trabajar después del taller de fundamentos:

- [Guía 5: Fibonacci: la secuencia que suma](guia-fibonacci.md) — la matemática de la serie ([recursión](../glosario.md#recursion), [proporción áurea](../glosario.md#proporcion-aurea) y [fórmula de Binet](../glosario.md#binet) incluida) y el paso de "lineal contra exponencial" con código.
- [Guía 6: Ordenamientos: bubble, merge y quicksort](guia-ordenamientos.md) — tres formas de ordenar, sus ventajas y cuál se usa más, y cómo medir cuánto trabajo hace cada una (logarítmica, lineal, cuadrática y exponencial) con el [gráfico de complejidad](grafico-complejidad.svg).

No son un [requisito](../glosario.md#requisito) del recorrido principal, pero conectan con la [profundización de algoritmia](../07-algoritmia/README.md) y con las preguntas de "¿qué tan eficiente es mi solución?" que aparecen en entrevistas y proyectos reales.

## Guía 7: POO, clases y abstracción

Esta guía es un paso obligatorio entre los fundamentos y el módulo 03. Parte de un problema concreto, escribe el código y explica las decisiones. Aquí el problema son los **datos que pertenecen a una misma entidad** (un producto, un carrito, un turno) y la pregunta central es una:

> **¿qué necesita saber esta aplicación para cumplir su propósito, y qué puede quedarse afuera?**

- [Guía 7: Programación orientada a objetos: clases y abstracción](guia-poo.md) — la abstracción explicada a fondo (datos, comportamiento e interfaz), clases con `constructor`, `this` y `new`, [encapsulamiento](../glosario.md#encapsulamiento) con `private` y getters, [composición](../glosario.md#composicion) frente a herencia ("¿es un?"), [polimorfismo](../glosario.md#polimorfismo) con `interface`, y una sección honesta sobre cuándo conviene un `type` con funciones puras en vez de una [clase](../glosario.md#clase).

Los ejemplos usan TypeScript y salen del mismo [dominio](../glosario.md#dominio) del curso (la tiendita), así que conectan directo con la [Guía de React](../03-react-typescript/guia-react.md), el [módulo 03](../03-react-typescript/README.md) y el [proyecto integrador](../glosario.md#proyecto-integrador).

## Siguiente paso

Cuando puedas explicar y resolver los retos 0-13, completa la [guía obligatoria de POO](guia-poo.md) y estudia HTML en la [guía independiente](../03-react-typescript/guia-html.md). Después, la [Guía de React](../03-react-typescript/guia-react.md) te muestra por qué una página estática se queda corta y cómo un [componente](../glosario.md#componente) con estado resuelve el problema, y luego continúa con el [recorrido de React con Vite](../03-react-typescript/README.md).

Si te queda la duda de por qué un producto necesita tantos datos, o de cuándo conviene una clase y cuándo no, la [Guía 7: POO](#guía-7-poo-clases-y-abstracción) responde exactamente eso y sirve de puente entre los dos módulos.

---

**Anterior:** [Guia del editor VS Code](../01-herramientas/editor-vscode.md) · **Siguiente:** [Guia 1: if](guia-if.md)
