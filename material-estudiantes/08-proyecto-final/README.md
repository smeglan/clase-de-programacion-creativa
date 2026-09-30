# 8. Proyecto final: una herramienta interactiva para tu mundo

## El reto

Diseña y publica una pequeña aplicación web que ayude a una persona a organizar, explorar o decidir algo que te importe. Puede ser un catálogo cultural, una biblioteca personal, un planificador de estudio, una colección de recetas, un registro de plantas o una idea propia.

La aplicación debe resolver una necesidad concreta y permitir que la persona interactúe con una colección de datos. El tema es libre; el alcance funcional de esta guía mantiene el proyecto realizable.

## Qué vas a construir

Una aplicación con React, TypeScript y CSS que incluya:

- **Un modelo de datos propio:** define un tipo con identificador y al menos tres campos pertinentes al tema.
- **Una colección inicial:** incluye al menos ocho registros de ejemplo, escritos como datos y no como tarjetas repetidas a mano.
- **Una vista de colección:** presenta los elementos en componentes reutilizables y con una jerarquía visual clara.
- **Búsqueda y filtros:** permite buscar por texto y filtrar por al menos una categoría o propiedad.
- **Una acción por elemento:** por ejemplo marcar favorito, completar, guardar o cambiar de estado.
- **Un formulario de creación:** permite agregar elementos y evita aceptar los campos obligatorios vacíos.
- **Resumen dinámico:** muestra un dato calculado útil, como cuántos elementos hay, cuántos están completos o cuál categoría predomina.
- **Persistencia local:** conserva los cambios al recargar la página usando `localStorage`.
- **Publicación:** guarda el trabajo en GitHub y despliega una versión accesible mediante Vercel.

No se requieren cuentas, servidor, base de datos ni librerías externas. Mantén el proyecto en una sola aplicación y en un alcance que puedas explicar.

## Lo que integrarás

El reto reúne los temas del curso en una sola experiencia:

- descomposición del problema, variables, condiciones, ciclos y funciones;
- diseño de tipos, objetos y arreglos;
- operaciones sobre colecciones con `map`, `filter`, `find` o `reduce` cuando tengan sentido;
- componentes, props, eventos y estado en React;
- estructura semántica y estilos responsivos con HTML y CSS;
- uso de Git y publicación;
- vibecoding responsable para explorar, construir y mejorar partes concretas.

Los algoritmos de profundización son opcionales: puedes incorporar una idea de ordenamiento o recomendación si ayuda a tu problema, pero no es requisito.

## Plan de trabajo sugerido

| Etapa | Resultado |
|---|---|
| 1. Delimitar | Describe para quién es la herramienta, qué necesidad atiende y qué queda fuera. |
| 2. Modelar y diseñar | Define el tipo de datos, crea ejemplos y dibuja las zonas principales de la pantalla. |
| 3. Construir la vista | Crea componentes y muestra la colección desde un arreglo. |
| 4. Añadir interacción | Implementa formulario, búsqueda, filtro, acción por elemento y resumen. |
| 5. Guardar el estado | Sincroniza la colección con `localStorage` para conservar cambios. |
| 6. Preparar la entrega | Ajusta los estilos, explica el proyecto, sube el código y publícalo. |

Como referencia, planea entre **8 y 12 horas** de trabajo. Si el tiempo es menor, acuerda con el docente qué funciones forman el núcleo y deja otras como extensión.

## Antes de programar

Completa esta ficha en tu README o documento de trabajo:

```text
Nombre del proyecto:
¿Para quién es?
¿Qué necesidad concreta atiende?
¿Qué información guarda cada elemento?
¿Qué podrá hacer una persona en la aplicación?
¿Qué función dejaré fuera para controlar el alcance?
```

Haz un boceto que muestre, como mínimo, el encabezado, el formulario, los controles de búsqueda/filtro, el resumen y la colección. No necesitas diseñar todas las pantallas antes de comenzar.

## Vibecoding durante el proyecto

Consulta la [guía de vibecoding responsable](../06-vibecoding/README.md). Usa la IA como apoyo para tareas delimitadas: discutir el modelo de datos, proponer una estructura de componentes, resolver una función, mejorar estilos o investigar un error.

Antes de pedir código, describe el comportamiento esperado y las restricciones. Pide cambios pequeños, revisa lo generado, intégralo en tu proyecto y explica qué hace. No entregues una parte que no puedas recorrer y explicar.

En el README final incluye una nota breve con:

- herramienta de IA utilizada, si usaste alguna;
- una solicitud concreta que hiciste;
- qué propuesta aprovechaste y qué modificaste;
- una decisión de código o diseño que puedas explicar.

No es necesario transcribir toda la conversación ni adjuntar cada prompt.

## Definición de terminado

- La aplicación resuelve una necesidad descrita y tiene un tema y contenido propios.
- La colección se representa con un modelo de datos y se muestra dinámicamente.
- La búsqueda y el filtro pueden combinarse.
- Se puede crear un elemento y cambiar su estado desde la interfaz.
- El resumen cambia según los datos actuales.
- Los cambios sobreviven a una recarga en el mismo navegador.
- La interfaz tiene etiquetas comprensibles y se puede usar en una pantalla pequeña.
- El repositorio incluye un README con propósito, instrucciones para ejecutar, enlace publicado y nota de vibecoding si aplica.
- El estudiante puede explicar el modelo y seguir el recorrido de una interacción importante, desde la acción hasta el cambio visible.

## Presentación

En una presentación breve, muestra la necesidad que elegiste, recorre las funciones principales y explica una decisión de modelado o implementación. Señala también una dificultad que resolviste y una mejora que harías con más tiempo.

La evaluación puede valorar la pertinencia de la solución, el uso de los conceptos del curso, la claridad de la experiencia, la explicación del trabajo y la publicación. El docente puede ajustar los pesos según la evaluación del curso.

## Extensiones opcionales

Solo después de completar el núcleo, elige una mejora acotada:

- ordenar por nombre, fecha o prioridad;
- editar o eliminar elementos;
- exportar la colección como JSON o CSV;
- agregar una visualización sencilla de los datos;
- incorporar una regla de recomendación explicable.

No añadas varias extensiones a la vez si hacen difícil terminar y explicar el proyecto.
