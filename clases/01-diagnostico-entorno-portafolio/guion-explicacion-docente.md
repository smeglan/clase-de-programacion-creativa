# Guion docente: explicación de la clase 1

## Idea central

La primera sesión crea una base de confianza y una orientación común. Cada persona comprueba que puede guardar y ejecutar un archivo pequeño, y empieza a definir para quién quiere construir algo durante el curso.

Hilo conductor:

```text
idea → archivo con instrucciones → ejecución con Node → salida observable
```

React, Vite, GitHub y publicación pertenecen a etapas posteriores. Hoy basta con explicar sus nombres de manera general si aparecen en el diagnóstico; no hay que abrir una plantilla de React.

## Guion para explicar

### Bienvenida

Puedes presentarte y compartir acuerdos prácticos para el curso. Explica que aprenderán por etapas, con tiempo para probar, equivocarse y preguntar. No es necesario que cada persona llegue con experiencia.

### Mapa de herramientas

Escribe en la pizarra:

```text
editor escribe → terminal da órdenes → Node ejecuta → resultado aparece
```

Puedes decir:

> Hoy vamos a comprobar este recorrido con un archivo pequeñito. El editor es donde lo escribimos; la terminal es donde damos el comando; Node lee el archivo y ejecuta sus instrucciones. Más adelante agregaremos herramientas para páginas web.

### El primer programa

Usa:

```js
const nombre = "Ada";
console.log(`Hola, ${nombre}`);
```

Pide que predigan la salida. Después guarda como `hola.js` y corre `node hola.js`. Cambia “Ada” por el nombre de una persona voluntaria, guarda y repite.

Aclara que el archivo se ejecuta desde el inicio cada vez que se corre. Si la salida no cambia, primero hay que revisar si el archivo se guardó y si la terminal está en la carpeta correcta.

### La idea del proyecto

Puedes decir:

> No necesitamos inventar una aplicación enorme. Busquemos algo concreto que una persona podría usar. Durante el curso aprenderemos las piezas para construir una primera versión y podremos ajustar la idea cuando entendamos mejor el problema.

Pide que describan usuario, necesidad, función y cómo se podría comprobar. Ayuda a reducir ideas muy grandes a una primera acción sencilla.

### Cierre

Solicita que expliquen qué hizo Node y qué hizo el editor. Recoge bloqueos y preguntas para preparar las siguientes clases.

## Definiciones para repasar

| Término | Definición corta |
|---|---|
| Archivo fuente | Archivo que contiene instrucciones o contenido del programa. |
| Editor | Herramienta para crear y modificar archivos. |
| Terminal | Herramienta de texto para dar órdenes al sistema. |
| Node.js | Entorno para ejecutar JavaScript fuera del navegador. |
| npm | Herramienta que instala y administra paquetes. |
| Programa | Instrucciones ordenadas que se ejecutan para producir un resultado. |
| Idea de proyecto | Propuesta inicial con propósito, usuario y una acción principal. |

## Preguntas de comprobación

1. ¿En qué lugar escribimos el código?
2. ¿Qué comando ejecutó el archivo?
3. ¿Qué hace Node con `hola.js`?
4. Si cambias el texto y la salida no se actualiza, ¿qué revisarías primero?
5. ¿Qué acción concreta debería poder realizar tu proyecto?

## Errores de explicación que conviene evitar

- No presentar terminal y editor como la misma herramienta.
- No decir que Node es un lenguaje; JavaScript es el lenguaje y Node es el entorno que lo ejecuta.
- No convertir el diagnóstico en una prueba calificada.
- No exigir una idea de proyecto definitiva en la primera clase.
- No adelantar la configuración de React o Vite; el recorrido web comenzará con HTML y CSS y continuará con React y Vite en el módulo 03.
