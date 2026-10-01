# Guía 5: Fibonacci: la secuencia que suma

**Estás aquí:** [Inicio](../README.md) › [2. Fundamentos de TypeScript](README.md) › **Guía 5: Fibonacci: la secuencia que suma**

## Objetivos

Al terminar esta guía deberías poder:

- Explicar la regla de Fibonacci con tus palabras: cada término suma los dos anteriores.
- Contar la historia detrás de la secuencia (el problema de los conejos) y separar la leyenda del uso real.
- Entender la definición matemática F(0)=0, F(1)=1, F(n)=F(n−1)+F(n−2) y aplicarla a cualquier posición.
- Escribir tres versiones en [TypeScript](../glosario.md#typescript): iterativa, recursiva y con memoria.
- Explicar por qué la versión recursiva ingenua se vuelve **exponencial** y la diferencia práctica entre ella y las demás.
- Reconocer a la [proporción áurea](../glosario.md#proporcion-aurea) y a la [fórmula de Binet](../glosario.md#binet), y saber cuándo (no) conviene usarlas.
- Conectar el tema con la naturaleza de forma honesta, sin exagerar "magia" donde no la hay.

> **Cómo probar los ejemplos**
>
> Son funciones de TypeScript con tipos, como las del módulo `02`. Guarda el [código](../glosario.md#codigo) en un [archivo](../glosario.md#archivo) `.ts`, por ejemplo `fibonacci.ts`, y ejecútalo con:
>
> ```bash
> npx tsx fibonacci.ts
> ```
> Si `tsx` no está disponible, puedes quitar las anotaciones de tipo y guardar el archivo como `.js`:
> ```bash
> node fibonacci.js
> ```

## 1. La historia: el problema de los conejos

Esta secuencia no nació en un laboratorio: nació en un libro de aritmética del año 1202, el *Liber Abaci*, escrito por **Leonardo de Pisa**, un matemático italiano al que hoy conocemos como **Fibonacci** ("hijo de Bonaccio").

El libro fue importante porque le enseñó a Europa la numeración hindú-arábiga (los números que usamos hoy, con el cero) en lugar de los números romanos. Pero la parte famosa fue una pregunta "inocente" sobre conejos:

> Una pareja de conejos tarda un mes en madurar. A partir del segundo mes, cada pareja produce una pareja nueva cada mes. Si empezamos con una pareja recién nacida, ¿cuántas parejas hay cada mes?

Enchufando la regla "una pareja nueva = la suma de las parejas de los dos meses anteriores", la cuenta da:

```text
mes 1: 1
mes 2: 1
mes 3: 2
mes 4: 3
mes 5: 5
mes 6: 8
...
```

La secuencia que aparece es la de Fibonacci. La forma moderna, empezando en 0, es:

```text
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, ...
```

Dato curioso: el [modelo](../glosario.md#modelo) de conejos es una simplificación (los conejos no son eternos ni tan prolíficos), y la secuencia ya era conocida en la India siglos antes con otro nombre (relacionada con la métrica poética). La lección: los problemas simples de conteo pueden esconder patrones profundos, y nombrar un patrón no lo convierte en regla universal de la naturaleza.

## 2. El concepto: una regla de tres palabras

Fibonacci es una **secuencia** construida con una regla muy corta:

> **Cada término es la suma de los dos anteriores.**

Para arrancar necesitas dos valores semilla. Aquí usamos:

```text
F(0) = 0,  F(1) = 1
```

A partir de ahí el resto se calcula solo:

```text
F(2) = F(1) + F(0) = 1 + 0 = 1
F(3) = F(2) + F(1) = 1 + 1 = 2
F(4) = F(3) + F(2) = 2 + 1 = 3
...
```

Escribe la secuencia en papel hasta F(12) sin mirar arriba. Cuando lo logres, habrás entendido más que quien memorizó la lista de memoria.

## 3. La matemática: definición formal y propiedades

### Definición recursiva

La definición "clásica" de Fibonacci tiene exactamente la forma de las reglas que usamos al programar:

```text
F(0) = 0
F(1) = 1
F(n) = F(n − 1) + F(n − 2)   para n ≥ 2
```

Fíjate en la estructura: hay **casos base** (los que no se calculan, se conocen) y un **paso** que reduce un problema a versiones más pequeñas de sí mismo. Cuando escribas una [función](../glosario.md#funcion) recursiva, estarás usando exactamente esta idea.

### Algunas propiedades elegantes

Estas identidades no hay que memorizarlas, pero probarlas por tu cuenta es un excelente ejercicio de razonamiento:

- **Suma de los primeros n términos:**

  ```text
  F(0) + F(1) + ... + F(n) = F(n + 2) − 1
  ```

  Verifica con n = 6: 0+1+1+2+3+5+8 = 20 y F(8)−1 = 21−1 = 20.

- **Identidad de Cassini:**

  ```text
  F(n + 1) · F(n − 1) − F(n)² = (−1)ⁿ
  ```

  Con n = 5: F(6)·F(4) − F(5)² = 8·3 − 25 = 24 − 25 = −1, y (−1)⁵ = −1. Coincide.

- **Paridad:** F(n) es par exactamente cuando n es múltiplo de 3. Con n = 6: F(6) = 8 (par); con n = 7: F(7) = 13 (impar).

- **Primos entre sí:** dos términos consecutivos no comparten factores, excepto el 1. Es decir, el máximo común divisor de F(n) y F(n+1) es siempre 1.

### Conexión con el triángulo de Pascal

Suma las diagonales "inclinadas" del [triángulo de Pascal](../glosario.md#triangulo-de-pascal) y aparece Fibonacci:

```text
1
1 1        → 1
1 2 1      → 1 + 1 = 2
1 3 3 1    → 1 + 2 = 3
1 4 6 4 1  → 1 + 3 + 1 = 5
```

Las diagonales suman 1, 1, 2, 3, 5... Esta conexión aparece en combinatoria: el número de formas de subir una escalera dando pasos de 1 o 2 escalones es Fibonacci. Pruébalo con pocos escalones.

### La proporción áurea φ

Toma dos términos consecutivos y divide el mayor entre el menor:

```text
F(5)/F(4) = 5/3 = 1.666...
F(6)/F(5) = 8/5 = 1.6
F(10)/F(9) = 55/34 ≈ 1.6176
F(20)/F(19) = 6765/4181 ≈ 1.61803...
```

La razón se va acercando a un número fijo, la **proporción áurea**:

```text
φ = (1 + √5) / 2 ≈ 1.6180339887...
```

Ojos: no es que los términos "tiendan" a redondearse; el límite matemático de F(n+1)/F(n) es exactamente φ. Y φ tiene la [propiedad](../glosario.md#propiedad) curiosa de que φ² = φ + 1 (la misma lógica de "sumar la versión anterior").

### La fórmula de Binet (fórmula cerrada)

¿Se puede calcular F(n) sin recorrer la secuencia? Sí, con la **fórmula de Binet**:

```text
F(n) = (φⁿ − ψⁿ) / √5

donde φ = (1 + √5)/2   y   ψ = (1 − √5)/2 ≈ −0.618...
```

Compruébalo con F(10) mentalmente si puedes, y en código más abajo. Atención a la trampa: ψⁿ se hace muy pequeño al crecer n, así que con decimales de coma flotante hay que redondear el resultado. En código real casi siempre conviene más la versión iterativa (sin redondeo de coma flotante mientras el resultado sea un entero seguro de [JavaScript](../glosario.md#javascript)) que la fórmula "bonita".

## 4. En la naturaleza: qué hay y qué no hay de cierto

Es común leer que Fibonacci está "en todas partes". Lo que sí es real y medible:

- **Girasoles:** sus semillas forman espirales y el número de espirales en un sentido y en el otro suele ser un par de términos consecutivos de Fibonacci (por ejemplo, 34 y 55, o 55 y 89).
- **Piñas y alcachofas:** las escamas y brácteas se acomodan en espirales con conteos de Fibonacci.
- **Filotaxis (arreglo de hojas):** las hojas a lo largo de un tallo suelen girar en ángulos relacionados con φ, lo que permite que cada hoja reciba sol sin tapar la anterior.

Lo que es *exageración*: la concha del nautilo no es una espiral de Fibonacci exacta (es una **espiral logarítmica**, que sí se relaciona con φ por construcción, pero no "cuenta" términos de Fibonacci directamente). El patrón aparece porque la regla "cada parte se construye aprovechando la anterior" es una manera barata y estable de crecer. Es matemática, no magia.

## 5. Del patrón al código en TypeScript

### Versión iterativa (la recomendada para calcular)

```ts
function fibonacci(n: number): number[] {
  if (!Number.isInteger(n) || n < 0) {
    throw new Error("n debe ser un entero no negativo");
  }

  const serie: number[] = [0, 1];

  for (let i = 2; i < n; i++) {
    serie.push(serie[i - 1] + serie[i - 2]);
  }

  return serie.slice(0, n);
}

console.log(fibonacci(8)); // [0, 1, 1, 2, 3, 5, 8, 13]
console.log(fibonacci(1)); // [0]
console.log(fibonacci(2)); // [0, 1]
console.log(fibonacci(0)); // []
```

Puntos por explicar en voz alta:

- el [ciclo](../glosario.md#ciclo) empieza en `i = 2` porque las posiciones 0 y 1 ya están ocupadas por las semillas;
- `slice(0, n)` protege los casos de borde: pedir `1` no debe entregar `[0, 1]`.

### Versión recursiva (la definición matemática, y la trampa)

```ts
function fibo(n: number): number {
  if (!Number.isInteger(n) || n < 0) {
    throw new Error("n debe ser un entero no negativo");
  }
  if (n <= 1) return n;
  return fibo(n - 1) + fibo(n - 2);
}

console.log(fibo(10)); // 55
console.log(fibo(1));  // 1
console.log(fibo(0));  // 0
```

Esta versión es idéntica a la definición matemática: casos base + paso recursivo. Pero tiene un costo oculto: `fibo(5)` llama a `fibo(4)` y `fibo(3)`, y cada uno vuelve a recalcular lo mismo muchas veces. El número de llamadas crece **exponencialmente** (parecido a 2ⁿ o, más precisamente, a φⁿ). Prueba `fibo(40)` o `fibo(45)` y observa cuánto tarda. En la [guía 6: Ordenamientos](guia-ordenamientos.md) aprenderás a hablar de esto como *[complejidad](../glosario.md#complejidad) exponencial*.

### Versión con memoria (programación dinámica en miniatura)

La solución al problema del recálculo: guardar resultados ya calculados.

```ts
function fiboMemo(n: number, memo: Map<number, number> = new Map()): number {
  if (n <= 1) return n;

  const guardado = memo.get(n);
  if (guardado !== undefined) return guardado;

  const valor = fiboMemo(n - 1, memo) + fiboMemo(n - 2, memo);
  memo.set(n, valor);
  return valor;
}

console.log(fiboMemo(50)); // 12586269025
```

Con memoria, cada [valor](../glosario.md#valor) se calcula una sola vez: el trabajo pasa de exponencial a **lineal** (proporcional a n). Este "guardar para no repetir" se llama **memoización** y es la idea base de la [programación dinámica](../glosario.md#programacion-dinamica) que verás en [algoritmia aplicada](../07-algoritmia/README.md).

### La fórmula de Binet en código (por curiosidad, no como primera opción)

```ts
function fiboBinet(n: number): number {
  const raizDe5 = Math.sqrt(5);
  const phi = (1 + raizDe5) / 2;
  const psi = (1 - raizDe5) / 2;
  return Math.round((Math.pow(phi, n) - Math.pow(psi, n)) / raizDe5);
}

console.log(fiboBinet(10)); // 55
```

Funciona hasta cierto punto; con n grandes, los errores de redondeo de coma flotante pueden ensuciar el resultado. Úsala para entender la matemática, no para reemplazar a la iterativa.

### Comparación rápida de las versiones

| Versión | Trabajo | Fácil de leer | Uso |
|---|---|---|---|
| Iterativa | lineal (O(n)) | sí | La de todos los días |
| Recursiva ingenua | exponencial (~φⁿ) | sí (es la definición) | aprender recursión; evita con n grande |
| Con memoria | lineal (O(n)) | sí | cuando necesites solo el término n |
| Binet | constante | requiere matemática | curiosidad / valores aislados pequeños |

## 6. Resolver antes de seguir

> Para cada ejercicio [entrega](../glosario.md#entrega): el código, una salida de ejemplo, y una explicación de la estrategia.

### Nivel 0 - perderle el miedo

1. `fibonacci(10)` con la versión iterativa. Explica por qué el ciclo empieza en `i = 2`.
2. Explica qué entrega `fibonacci(1)` y `fibonacci(2)` y por qué no se desborda.

### Nivel 1 - aplicarlo

3. Escribe `esDeFibonacci(numero: number): boolean` que diga si un número pertenece a la secuencia (pista: genera términos hasta superarlo).
4. Escribe `primerTerminoSobre(valor: number): number` que devuelva la posición del primer término mayor que `valor`.
5. Modifica la iterativa para que empiece con semillas `1, 1` y compara con la `0, 1`. ¿En qué cambia la serie?

### Nivel 2 - la matemática y la memoria

6. Implementa `fiboMemo` y prueba `fiboMemo(100)` contra `fibo(40)`. Registra tiempos u observa la diferencia.
7. Verifica la identidad de Cassini en código para n del 2 al 10: `f(n+1)*f(n-1) - f(n)^2` debe alternar entre 1 y −1.
8. Verifica que la razón `f(n+1)/f(n)` se acerca a φ. Imprime la razón para n = 5, 10, 15, 20 y nota la convergencia.

### Nivel 3 - creatividad y profundización

9. Dibuja la espiral de Fibonacci: sobre un lienzo o con caracteres en la [terminal](../glosario.md#terminal), traza cuadrados de lado F(1), F(2), F(3)... y el arco que los conecta.
10. Cuenta las llamadas: agrega un [contador](../glosario.md#contador) a la versión recursiva y prueba cuántas llamadas hace `fibo(30)` (pista: el resultado te sorprenderá y es exponencial de verdad).
11. Investigando en la 07-algoritmia: explica con un ejemplo por qué "Fibonacci con y sin memoria" es un caso modelo de programación dinámica.

## 7. Vocabulario

Estas son las palabras que usa esta guía. Si alguna no te queda clara, el [glosario del curso](../glosario.md#capitulo-3) la explica con calma: qué es, un ejemplo y dónde la verás.

| Término | Definición corta |
| --- | --- |
| Secuencia | Lista ordenada de valores que sigue una regla. |
| Término | Un valor particular de la secuencia (F(6) es un término). |
| Caso base | Valor que se conoce sin calcular (en Fibonacci, F(0) y F(1)). |
| Paso recursivo | Regla que reduce el problema a versiones más pequeñas: F(n) = F(n−1) + F(n−2). |
| Recursión | Técnica donde una función se llama a sí misma. |
| Memoización | Guardar resultados ya calculados para no repetir trabajo. |
| Proporción áurea | φ = (1+√5)/2 ≈ 1.618; límite de la razón entre términos consecutivos. |
| Fórmula cerrada / de Binet | Fórmula que calcula F(n) sin recorrer la secuencia. |
| Exponencial | Crecimiento que se duplica o se multiplica por una base constante al crecer n. |
| Lineal | Crecimiento directamente proporcional a n. |

## 8. Herramientas para verlo

- **Numberphile** en YouTube (canal: `@Numberphile`): videos sobre Fibonacci, la proporción áurea y la espiral con explicaciones visuales.
- Dibuja tu propia versión: genera los términos y conviértelos en cuadrados o barras en la [consola](../glosario.md#consola). Comparar visualmente los tamaños de los cuadrados te da una intuición de "cómo explota" la secuencia.
- Cuando llegues al gráfico de complejidad de la [guía 6](guia-ordenamientos.md), ubica la curva **exponencial (2ⁿ)**: ahí vive la [recursión](../glosario.md#recursion) ingenua.

## 9. Lo que viene después

Fibonacci no es un tema aislado: te dio un modelo perfecto para ver las diferencias entre **lineal** (iterativa), **exponencial** (recursiva ingenua) y **con memoria** (no repites trabajo). Esas mismas palabras —lineal, exponencial, cuánto trabajo según el tamaño— son el corazón de la siguiente guía, donde las usarás para comparar formas de **ordenar** una lista. Continúa con la [Guía 6: Ordenamientos: bubble, merge y quicksort](guia-ordenamientos.md).

---

**Anterior:** [Guia 4: listas](guia-listas.md) · **Siguiente:** [Guia 6: ordenamientos](guia-ordenamientos.md)
