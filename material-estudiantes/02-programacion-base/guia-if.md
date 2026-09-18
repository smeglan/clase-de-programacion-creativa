# Guía 1: Tomar decisiones con `if`

## Objetivos

Al terminar esta guía deberías poder:

- Explicar qué problema resuelve `if` en un programa.
- Distinguir un valor booleano (`true` / `false`) de otros datos.
- Escribir condiciones con los operadores de comparación (`===`, `!==`, `>`, `<`, `>=`, `<=`).
- Combinar condiciones con los operadores lógicos (`&&`, `||`, `!`).
- Construir estructuras `if`, `if / else` e `if / else if / else`.
- Convertir un problema escrito en una condición.
- Reconocer y corregir los errores más frecuentes al usar `if`.

> **Cómo probar los ejemplos**
>
> Todo el código de esta guía es JavaScript básico. Puedes ejecutarlo de dos maneras:
>
> - **Con Node:** copia el código en un archivo como `ejemplo.js` y ejecuta `node ejemplo.js` en la terminal.
> - **Sin computadora propia o sin instalar nada:** abre la consola del navegador (F12 → pestaña *Console*), pega el código y presiona Enter.
>
> Si no tienes computadora en este momento, la consola del navegador de un celular también sirve para los ejemplos pequeños.

## 1. El problema

Un programa que solo hace lo mismo siempre no es muy útil. Casi cualquier problema real necesita **decidir**:

- ¿Es mayor de edad?
- ¿El número es par?
- ¿La contraseña es correcta?
- ¿Tiene saldo suficiente?

`if` es la herramienta que le permite a un programa elegir entre dos o más caminos según una condición.

## 2. Concepto

Una **condición** es una pregunta que tiene exactamente dos respuestas posibles:

- `true` (verdadero, sí, se cumple).
- `false` (falso, no, no se cumple).

A esos dos valores se les llama **booleanos**. En JavaScript se escriben tal cual:

```js
const esMayor = true;
const esMenor = false;
```

La idea de un `if` es:

> *"Si esta pregunta es verdadera, haz esto. Si no, haz otra cosa."*

Antes de escribir código, debes poder formular la pregunta en lenguaje natural. Por ejemplo: *"¿la edad es al menos 18?"*. Si no puedes formular la pregunta, probablemente todavía no tienes clara la regla.

## 3. Sintaxis

### `if` simple

```js
if (condicion) {
    // código que se ejecuta SOLO si la condición es true
}
```

### `if / else`

```js
if (condicion) {
    // código si la condición es true
} else {
    // código si la condición es false
}
```

### `if / else if / else`

```js
if (condicion1) {
    // código si condicion1 es true
} else if (condicion2) {
    // código si condicion1 es false y condicion2 es true
} else {
    // código si ninguna condición es true
}
```

Javascript evalúa las condiciones **en orden** y ejecuta únicamente el primer bloque que se cumple.

Una pregunta frecuente: ¿dónde va el `;`? En las estructuras de control **no se escribe `;` después de los `)`**. Los `;` van al final de las instrucciones internas.

## 4. Ejemplos progresivos

### Ejemplo 1: ¿Es mayor de edad?

**Problema:** mostrar un mensaje si la persona es mayor de edad.

**Código:**

```js
const edad = 18;

if (edad >= 18) {
    console.log("Eres mayor de edad.");
}
```

**Explicación:** el programa pregunta *"¿la edad es mayor o igual que 18?"*. Si la respuesta es `true`, ejecuta la línea de adentro. Si es `false`, no ejecuta nada.

**Resultado esperado:**

```
Eres mayor de edad.
```

Si cambias `edad` a `15`, el programa no imprime nada.

### Ejemplo 2: `if` / `else`

**Problema:** mostrar un mensaje según la edad, con respuesta para ambos casos.

**Código:**

```js
const edad = 15;

if (edad >= 18) {
    console.log("Eres mayor de edad.");
} else {
    console.log("Todavía eres menor de edad.");
}
```

**Explicación:** con `else` el programa siempre hace una de las dos cosas. No hay caso intermedio: si es `true` un mensaje, si es `false` el otro.

**Resultado esperado:**

```
Todavía eres menor de edad.
```

### Ejemplo 3: ¿Par o impar? (conocerás `%`)

**Problema:** decir si un número es par o impar.

**Código:**

```js
const numero = 7;

if (numero % 2 === 0) {
    console.log("El número es par.");
} else {
    console.log("El número es impar.");
}
```

**Explicación:** el operador `%` (módulo) devuelve el **resto** de una división. Por ejemplo, `7 % 2` es `1`, porque 7 entre 2 da 3 y sobra 1. Si el resto de dividir entre 2 es `0`, el número es par; si es `1`, es impar. Es una de las condiciones más usadas en programación.

**Resultado esperado:**

```
El número es impar.
```

### Ejemplo 4: Positivo, negativo o cero (`if / else if / else`)

**Problema:** clasificar un número en tres casos posibles.

**Código:**

```js
const numero = -3;

if (numero > 0) {
    console.log("El número es positivo.");
} else if (numero < 0) {
    console.log("El número es negativo.");
} else {
    console.log("El número es cero.");
}
```

**Explicación:** hace falta más de una decisión, pero solo puede cumplirse una. JavaScript revisa la primera pregunta; si falla, revisa la segunda; si tampoco, ejecuta el `else`. El orden importa: si la primera condición fuera `numero >= 0`, el cero caería en el primer bloque.

**Resultado esperado:**

```
El número es negativo.
```

### Ejemplo 5: Comparar dos números

**Problema:** dados dos números, decir si son iguales o cuál es mayor.

**Código:**

```js
const a = 10;
const b = 20;

if (a === b) {
    console.log("Los números son iguales.");
} else if (a > b) {
    console.log("El mayor es a:", a);
} else {
    console.log("El mayor es b:", b);
}
```

**Explicación:** `===` compara dos valores y responde `true` si son iguales. Las condiciones se revisan en orden y solo se ejecuta el primer bloque que se cumpla.

**Resultado esperado:**

```
El mayor es b: 20
```

### Ejemplo 6: Validar una contraseña

**Problema:** comprobar si la contraseña escrita es la correcta.

**Código:**

```js
const contrasena = "violeta123";

if (contrasena === "violeta123") {
    console.log("Contraseña correcta.");
} else {
    console.log("Contraseña incorrecta.");
}
```

**Explicación:** los strings también se comparan con `===`. La condición pregunta *"¿el texto de `contrasena` es exactamente 'violeta123'?"*.

**Resultado esperado:**

```
Contraseña correcta.
```

**Variación útil:** comprobar el largo de un texto con `length`:

```js
const contrasena = "hola";

if (contrasena.length < 8) {
    console.log("La contraseña es demasiado corta.");
} else {
    console.log("La contraseña tiene un largo válido.");
}
```

`contrasena.length` es un número: cuántos caracteres tiene el texto.

**Resultado esperado:**

```
La contraseña es demasiado corta.
```

### Ejemplo 7: Comprobar una nota

**Problema:** convertir una nota numérica en una palabra.

**Código:**

```js
const nota = 3.8;

if (nota >= 4.5) {
    console.log("Excelente");
} else if (nota >= 4.0) {
    console.log("Muy bien");
} else if (nota >= 3.0) {
    console.log("Aprobado");
} else {
    console.log("Reprobado");
}
```

**Explicación:** las condiciones van de la más exigente a la menos exigente. Como se evalúan en orden, `nota = 3.8` no cumple las dos primeras pero sí la tercera, así que imprime `Aprobado`. Si ordenaras las condiciones al revés, todas las notas caerían en la primera y ya no se evaluaría el resto.

**Resultado esperado:**

```
Aprobado
```

### Ejemplo 8: Clasificar una edad

**Problema:** decir a qué grupo de edad pertenece una persona.

**Código:**

```js
const edad = 25;

if (edad < 13) {
    console.log("Niño");
} else if (edad < 18) {
    console.log("Adolescente");
} else if (edad < 65) {
    console.log("Adulto");
} else {
    console.log("Adulto mayor");
}
```

**Explicación:** otro caso de `else if` encadenado. Nota que las condiciones se apoyan en las anteriores: al llegar a la segunda, ya sabemos que la edad es al menos 13.

**Resultado esperado:**

```
Adulto
```

### Ejemplo 9: Precio de una entrada según la edad

**Problema:** el precio base es 10 000. Los menores de 12 pagan la mitad y los adultos mayores de 65 entran gratis.

**Código:**

```js
const edad = 70;
const precioBase = 10000;
let precio = precioBase;

if (edad < 12) {
    precio = precio / 2;
} else if (edad >= 65) {
    precio = 0;
}

console.log("Precio de la entrada:", precio);
```

**Explicación:** aquí la variable `precio` se **modifica** dentro de la condición. Por eso se declara con `let` y no con `const`: su valor cambia. Este ejemplo muestra una diferencia importante entre **consultar** una variable y **modificarla**; el `if` modifica `precio` solo si corresponde.

**Resultado esperado:**

```
Precio de la entrada: 0
```

### Ejemplo 10: Varias condiciones a la vez (operadores lógicos)

**Operadores lógicos:**

- `&&` significa **y**: `true` solo si las dos condiciones son `true`.
- `||` significa **o**: `true` si al menos una de las dos es `true`.
- `!` significa **no**: invierte el valor (`!true` es `false`).

**Problema:** iniciar sesión solo si el usuario **y** la contraseña son correctos, siempre que la cuenta no esté bloqueada.

**Código:**

```js
const usuario = "ana";
const contrasena = "clave123";
const bloqueado = false;

if (usuario === "ana" && contrasena === "clave123" && !bloqueado) {
    console.log("Bienvenida, Ana.");
} else {
    console.log("No se pudo iniciar sesión.");
}
```

**Explicación:** las tres condiciones deben ser `true` para entrar. `!bloqueado` pregunta *"¿no está bloqueado?"*; como `bloqueado` es `false`, `!bloqueado` es `true`.

**Resultado esperado:**

```
Bienvenida, Ana.
```

**Otro ejemplo con `||`:**

```js
const dia = "domingo";

if (dia === "sábado" || dia === "domingo") {
    console.log("Es fin de semana.");
} else {
    console.log("Es entre semana.");
}
```

La respuesta es `true` si el día es sábado **o** si es domingo.

### Ejemplo 11: Condiciones anidadas

**Problema:** decidir si salir de casa según el clima y el trabajo.

**Código:**

```js
const llueve = true;
const hayTrabajo = false;

if (llueve) {
    if (hayTrabajo) {
        console.log("Salgo con paraguas.");
    } else {
        console.log("Me quedo en casa.");
    }
} else {
    console.log("Hace buen día para salir.");
}
```

**Explicación:** un `if` dentro de otro `if` se llama **anidación**. Se evalúa la condición exterior primero y, si se cumple, se evalúa la interior. Anidar es útil cuando una decisión solo tiene sentido si ya se tomó otra, pero a veces se puede evitar combinando condiciones con `&&`.

**Resultado esperado:**

```
Me quedo en casa.
```

### Ejemplo 12: Valores verdaderos o falsos (truthy y falsy)

En JavaScript, una condición no tiene que ser un booleano literal. Cada valor se comporta como verdadero o falso cuando se usa como condición:

- **Falsy** (se comportan como `false`): `false`, `0`, `""` (texto vacío), `null`, `undefined`, `NaN`.
- **Truthy** (casi todo lo demás): números distintos de 0, textos no vacíos, arreglos aunque estén vacíos.

**Problema:** saludar solo si hay un nombre escrito.

**Código:**

```js
const nombre = "";

if (nombre) {
    console.log("Hola, " + nombre);
} else {
    console.log("No has escrito tu nombre.");
}
```

**Explicación:** `""` es un texto vacío, y los textos vacíos son `falsy`. La condición `if (nombre)` es equivalente a *"si nombre no está vacío"*. Es cómodo, pero conviene saberlo para no asustarte cuando lo veas.

**Resultado esperado:**

```
No has escrito tu nombre.
```

### Ejemplo 13: Guardar la condición en una variable

**Problema:** decidir si se puede comprar cuando hay dinero, hay stock y no hay oferta de entrega mínima.

**Código:**

```js
const dinero = 50000;
const precio = 45000;
const disponible = true;

const puedeComprar = dinero >= precio && disponible;

if (puedeComprar) {
    console.log("Puedes comprar.");
} else {
    console.log("No puedes comprar.");
}
```

**Explicación:** la condición se guarda en una variable booleana con un nombre que explica la decisión: `puedeComprar`. Esto hace el `if` mucho más legible y permite probar la condición por separado. Regla práctica: si una condición es larga o importante, dale nombre.

**Resultado esperado:**

```
Puedes comprar.
```

### Ejemplo 14: `==` vs `===`

**Código:**

```js
console.log(1 == "1");   // true
console.log(1 === "1");  // false
```

**Explicación:** `==` compara los valores **convirtiendo los tipos de forma implícita**: convierte `"1"` en número y responde `true`. `===` compara **valor y tipo**: un número no es un texto, aunque se vean igual, así que responde `false`. Este comportamiento sorpresivo de `==` es la razón por la que en este curso (y en la mayoría de los proyectos serios) se recomienda usar siempre `===` y `!==`. Evitar `==` te ahorra errores difíciles de encontrar.

## 5. ¿Cómo pienso este problema?

Antes de escribir un `if`, responde estas cinco preguntas en orden:

1. **¿Qué información tengo?** (las variables disponibles: edad, nota, contraseña…).
2. **¿Qué necesito comprobar?** (la regla o pregunta del problema).
3. **¿Qué condición necesito?** (traducir esa regla a operadores).
4. **¿Qué acción debo realizar?** (qué debe ocurrir si se cumple y si no).
5. **¿Qué resultado necesito guardar?** (si hay que crear o modificar una variable).

Tu condición puede escribirse como una pregunta natural que responde `true` o `false`:

| Regla en palabras | Condición |
|---|---|
| "La edad es al menos 18" | `edad >= 18` |
| "El número es par" | `numero % 2 === 0` |
| "La contraseña coincide" | `contrasena === "clave123"` |
| "Tiene dinero y el producto está disponible" | `dinero >= precio && disponible` |
| "No está bloqueado" | `!bloqueado` |

Si la pregunta natural no se puede formular, todavía no entiendes el problema. Vuelve y piensa antes de escribir `if`.

## 6. Errores frecuentes

### Error 1: usar `=` en lugar de `===`

```js
if (edad = 18) {
    console.log("Mayor de edad.");
}
```

**Error:** `=` **asigna** un valor, no compara. Este código pone `edad` en 18 (lo modifica) y la condición queda como `true`. No es el error que parece: cambia tus datos y nunca detecta el problema como esperarías. Comparar se hace con `===`.

**Corrección:**

```js
if (edad === 18) {
    console.log("Tiene exactamente 18 años.");
}
```

### Error 2: poner `;` después de la condición

```js
if (edad >= 18); {
    console.log("Mayor de edad.");
}
```

**Error:** el `;` corta el `if` y el bloque `{ … }` se ejecuta **siempre**, sin importar la edad. Es un error muy silencioso: como la línea del `console.log` sigue funcionando, parece correcto hasta que pruebas con un menor de edad.

**Corrección:** quitar el `;`.

```js
if (edad >= 18) {
    console.log("Mayor de edad.");
}
```

### Error 3: dejar un `if` sin bloque `{ }`

```js
if (edad >= 18)
    console.log("Mayor de edad.");
    console.log("Puede votar.");
```

**Error:** sin llaves, el `if` solo controla la **primera** línea. `"Puede votar."` se imprime siempre. Cuando agregues una segunda línea, ya no quedará dentro de la condición.

**Corrección:** usar siempre `{ }`, aunque el bloque sea de una sola línea.

```js
if (edad >= 18) {
    console.log("Mayor de edad.");
    console.log("Puede votar.");
}
```

### Error 4: comparar tipos distintos con `==`

```js
if (numero == "10") {
    // se cumple aunque numero sea un número y "10" un texto
}
```

**Explicación:** `==` convierte los tipos y responde `true`. Mejor usar `===` para que el tipo importe.

### Error 5: ordenar mal las condiciones encadenadas

```js
const nota = 2;

if (nota < 5) {
    console.log("Reprobado");
} else if (nota < 3) {
    console.log("Mejorable");
}
```

**Error:** el primer `if` captura casi todas las notas, y el `else if` nunca se alcanza. Las condiciones deben ordenarse de la más **restrictiva** a la menos restrictiva, de modo que cada una descarte un rango.

### Error 6: complicar una condición con anidamientos innecesarios

```js
if (dinero >= precio) {
    if (disponible) {
        console.log("Puedes comprar.");
    }
}
```

Se puede — y se lee mejor — como una sola condición con `&&`:

```js
if (dinero >= precio && disponible) {
    console.log("Puedes comprar.");
}
```

## 7. Tips

- **Escribe primero la pregunta en palabras.** "¿La edad es al menos 18?" antes de escribir `edad >= 18`.
- **Usa `===` y `!==` siempre.** Evita `==` desde el principio; te hará más fácil encontrar errores.
- **Usa `{ }` siempre**, incluso con una sola línea. Evita el error 3.
- **Ordena los `else if` de la condición más exigente a la menos exigente.**
- **Pon nombre a las condiciones importantes:** `const puedeComprar = ...` es más claro que repetir la fórmula dentro del `if`.
- **Piensa en el caso límite.** Prueba con valores justo en el borde: `17` y `18` para la mayoría de edad, `0` para positivo/negativo, el caso exacto de la regla.
- **`else` no siempre es obligatorio.** Si no hay acción para el caso `false`, puedes omitirlo.

## 8. Ejercicios

Resuelve por tu cuenta. No hay soluciones en esta guía: la idea es que compares tu razonamiento con el código, no que copies una respuesta. Prueba cada ejercicio modificando valores.

### 🟢 Muy fáciles

1. **Número positivo o negativo.** Recibe un número y muestra un mensaje según su signo. ¿Qué pasa con el cero?
2. **Saludo condicional.** Si un nombre no está vacío, saluda; si no, pide que se escriba un nombre.
3. **Aprobado o reprobado.** Con una nota sobre 5, muestra `Aprobado` si es mayor o igual a 3, y `Reprobado` en caso contrario.

### 🟡 Básicos

4. **Edad y mensaje.** Clasifica a una persona como `Menor`, `Adulto` o `Adulto mayor` según su edad (usa los límites que prefieras y documenta tu decisión).
5. **Número entre dos.** Indica si un número está entre 10 y 20 (incluidos los extremos). Después cambia la regla para excluir los extremos.
6. **Descuento por monto.** Si una compra supera 50 000, aplica un 10 % de descuento y muestra el total final. Si no, muestra el precio sin cambios.
7. **¿Puede votar?** Una persona puede votar si tiene al menos 18 años Y no está inhabilitada (usa una variable booleana para la inhabilitación).

### 🟠 Intermedios

8. **Año bisiesto.** Un año es bisiesto si es divisible por 4, pero no por 100, salvo que también sea divisible por 400. Indica si el año que tienes es bisiesto. (Pista: ya conoces `%`.)
9. **Tipo de triángulo.** Con tres lados, indica si el triángulo es equilátero (los tres iguales), isósceles (dos iguales) o escaleno (todos distintos). Considera además qué pasa si los lados no forman un triángulo válido.
10. **Login completo.** Combina usuario, contraseña y bloqueo en una sola condición con `&&`. Agrega un mensaje distinto para cada caso de fallo usando `else if`.

### 🔴 Desafío

11. **Precio de entrada de un cine.** Una entrada cuesta 12 000. Los menores de 12 pagan 40 %, los de 65 o más entran gratis, y los estudiantes (usa una variable booleana) pagan 70 %. Calcula el precio final con combinación de condiciones. Prueba al menos cuatro casos.
12. **Evaluador de crédito.** Dadas una edad, un ingreso y un historial (booleano de "buen historial"), decide si se aprueba un crédito con esta regla: se aprueba si el ingreso supera 2 000 000 y la edad está entre 21 y 65, O si tiene buen historial y un ingreso de al menos 1 000 000. Nombra cada condición con una variable booleana antes del `if`.

## 9. Chuleta rápida

```js
// Booleanos
const verdadero = true;
const falso = false;

// Comparación (siempre === y !==)
a === b;   // igualdad (valor y tipo)
a !== b;   // desigualdad
a > b;     // mayor
a < b;     // menor
a >= b;    // mayor o igual
a <= b;    // menor o igual

// Lógicos
condicion1 && condicion2;  // true si ambas
condicion1 || condicion2;  // true si al menos una
!condicion;                // invierte

// if / else if / else
if (condicion) {
    // acción si es true
} else if (otraCondicion) {
    // acción si la primera falla y la segunda se cumple
} else {
    // acción si ninguna se cumple
}

// Ejemplo completo
const edad = 18;
const puedeComprar = edad >= 18 && tieneDinero;
if (puedeComprar) {
    console.log("Puedes comprar.");
}
```

## 10. ¿Qué puedo hacer ahora?

Con `if` un programa puede **decidir** una vez. Pero los problemas reales no hacen una sola pregunta: hacen miles.

*"Recorre esta lista de 50 edades y dime cuántas son mayores de edad."* Ahí necesitas tomar una decisión (el `if` de esta guía) **una y otra vez**. Eso es exactamente lo que resuelve el ciclo `for`:

- **`if`** decide (*¿se cumple esta condición?*).
- **`for`** repite la decisión (*hazlo para cada elemento*).

Cuando conectes ambos, podrás escribir el patrón que aparece constantemente al programar: **recorrer → preguntar → actuar**. Esa es la meta de la siguiente guía: [Guía 2: Repetir con `for`](guia-for.md).