# Guía 2: Repetir con `for`

## Objetivos

Al terminar esta guía deberías poder:

- Explicar qué problema resuelve un ciclo.
- Reconocer cuándo conviene usar `for`.
- Leer y escribir las tres partes del `for`: inicialización, condición y actualización.
- Contar hacia adelante, hacia atrás y con saltos.
- Recorrer listas (arrays) y textos (strings).
- Usar `for` junto con `if` para filtrar, buscar y contar.
- Distinguir un **contador** de un **acumulador**.
- Usar `break` y `continue` con intención.
- Evitar los errores de índices y los ciclos infinitos.

> **Cómo probar los ejemplos**
>
> Igual que en la [Guía 1](guia-if.md): copia el código en un archivo (`node ejemplo.js`) o pégalo en la consola del navegador. Las dos formas funcionan, elige la que tengas disponible.

## 1. El problema

> *"Tengo que repetir una acción varias veces."*

Imprimir los números del 1 al 100. Sumar los valores de una lista. Contar cuántas vocales tiene una frase. Ninguno de esos problemas se resuelve escribiendo la acción cien veces: se resuelve pidiéndole al programa que **repita** con una estructura.

Hacer una copia y pegar la misma línea cien veces no es programar: es escribir. Un ciclo le dice a la máquina *"esto se repite N veces"* en unas pocas líneas.

## 2. Concepto

Un **ciclo** (también llamado *bucle* o *iteración*) es una estructura que ejecuta un bloque de código **más de una vez**.

Antes de usar un ciclo, debes poder responder cuatro cosas:

1. **¿Qué se repite?** (la acción).
2. **¿Cuándo termina?** (la condición).
3. **¿Qué cambia entre repetición y repetición?** (la actualización).
4. **¿Qué resultado espero?** (el producto del recorrido).

`for` es la herramienta adecuada cuando **sabes, o puedes expresar claramente, cuántas veces repetir** — por ejemplo, "del 1 al 10", "una vez por cada elemento de la lista". Para esos casos, `for` reúne en una sola línea el punto de partida, la condición y el paso.

## 3. Sintaxis

```js
for (inicializacion; condicion; actualizacion) {
    // código que se repite
}
```

- **Inicialización:** se ejecuta una sola vez, antes de empezar. Crea la variable que controla el ciclo (por ejemplo, `let i = 0`).
- **Condición:** se evalúa antes de cada repetición. Si es `true`, el bloque se ejecuta; si es `false`, el ciclo termina.
- **Actualización:** se ejecuta después de cada repetición. Cambia la variable de control (por ejemplo, `i++`).

Esto se lee casi como una frase: *"desde i = 0, mientras i sea menor que 5, y sumándole 1 a i en cada vuelta, haz esto"*.

## 4. Ejemplos progresivos

### Ejemplo 1: Imprimir del 1 al 10

**Problema:** mostrar los números del 1 al 10.

**Código:**

```js
for (let i = 1; i <= 10; i++) {
    console.log(i);
}
```

**Explicación:** `i` empieza en `1`. Antes de cada vuelta se pregunta `i <= 10`. Al final de cada vuelta se ejecuta `i++`, que le suma 1 a `i` (`i++` es una abreviatura de `i = i + 1`). Cuando `i` llega a 11, la condición es `false` y el ciclo se detiene.

**Resultado esperado:**

```
1
2
3
4
5
6
7
8
9
10
```

### Ejemplo 2: Qué ocurre en cada iteración

**Problema:** entender paso a paso un `for` clásico.

**Código:**

```js
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

**Explicación:** cada repetición se llama **iteración**. Mira la tabla: la inicialización ocurre una sola vez; luego se alternan condición → bloque → actualización.

| Iteración | ¿Cuánto vale `i` al entrar? | ¿Se cumple `i < 5`? | Qué hace | ¿Cuánto vale `i` al salir? |
|---|---:|---:|---|---:|
| 1 | 0 | `true` | imprime `0` | 1 |
| 2 | 1 | `true` | imprime `1` | 2 |
| 3 | 2 | `true` | imprime `2` | 3 |
| 4 | 3 | `true` | imprime `3` | 4 |
| 5 | 4 | `true` | imprime `4` | 5 |
| — | 5 | `false` | el ciclo termina | — |

**Resultado esperado:**

```
0
1
2
3
4
```

Fíjate en un detalle importante: aunque `i < 5`, se imprimen los números `0` a `4`, no `1` a `5`. En programación contamos desde `0` con mucha frecuencia. No es un error: es la convención que usa JavaScript para listas e índices, y la verás mucho.

### Ejemplo 3: Contar hacia atrás

**Problema:** mostrar los números del 10 al 1.

**Código:**

```js
for (let i = 10; i >= 1; i--) {
    console.log(i);
}
```

**Explicación:** `i--` resta 1 en cada vuelta (es una abreviatura de `i = i - 1`). La condición es `i >= 1`, así que el ciclo llega hasta el 1 y se detiene en el 0.

**Resultado esperado:**

```
10
9
8
7
6
5
4
3
2
1
```

### Ejemplo 4: Imprimir los pares (dos caminos)

**Problema:** mostrar los números pares del 2 al 10.

**Camino A — el salto:**

```js
for (let i = 2; i <= 10; i += 2) {
    console.log(i);
}
```

**Explicación:** la actualización no tiene que ser `i++`. Con `i += 2` avanzamos de dos en dos (`i += 2` significa `i = i + 2`). Si en lugar de eso hubiéramos escrito **saltos de 5** desde 5 hasta 50, usaríamos `i += 5`.

**Resultado esperado:**

```
2
4
6
8
10
```

**Camino B — el `for` + `if`:**

```js
for (let i = 1; i <= 10; i++) {
    if (i % 2 === 0) {
        console.log(i);
    }
}
```

**Explicación:** aquí el ciclo visita todos los números y el `if` se queda con los pares, usando el operador `%` que conoces de la [Guía 1](guia-if.md). Es el patrón **recorrer → preguntar → actuar**. El salto (camino A) es más eficiente, pero el `if` es más flexible: si mañana quisieras los pares *y* los que no terminan en 0, el salto no bastaría.

### Ejemplo 5: Imprimir los impares

**Problema:** mostrar los números impares del 1 al 9.

**Código:**

```js
for (let i = 1; i <= 9; i += 2) {
    console.log(i);
}
```

**Resultado esperado:**

```
1
3
5
7
9
```

**Variación con `if`:**

```js
for (let i = 1; i <= 10; i++) {
    if (i % 2 !== 0) {
        console.log(i);
    }
}
```

### Ejemplo 6: Sumar del 1 al 100 (acumulador)

**Problema:** calcular la suma de los números del 1 al 100.

**Código:**

```js
let suma = 0;

for (let i = 1; i <= 100; i++) {
    suma = suma + i;
}

console.log("La suma es:", suma);
```

**Explicación:** la variable `suma` va **acumulando** cada valor de `i`. Primero suma 1, luego 2, luego 3, y así hasta 100. Esta variable guarda un resultado que crece con cada iteración; se llama **acumulador** (lo verás en detalle en la sección 5). Nota que `suma` se modifica, por eso se declara con `let`.

**Resultado esperado:**

```
La suma es: 5050
```

### Ejemplo 7: Recorrer una lista con índices

**Problema:** mostrar cada fruta de una lista, una por línea.

**Código:**

```js
const frutas = ["manzana", "pera", "uva"];

for (let i = 0; i < frutas.length; i++) {
    console.log(frutas[i]);
}
```

**Explicación:** aquí aparecen dos ideas nuevas, explicadas apenas lo necesario (en la [Guía 4](guia-listas.md) las trabajas a fondo):

- `["manzana", "pera", "uva"]` es una **lista** de valores.
- `frutas[i]` accede al elemento en la posición `i` (**índice**), y el primer elemento está en la posición `0`.
- `frutas.length` es la **cantidad de elementos** (3). La condición `i < frutas.length` garantiza que visitemos exactamente las posiciones `0`, `1` y `2`, sin pasarnos.

**Resultado esperado:**

```
manzana
pera
uva
```

### Ejemplo 8: Recorrer un string (texto)

**Problema:** imprimir cada letra de una palabra, una por línea.

**Código:**

```js
const palabra = "hola";

for (let i = 0; i < palabra.length; i++) {
    console.log(palabra[i]);
}
```

**Explicación:** un texto también se puede recorrer letra por letra. `palabra.length` es cuántos caracteres tiene, y `palabra[i]` es el carácter en la posición `i`. Los strings y las listas comparten esta lógica de índice.

**Resultado esperado:**

```
h
o
l
a
```

### Ejemplo 9: Contar elementos que cumplen una condición

**Problema:** contar cuántas edades son mayores o iguales a 18.

**Código:**

```js
const edades = [12, 18, 25, 15, 30];
let mayores = 0;

for (let i = 0; i < edades.length; i++) {
    if (edades[i] >= 18) {
        mayores++;
    }
}

console.log("Mayores de edad:", mayores);
```

**Explicación:** `mayores++` suma 1 al contador cuando la condición se cumple. Ejecutamos el patrón **recorrer → preguntar → actuar**: recorremos la lista, preguntamos por cada edad y actuamos (contamos) solo cuando corresponde. Este patrón aparece constantemente al programar y la meta de esta guía es que se vuelva natural.

**Resultado esperado:**

```
Mayores de edad: 3
```

### Ejemplo 10: Buscar un elemento

**Problema:** saber si el nombre "Pedro" está en la lista.

**Código:**

```js
const nombres = ["Ana", "Carlos", "Pedro", "Luis"];
let encontrado = false;

for (let i = 0; i < nombres.length; i++) {
    if (nombres[i] === "Pedro") {
        encontrado = true;
    }
}

if (encontrado) {
    console.log("Pedro está en la lista.");
} else {
    console.log("Pedro no está en la lista.");
}
```

**Explicación:** usamos una variable booleana `encontrado` que empieza en `false` y cambia a `true` si aparece el nombre. Después del ciclo, un `if` (de la [Guía 1](guia-if.md)) muestra el resultado. La variable "estado" guarda la respuesta de *¿lo encontré?* mientras el ciclo trabaja.

**Resultado esperado:**

```
Pedro está en la lista.
```

### Ejemplo 11: Encontrar el mayor

**Problema:** dado una lista de números, encontrar el más grande.

**Código:**

```js
const numeros = [4, 12, 7, 20, 3];
let mayor = numeros[0];

for (let i = 1; i < numeros.length; i++) {
    if (numeros[i] > mayor) {
        mayor = numeros[i];
    }
}

console.log("El mayor es:", mayor);
```

**Explicación:** la estrategia es suponer que el primero es el mayor y luego **comparar contra cada uno**, actualizando `mayor` cuando aparece uno más grande. Fíjate que el ciclo empieza en `i = 1`, porque el elemento `0` ya lo usamos como punto de partida.

**Resultado esperado:**

```
El mayor es: 20
```

**Variación:** el mismo esquema con `<` te da el menor.

### Ejemplo 12: Tabla de multiplicar

**Problema:** mostrar la tabla de multiplicar del 7.

**Código:**

```js
const numero = 7;

for (let i = 1; i <= 10; i++) {
    console.log(numero + " x " + i + " = " + numero * i);
}
```

**Explicación:** en cada vuelta se construye una línea con el cálculo. La variable `i` no solo cuenta: también es la parte de la tabla que cambia.

**Resultado esperado:**

```
7 x 1 = 7
7 x 2 = 14
7 x 3 = 21
7 x 4 = 28
7 x 5 = 35
7 x 6 = 42
7 x 7 = 49
7 x 8 = 56
7 x 9 = 63
7 x 10 = 70
```

### Ejemplo 13: `break` — detener un ciclo antes de tiempo

**Problema:** recorrer del 1 al 10 pero detenerse al llegar al 5.

**Código:**

```js
for (let i = 1; i <= 10; i++) {
    if (i === 5) {
        console.log("Llegamos a 5, detengo el ciclo.");
        break;
    }
    console.log(i);
}
```

**Explicación:** `break` **termina el ciclo por completo** en el momento en que se ejecuta. No pregunta si quedan iteraciones: sale. Es útil cuando ya encontraste lo que buscabas y no tiene sentido seguir recorriendo.

**Resultado esperado:**

```
1
2
3
4
Llegamos a 5, detengo el ciclo.
```

### Ejemplo 14: `continue` — saltarse una vuelta

**Problema:** imprimir los números del 1 al 10 excepto los múltiplos de 3.

**Código:**

```js
for (let i = 1; i <= 10; i++) {
    if (i % 3 === 0) {
        continue;
    }
    console.log(i);
}
```

**Explicación:** `continue` **termina solo la iteración actual**: salta al final del bloque y pasa directo a la actualización. La diferencia con `break`: `break` sale del ciclo; `continue` se salta esa vuelta y sigue con la siguiente.

**Resultado esperado:**

```
1
2
4
5
7
8
10
```

### Ejemplo 15: `for` anidado (un ciclo dentro de otro)

**Problema:** generar las tablas de multiplicar del 1 al 3.

**Código:**

```js
for (let fila = 1; fila <= 3; fila++) {
    for (let columna = 1; columna <= 3; columna++) {
        console.log(fila + " x " + columna + " = " + fila * columna);
    }
}
```

**Explicación:** un **ciclo anidado** se ejecuta de adentro hacia fuera: por cada valor de `fila`, el ciclo interior completa todas sus vueltas. Con `fila = 1` recorre `columna` 1, 2 y 3; luego `fila = 2`, y así. Elegí nombres descriptivos (`fila`, `columna`) para que no se confundan las dos variables.

**Resultado esperado:**

```
1 x 1 = 1
1 x 2 = 2
1 x 3 = 3
2 x 1 = 2
2 x 2 = 4
2 x 3 = 6
3 x 1 = 3
3 x 2 = 6
3 x 3 = 9
```

## 5. Contadores vs acumuladores

Dos variables viven dentro de casi todo ciclo. Se parecen, pero cumplen misiones distintas.

**Contador:** cuenta **cuántas veces** ocurrió algo. Siempre suma de a uno.

```js
let pares = 0;

for (let i = 1; i <= 10; i++) {
    if (i % 2 === 0) {
        pares++;
    }
}

console.log("Cantidad de pares:", pares);
```

**Acumulador:** guarda un resultado que **crece sumando (o restando, multiplicando…) valores reales**, no cuentas ocurrencias.

```js
let suma = 0;

for (let i = 1; i <= 100; i++) {
    suma = suma + i;   // también: suma += i
}

console.log("Suma total:", suma);
```

| | Contador | Acumulador |
|---|---|---|
| ¿Qué pregunta responde? | "¿Cuántos?" | "¿Cuánto en total?" |
| ¿Qué operación usa normalmente? | `++` (sumar 1) | `+`, `-`, `*`, `+=`… |
| Ejemplo | cantidad de aprobados | suma de notas |
| Resultado típico | un entero pequeño | un total |

Si al final necesitas *"cuántos"*, es contador. Si necesitas *"cuánto suma"*, es acumulador. En el ejemplo de las notas: para contar aprobados usas contador; para sumar las notas usas acumulador. Muchos problemas usan ambos a la vez.

## 6. ¿Cómo pienso este problema?

Antes de escribir un `for`, identifica las cuatro partes con la regla:

> **inicio → condición → repetición → actualización**

Pongámoslo en práctica con el problema *"sumar los números del 1 al 100"*:

1. **Inicio:** la suma empieza en `0` y el primer número es `1`.
2. **Condición:** repite mientras el número sea menor o igual a 100.
3. **Repetición (acción):** agrega el número actual a la suma.
4. **Actualización:** pasa al siguiente número.

Traducido: `suma = 0`, `for (let i = 1; i <= 100; i++)`, y dentro `suma = suma + i`.

Cuando el problema involucra una **lista**, el patrón cambia de forma pero la esencia es la misma:

> **recorrer → preguntar → actuar**

1. **Recorrer:** `for (let i = 0; i < lista.length; i++)`.
2. **Preguntar:** un `if` sobre `lista[i]`.
3. **Actuar:** contar, guardar, imprimir, sumar… solo si la respuesta es `true`.

Si el problema dice *"para cada…"*, *"todos los…"* o *"del … al …"*, muy probablemente necesitas un `for`.

## 7. Errores frecuentes

### Error 1: equivocarse por uno (off-by-one)

```js
for (let i = 1; i < 10; i++) {
    console.log(i);
}
```

**Error:** si querías imprimir del 1 al 10, esto imprime del 1 al **9**. La condición `< 10` excluye al 10. Recuerda la diferencia:

- `i < 10` → del 1 al 9 (o del 0 al 9).
- `i <= 10` → del 1 al 10 (incluye el 10).

Elige la condición según **el último valor que quieras incluir**.

### Error 2: empezar desde 1 y olvidar que los índices empiezan en 0

```js
const frutas = ["manzana", "pera", "uva"];

for (let i = 1; i <= frutas.length; i++) {
    console.log(frutas[i]);
}
```

**Error:** la lista tiene elementos en las posiciones `0`, `1` y `2`. Este ciclo visita `1`, `2` y `3`, así que imprime `pera`, `uva` y `undefined` (la posición 3 no existe). La regla segura para recorrer listas es `i = 0` y `i < lista.length`.

**Corrección:**

```js
for (let i = 0; i < frutas.length; i++) {
    console.log(frutas[i]);
}
```

### Error 3: olvidar la actualización (ciclo infinito)

```js
for (let i = 0; i < 5;) {
    console.log(i);
}
```

**Error:** la actualización está vacía, así que `i` nunca cambia y la condición `i < 5` siempre es `true`. El programa se queda repitiendo `0` para siempre (o hasta que se bloquee). El `for` facilita evitar esto porque la actualización vive en la primera línea, pero sigue siendo obligatoria: si no cambia la variable de control, el ciclo no termina.

### Error 4: usar `const` para la variable del ciclo

```js
for (const i = 0; i < 5; i++) {
    console.log(i);
}
```

**Error:** `const` no admite reasignaciones, y `i++` intenta modificar a `i`. La variable que controla el `for` cambia en cada vuelta, así que debe declararse con `let`.

### Error 5: punto y coma después del `for`

```js
for (let i = 0; i < 5; i++); {
    console.log(i);
}
```

**Error:** el `;` desconecta el ciclo del bloque. El `for` gira 5 veces sin hacer nada y luego el bloque `{ … }` se ejecuta una sola vez (y además `i` ya no está disponible). Es el mismo tipo de error que con `if`: no se escribe `;` después de la estructura de control.

### Error 6: condiciones invertidas

```js
for (let i = 10; i < 5; i++) {
    console.log(i);
}
```

**Error:** la condición nunca se cumple desde el inicio (10 no es menor que 5), así que el ciclo **no ejecuta nada**. Revisa las tres partes antes de ejecutar: *¿el inicio y la condición tienen sentido juntos?* Aquí faltó `i >= 5` (o usar `i--`).

## 8. Tips

- **Cuando veas "del … al …", "para cada …" o "todos los …", piensa en `for`.**
- **Traza el ciclo en papel o en tu cabeza** con valores pequeños: escribe qué vale `i`, qué imprime y cuándo para. Es la habilidad más útil para dominar ciclos.
- **Prueba los bordes:** con `i <= 10`, ¿qué pasa con el 10? ¿y con el 11?
- **Nombra bien la variable del ciclo.** `i` es la convención tradicional para índices, pero en ciclos anidados o cuando el número significa algo, usa nombres como `fila`, `columna`, `multiplicador`.
- **Para recorrer una lista, la forma estándar es `i = 0` con `i < lista.length`.** Cópiala hasta que se vuelva automática.
- **Prefiere viajar con paso `1` y filtrar con `if`** cuando la regla sea compleja. Usa saltos (`i += 2`) solo cuando la regla sea un paso fijo.
- **`break` no es un error**, pero úsalo con intención: detener el ciclo porque ya encontraste la respuesta es legítimo.

## 9. Ejercicios

Resuelve por tu cuenta y verifica cada resultado con `console.log`. Revisa lo que ya sabes antes de continuar: estas guías se apoyan una en la otra.

### 🟢 Muy fáciles

1. **Cuenta hacia adelante.** Imprime los números del 1 al 20.
2. **Cuenta hacia atrás.** Imprime los números del 15 al 1.
3. **Múltiplos.** Imprime los múltiplos de 3 hasta el 30.
4. **Letra por letra.** Imprime cada carácter de tu nombre, uno por línea.

### 🟡 Básicos

5. **Suma de pares.** Suma los números pares del 2 al 50 y muestra el total.
6. **Contador de vocales.** Cuenta cuántas vocales tiene una palabra que elijas (recórrela letra por letra con un `if`).
7. **Tabla elegida.** Pide una tabla de multiplicar (una variable con el número) y muéstrala del 1 al 10.
8. **Filtrar una lista.** Dada la lista `[3, 15, 8, 21, 4]`, imprime solo los números mayores que 10.

### 🟠 Intermedios

9. **Extremos de una lista.** Encuentra el mayor **y** el menor de la lista `[42, 7, 90, 3, 55]` en un solo recorrido.
10. **FizzBuzz.** Recorre del 1 al 100: imprime `Fizz` para múltiplos de 3, `Buzz` para múltiplos de 5 y `FizzBuzz` para ambos. (Combina `for`, `if` y el orden correcto de condiciones.)
11. **Redondeo… no, suma condicionada.** Suma solo los valores pares de la lista `[5, 8, 12, 7, 20, 3]`.
12. **Frecuencia de una letra.** Dado un texto, cuenta cuántas veces aparece la letra `a`. Prueba con una frase completa, no solo una palabra.

### 🔴 Desafío

13. **Nombres largos.** Dada la lista `["Ana", "Carlos", "Pedro", "Gabriela", "Luis"]`, imprime solo los nombres con 5 o más letras. Necesitas `name.length`.
14. **Clasificar en un recorrido.** Dada una lista de números, cuenta en un solo ciclo cuántos son positivos, cuántos negativos y cuántos son cero. Tres contadores en el mismo `for`.
15. **Tablas juntas.** Imprime las tablas de multiplicar del 1 al 5 usando un `for` anidado. Las líneas deben verse así: `3 x 7 = 21`.

## 10. Chuleta rápida

```js
// Contar de 1 a N
for (let i = 1; i <= 10; i++) {
    console.log(i);
}

// Contar hacia atrás
for (let i = 10; i >= 1; i--) {
    console.log(i);
}

// Saltos
for (let i = 0; i <= 20; i += 5) {
    console.log(i);
}

// Recorrer una lista
const frutas = ["manzana", "pera", "uva"];
for (let i = 0; i < frutas.length; i++) {
    console.log(frutas[i]);
}

// Recorrer un string
const palabra = "hola";
for (let i = 0; i < palabra.length; i++) {
    console.log(palabra[i]);
}

// Acumulador
let suma = 0;
for (let i = 1; i <= 100; i++) {
    suma += i;
}

// Contador
let pares = 0;
for (let i = 1; i <= 10; i++) {
    if (i % 2 === 0) {
        pares++;
    }
}

// break y continue
for (let i = 1; i <= 10; i++) {
    if (i === 5) break;       // detiene el ciclo completo
    if (i % 3 === 0) continue; // salta esta vuelta
    console.log(i);
}

// for anidado
for (let fila = 1; fila <= 3; fila++) {
    for (let columna = 1; columna <= 3; columna++) {
        console.log(fila, "x", columna);
    }
}
```

## 11. ¿Qué puedo hacer ahora?

`for` repite **una cantidad predecible** de veces: del 1 al 10, una vez por elemento, N intentos.

Pero a veces no sabes cuántas veces vas a repetir:

- *"Repite hasta que la contraseña sea correcta."*
- *"Sigue sumando mientras el resultado sea menor que 100."*
- *"Busca mientras queden elementos."*

Ahí la cantidad de repeticiones depende de **una condición que cambia durante el proceso**, y no la conoces de antemano. Para esos casos existe otro ciclo: `while`. La siguiente guía explica cuándo conviene cada uno, sin afirmar que uno sea siempre mejor: [Guía 3: Repetir con `while`](guia-while.md).