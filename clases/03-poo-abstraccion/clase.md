# Clase 3: Programación orientada a objetos y abstracción

## Datos generales

- Duración presencial: 3 horas.
- Trabajo no presencial asociado: 9 horas.
- Unidad: Modelado de datos y programación orientada a objetos.
- Resultado de aprendizaje: el estudiante modela entidades relacionadas con un problema y explica cómo una clase organiza estado, operaciones y reglas.
- Meta de la clase: diseñar una clase TypeScript pequeña, crear varias instancias y aplicar una regla del dominio.
- Prerrequisitos: tipos básicos, funciones, arreglos y ejecución de archivos TypeScript.

## Objetivos de la sesión

Al terminar, cada estudiante podrá:

1. explicar abstracción como selección de los datos pertinentes a un propósito;
2. distinguir clase, objeto e instancia;
3. escribir una clase con constructor, propiedades, métodos y `this`;
4. validar una regla con encapsulamiento;
5. reconocer composición e interfaces como herramientas para expresar relaciones y contratos;
6. decidir cuándo una clase aporta valor y cuándo bastan `type` y funciones.

## Preparación docente

- Probar `npx tsx` y preparar alternativa JavaScript sin tipos.
- Dibujar tres columnas: situación, modelo y operaciones.
- Preparar una lista de atributos posibles de un producto y fijar el propósito: catálogo sencillo de una tienda escolar.
- Leer la [Guía 7 de POO para estudiantes](../../material-estudiantes/02-programacion-base/guia-poo.md).

## Núcleo común - 180 minutos

### 1. Activación: qué modelamos - 25 minutos

Mostrar la ficha de un producto con varios atributos (proveedor, código, precio, vencimiento, peso, categoría, disponibilidad). Acordar primero qué necesita una aplicación que solo muestra y filtra un catálogo. En parejas, seleccionar los campos necesarios y justificar qué queda fuera.

### 2. Explicación y demostración - 40 minutos

Seguir el [guion docente](guion-explicacion-docente.md): variables sueltas → objeto literal descrito con `type` → clase con una operación. Explicar clase, instancia, constructor, `new`, método y `this` antes de introducir encapsulamiento.

### 3. Ejercicio guiado: Producto - 45 minutos

Todo el grupo implementa una clase `Producto` con nombre y precio. Añade un método `aplicarDescuento` que acepte porcentajes entre 0 y 100, y un método para consultar el precio actual. Crear dos productos y comprobar que el cambio a uno no modifica el otro.

### 4. Laboratorio: Turno - 50 minutos

Cada estudiante modela un turno con nombre y motivo, crea dos instancias y escribe un método `descripcion()`. Después añade una regla de validación que pueda explicar. El docente recorre el aula usando el [checklist](checklist-docente.md) y ofrece el [apoyo](apoyos-y-extensiones.md) cuando sea necesario.

### 5. Composición, contrato y cierre - 20 minutos

Comparar composición (“el carrito tiene productos”) con herencia (“un gato es un animal”). Presentar una `interface` como contrato: dice qué operación ofrece un objeto, no cómo está implementada. Cerrar con una explicación breve del estudiante:

> Elegí guardar ___ porque la aplicación necesita ___. La clase me aporta ___ y valida la regla ___ .

## Observación y retroalimentación

Comprobar que el estudiante parte del propósito antes de elegir atributos; diferencia la definición de cada objeto creado; usa `this` para referirse a la instancia actual; y puede explicar qué regla protege. No exigir herencia ni una jerarquía de clases como señal de dominio de POO.

## Trabajo no presencial - 9 horas

1. Reescribir la clase `Turno` sin copiar el ejemplo y documentar dos pruebas.
2. Leer la [Guía 7 de POO](../../material-estudiantes/02-programacion-base/guia-poo.md), incluyendo encapsulamiento, composición y contratos con `interface`.
3. Modelar una entidad para el proyecto del estudiante; listar dos detalles incluidos y dos excluidos, con su justificación.
4. Comparar en una bitácora una solución con clase y otra con `type` más funciones.

## Criterios de logro

- El modelo responde a un propósito explícito.
- La clase tiene constructor y un método que funciona.
- El estudiante crea dos instancias independientes.
- Puede explicar el papel de `this`.
- Incluye una regla probada con un caso válido y uno inválido.
- Puede decir cuándo elegiría `type` y funciones en vez de una clase.

## Materiales

- Editor y terminal con Node, npm y `tsx`.
- Archivo de ejemplo `producto.ts`.
- [Guion docente](guion-explicacion-docente.md), [checklist](checklist-docente.md), [apoyos y extensiones](apoyos-y-extensiones.md) y [guía del estudiante](../../material-estudiantes/02-programacion-base/guia-poo.md).

## Bloqueos previsibles y respuestas

- **No sabe qué atributos poner:** volver al propósito de la aplicación y preguntar qué decisión requiere ese dato.
- **Llama objeto a la clase:** comparar clase con molde o receta e instancia con el objeto concreto creado con `new`.
- **Piensa que `this` significa el nombre de la clase:** llamar un método en dos instancias y observar que `this` refiere a la instancia que lo recibió.
- **Cree que siempre necesita clases:** mostrar que para datos planos y operaciones simples un `type` con funciones puede ser más claro.
