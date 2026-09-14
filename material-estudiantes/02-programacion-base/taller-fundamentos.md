# Taller de fundamentos: de tostada a constructor

Este taller es la práctica base del curso. No se trata de correr para completar muchos ejercicios, sino de resolver, explicar y modificar soluciones hasta ganar autonomía.

## Cómo trabajar cada reto

Para cada reto entrega:

1. una descripción breve del problema;
2. una función o solución que lo resuelva;
3. dos ejemplos de prueba;
4. una explicación de la estrategia;
5. una mejora o caso límite.

Puedes resolver primero en un archivo `.ts` y luego reutilizar la función dentro de React.

Antes de entregar, revisa la [lista de comprobación de buenas prácticas](README.md#lista-de-comprobación-antes-de-entregar-un-reto): nombres claros, cláusulas de guarda, ciclos que terminan y pruebas de borde.

## Nivel 0 - perderle el miedo

### Reto 0. Hola, programa

Crea una función que reciba un nombre y devuelva un saludo.

### Reto 1. Conversor de temperatura

Convierte grados Celsius a Fahrenheit y Kelvin.

### Reto 2. Mayor de dos

Recibe dos números y devuelve cuál es mayor o si son iguales.

### Reto 3. Entrada válida

Recibe una edad y devuelve si una persona puede entrar a una actividad.

**Conceptos:** variables, tipos, operadores, `if` y retorno.

## Nivel 1 - dominar condiciones y ciclos

### Reto 4. Par o impar

Indica si un número es par o impar.

### Reto 5. Tabla de multiplicar

Genera la tabla de un número entre 1 y 10.

### Reto 6. Contador de vocales

Cuenta cuántas vocales aparecen en una frase.

### Reto 7. Suma acumulada

Recibe una lista de números y calcula la suma sin usar una función de suma ya creada.

### Reto 8. FizzBuzz

Recorre del 1 al 100. Escribe `Fizz` para múltiplos de 3, `Buzz` para múltiplos de 5 y `FizzBuzz` para ambos.

**Conceptos:** ciclos, acumuladores, condiciones combinadas y casos límite.

## Nivel 2 - pensar en funciones y datos

### Reto 9. Promedio y aprobación

Recibe notas, calcula el promedio y devuelve una decisión explicada.

### Reto 10. Mayor de una lista

Recibe una lista de números y devuelve el mayor. Define qué ocurre si la lista está vacía.

### Reto 11. Contar positivos

Recibe una lista de números y cuenta cuántos son mayores que cero.

### Reto 12. Factorial seguro

Calcula el factorial de un número entero no negativo. Usa una cláusula de guarda para rechazar valores inválidos.

### Reto 13. Contraseña válida

Recibe un texto y decide si tiene al menos ocho caracteres, una mayúscula y un número. Nombra las condiciones de forma legible.

**Conceptos:** funciones, tipos, arreglos básicos, ciclos, acumuladores, cláusulas de guarda y pruebas de borde.

## Orden recomendado

Completa los retos 0-8 antes de pasar a una interfaz compleja. Los retos 9-13 consolidan TypeScript, funciones, arreglos básicos y lógica de negocio. Después continúa con el [Taller de modelado, estructuras y React](../03-react-typescript/taller-modelado-react.md).

## HackerRank como práctica adicional

Puedes usar la serie [10 Days of JavaScript de HackerRank](https://www.hackerrank.com/domains/tutorials/10-days-of-javascript) como banco de retos. La plataforma practica JavaScript, no TypeScript, así que la usaremos para reforzar lógica y después trasladaremos algunas soluciones a archivos `.ts` y componentes React.

Retos sugeridos:

- Día 0: tipos de datos;
- Día 1: funciones;
- Día 2: operadores;
- Día 3: condicionales;
- Día 4: clases, opcional;
- Día 5: template literals;
- Día 6: fechas, opcional.

No es obligatorio completar toda la serie. El objetivo es resolver con comprensión, no acumular puntos.
