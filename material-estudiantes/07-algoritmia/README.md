# 7. Algoritmia aplicada (profundización opcional)

**Estás aquí:** [Inicio](../README.md) › **7. Algoritmia aplicada (profundización opcional)**

> **¿Te se frenó una palabra?** El [glosario del curso](../glosario.md#capitulo-8) explica complejidad, divide y vencerás, greedy y backtracking, una por una y con ejemplos.

Esta sección es una profundización opcional para cuando ya te sientas cómodo resolviendo los retos de fundamentos. Sus ideas pueden enriquecer cualquier proyecto.

## Preguntas antes del algoritmo

1. ¿Cuál es el problema exacto?
2. ¿Qué datos entran?
3. ¿Qué resultado debe salir?
4. ¿Qué casos especiales existen?
5. ¿Cómo sabemos que la solución es correcta?
6. ¿Podemos comparar dos estrategias?

## Divide y vencerás

Divide un problema en partes más pequeñas, resuelve cada parte y combina los resultados. Ejemplos clásicos son la [búsqueda binaria](../glosario.md#busqueda-binaria) y [merge sort](../glosario.md#merge-sort). Separar una interfaz en componentes puede ayudar a organizar código, pero por sí solo no es un algoritmo de divide y vencerás.

## Ejemplos con código

Estos ejemplos usan TypeScript, que también puedes ejecutar en un proyecto Vite. Léelos paso a paso: identifica la entrada, la salida, la estrategia y los casos que conviene revisar.

### Búsqueda binaria: divide y vencerás

Busca un número en una lista **ordenada**. En cada paso descarta la mitad donde el valor no puede estar. Devuelve la posición o `-1` si no aparece.

```ts
function busquedaBinaria(valores: number[], objetivo: number): number {
  let izquierda = 0;
  let derecha = valores.length - 1;

  while (izquierda <= derecha) {
    const medio = Math.floor((izquierda + derecha) / 2);
    const actual = valores[medio];

    if (actual === objetivo) return medio;
    if (actual < objetivo) izquierda = medio + 1;
    else derecha = medio - 1;
  }

  return -1;
}

const numeros = [2, 5, 8, 12, 16, 23, 38];
console.log(busquedaBinaria(numeros, 16)); // 4
console.log(busquedaBinaria(numeros, 7));  // -1
```

La lista debe estar ordenada. Si no lo está, primero habría que ordenarla o elegir otra búsqueda. Para una lista de `n` elementos, la búsqueda binaria tarda aproximadamente `log₂(n)` pasos en el peor caso.

### Fibonacci con memoria: programación dinámica

La definición recursiva de Fibonacci repite cálculos. Esta versión guarda los resultados anteriores en un arreglo y los reutiliza.

```ts
function fibonacci(n: number): number {
  if (!Number.isInteger(n) || n < 0) {
    throw new Error("n debe ser un entero no negativo");
  }

  const memoria: number[] = [0, 1];
  for (let i = 2; i <= n; i++) {
    memoria[i] = memoria[i - 1] + memoria[i - 2];
  }
  return memoria[n];
}

console.log(fibonacci(0)); // 0
console.log(fibonacci(1)); // 1
console.log(fibonacci(7)); // 13
```

Cada posición se calcula una sola vez: el tiempo crece linealmente con `n`. La memoria también crece linealmente. Piensa qué cambiarías para calcular solo con las dos posiciones anteriores.

### Cambio de monedas: greedy y contraejemplo

La estrategia greedy elige repetidamente la moneda de mayor valor que no exceda lo que falta. En algunas denominaciones da una respuesta óptima, pero no siempre.

```ts
function cambioGreedy(denominaciones: number[], monto: number): number[] {
  const monedas = [...denominaciones].sort((a, b) => b - a);
  const resultado: number[] = [];
  let falta = monto;

  for (const moneda of monedas) {
    while (moneda <= falta) {
      resultado.push(moneda);
      falta -= moneda;
    }
  }

  if (falta !== 0) throw new Error("No se puede formar ese monto");
  return resultado;
}

console.log(cambioGreedy([1, 5, 10, 25], 30)); // [25, 5]
console.log(cambioGreedy([1, 3, 4], 6));       // [4, 1, 1]
// Para [1, 3, 4], la solución óptima es [3, 3].
```

El segundo caso demuestra que una decisión local razonable puede llevar a una solución peor. La estrategia encuentra una forma de pagar, pero no garantiza usar la menor cantidad de monedas para cualquier conjunto de denominaciones.

### Suma de subconjunto: backtracking

Prueba incluir o no incluir cada número. Si la suma ya supera el objetivo (con números positivos), esa rama no necesita seguir explorándose.

```ts
function existeSuma(valores: number[], objetivo: number): boolean {
  function explorar(indice: number, suma: number): boolean {
    if (suma === objetivo) return true;
    if (suma > objetivo || indice === valores.length) return false;

    // Explora dos posibilidades: usar este valor o saltarlo.
    return explorar(indice + 1, suma + valores[indice]) ||
      explorar(indice + 1, suma);
  }

  return explorar(0, 0);
}

console.log(existeSuma([2, 4, 7, 9], 11)); // true: 2 + 9
console.log(existeSuma([2, 4, 7, 9], 5));  // false
```

Backtracking puede explorar muchas combinaciones; aquí hay hasta `2ⁿ` posibilidades para `n` valores. La poda `suma > objetivo` ayuda porque los valores son positivos. Si permites números negativos, esa poda ya no es válida.

### Para explorar

- Cambia la búsqueda binaria para devolver `true` o `false` en vez del índice.
- Calcula Fibonacci guardando solo los dos resultados anteriores.
- Prueba greedy con varios sistemas de monedas y busca uno donde falle.
- Modifica `existeSuma` para devolver también los números que forman la solución.

## Programación dinámica

Busca problemas con subproblemas repetidos. Guarda resultados ya calculados para no resolver lo mismo muchas veces.

Ejemplos atractivos para visualizar:

- cambio de monedas;
- camino de menor costo;
- mochila;
- Fibonacci con y sin memoria;
- planificación de tareas.

## Greedy

Toma la mejor decisión local disponible. Es rápido y elegante en algunos problemas, pero debes demostrar cuándo funciona y cuándo puede fallar.

## Backtracking

Explora posibilidades y retrocede cuando una decisión lleva a un callejón sin salida. Es útil para laberintos, combinaciones, sudoku y configuraciones.

---

**Anterior:** [Vibecoding responsable](../06-vibecoding/README.md) · **Siguiente:** [Proyecto final](../08-proyecto-final/README.md)
