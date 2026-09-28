# Clase 2: apoyos y extensiones

Este documento acompaña una única experiencia de clase. No divide al grupo: ofrece opciones para responder a necesidades concretas mientras todos construyen sus dos funciones con comprensión.

## Apoyo puntual

Úsalo cuando una persona no logra arrancar con la regla, se pierde en el ciclo anidado o se bloquea con los casos de borde.

### 1. La regla antes que el código

Pedir que dicte los primeros términos de Fibonacci en voz alta y que explique con sus palabras por qué un "8" viene después de "3, 5". Si lo logra, que repita con otro punto de partida (por ejemplo, empezando en 5 y 8) para separar la regla de los números específicos.

### 2. Plantilla con huecos

Para quien todavía no escribe la función completa, entregar el esqueleto:

```ts
function generarFibonacci(n: number): number[] {
  const serie = [0, 1];

  for (let i = 2; i < n; i++) {
    serie.push(serie[___ - 1] + serie[___ - 2]);
  }

  return serie.slice(0, n);
}
```

Solo debe completar las dos posiciones. Después, borrar un fragmento más y repetir hasta que la escriba sola.

### 3. Bubble sort en papel antes que en código

Usar tarjetas o papelitos con `3 1 4 2`. Pedir que realicen una pasada completa moviendo los papelitos físicamente, y que anoten el resultado. Después repetir con una pasada menos. Cuando el movimiento esté claro, trasladarlo al ciclo doble comparando ambas cosas lado a lado.

### 4. Uno solo de los dos ciclos primero

Para quien se pierde en el anidado: primero escribir solo el ciclo interno (una pasada con `for...of` o un `for` desde 0 hasta el final) e imprimir el resultado. Comprobar que "el más grande llegó al final". Después agregar el ciclo externo que repite la pasada. Distinguir los dos trabajos (caminar de vecino en vecino vs. repetir el paseo).

### 5. Casos de borde como algo bueno, no como trampa

Explicar que probar con listas vacías, de un elemento o con duplicados es lo que distingue a alguien que copió de alguien que entendió. Dar un caso y preguntar "¿qué esperas que pase aquí?" antes de ejecutar.

### Criterio de logro del apoyo

La persona puede explicar la regla, completar la plantilla sin consultar y decir qué salida espera de cada caso de borde antes de correrlo.

## Extensión opcional

Úsalo si alguien ya implementó y probó ambas funciones con autonomía.

### 1. Ordenamiento descendente

Copiar `bubbleSort` y cambiar la comparación de `>` a `<`. Notar si la lista se ordena de mayor a menor y por qué.

### 2. Fibonacci sin lista intermedia (solo imprimir)

Variante que no acumula en un arreglo: guardar `anterior` y `actual`, imprimir y desplazar los valores en cada vuelta. Comparar con la versión con lista: ¿cuál le parece más fácil de entender?

### 3. Fibonacci recursivo y con memoria

Ver el módulo [07-algoritmia](../../material-estudiantes/07-algoritmia/README.md), que menciona "Fibonacci con y sin memoria" como ejemplo de programación dinámica. Como reto: escribir la versión recursiva simple y luego la versión que guarda resultados ya calculados. Probar con `n` grande (por ejemplo 40) y comparar cuál tarda más.

### 4. Contar comparaciones

Agregar un contador que sume una unidad por cada comparación de vecinos en `bubbleSort` y devolverlo junto al resultado. Comparar cuántas comparaciones se hacen con listas de 5, 10 y 20 elementos. Esta es la semilla de la idea "este algoritmo hace mucho trabajo".

### 5. Dibujar las barras en la terminal

Imprimir cada lista como barras de caracteres (`*` o `#`) al final de cada pasada para "ver" ordenarse los datos. Es una versión en consola de lo que luego podrá hacerse como visualización en React.

### 6. Reto de merge sort

Implementar `mergeSort` con la idea de dividir y combinar, probándolo con las mismas listas que bubble. No es obligatorio y no se revisa en clase; quien lo logre, que lo explique con palabras a un compañero.

### 7. De variables sueltas a un modelo con clases

Convertir el programa de la clase en algo que se parezca a la tiendita del curso: en vez de `let serie = []` suelto, escribir una clase `Serie` con `constructor`, un método `siguiente()` y un getter `valores`. Es el paso natural de "función que recibe datos" a "objeto que sabe lo suyo", y conecta con la [Guía 7: Programación orientada a objetos](../../material-estudiantes/02-programacion-base/guia-poo.md), especialmente con la sección 2 sobre qué datos vale la pena modelar.

No es una revisión obligatoria: sirve para quien ya terminó los dos ejercicios de la clase y quiere ver el mismo problema resuelto con otro estilo.

### Criterio de logro de la extensión

La persona propone una variante, la implementa, la prueba con casos propios y puede explicar qué cambió y por qué funcionó o falló.