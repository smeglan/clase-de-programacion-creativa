# Clase 2: Fibonacci, ordenamiento y bubble sort

## Datos generales

- Duración presencial: 3 horas.
- Trabajo no presencial asociado: 9 horas.
- Unidad: Fundamentos de programación y pensamiento algorítmico.
- Resultado de aprendizaje: el estudiante reconoce una secuencia numérica con regla, implementa funciones sencillas con ciclos y explica qué significa ordenar datos y por qué conviene hacerlo.
- Meta de la clase: construir con confianza dos funciones desde cero —generar los primeros términos de Fibonacci y ordenar una lista con bubble sort— y entender, con palabras, en qué se diferencia el ordenamiento "simple" del ordenamiento "inteligente" (merge sort).

## Objetivos de la sesión

Al terminar la clase, el estudiante podrá:

1. explicar en palabras propias la regla de la secuencia de Fibonacci (cada término suma los dos anteriores);
2. escribir una función `generarFibonacci(n)` que devuelva los primeros `n` términos usando un ciclo;
3. explicar qué es ordenar y por qué importa en la vida real y en los programas;
4. escribir una función `bubbleSort(arr)` que ordene una lista de números de menor a mayor;
5. describir con palabras la idea general de merge sort (dividir en mitades y combinar ordenado) y compararla con bubble sort.

## Preparación docente

Antes de la clase, comprobar:

- Node.js y npm disponibles;
- que `npx tsx --version` descarga y ejecuta sin problemas (o preparar la alternativa sin tipos con `node archivo.js`);
- crear una carpeta de práctica por estudiante o grupo para que todas las funciones queden en archivos guardados;
- preparar en la pizarra o en diapositivas: números sueltos para la pasada de bubble sort, la serie inicial de Fibonacci y una lista ya terminada de referencia;
- preparar una pareja de ejemplos esperados para comprobar resultados:
  - `generarFibonacci(8) → [0, 1, 1, 2, 3, 5, 8, 13]`;
  - `bubbleSort([5, 2, 9, 1, 7]) → [1, 2, 5, 7, 9]`.

Los archivos de práctica se guardan con extensión `.ts` y se ejecutan con `npx tsx nombre.ts`. Si el entorno no permite `tsx` o no hay conexión, se pueden quitar las anotaciones de tipo y ejecutar con `node nombre.js`; la lógica es la misma.

## Núcleo común - 180 minutos

### 1. Explicación y demostración - 40 minutos

Para preparar definiciones, ejemplos y preguntas de comprobación, usar el [guion de explicación docente](guion-explicacion-docente.md).

Presentar en este orden y sin saltar pasos:

1. **La regla de Fibonacci:** cada término nuevo es la suma de los dos anteriores, empezando por `0, 1`. Escribir la serie en la pizarra pasando por cada término.
2. **De la regla al código:** transformar ese patrón en una función con un ciclo `for` que vaya agregando términos a una lista.
3. **Qué es ordenar:** acomodar datos siguiendo una regla (cartas de menor a mayor, fila de estaturas, notas de mayor a menor) y por qué importa: es más fácil encontrar algo en una lista ordenada.
4. **Bubble sort:** con números físicos o dibujados, mostrar una pasada completa: comparar de vecino en vecino, intercambiar cuando el de la izquierda es mayor y dejar que el más grande "suba como burbuja" hasta el final. Repetir para cada posición restante.
5. **Merge sort como idea (no se implementa):** dividir la lista en mitades, seguir dividiendo hasta tener elementos sueltos y volver a combinar montones ordenados. Comparar con bubble sort en una frase: bubble es el más simple de explicar, merge aprovecha dividir y hace menos trabajo con listas grandes.

Cerrar con dos o tres de las [preguntas de comprobación](guion-explicacion-docente.md#preguntas-de-comprobacion) antes de iniciar el ejercicio.

### 2. Ejercicio guiado - 40 minutos

Todos escriben en un archivo `primeros-pasos.ts` las dos funciones guiadas por el docente:

```ts
function generarFibonacci(n: number): number[] {
  const serie = [0, 1];

  for (let i = 2; i < n; i++) {
    serie.push(serie[i - 1] + serie[i - 2]);
  }

  return serie.slice(0, n);
}

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

El docente explica cada línea en voz alta y todos comprueban, uno a uno:

```ts
console.log(generarFibonacci(8));
console.log(generarFibonacci(1));
console.log(bubbleSort([5, 2, 9, 1, 7]));
console.log(bubbleSort([3, 3, 2, 1]));
```

Antes de pasar al laboratorio, todos deben poder ejecutar el archivo y ver 4 salidas.

### 3. Laboratorio y resolución de dudas - 60 minutos

Cada estudiante debe:

1. crear una segunda función `bubbleSortEnOrdenDescendente` cambiando solo la comparación (`<` en vez de `>`);
2. probar `generarFibonacci` con valores pequeños y con `n = 1` y `n = 2` para ver qué hace la función en el borde;
3. probar `bubbleSort` con: lista desordenada, lista ya ordenada, lista con números repetidos, lista de un solo elemento;
4. responder en su bitácora: ¿cuántas veces se compararon vecinos en `[3, 2, 1]`? ¿Qué cambió en cada vuelta?
5. si alguien termina antes, seguir con las [extensiones](apoyos-y-extensiones.md) del documento de apoyos.

El docente recorre el aula usando el checklist de `checklist-docente.md`, registra bloqueos frecuentes y propone apoyos o extensiones puntuales según sea necesario. No se corrige por nota; se corrige para comprender.

### 4. Taller, reflexión y cierre - 40 minutos

- Conversación breve en grupo: comparar las dos formas de ordenar. Preguntas guía: ¿cuál es más fácil de explicar? ¿Cuál haría menos trabajo con una lista de 1000 números? ¿Por qué crees?
- Cada estudiante registra en su bitácora: qué entendió, qué le costó y una pregunta que llevará a la próxima clase.
- Si el grupo ya usa Git en el portafolio, realizar un commit con los archivos de práctica; si no, basta con guardarlos en una carpeta ordenada.

## Observación y retroalimentación

Durante la sesión, observar si el estudiante puede explicar la regla de Fibonacci sin mirar el código, si se pierde o no en el ciclo anidado de bubble sort y si prueba casos de borde de forma autónoma. Un indicador clave de comprensión no es que el código corra: es que la persona sepa decir qué hace cada ciclo y qué cambia en cada vuelta.

## Trabajo no presencial - 9 horas

### Propuesta de continuidad

1. Reescribir `generarFibonacci` y `bubbleSort` desde cero, sin copiar el archivo de clase, y probar los tres casos de prueba (normal, límite e inválido).
2. Ordenar listas de texto por cantidad de letras y listas de números en orden descendente.
3. Escribir en la bitácora, con palabras propias, qué es ordenar y qué hace bubble sort "vuelta a vuelta".
4. (Opcional) Ver un video corto animado de bubble sort y de merge sort, y anotar qué diferencia se ve.

### Distribución sugerida

- 2 h: repasar regla de Fibonacci y practicar `generarFibonacci` con varios valores;
- 2 h: reescribir `bubbleSort` sin ayuda y probar casos de borde;
- 2 h: describir la idea de merge sort con palabras y un dibujo propio;
- 2 h: ordenar listas nuevas (textos por longitud, descendente);
- 1 h: escribir la bitácora y registrar bloqueos o preguntas para la próxima clase.

## Criterios de logro

- puede explicar la regla de Fibonacci con un ejemplo propio;
- escribe `generarFibonacci(n)` que funciona para `n` pequeño y para valores de borde como `1` y `2`;
- explica qué es ordenar y nombra una razón para ordenar datos;
- escribe `bubbleSort` que ordena correctamente listas desordenadas, con duplicados y de un elemento;
- describe con palabras la idea de merge sort y dice en qué se diferencia de bubble sort;
- prueba su código y explica sus resultados.