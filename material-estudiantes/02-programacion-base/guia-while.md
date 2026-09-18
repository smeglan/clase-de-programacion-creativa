# Guía 3: Repetir con `while`

## Objetivos

Al terminar esta guía deberías poder:

- Explicar qué problema resuelve `while` y en qué se diferencia de `for`.
- Leer y escribir un `while` con su condición, acción y actualización.
- Reconocer cuándo una repetición depende de una condición que cambia durante el proceso.
- Evitar y corregir ciclos infinitos.
- Usar `while` con contadores, acumuladores, `if`, `break` y `continue`.
- Elegir entre `for` y `while` según el problema, sin pensar que uno es "mejor" que otro.

> **Cómo probar los ejemplos**
>
> Como en las guías anteriores: guarda el código en un archivo y ejecútalo con `node ejemplo.js`, o pégalo en la consola del navegador (F12 → *Console*). En esta guía hay un ejemplo que **nunca termina**: si lo ejecutas, aprende a detenerlo (más abajo te explico cómo).

## 1. El problema

En la guía anterior decíamos que `for` sirve cuando puedes expresar claramente cuántas veces repetir. Pero muchos problemas no se comportan así:

- *"Repite mientras la contraseña sea incorrecta."*
- *"Sigue sumando mientras el total sea menor que 100."*
- *"Busca mientras queden elementos por revisar."*

En estos casos no sabes de antemano cuántas vueltas dará el ciclo: la cantidad depende de **una condición que puede cambiar dentro del propio ciclo**. Para eso existe `while`.

La diferencia conceptual:

> **`for`**: normalmente sé o puedo expresar claramente cómo controlar la cantidad de repeticiones.

> **`while`**: quiero repetir algo mientras una condición siga siendo verdadera.

## 2. Concepto

Un `while` evalúa una condición. Si es `true`, ejecuta el bloque y **vuelve a preguntar**. Repite ese ciclo *mientras* la condición siga siendo `true`. En el momento en que la condición da `false`, termina.

La clave está en el nombre: **mientras**. *"Mientras quede comida, come."* No se sabe cuántas veces: se decide en cada momento.

La responsabilidad de terminar no está en una "tercera parte" del ciclo (como en `for`): está en tu código. Algo dentro del bloque debe **cambiar una variable que la condición usa**, o el ciclo nunca terminará.

## 3. Sintaxis

```js
while (condicion) {
    // acción que se repite MIENTRAS la condición sea true
    // aquí debe cambiar algo que la condición usa
}
```

Las tres preguntas que debes responder antes de escribirlo:

1. **¿Cuál es la condición de entrada?** (qué debe ser `true` para entrar/continuar).
2. **¿Qué acción se repite?**
3. **¿Qué variable cambia** dentro del bloque para que la condición pueda volverse `false`**?**

Si la respuesta a la tercera pregunta no existe, tienes un ciclo infinito.

## 4. Advertencia: el ciclo infinito

Este código **nunca va a terminar**. No lo ejecutes a menos que sepas cómo detenerlo:

```js
let i = 0;

while (i < 5) {
    console.log(i);
}
```

**¿Por qué es incorrecto?** Dentro del bloque no cambia nada. `i` vale `0` y seguirá valiendo `0` para siempre, así que la condición `i < 5` siempre es `true`. El programa imprime `0`, `0`, `0`, … sin parar.

En la terminal de Node se detiene con `Ctrl + C`; en una pestaña del navegador, cierra o recarga la pestaña. Pero lo importante es entender que el programa **no tiene escape**: la pregunta siempre se responde igual.

La corrección:

```js
let i = 0;

while (i < 5) {
    console.log(i);
    i++;
}
```

**¿Qué cambió?** Ahora `i++` modifica la variable de la condición. Después de `0, 1, 2, 3, 4`, la condición `i < 5` da `false` con `i = 5` y el ciclo termina. La regla de oro de `while` es: *la condición debe poder dejar de cumplirse, y algo en el bloque debe hacer que eso ocurra.*

## 5. Ejemplos progresivos

### Ejemplo 1: Contar del 1 al 10

**Problema:** mostrar los números del 1 al 10 con `while`.

**Código:**

```js
let i = 1;

while (i <= 10) {
    console.log(i);
    i++;
}
```

**Explicación:** `i` empieza en 1. Se evalúa `i <= 10`: como es `true`, imprime y aumenta `i`. Al llegar a 11, la condición es `false` y se detiene. Para que el ciclo avance, el `i++` es obligatorio.

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

### Ejemplo 2: Qué ocurre en cada vuelta

**Problema:** entender paso a paso un `while`.

**Código:**

```js
let i = 1;

while (i <= 5) {
    console.log(i);
    i++;
}
```

**Explicación:** cada vuelta se llama **iteración**. El orden es siempre: condición → acción → (vuelta a la condición).

| Vuelta | ¿Cuánto vale `i` al entrar? | ¿Se cumple `i <= 5`? | Qué hace | ¿Cuánto vale `i` al salir? |
|---|---:|---:|---|---:|
| 1 | 1 | `true` | imprime `1` | 2 |
| 2 | 2 | `true` | imprime `2` | 3 |
| 3 | 3 | `true` | imprime `3` | 4 |
| 4 | 4 | `true` | imprime `4` | 5 |
| 5 | 5 | `true` | imprime `5` | 6 |
| — | 6 | `false` | el ciclo termina | — |

**Resultado esperado:**

```
1
2
3
4
5
```

### Ejemplo 3: Contar hacia atrás

**Problema:** mostrar los números del 5 al 1.

**Código:**

```js
let i = 5;

while (i >= 1) {
    console.log(i);
    i--;
}
```

**Explicación:** ahora la actualización resta (`i--`). La condición `i >= 1` se vuelve `false` cuando `i` llega a 0. Contar hacia atrás con `while` solo cambia la dirección de la actualización.

**Resultado esperado:**

```
5
4
3
2
1
```

### Ejemplo 4: Sumar hasta alcanzar una condición (acumulador)

**Problema:** sumar los números de 1 en adelante hasta el total más alto que no supere 100.

**Código:**

```js
let suma = 0;
let numero = 1;

while (suma + numero <= 100) {
    suma = suma + numero;
    numero++;
}

console.log("Suma total:", suma);
```

**Explicación:** aquí la condición no controla un número fijo de vueltas: depende de cómo va creciendo `suma`. Antes de sumar, nos aseguramos de que el resultado no se pase de 100. El número de iteraciones **no se conoce de antemano**: se decide en cada vuelta. Este es el caso típico para `while`.

**Resultado esperado:**

```
Suma total: 91
```

(Verifícalo: la suma del 1 al 13 es 91; la del 1 al 14 es 105, que se pasa.)

### Ejemplo 5: Repetir mientras una condición siga siendo cierta

**Problema:** partir de 100 y dividir a la mitad hasta que el valor sea menor que 1.

**Código:**

```js
let cantidad = 100;

while (cantidad >= 1) {
    console.log("faltan", cantidad);
    cantidad = cantidad / 2;
}

console.log("Terminó. Última cantidad:", cantidad);
```

**Explicación:** la condición se cumple mientras haya "algo" que dividir. Como `cantidad` se va dividiendo entre 2, cada vez se acerca más a 0 y **eventualmente** la condición deja de cumplirse. Fíjate que aquí no se cuenta con un entero: la actualización es otra operación válida (dividir, sumar, restar…).

**Resultado esperado:**

```
faltan 100
faltan 50
faltan 25
faltan 12.5
faltan 6.25
faltan 3.125
faltan 1.5625
faltan 0.78125
Terminó. Última cantidad: 0.390625
```

### Ejemplo 6: Recorrer una lista usando un índice

**Problema:** mostrar cada número de una lista usando `while`.

**Código:**

```js
const numeros = [4, 12, 7, 20, 3];
let indice = 0;

while (indice < numeros.length) {
    console.log(numeros[indice]);
    indice++;
}
```

**Explicación:** la condición usa `numeros.length` para no pasarse de las posiciones válidas. En este caso `for` resultaría más directo (toda la estructura cabe en una línea), pero es importante ver que `while` también puede recorrer listas, sobre todo cuando además quieres detenerte antes de tiempo (como en el próximo ejemplo).

**Resultado esperado:**

```
4
12
7
20
3
```

### Ejemplo 7: Buscar un valor y detenerse al encontrarlo

**Problema:** encontrar el primer número `7` en una lista y mostrar su posición.

**Código:**

```js
const numeros = [4, 12, 7, 20, 7, 3];
let indice = 0;
let encontrado = false;

while (!encontrado && indice < numeros.length) {
    if (numeros[indice] === 7) {
        encontrado = true;
        console.log("Encontré el 7 en la posición", indice);
    }
    indice++;
}

if (!encontrado) {
    console.log("El 7 no está en la lista.");
}
```

**Explicación:** la condición combina dos preguntas con `&&`: *"¿todavía no encontré el 7?"* **y** *"¿quedan elementos?"*. En cuanto `encontrado` pasa a `true`, la condición es falsa y el ciclo se detiene **sin recorrer el resto**. Ese comportamiento de "detente cuando algo suceda" es donde `while` brilla.

**Resultado esperado:**

```
Encontré el 7 en la posición 2
```

### Ejemplo 8: Simular intentos de contraseña

**Problema:** simular a una persona escribiendo contraseñas, con máximo 3 intentos.

**Código:**

```js
const contrasenaCorrecta = "1234";
const escritos = ["1111", "1234", "0000"];
let intentos = 0;
let permitido = false;

while (!permitido && intentos < escritos.length) {
    const texto = escritos[intentos];
    intentos++;

    if (texto === contrasenaCorrecta) {
        permitido = true;
        console.log("Acceso concedido en el intento", intentos);
    }
}

if (!permitido) {
    console.log("Se agotaron los intentos.");
}
```

**Explicación:** es un ejemplo muy realista: la cantidad de vueltas depende de cuándo (o si) se acierta. En una aplicación la lista `escritos` sería el texto que la persona escribe en un formulario; aquí la simulamos. Combinamos `while`, `if` y un límite de seguridad (`intentos < escritos.length`) para que el ciclo nunca quede atrapado.

**Resultado esperado:**

```
Acceso concedido en el intento 2
```

### Ejemplo 9: `while` + `if`

**Problema:** imprimir los números de una lista *mientras* sean negativos, y detenerse en el primero que no lo sea.

**Código:**

```js
const numeros = [-3, -1, 4, 7, 0];
let indice = 0;

while (indice < numeros.length && numeros[indice] < 0) {
    console.log(numeros[indice]);
    indice++;
}
```

**Explicación:** la condición del `while` incluye la pregunta sobre el elemento actual: *"¿quedan elementos Y este es negativo?"*. En cuanto aparece el `4`, la condición da `false` y el ciclo termina, sin tocar los números siguientes.

**Resultado esperado:**

```
-3
-1
```

### Ejemplo 10: `break`

**Problema:** detener un `while` antes de que termine su recorrido.

**Código:**

```js
let contador = 1;

while (contador <= 10) {
    if (contador === 5) {
        console.log("Detengo en", contador);
        break;
    }
    console.log(contador);
    contador++;
}
```

**Explicación:** `break` funciona igual que en `for`: salta **fuera** del ciclo completo en cuanto se ejecuta, sin evaluar la condición de nuevo.

**Resultado esperado:**

```
1
2
3
4
Detengo en 5
```

### Ejemplo 11: `continue`

**Problema:** imprimir del 1 al 10 saltándose los múltiplos de 3.

**Código:**

```js
let contador = 0;

while (contador < 10) {
    contador++;

    if (contador % 3 === 0) {
        continue;
    }

    console.log(contador);
}
```

**Explicación:** `continue` se salta solo esa vuelta. Observa un detalle clave: `contador++` está **antes** del `continue`. Si estuviera después, el `continue` saltaría la actualización y la condición quedaría congelada… ciclo infinito. Con `while` y `continue` tienes que cuidar que la actualización siempre ocurra.

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

## 6. For vs While

Ninguno es universalmente mejor: son herramientas para situaciones distintas.

| Criterio | `for` | `while` |
|---|---|---|
| Quién controla las repeticiones | la misma estructura (inicio, condición, paso juntos) | una condición que tú debes actualizar |
| Cuándo elegirlo | sabes o puedes expresar la cantidad de veces | la cantidad depende de una condición que cambia durante el proceso |
| Recorrer una lista por índice | directo: `for (let i = 0; i < l.length; i++)` | posible, pero más verboso |
| Detenerse "cuando algo suceda" durante el recorrido | con `break` | natural, hasta se escribe en la condición |
| Riesgo de ciclo infinito | menor (todo está a la vista en una línea) | mayor si olvidas la actualización |
| Ejemplo típico | tabla de multiplicar, visitar N elementos | intentos, sumar "hasta que…", esperar una condición |

**Guía rápida de decisión:** si en el enunciado piensas *"del X al Y"* o *"todos los elementos"*, usa `for`. Si piensas *"hasta que…"*, *"mientras…"* o *"cualquier cantidad que sea necesaria"*, usa `while`. Ambos son ciclos y casi todo se puede hacer con el otro, pero elegir el más claro hace que tu código se lea como tu intención.

## 7. ¿Cómo pienso este problema?

Para un `while`, usa esta secuencia:

> **condición de entrada → acción → actualización → condición de salida**

Con el problema *"sumar números hasta superar 500"*:

1. **Condición de entrada:** `while (suma <= 500)` — ¿qué debe ser `true` para seguir?
2. **Acción:** agregar el número siguiente a la suma.
3. **Actualización:** pasar al siguiente número (¡la parte que siempre debes revisar!).
4. **Condición de salida:** la misma condición de entrada, pero cuando deje de cumplirse: aquí, cuando `suma > 500`.

La condición de entrada y la de salida son **la misma condición vista desde los dos lados**: se entra mientras sea `true`, se sale cuando es `false`. Al escribir un `while`, pregúntate siempre: *"¿qué variable hace que esta condición pueda volverse `false`, y dónde se cambia?"*. Si no encuentras respuesta, estás a punto de escribir un ciclo infinito.

## 8. Errores frecuentes

### Error 1: olvidar la actualización (ciclo infinito)

```js
let i = 0;

while (i < 5) {
    console.log(i);
}
```

**Error:** nadie cambia a `i`. La condición siempre es `true`. Este es el error más típico de `while`, y ya lo viste en la sección 4. La corrección pide `i++` dentro del bloque:

```js
let i = 0;

while (i < 5) {
    console.log(i);
    i++;
}
```

### Error 2: `continue` después de la actualización

```js
let contador = 0;

while (contador < 10) {
    if (contador % 3 === 0) {
        continue;
    }
    contador++;
    console.log(contador);
}
```

**Error:** cuando `contador` es múltiplo de 3 (empezando por 0), el `continue` salta directamente a la condición sin ejecutar `contador++`. `contador` queda en el mismo valor… y el mismo vuelve a ser múltiplo de 3… ciclo infinito. Regla: en `while`, la actualización debe ocurrir **antes** de cualquier `continue` (o justo al inicio del bloque, como en el ejemplo 11).

### Error 3: una condición que nunca puede ser falsa

```js
let energia = 10;

while (energia > 0) {
    console.log("Energía:", energia);
    energia = energia + 1;   // ¡se está recargando, no gastando!
}
```

**Error:** la intención era gastar energía hasta que llegara a 0, pero la actualización la aumenta. La condición `energia > 0` será verdadera para siempre. Revisa que la actualización **vaya en la dirección** que apaga la condición.

### Error 4: condición falsa desde el principio → el ciclo no ejecuta

```js
let i = 10;

while (i < 5) {
    console.log(i);
}
```

**Explicación:** esto no es un error grave: a veces *"si no se cumple, no hago nada"* es exactamente lo que quieres. Pero es un comportamiento que debe ser intencional. Si esperabas que se ejecutara, revisa el valor inicial o la dirección de la comparación.

### Error 5: usar `=` en lugar de `===` en la condición

```js
let intento = 0;

while (intento = 3) {   // ¡asigna 3, no compara!
    console.log(intento);
}
```

**Error:** `=` asigna, no pregunta. Esto pone `intento` en 3 y la "condición" es un número distinto de 0, que es `true`… y nadie lo vuelve a cambiar. Lo mismo que aprendiste con `if` en la [Guía 1](guia-if.md).

### Error 6: querer predecir un número de vueltas que no es predecible

Usar `while` para algo que era un recorrido fijo (por ejemplo, "imprime del 1 al 100") no está mal, pero es más fácil equivocarse con la actualización. Si sabes cuántas veces repetir, `for` suele ser la opción más clara. Esto no hace mejor a `for`: te hace escribir la intención correctamente.

## 9. Tips

- **Escribe la condición como una pregunta natural:** *"¿la suma sigue siendo menor que 100?"*. Si no sabes formularla, no programes todavía.
- **Antes de ejecutar, traza una vuelta en tu cabeza:** ¿qué cambia? Si nada cambia, ya sabes lo que sigue.
- **Pon un límite de seguridad cuando simules entradas reales** (intentos, lecturas de usuario). Es buena práctica y evita programas colgados.
- **Mueve la actualización al inicio del bloque** si usas `continue`, para no olvidarla.
- **Elige `for` cuando la cantidad de vueltas esté clara y `while` cuando dependa de una condición.** No llevan "puntaje": lo importante es que el código diga lo que piensas.
- **Si tu ciclo no termina y no sabes por qué**, imprime el valor de la variable de la condición en la primera línea del bloque. Verás al instante si deja de avanzar.

## 10. Ejercicios

Resuelve por tu cuenta. Para cada uno, pregúntate primero: *"¿sé cuántas vueltas daré (mejor `for`) o depende de una condición (mejor `while`)?"*. En estos ejercicios la indicación es usar `while`.

### 🟢 Muy fáciles

1. **Cuenta con `while`.** Imprime los números del 1 al 10 usando `while`.
2. **Cuenta hacia atrás.** Imprime del 10 al 1.
3. **Pares.** Imprime los números pares hasta el 20 (avanza de 2 en 2).
4. **Primera suma.** Suma los números del 1 al 50 con un acumulador dentro de `while`.

### 🟡 Básicos

5. **Hasta superar 1000.** Suma de 1 en 1 hasta que el total supere 1000. ¿En qué número te detienes y cuál es el total?
6. **Las mitades.** Parte de 128 y muestra cada mitad hasta que el número sea menor que 1.
7. **Vocales con `while`.** Cuenta las vocales de una palabra recorriéndola con un índice y `while`.
8. **Múltiplos.** Imprime los primeros 6 múltiplos de 7 (con `while` y un contador de cuántos llevas impresos).

### 🟠 Intermedios

9. **Intentos limitados.** Simula una contraseña que debe ser `"creativa"` con máximo 3 intentos. Parte de una lista de textos simulados y muestra si se logró acceso o si se agotaron los intentos.
10. **Primer mayor que 50.** En la lista `[3, 18, 42, 77, 5]`, recorre con `while` y detente en el primer número mayor que 50. Muestra su valor y su posición.
11. **Hasta el primer negativo.** Recorre una lista y suma los elementos, deteniéndote en el primer número negativo (sin sumarlo).
12. **Doblando.** Empieza en 1 y multiplícalo por 2 mientras el resultado sea menor que 1000. Muestra cada valor de la secuencia.

### 🔴 Desafío

13. **División sin operador `/`.** Calcula cuántas veces cabe `b` dentro de `a` restando `b` de `a` repetidamente con `while`. Por ejemplo, con `a = 17` y `b = 5`, la respuesta es 3 (y sobran 2).
14. **Adivina el número.** Un número secreto vale `56`. Simula una secuencia de intentos y dan pistas: si el intento es menor, imprime "muy bajo"; si es mayor, "muy alto"; si acierta, detén el ciclo y muestra en cuál intento acertó. Asegúrate de que el ciclo no pueda ser infinito.
15. **Centinela.** Lee una lista mientras los números no sean `-1` y acumula su suma; al encontrar `-1`, detente y muestra el total. Prueba con una lista que contenga `-1` en posiciones distintas.

## 11. Chuleta rápida

```js
// Estructura base
let i = 0;

while (i < 5) {
    console.log(i);
    i++;            // ¡obligatorio para terminar!
}

// Contador hacia atrás
let j = 5;

while (j >= 1) {
    console.log(j);
    j--;
}

// Acumulador hasta condición
let suma = 0;
let numero = 1;

while (suma + numero <= 100) {
    suma += numero;
    numero++;
}

// Recorrer una lista
const numeros = [4, 12, 7, 20, 3];
let indice = 0;

while (indice < numeros.length) {
    console.log(numeros[indice]);
    indice++;
}

// Detenerse al encontrar (break o condición combinada)
let encontrado = false;
indice = 0;

while (!encontrado && indice < numeros.length) {
    if (numeros[indice] === 7) {
        encontrado = true;
    }
    indice++;
}

// continue: la actualización va ANTES
let contador = 0;

while (contador < 10) {
    contador++;
    if (contador % 3 === 0) {
        continue;
    }
    console.log(contador);
}
```

## 12. ¿Qué puedo hacer ahora?

Con `if`, `for` y `while` ya puedes **decidir** y **repetir**. El ingrediente que falta es organizar los datos: hasta ahora nuestras listas de números han aparecido "de paso". Es hora de estudiarlas a fondo.

En la siguiente guía aprenderás a **almacenar muchos valores relacionados** en una sola variable (una lista / array), a acceder a sus elementos, modificarlos, agregarlos y recorrerlos combinando todo lo visto: [Guía 4: Listas (arrays)](guia-listas.md).