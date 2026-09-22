# Checklist docente - Clase 2

## Antes de la clase

- [ ] Probé `npx tsx` con un archivo `.ts` de ejemplo o preparé la alternativa `node archivo.js` sin tipos.
- [ ] Preparé la serie de Fibonacci y la pasada de bubble sort para la pizarra.
- [ ] Preparé ejemplos de referencia: `generarFibonacci(8)` y `bubbleSort([5, 2, 9, 1, 7])`.
- [ ] Preparé una plantilla con huecos y un reto opcional de extensión.
- [ ] Decidí dónde guardarán cada estudiante sus archivos de práctica.

## Durante la clase

- [ ] Todos pueden explicar la regla de Fibonacci con un ejemplo propio.
- [ ] Todos escribieron y ejecutaron `generarFibonacci` con al menos dos valores.
- [ ] Todos pueden explicar qué hace el `slice(0, n)`.
- [ ] Todos escribieron y ejecutaron `bubbleSort`.
- [ ] Todos hicieron una copia de la lista (`[...numeros]`) antes de ordenar.
- [ ] Todos probaron al menos un caso de borde (lista de un elemento, duplicados o ya ordenada).
- [ ] Todos escucharon y pueden repetir la idea general de merge sort con palabras.
- [ ] Registré las necesidades de apoyo y las posibles extensiones por estudiante.
- [ ] Hice el cierre con la comparación bubble vs. merge y un checkpoint en la bitácora.

## Señales de apoyo adicional

- no logra formular la regla "sumar los dos anteriores" con un ejemplo;
- confunde las posiciones de la lista con los valores (piensa que `serie[i]` es "el número i");
- se pierde en el ciclo anidado aunque la pizarra esté a la vista;
- escribe `arr[i]` en vez de `arr[i + 1]` en la comparación sin darse cuenta;
- olvida que su función debe devolver un resultado (escribe pero no retorna);
- no prueba casos de borde o solo copia los ejemplos del profesor.

## Señales para proponer una extensión

- implementa `generarFibonacci` y `bubbleSort` sin mirar el ejercicio;
- varía la lógica por su cuenta (descendente, contar pasos, listas de texto);
- pregunta por qué bubble es lento o por qué merge divide el problema;
- intenta ordenar sin ayuda listas con duplicados o de textos;
- documenta y explica sus pruebas con ejemplos propios.