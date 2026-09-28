# Guion docente: explicación de la clase 2

## Idea central

La meta de hoy no es aprender sintaxis nueva. Es darse cuenta de que un problema "aterrador" como Fibonacci o "ordenar" se convierte en una receta de pasos pequeños que cualquier persona puede entender y escribir. Si el grupo sale diciendo "eso no era tan difícil", la clase cumplió su propósito.

El hilo conductor:

```text
regla (cómo crece la serie) → ciclos (repito el paso) → función (le pongo nombre y la reutilizo)
acomodar datos (qué es ordenar) → pares de vecinos (cómo funciona bubble) → dividir y combinar (cómo funciona merge)
```

## Guion para explicar

### 0–4 min: abrir la clase

Puedes decir:

> Buenas noches. La clase pasada vimos que una idea se puede convertir en código, y ese código en algo visible. Hoy vamos a usar la parte más pura de programar: la lógica, sin pestañitas ni botones. Solo vamos a resolver dos "acertijos" con la terminal. Y les adelanto: ninguno es tan difícil como parece.

Comprueba el entorno con el grupo:

> ¿Quién tiene Node funcionando? Vamos a escribir archivos `.ts` y ejecutarlos con `npx tsx nombre-del-archivo.ts`. Si a alguien se le complica, no pasa nada: lo resolvemos juntos y si no, quitamos los tipos y usamos `node`.

### 4–14 min: el acertijo de Fibonacci

Empieza con el patrón, no con el código:

> Fibonacci es una secuencia de números con una regla muy tonta: cada número es la suma de los dos anteriores. Nada más. Empieza así: 0, 1. ¿Cuál sigue? 0 + 1 = 1. ¿Después? 1 + 1 = 2. ¿Después? 1 + 2 = 3. ¿Después? 2 + 3 = 5. Y así para siempre.

Escribe en la pizarra a medida que el grupo te dicta:

```text
0, 1, 1, 2, 3, 5, 8, 13, 21, ...
```

Refuerza la regla con un ejemplo tirado a la vida:

> Es como construir una escalera donde cada escalón se apoya en los dos anteriores. Por eso se ve tanto en la naturaleza: en las espirales de los girasoles, en las piñas, en las conchas. Pero no necesitan saber nada de eso para programarlo; solo necesitan la regla: sumar los dos de atrás.

Guarda el código para después del concepto:

> El truco es que un programa puede hacer eso solo: guardar la serie y, cada paso, pedir "el de la posición anterior más el de dos posiciones antes". Eso se logra con un ciclo.

### 14–24 min: de la regla al código

Escriban la función juntos, línea por línea, y explica cada parte:

```ts
function generarFibonacci(n: number): number[] {
  const serie = [0, 1];

  for (let i = 2; i < n; i++) {
    serie.push(serie[i - 1] + serie[i - 2]);
  }

  return serie.slice(0, n);
}
```

Puedes decir:

> La lista arranca con 0 y 1. El ciclo empieza en la posición 2 porque las posiciones 0 y 1 ya están ocupadas. En cada vuelta agrego la suma de los dos números que quedaron antes. Y la última línea solo me asegura de no entregar de más si piden un número chiquito.

Pide que corran el ejemplo y comparen:

```ts
console.log(generarFibonacci(8));
console.log(generarFibonacci(1));
```

> Con 8 sale la lista que dictamos. Con 1 sale `[0]`. Esa última línea `slice` es la que evita que el programa "se emborrache" entregando 0 y 1 aunque pidamos solo uno.

### 24–34 min: qué es ordenar y por qué importa

> Antes del código, una pregunta de la vida real: si tienen un mazo de cartas revuelto y quieren encontrar el 7 de tréboles, ¿cómo lo buscan? Ahora, si el mazo está ordenado del 1 al rey, ¿cómo lo buscan? En un orden conocido, encontrarlo es instantáneo: saben por dónde ir.

Conecta con que esto no es un lujo:

> Ordenar es acomodar los datos siguiendo una regla: de menor a mayor, de mayor a menor, por fecha, por letras. Casi todo lo que hacen las aplicaciones detrás de escena usa listas ordenadas. Ordenar no es "hacer bonito"; es hacer que encontrar cosas sea rápido y predecible.

### 34–44 min: bubble sort paso a paso

Usa números físicos o dibujados en la pizarra, por ejemplo `5 2 9 1`:

> Bubble sort se ve lento, pero la idea es tonta y eso es bueno: camino de vecino en vecino y, si el de la izquierda es mayor que el de la derecha, los cambio. Al terminar la pasada, el número más grande quedó al final, como una burbuja que subió. Repito esa pasada con lo que queda.

Demuestra una pasada completa en la pizarra:

```text
5 2 9 1   →  2 5 9 1   (cambio 5 y 2)
2 5 9 1   →  2 5 9 1   (5 < 9, no cambio)
2 5 9 1   →  2 5 1 9   (cambio 9 y 1)

Fin de la vuelta: el 9 ya está en su lugar. Ahora repito con 2 5 1.
```

Aclara el detalle por el que "se encoge" el rango:

> Fíjense que en cada vuelta ya no necesito llegar tan lejos: lo que ya subió ya está ordenado.

Escriban juntos:

```ts
function bubbleSort(numeros: number[]): number[] {
  const arr = [...numeros];

  for (let vuelta = 0; vuelta < arr.length - 1; vuelta++) {
    for (let i = 0; i < arr.length - 1 - vuelta; i++) {
      if (arr[i] > arr[i + 1]) {
        const temporal = arr[i];
        arr[i] = arr[i + 1];
        arr[i + 1] = temporal;
      }
    }
  }

  return arr;
}
```

Puedes decir:

> Hay dos ciclos y eso asusta al principio, pero es solo "repetición de una repetición": el ciclo de adentro camina de vecino en vecino haciendo cambios; el ciclo de afuera dice "haz otro recorrido, pero ahora una casilla menos de largo". Si separan el problema en esos dos trabajos, el código se lee solo.
>
> Y ojo: la primera línea con `[...numeros]` hace una **copia**. Ordenamos una copia, no la lista original. En muchos programas cambiar los datos de entrada por accidente es un error grave; esta es una costumbre que les va a salvar después.

### 44–50 min: merge sort, el ordenamiento "inteligente" (solo hablar de él)

No se implementa hoy. Solo se muestra la idea:

> Bubble sort es fácil de entender, pero con mil números hace muchísimas comparaciones. Existe otro método que usa una idea famosa: dividir y vencerás. En vez de ordenar toda la lista de una, la parto por la mitad, y esa mitad en otra mitad, hasta tener tarjetas sueltas. Y luego las voy juntando de dos en dos, ya ordenadas, hasta reconstruir el mazo entero. A eso le llaman merge sort.

Usa la analogía de un mazo:

> Imaginen separar un mazo en dos montones, cada montón en dos más, hasta tener cartas sueltas. Después toman dos cartas sueltas, las dejan ordenadas, y se convierten en un montoncito ordenado de dos. Juntan dos montoncitos ordenados y el resultado es un montón ordenado de cuatro. Ese es el juego: dividir hasta lo mínimo y luego combinar en orden.

Cierra la comparación:

> No tienen que escribirlo hoy. Pero ya conocen dos maneras de llegar al mismo resultado: una simple y repetitiva (bubble) y otra que divide el problema para tardar menos. Cuando en el curso lleguemos a algoritmia avanzada, van a aprender a medir esa diferencia con números.

### 50–56 min: demostración narrada y errores esperados

Mientras el grupo escribe y corre:

> Cuando algo no funcione, lean el error de arriba: les dice el archivo y la línea. Espera: ¿de verdad quiero comparar `arr[i]` con `arr[i]`? No: con `arr[i + 1]`. Ése es el error clásico de bubble sort y todo el mundo lo comete alguna vez.

### 56–60 min: cierre de la explicación

> Vamos a hacer el ejercicio en dos tandas. Primero coparten los comandos y la función de Fibonacci; después pasamos a ordenar. Si algo se atora, no es falta de inteligencia: es un problema nuevo de un tipo que aprendemos a resolver.

## Definiciones para repasar

| Término | Definición corta |
| --- | --- |
| Secuencia | Lista de valores que siguen un orden. |
| Término | Cada valor de una secuencia (el número 8 es el término de la serie, no la serie completa). |
| Regla (de la serie) | Instrucción que define cómo se construye cada valor nuevo. En Fibonacci: sumar los dos anteriores. |
| Ordenar | Acomodar datos siguiendo una regla de comparación (menor a mayor, por fecha, etc.). |
| Vecino | Elemento adyacente en la lista (en la posición siguiente o anterior). |
| Pasada / vuelta | Recorrido completo de la lista comparando de vecino en vecino. |
| Intercambio (swap) | Cambiar de lugar dos valores usando una variable temporal. |
| Copia de lista | Nueva lista con los mismos valores, para no alterar la original al procesarla. |
| Divide y vencerás | Estrategia: partir un problema en partes pequeñas, resolver cada parte y combinar resultados. |
| Bubble sort | Ordenamiento simple que compara vecinos y "sube" el mayor hasta el final en cada pasada. |
| Merge sort | Ordenamiento que divide la lista en mitades y combina montones ya ordenados. |
| Abstracción | Dejar fuera los detalles que no importan y modelar solo lo que el programa necesita. |
| Clase | Plantilla que describe datos y operaciones; se convierte en objetos al instanciarla con `new`. |
| Método | Función que pertenece a un objeto y usa sus datos. |
| Encapsulamiento | Exponer un dato mediante lecturas y cambios controlados, en vez de dejarlo abierto. |

## Errores de explicación que conviene evitar

- Fibonacci no empieza en `1, 1` ni en `1, 2`: aquí usamos `0, 1` como arranque; decir la regla siempre con los dos primeros términos.
- No llenar la explicación de jerga matemática (sucesión, recursión, inducción): hoy la recursión no se ve y solo confundiría.
- Un ciclo anidado no es "un bucle de terror": es una repetición que ocurre dentro de otra repetición; explicarlo como dos trabajos separados.
- Bubble sort no debe mutar la lista original en el ejemplo: la función ordena una copia (`[...numeros]`). Si el grupo compara lista de entrada y de salida, deben notar que la original no cambió.
- No decir que merge sort "es mejor porque es más largo": es mejor porque con listas grandes hace menos comparaciones; hoy solo se anuncia, no se demuestra.
- `generateFibonacci` con `n` pequeño usa `slice`; sin él, pedir `1` entregaría `[0, 1]` por error. Este caso de borde es parte del aprendizaje, no un detalle menor.

## Preguntas de comprobación

Usa dos o tres antes de iniciar el ejercicio:

1. ¿Cuál es la regla de Fibonacci? Escríbanla con un ejemplo que no sea de la pizarra.
2. ¿Por qué el ciclo de `generarFibonacci` empieza en `i = 2`?
3. ¿Qué hace el `slice(0, n)` al final y en qué caso evita un error?
4. ¿Qué significa "ordenar" y den un ejemplo de la vida diaria?
5. ¿Cuál es el paso básico que se repite en bubble sort?
6. ¿Qué pasa en la pizarra si no hago el segundo recorrido en bubble sort?
7. ¿En qué idea se apoya merge sort y cuál es la diferencia clave con bubble?
8. ¿Por qué hacemos una copia de la lista con `[...numeros]` antes de ordenar?

## Si alguien pregunta "¿cuándo uso objetos?"

No es un tema de la clase, pero la pregunta aparece en cuanto el grupo empieza a pensar en datos reales ("¿y si tengo muchos productos?"). No la conviertas en un bloque nuevo: responde en una frase y remite a la guía.

Puedes decir:

> Los datos que siempre viajan juntos se pueden agrupar en un objeto, y si además comparten las mismas operaciones, en una clase. Pero lo importante no es la clase: es decidir qué datos vale la pena guardar y cuáles no. Eso se llama abstracción, y hay una guía entera sobre eso.

Remisión: [Guía 7: Programación orientada a objetos](../../material-estudiantes/02-programacion-base/guia-poo.md). Sirve como lectura opcional entre esta clase y el módulo 03, y su sección 2 conecta con lo que ya se hizo hoy en la pizarra: por qué `bubbleSort` recibe una **copia** y no la lista original. Eso es abstracción de interfaz: quien llama no necesita saber cómo están guardados los datos por dentro.