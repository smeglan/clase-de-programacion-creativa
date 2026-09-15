# Clase 1: Diagnóstico, entorno y portafolio

## Datos generales

- Duración presencial: 3 horas.
- Trabajo no presencial asociado: 9 horas.
- Momento: diagnóstico y planteamiento transversal del proyecto de curso.
- Resultado de aprendizaje: el estudiante identifica su punto de partida, configura el entorno de trabajo y define una primera idea para su portafolio y proyecto integrador.
- Meta de la clase: reconocer el punto de partida, explorar el entorno de trabajo y definir un primer avance para el portafolio.

## Objetivos de la sesión

Al terminar la clase, el estudiante podrá:

1. explicar qué espera aprender y qué experiencia previa tiene;
2. abrir, ejecutar y modificar un proyecto React con TypeScript;
3. crear un repositorio para su portafolio;
4. publicar una primera versión funcional;
5. proponer una idea inicial de proyecto integrador.

## Preparación docente

Antes de la clase, comprobar:

- Node.js y npm instalados;
- editor de código disponible;
- Git funcionando;
- acceso a GitHub;
- acceso a Vercel o una alternativa de publicación;
- una plantilla base de Vite con React y TypeScript;
- una página de respaldo en caso de que alguien no pueda instalar herramientas.

La plantilla base debe ser mínima: React, TypeScript, Vite y CSS nativo. No se deben añadir librerías adicionales en esta primera sesión.

## Núcleo común - primeras 2 horas

### 1. Explicación y demostración - 30 minutos

Para preparar definiciones, ejemplos y preguntas de comprobación, usar el [guion de explicación docente](guion-explicacion-docente.md).

Presentar:

- qué es programación creativa;
- qué se construirá durante el curso;
- diferencia entre TypeScript, React, Vite y Vercel;
- estructura básica de un proyecto;
- relación entre código, repositorio y URL publicada;
- propósito del portafolio.

Demostrar el flujo completo:

```bash
npm create vite@latest portafolio-programacion -- --template react-ts
cd portafolio-programacion
npm install
npm run dev
```

Después mostrar una modificación sencilla en `src/App.tsx`, subirla a GitHub y explicar cómo se conecta con Vercel.

### 2. Diagnóstico y ejercicio - 30 minutos

Aplicar el diagnóstico de `diagnostico.md`. No se califica como examen; sirve para identificar apoyos necesarios y retos opcionales adecuados, no para etiquetar a nadie.

Luego, todos deben modificar el texto de la página inicial para incluir:

- nombre o seudónimo;
- una frase sobre sus intereses;
- una lista de tres cosas que esperan aprender;
- una sección vacía para los proyectos del curso.

### 3. Laboratorio y resolución de dudas - 60 minutos

Cada estudiante debe:

1. crear o clonar su proyecto;
2. ejecutarlo localmente;
3. modificar la página inicial;
4. crear el repositorio;
5. realizar el primer commit;
6. publicar la primera versión;
7. registrar el enlace en su ficha de curso.

El docente recorre el aula usando el checklist de `checklist-docente.md`, registra bloqueos frecuentes y propone apoyos o extensiones puntuales según sea necesario.

### 4. Taller de acompañamiento y proyección - 60 minutos

Usar esta hora para que cada estudiante continúe el flujo con el que tenga más sentido trabajar: resolver una instalación pendiente, crear el repositorio, publicar una primera versión, mejorar su página inicial o delimitar una idea de proyecto. El docente puede alternar acompañamiento técnico, revisión breve entre pares y conversación individual sobre el punto de partida.

## Observación y retroalimentación

Durante la sesión, observar si el estudiante puede explorar el diagnóstico, ejecutar o reconocer un proyecto React, localizar el componente principal y explicar qué desea construir. Según las condiciones técnicas, puede registrar un repositorio, una aplicación local, una primera URL o una nota de planificación; ninguna de estas opciones debe impedir el avance de quien tenga dificultades de instalación.

## Trabajo no presencial - 9 horas

### Propuesta de continuidad

Como práctica sugerida, mejorar la página inicial del portafolio con algunos de estos elementos:

- presentación breve;
- objetivos personales;
- sección de proyectos;
- sección "qué aprendí en esta clase";
- enlace al repositorio;
- enlace a la aplicación publicada.

### Distribución sugerida

- 2 h: repasar la estructura de un proyecto Vite y React;
- 2 h: practicar edición de JSX y estilos CSS básicos;
- 2 h: revisar Git, commits y repositorios;
- 2 h: mejorar la página inicial;
- 1 h: escribir la idea del proyecto integrador o documentar bloqueos y preguntas para la siguiente clase.

## Criterios de logro

- puede ejecutar el proyecto sin seguir una guía paso a paso completa;
- puede localizar el componente principal;
- puede modificar contenido y estilos básicos;
- tiene un repositorio y una URL funcionales;
- puede explicar qué hizo y qué necesita aprender.
