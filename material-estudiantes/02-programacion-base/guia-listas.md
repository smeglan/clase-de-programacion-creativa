# Guía 4: Listas (arrays)

## Objetivos

Al terminar esta guía deberías poder:

- Explicar qué problema resuelve una lista y por qué no se guardan valores relacionados en variables separadas.
- Crear un array en JavaScript, acceder a sus elementos y modificarlos.
- Explicar por qué el primer índice es `0` y reconocer cuándo un índice está fuera de rango.
- Usar `length`, `push`, `pop`, `shift`, `unshift`, `includes` e `indexOf`.
- Recorrer un array con `for` y con `while`.
- Resolver problemas de sumar, promediar, buscar, contar, filtrar y encontrar extremos.
- Aplicar el patrón **recorrer → preguntar → actuar** sobre listas.

> **Cómo probar los ejemplos**
>
> Igual que en las guías anteriores: `node ejemplo.js` con el código en un archivo, o pegar el código en la consola del navegador. Todo es JavaScript básico.

## 1. El problema

> *"Tengo muchos valores relacionados y no quiero guardarlos en variables separadas."*

Cuando tenías tres nombres, la tentación es hacer esto:

```js
let nombre1 = "Ana";
let nombre2 = "Carlos";
let nombre3 = "Pedro";
```

Pero ¿qué pasa si son 50 nombres? ¿Y si no sabes cuántos van a ser? Guardarlos en variables separadas no escala: no puedes "recorrerlas" ni "contarlas" con un ciclo.

La solución es agruparlos en una sola variable:

```js
const nombres = ["Ana", "Carlos", "Pedro"];
```

Ahora es **una sola cosa** que contiene varios valores. Puedes recorrerla, preguntarle cuántos tiene, buscar adentro, agregar y quitar elementos. Combinada con los ciclos de las guías 2 y 3, una lista es la herramienta que convierte "un dato" en "muchos datos".

## 2. Concepto

Una **lista** es una colección ordenada de valores. En JavaScript, la forma de representar una lista se llama **array** (arreglo). Por eso decimos *"Lista (Array)"*: el concepto general y su implementación concreta son la misma idea.

Características básicas:

- Se escribe entre corchetes `[ ]`, con los valores separados por comas.
- Los elementos tienen un **orden**: cada uno ocupa una **posición**.
- Puede contener textos, números, booleanos… (veremos mezclas más adelante).
- Puedes preguntarle **cuántos** elementos tiene con `.length`.

```js
const frutas = ["manzana", "pera", "uva"];
const numeros = [10, 20, 30];
const estados = [true, false, true];
const vacia = [];
```

`vacia` es una lista sin elementos: su `length` es `0`. No es un error; es útil cuando una lista se irá llenando.

## 3. Sintaxis

### Crear y consultar

```js
const frutas = ["manzana", "pera", "uva"];

console.log(frutas[0]);   // manzana
console.log(frutas[1]);   // pera
console.log(frutas[2]);   // uva
console.log(frutas.length); // 3
```

- `frutas[i]` lee el elemento en la posición `i` (llamada **índice**).
- `frutas.length` es un número: cuántos elementos hay.

### Modificar

```js
const frutas = ["manzana", "pera", "uva"];

frutas[1] = "mango";     // reemplaza "pera" por "mango"

console.log(frutas);     // ["manzana", "mango", "uva"]
```

Puedes modificar el contenido de un array declarado con `const`, porque `const` impide **reasignar la variable** (hacer `frutas = otras`) pero no **cambiar los elementos** de su interior.

### Agregar y eliminar (métodos básicos)

| Método | Qué hace | Dónde | Código |
|---|---|---|---|
| `push(valor)` | agrega un elemento | al **final** | `frutas.push("kiwi")` |
| `pop()` | quita y devuelve el último | del **final** | `const ultima = frutas.pop()` |
| `unshift(valor)` | agrega un elemento | al **inicio** | `frutas.unshift("papaya")` |
| `shift()` | quita y devuelve el primero | del **inicio** | `const primera = frutas.shift()` |

### Preguntar

| Método | Qué responde | Ejemplo |
|---|---|---|
| `lista.includes(valor)` | `true` si el valor existe, `false` si no | `frutas.includes("pera")` |
| `lista.indexOf(valor)` | la **posición** del valor, o `-1` si no existe | `frutas.indexOf("pera")` |

Escrito como oración: *"push agrega al final, pop quita del final, unshift agrega al inicio, shift quita del inicio, includes pregunta si existe y indexOf busca su posición."*

## 4. Índices: la trampa número uno

> El primer elemento está en el índice `0`, y el último está en `length - 1`.

Mira este ejemplo y la posición de cada elemento:

```js
const frutas = ["manzana", "pera", "uva"];

console.log(frutas[0]);   // manzana
console.log(frutas[1]);   // pera
console.log(frutas[2]);   // uva
```

| Índice | 0 | 1 | 2 |
|---|---|---:|---:|
| Elemento | `"manzana"` | `"pera"` | `"uva"` |

Ahora piensa: `frutas[3]` **no corresponde a ningún elemento existente**. La lista tiene `length === 3`, y sus posiciones válidas van de `0` a `2`, es decir, hasta `length - 1`.

¿Qué devuelve `frutas[3]`?

```js
console.log(frutas[3]);   // undefined
```

Devuelve `undefined`: el valor "no existe" de JavaScript. El programa no se cae, pero tampoco hay nada ahí. **El error más común con listas es usar `length` como si fuera el último índice válido.** No lo es: el último válido es `length - 1`.

¿Por qué se cuenta desde `0`? Es una convención que JavaScript hereda de los lenguajes más antiguos: la primera posición se llama `0`, como en la guía anterior viste que `i < length` recorre exactamente las posiciones `0, 1, …, length - 1`. Una vez que lo interiorizas, los ciclos y las listas encajan sin fricción.

## 5. Ejemplos progresivos

### Ejemplo 1: Crear una lista y mostrar todos sus elementos

**Problema:** guardar una lista de nombres e imprimir cada uno.

**Código:**

```js
const nombres = ["Ana", "Carlos", "Pedro"];

for (let i = 0; i < nombres.length; i++) {
    console.log(nombres[i]);
}
```

**Explicación:** el ciclo (de la [Guía 2](guia-for.md)) recorre las posiciones `0`, `1` y `2` porque la condición es `i < nombres.length`. Si en lugar de `i <` escribieras `i <=`, el índice `3` caería fuera de rango y aparecería `undefined` (la trampa de la sección 4).

**Resultado esperado:**

```
Ana
Carlos
Pedro
```

### Ejemplo 2: Mostrar y modificar un elemento específico

**Problema:** ver la segunda fruta y reemplazarla.

**Código:**

```js
const frutas = ["manzana", "pera", "uva"];

console.log("La segunda fruta es:", frutas[1]);

frutas[1] = "mango";

console.log(frutas);
```

**Resultado esperado:**

```
La segunda fruta es: pera
[ 'manzana', 'mango', 'uva' ]
```

**Explicación:** para cambiar un elemento, se asigna sobre su índice: `frutas[1] = "mango"`. El resto de la lista no se ve afectado.

### Ejemplo 3: Agregar y eliminar elementos

**Problema:** una lista de la compra que va cambiando.

**Código:**

```js
const compra = ["pan", "leche"];

compra.push("huevos");        // al final
compra.unshift("café");       // al inicio

console.log(compra);

const ultimo = compra.pop();  // quita el último
const primero = compra.shift(); // quita el primero

console.log("Quité del final:", ultimo);
console.log("Quité del inicio:", primero);
console.log(compra);
```

**Explicación:** `push`/`unshift` agregan; `pop`/`shift` quitan **y devuelven** el elemento quitado, por eso podemos guardarlo en una variable. Fíjate que las operaciones se realizan sobre la misma lista: la reasignamos a otra variable solo cuando queremos conservar el valor quitado.

**Resultado esperado:**

```
[ 'café', 'pan', 'leche', 'huevos' ]
Quité del final: huevos
Quité del inicio: café
[ 'pan', 'leche' ]
```

### Ejemplo 4: Saber si existe un elemento y su posición

**Problema:** saber si "Pedro" está en la lista y en qué posición.

**Código:**

```js
const nombres = ["Ana", "Carlos", "Pedro", "Luis"];

console.log("¿Existe Pedro?", nombres.includes("Pedro"));
console.log("Posición de Pedro:", nombres.indexOf("Pedro"));
console.log("¿Existe María?", nombres.includes("María"));
console.log("Posición de María:", nombres.indexOf("María"));
```

**Explicación:** `includes` responde `true`/`false` (un booleano, como en la [Guía 1](guia-if.md)). `indexOf` responde un número: la posición, o `-1` cuando no existe. Esa es la convención de JavaScript: `-1` significa "no está".

**Resultado esperado:**

```
¿Existe Pedro? true
Posición de Pedro: 2
¿Existe María? false
Posición de María: -1
```

### Ejemplo 5: Contar números mayores que 10

**Problema:** ¿cuántos números de la lista superan el 10?

**Código:**

```js
const numeros = [4, 12, 7, 20, 3];
let contador = 0;

for (let i = 0; i < numeros.length; i++) {
    if (numeros[i] > 10) {
        contador++;
    }
}

console.log("Números mayores que 10:", contador);
```

**Explicación:** el patrón **recorrer → preguntar → actuar** con un **contador** (de la [Guía 2](guia-for.md)): se recorre con el ciclo, se pregunta con `if` y se actúa sumando al contador solo cuando corresponde.

**Resultado esperado:**

```
Números mayores que 10: 2
```

### Ejemplo 6: Sumar todos los números

**Problema:** calcular la suma total de una lista de precios.

**Código:**

```js
const precios = [100, 250, 75, 300];
let suma = 0;

for (let i = 0; i < precios.length; i++) {
    suma = suma + precios[i];   // también: suma += precios[i]
}

console.log("Total:", suma);
```

**Explicación:** la variable `suma` es un **acumulador**: guarda la suma de todo lo visitado hasta el momento. Es exactamente el mismo acumulador de la guía 2, pero ahora los valores vienen de una lista.

**Resultado esperado:**

```
Total: 725
```

### Ejemplo 7: Calcular el promedio

**Problema:** promedio de las notas de un estudiante.

**Código:**

```js
const notas = [4.5, 3.0, 5.0, 2.5];
let suma = 0;

for (let i = 0; i < notas.length; i++) {
    suma += notas[i];
}

const promedio = suma / notas.length;

console.log("Promedio:", promedio);
```

**Explicación:** un promedio siempre tiene dos partes: la **suma** de todos los valores y la **cantidad** de valores (`length`). Primero acumulas, luego divides. Siempre que veas "promedio", piensa "suma entre cuántos son".

**Resultado esperado:**

```
Promedio: 3.75
```

### Ejemplo 8: Encontrar el número mayor

**Problema:** ¿cuál es el número más grande de la lista?

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

**Explicación:** la estrategia "el primero es el campeón": suponemos que el primero es el mayor y vamos retando al resto. Cuando uno supera al actual, se convierte en el nuevo mayor. Por eso el ciclo empieza en `i = 1`.

**Resultado esperado:**

```
El mayor es: 20
```

### Ejemplo 9: Encontrar el número menor

**Problema:** ¿cuál es el número más pequeño?

**Código:**

```js
const numeros = [4, 12, 7, 20, 3];
let menor = numeros[0];

for (let i = 1; i < numeros.length; i++) {
    if (numeros[i] < menor) {
        menor = numeros[i];
    }
}

console.log("El menor es:", menor);
```

**Explicación:** idéntico al ejemplo anterior, cambiando `>` por `<`. Cuando entiendes el patrón mayor, el menor es una variación de una línea.

**Resultado esperado:**

```
El menor es: 3
```

### Ejemplo 10: Separar pares e impares (`for` + `if`)

**Problema:** partir la lista en dos listas nuevas: pares e impares.

**Código:**

```js
const numeros = [1, 2, 3, 4, 5, 6];
const pares = [];
const impares = [];

for (let i = 0; i < numeros.length; i++) {
    if (numeros[i] % 2 === 0) {
        pares.push(numeros[i]);
    } else {
        impares.push(numeros[i]);
    }
}

console.log("Pares:", pares);
console.log("Impares:", impares);
```

**Explicación:** cuando un elemento cumple la condición, lo **pasamos** a otra lista con `push`. Aquí aparece una idea clave para no romper tus datos: en lugar de quitar elementos de la original mientras la recorres, **creas listas nuevas** y vas agregando. Es más seguro y fácil de razonar.

**Resultado esperado:**

```
Pares: [ 2, 4, 6 ]
Impares: [ 1, 3, 5 ]
```

### Ejemplo 11: Recorrer una lista con `while`

**Problema:** imprimir cada elemento usando `while`.

**Código:**

```js
const ciudades = ["Bogotá", "Medellín", "Cali"];
let indice = 0;

while (indice < ciudades.length) {
    console.log(ciudades[indice]);
    indice++;
}
```

**Explicación:** como recordarás de la [Guía 3](guia-while.md), `while` también recorre listas; solo tienes que actualizar el índice al final de cada vuelta. Úsalo cuando quieras combinar el recorrido con una condición de detención (por ejemplo, detenerte cuando un elemento cumpla algo).

**Resultado esperado:**

```
Bogotá
Medellín
Cali
```

### Ejemplo 12: Encontrar la posición de un elemento (a mano)

**Problema:** saber en qué posición está el primer `20`, sin usar `indexOf`.

**Código:**

```js
const numeros = [10, 15, 20, 25, 20];
const buscado = 20;
let posicion = -1;

for (let i = 0; i < numeros.length; i++) {
    if (numeros[i] === buscado) {
        posicion = i;
        break;
    }
}

if (posicion !== -1) {
    console.log("El 20 está en la posición", posicion);
} else {
    console.log("No está en la lista.");
}
```

**Explicación:** replicamos el comportamiento de `indexOf` para entender qué hay detrás: recorremos hasta encontrar el valor y guardamos su índice. `break` corta el ciclo en el primer hallazgo (recuerda: `break` de la guía 2). Si nunca lo encontramos, `posicion` sigue en `-1`, la misma convención de `indexOf`.

**Resultado esperado:**

```
El 20 está en la posición 2
```

### Ejemplo 13: Distintos tipos de listas

**Problema:** practicar con listas de textos, números y booleanos.

**Código:**

```js
const dias = ["lunes", "martes", "miércoles"];
const temperaturas = [18, 22, 19, 25];
const encendidos = [true, false, true];

console.log("Primer día:", dias[0]);
console.log("Última temperatura:", temperaturas[temperaturas.length - 1]);
console.log("¿Todo encendido?", encendidos[0] && encendidos[1] && encendidos[2]);
```

**Explicación:** las listas funcionan igual con cualquier tipo de dato. Para el último elemento usamos `temperaturas.length - 1`, porque el último índice válido es ese (sección 4). También puedes tener listas con **tipos mezclados** (`["Ana", 25, true]`) — JavaScript lo permite — pero para aprender conviene mantener listas coherentes: una lista de números para sumar, una de textos para buscar, etc. Mezclar tipos complica pensar el problema.

**Resultado esperado:**

```
Primer día: lunes
Última temperatura: 25
¿Todo encendido? false
```

### Ejemplo 14: El patrón que combina todo (listas + for + if)

**Problema:** mostrar las edades que son mayores o iguales a 18.

**Código:**

```js
const edades = [12, 18, 25, 15, 30];

for (let i = 0; i < edades.length; i++) {
    if (edades[i] >= 18) {
        console.log("Mayor de edad:", edades[i]);
    }
}
```

**Explicación:** este es el tipo de problema que las cuatro guías prepararon: una **lista** de datos (`for`), un filtro con condición (`if`) y una **acción** por cada elemento que la cumple. La versión con `for...of` (que lee cada valor directamente, sin índice) es equivalente y también la verás mucho:

```js
const edades = [12, 18, 25, 15, 30];

for (const edad of edades) {
    if (edad >= 18) {
        console.log("Mayor de edad:", edad);
    }
}
```

**Resultado esperado (ambas versiones):**

```
Mayor de edad: 18
Mayor de edad: 25
Mayor de edad: 30
```

### Ejemplo 15: Listas anidadas (solo una mirada)

**Problema:** tener una lista de listas, como una tabla.

**Código:**

```js
const agenda = [
    ["lunes", 9, true],
    ["martes", 14, false]
];

console.log(agenda[0][0]);   // "lunes"
console.log(agenda[1][2]);   // false
console.log(agenda[0]);      // ["lunes", 9, true]
```

**Explicación:** una lista puede contener otras listas: `agenda[0]` es la primera fila, y `agenda[0][0]` es el primer elemento de esa fila. Es útil para tablas, pero por ahora solo te presento la idea; no es el foco de esta guía. Con la práctica ganarás la lectura necesaria para usarlas con comodidad más adelante.

## 6. ¿Cómo pienso este problema?

Cuando el problema diga *"tengo una lista y quiero encontrar todos los elementos que cumplen X"*, el camino es siempre el mismo:

> **recorrer → preguntar → actuar**

1. **Recorrer:** un ciclo sobre toda la lista (casi siempre `for (let i = 0; i < lista.length; i++)`).
2. **Preguntar:** un `if` con la condición que define "X".
3. **Actuar:** hacer algo con los que cumplen: imprimir, contar, sumar, guardar en otra lista…

Veámoslo con *"elementos mayores que 10"*:

```js
const numeros = [4, 12, 7, 20, 3];

for (let i = 0; i < numeros.length; i++) {
    if (numeros[i] > 10) {
        console.log(numeros[i]);
    }
}
```

- ¿Qué información tengo? La lista `numeros`.
- ¿Qué necesito comprobar o repetir? Cada elemento.
- ¿Qué condición necesito? `> 10`.
- ¿Qué acción debo realizar? Imprimir.
- ¿Qué resultado necesito guardar? Ninguno acumulado aquí, pero en variaciones podría ser un contador, una suma o una lista nueva.

Si el problema no involucra "todos los que cumplen" sino "el primero que…", cambia la acción a "guardar y `break`" (ejemplo 12). La estructura mental es la misma.

## 7. Errores frecuentes

### Error 1: índice fuera de rango

```js
const frutas = ["manzana", "pera", "uva"];

for (let i = 0; i <= frutas.length; i++) {
    console.log(frutas[i]);
}
```

**Error:** `i <= frutas.length` visita los índices `0, 1, 2, 3`. El `3` no existe y se imprime `undefined`. Usa `i < frutas.length` (la regla de la sección 4).

### Error 2: confundir `length` con el último índice

```js
const frutas = ["manzana", "pera", "uva"];

console.log(frutas[frutas.length]);   // undefined
```

**Error:** `frutas.length` es `3`, pero no existe el índice `3`. El último elemento es `frutas[frutas.length - 1]`.

### Error 3: comparar listas con `===`

```js
const a = [1, 2];
const b = [1, 2];

console.log(a === b);   // false
```

**Error:** las listas se comparan por **referencia** (¿es *la misma* lista?), no por su contenido. Aunque tengan los mismos valores, son dos listas distintas y `===` responde `false`. Para saber si son iguales elemento a elemento, ¿qué harías? Exacto: recorrerlas y comparar posiciones. Este es un caso clásico donde la intuición se equivoca.

### Error 4: usar `=` en lugar de `===` dentro de un filtro

```js
if (numeros[i] = 10) {
    // asigna 10, no compara: modifica tu lista sin querer
}
```

El mismo error de la guía 1, pero ahora además **modifica los datos** de la lista. Comparación es `===`.

### Error 5: llamar a un método sin los paréntesis

```js
compra.push "huevos";    // Error de sintaxis
```

**Error:** `push`, `pop`, `includes`… son métodos: se llaman con `()`. Sin paréntesis solo estás haciendo referencia a la función, no ejecutando la acción. Recuerda `compra.push("huevos")`.

### Error 6: modificar la lista mientras la recorres sin un plan

```js
const numeros = [1, 2, 3, 4, 5];

for (let i = 0; i < numeros.length; i++) {
    if (numeros[i] % 2 !== 0) {
        numeros.splice(i, 1);   // borra... y los índices cambian
    }
}
```

**Error:** al eliminar elementos dentro del recorrido, los índices se "corren" y el ciclo se salta valores. El resultado es impredecible. Prefiere crear una lista nueva con los que cumplen la condición (como en el ejemplo 10).

## 8. Tips

- **Cuando sea "varios valores del mismo tipo", piensa en lista** antes que en muchas variables.
- **Recuerda el dúo:** primer elemento `[0]`, último `[length - 1]`.
- **Escríbete la regla de oro:** *"`length` es cuántos hay, no la última posición."*
- **Para agregar/quitar usa `push` (final), `pop` (final), `unshift` (inicio), `shift` (inicio)`.** Si solo vas a agregar al final, usa `push` y no pienses más.
- **`includes` para *"¿existe?"*, `indexOf` para *"¿en qué posición?"*.** Un `indexOf` que devuelve `-1` significa "no está".
- **No borres mientras recorres:** construye una lista nueva con `push`.
- **Prueba con una lista pequeña y visible** (3 o 4 elementos) antes de tu lista real. Recorrer en tu cabeza con valores chicos te ahorra depuración.
- **Ordena el `if` de un solo filtro para que se lea como una frase:** *"si el elemento es mayor que 10, entonces…"*.

## 9. Ejercicios

Resuelve por tu cuenta y verifica con `console.log`. Usa listas pequeñas para comprobar tu razonamiento.

### 🟢 Muy fáciles

1. **Mi lista de frutas.** Crea una lista de 4 frutas e imprime cada una con `for`.
2. **Extremos.** De tu lista anterior, imprime solo el primer y el último elemento sin recorrerla completa.
3. **¿Cuántos hay?** Imprime cuántos elementos tiene una lista.
4. **Compras.** Crea una lista con 3 artículos; agrega uno al final con `push`, uno al inicio con `unshift`, y quita el último con `pop`. Imprime la lista y el elemento quitado.

### 🟡 Básicos

5. **Suma.** Suma todos los números de `[8, 15, 22, 40, 5]` con un recorrido.
6. **Promedio.** Calcula el promedio de esa misma lista.
7. **Mayores que 10.** Cuenta cuántos números de esa lista son mayores que 10.
8. **¿Existe?** Dada una lista de nombres, responde si existe el nombre `"Sofía"` usando `includes`. Después repite con `indexOf` y muestra su posición.
9. **El mayor.** Encuentra el número más grande de la lista.

### 🟠 Intermedios

10. **Pares e impares.** Separa `[1, 2, 3, 4, 5, 6, 7, 8]` en dos listas nuevas y muéstralas.
11. **El menor y dónde está.** Encuentra el número menor de una lista y **su posición** en un solo recorrido.
12. **Frecuencia.** Cuenta cuántas veces aparece el número `4` en `[4, 2, 4, 9, 4, 1]`. Después intenta con `indexOf` en un ciclo para saltar de aparición en aparición (pista: busca desde la posición siguiente).
13. **Combinar listas.** Dadas dos listas de números, crea una tercera con la suma de las posiciones correspondientes: `[1, 2, 3]` y `[10, 20, 30]` → `[11, 22, 33]`.

### 🔴 Desafío

14. **Invertir una lista.** Crea una lista nueva con los elementos en orden inverso, sin usar métodos de inversión. Recorre la original desde el final.
15. **Duplicar.** Crea una lista nueva donde cada número original aparezca multiplicado por 2.
16. **FizzBuzz con lista.** Dada una lista de números, imprime por cada uno `"Fizz"` si es múltiplo de 3, `"Buzz"` si es múltiplo de 5 y `"FizzBuzz"` si es ambos.
17. **Calificaciones.** Dada una lista de notas, calcula el promedio y luego crea una lista con las notas que están por encima del promedio. Estamos combinando casi todo lo aprendido.

## 10. Chuleta rápida

```js
// Crear
const frutas = ["manzana", "pera", "uva"];
const vacia = [];

// Leer y escribir
frutas[0];            // "manzana"
frutas[1] = "mango";  // modificar
frutas.length;        // 3 (cuántos hay)

// Agregar y quitar
frutas.push("kiwi");     // agrega al final
const ultima = frutas.pop();      // quita el último
frutas.unshift("papaya"); // agrega al inicio
const primera = frutas.shift();     // quita el primero

// Preguntar
frutas.includes("pera");      // true / false
frutas.indexOf("pera");       // posición o -1

// Recorrer con for
for (let i = 0; i < frutas.length; i++) {
    console.log(frutas[i]);
}

// Recorrer con while
let indice = 0;
while (indice < frutas.length) {
    console.log(frutas[indice]);
    indice++;
}

// Recorrer con for...of (lo aprendiste en la guía for)
for (const fruta of frutas) {
    console.log(fruta);
}

// Patrón: recorrer → preguntar → actuar
const numeros = [4, 12, 7, 20, 3];
for (let i = 0; i < numeros.length; i++) {
    if (numeros[i] > 10) {
        console.log(numeros[i]);
    }
}

// Acumulador suma
let suma = 0;
for (let i = 0; i < numeros.length; i++) {
    suma += numeros[i];
}
console.log("Promedio:", suma / numeros.length);
```

## 11. Lo que viene después

Esta guía trabajó las listas con ciclos, que es la base correcta para pensar los problemas. En los próximos módulos verás métodos funcionales que JavaScript ofrece y que hacen el mismo trabajo con menos líneas, como `map`, `filter` y `reduce`:

```js
const numeros = [4, 12, 7, 20, 3];
const mayores = numeros.filter((numero) => numero > 10);
```

Esto filtra en una línea. Pero es importante que primero domines el recorrido con ciclos (como en esta guía): así, cuando veas `filter`, entenderás **qué está haciendo**, no solo cómo copiarlo. Además, en el curso estas listas se convertirán en el modelado de datos con objetos y en la base para mostrar listas en la interfaz con React.

## 12. ¿Qué puedo hacer ahora?

Ya tienes el trío completo:

- **`if`** → tomar decisiones.
- **`for`** → repetir una cantidad controlada de veces.
- **`while`** → repetir mientras una condición sea verdadera.
- **Listas (arrays)** → almacenar múltiples valores relacionados.

Y la combinación poderosaque tantos problemas resuelve:

```js
const edades = [12, 18, 25, 15, 30];

for (const edad of edades) {
    if (edad >= 18) {
        console.log("Mayor de edad:", edad);
    }
}
```

Ese patrón — **listas + for + if** — aparece en casi todo el código real: en un catálogo, en una tiendita, en filtros y búsquedas. Con estas cuatro guías tienes las bases para leer, escribir y explicar programas pequeños.

El siguiente paso en el curso es convertir este razonamiento en código con **tipo**: declarar qué contiene cada lista, cada variable y cada función con TypeScript. Todo lo que aprendiste aquí se mantiene; simplemente ganas herramientas para que el programa se revise a sí mismo. Continúa con el módulo [Fundamentos de TypeScript y lógica](README.md), cuya parte práctica es el [Taller de fundamentos](taller-fundamentos.md).