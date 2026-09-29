# Clase 1: Diagnóstico, herramientas e idea de proyecto

## Datos generales

- Duración presencial: 3 horas.
- Trabajo no presencial asociado: 9 horas.
- Momento: inicio del curso y diagnóstico.
- Resultado de aprendizaje: el estudiante identifica su punto de partida, prepara su espacio de trabajo y define una idea inicial para el portafolio o proyecto integrador.
- Meta de la clase: explicar para qué sirven editor, terminal y Node, ejecutar un archivo JavaScript sencillo y formular una idea de proyecto.

## Objetivos de la sesión

Al terminar la clase, el estudiante podrá:

1. explicar qué experiencia previa tiene y qué espera aprender;
2. distinguir editor, terminal y Node;
3. crear y guardar un archivo `.js`;
4. ejecutar el archivo con Node y leer su salida;
5. describir una idea de proyecto con usuario, propósito y una función principal.

## Preparación docente

- Comprobar Node.js, npm y un editor de texto en los equipos disponibles.
- Preparar un archivo `hola.js` y una carpeta de práctica.
- Tener a mano la [guía de herramientas](../../material-estudiantes/01-herramientas/README.md) y la [guía de VS Code](../../material-estudiantes/01-herramientas/editor-vscode.md).
- Preparar el formulario de diagnóstico y una pizarra para recoger ideas.
- No se crea todavía un proyecto React/Vite ni se publica el portafolio; ese flujo se introduce después de HTML y CSS.

## Núcleo común - 180 minutos

### 1. Bienvenida y propósito del curso - 20 minutos

Presentar la meta del curso: resolver problemas y construir progresivamente un portafolio con proyectos que cada persona pueda explicar. Compartir canales y acuerdos del curso, y explicar que el grupo avanzará por etapas.

### 2. Diagnóstico inicial - 25 minutos

Aplicar el [diagnóstico](diagnostico.md). No asignar nota: permite conocer experiencia con programación, HTML/CSS, Git, React, IA y herramientas básicas para ofrecer apoyos pertinentes.

### 3. Herramientas y flujo mínimo - 30 minutos

Explicar con ejemplos concretos:

- el editor crea y modifica archivos;
- la terminal ejecuta comandos desde una carpeta;
- Node ejecuta archivos JavaScript fuera del navegador;
- npm administra paquetes y permite ejecutar herramientas como `tsx`.

Mencionar que Vite y React se usarán en una etapa posterior. Hoy no hace falta instalarlos ni aprender sus comandos.

### 4. Primer archivo ejecutable - 40 minutos

El grupo crea una carpeta `portafolio-programacion`, abre esa carpeta en el editor y guarda `hola.js`:

```js
const nombre = "Ada";
console.log(`Hola, ${nombre}`);
```

Desde la terminal ubicada en esa carpeta, ejecuta:

```bash
node hola.js
```

Pide que cambien el nombre, guarden, ejecuten otra vez y describan qué cambió. El objetivo es observar el ciclo archivo → ejecución → salida, no memorizar sintaxis.

### 5. Idea inicial de proyecto - 45 minutos

Cada estudiante completa una ficha breve:

- ¿Quién podría usar mi proyecto?
- ¿Qué necesidad, pregunta o interés atiende?
- ¿Qué acción principal debería permitir?
- ¿Cómo sabré que funciona?
- ¿Qué todavía no sé construir?

En parejas, explicar la idea en un minuto y recibir una pregunta que ayude a delimitarla. El proyecto puede cambiar durante el curso.

### 6. Cierre y bitácora - 20 minutos

Cada estudiante escribe qué herramienta ejecutó el archivo, en qué carpeta lo guardó y qué pregunta tiene para la próxima sesión. Recoger bloqueos para preparar apoyos.

## Observación y retroalimentación

Observar si cada estudiante puede abrir una carpeta, localizar su archivo, distinguir editor de terminal y relacionar el comando con la salida. En la idea de proyecto, comprobar si hay un usuario o propósito comprensible y si se puede describir una función inicial.

## Trabajo no presencial - 9 horas

1. Repetir el ejercicio `hola.js` con tres mensajes distintos y explicar cada cambio.
2. Revisar la guía de herramientas y anotar las dudas que aún quedan sobre terminal, carpetas o Node.
3. Expandir la ficha de proyecto con tres funciones posibles y priorizar la más pequeña.
4. Buscar dos proyectos o sitios que inspiren la idea y anotar qué elemento visual o funcional resulta interesante.
5. Registrar en la bitácora qué se logró y qué necesita apoyo.

## Criterios de logro

- El estudiante describe su punto de partida sin que el diagnóstico se use como calificación.
- Crea, guarda y ejecuta `hola.js`.
- Distingue editor, terminal y Node en el flujo de trabajo.
- Formula una idea inicial con propósito y una función abordable.

## Materiales

- Computador, editor de texto, terminal y Node.js.
- Carpeta de práctica `portafolio-programacion`.
- [Diagnóstico](diagnostico.md), [guion docente](guion-explicacion-docente.md), [checklist](checklist-docente.md), [apoyos](apoyos-y-extensiones.md) y [guía de herramientas para estudiantes](../../material-estudiantes/01-herramientas/README.md).

## Bloqueos previsibles y respuestas

- **No encuentra la terminal:** abrir una integrada del editor o usar la del sistema; comprobar la carpeta actual.
- **`node` no se reconoce:** revisar la instalación y abrir una terminal nueva.
- **El archivo no aparece:** confirmar que se abrió la carpeta del proyecto y revisar el nombre y la extensión `.js`.
- **El resultado no cambia:** guardar el archivo antes de volver a ejecutar el comando.
- **No sabe qué proyecto proponer:** partir de una actividad cotidiana, una afición o una dificultad que haya encontrado.
