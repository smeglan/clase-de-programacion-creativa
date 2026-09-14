# Guion docente: explicación de la clase 1

## Idea central

La meta de hoy no es memorizar comandos ni hacer una web bonita. Es recorrer por primera vez este camino completo:

```text
idea → código en el computador → repositorio → página pública
```

Al final, cada estudiante debe entender qué función cumple cada herramienta y comprobar que puede transformar una idea pequeña en algo visible y compartible.

## Guion para explicar

### 0–4 min: abrir el curso

Puedes decir:

> Buenas noches soy su profesor, mi nombre es Sebastián Noreña Meglan, normalmente este seria el espacio para que se presenten, pero yo tengo mala memoria para las caras, aunque me digan su nombre probablemente no me lo aprenda de buenas a primera, asi que lo mejor que puedo hacer es irme aprendiendomelos a medida que vayamos trabajando.

> Lo primero entonces es darles este link, para que por favor se unan al grupo de whatsapp y pues en general tengan mis datos, anexe tambien un pequeño examen que me gustaria que hicieran, para yo medir como estamos

> La idea con este curso, es que ustedes salgan con la capacidad transformar ideas en soluciones de software, la parte de programar es mas un tramite.

### 4–9 min: mapa de herramientas

Muestra este mapa antes de entrar en detalle:

```text
TypeScript → React → Vite → navegador
     ↓          ↓        ↓
   código   interfaz   entorno de trabajo

Git → GitHub → Vercel → URL pública
versiones   respaldo   publicación
```

Puedes decir:

> No se asusten si no saben algo, yo les voy a explicar y les voy a dar tareas para que entiendan claramente lo que estamos haciendo.

### 9–17 min: definiciones esenciales

#### Programación y programación creativa

**Programación:** diseñar y escribir instrucciones que una computadora pueda ejecutar para producir un resultado.

Puedes decir:

> Una receta y un programa se parecen: ambos indican pasos para lograr algo. La diferencia es que una computadora necesita instrucciones exactas. Si una receta dice “agrega un poco”, una persona interpreta; una computadora necesita saber cuánto, cuándo y qué debe hacer si falta algo.

Ejemplo: “Si el carrito está vacío, mostrar ‘Aún no tienes productos’; si tiene productos, mostrar el total.” Esa es una decisión programable.

**Programación creativa:** usar programación para expresar ideas, explorar, comunicar o construir experiencias interactivas, además de resolver una necesidad funcional.

> ¿Qué es la creatividad? Es una habilidad mental del ser humano. Combina conocimientos previos, memoria e imaginación. Produce resultados novedosos y útiles. No se limita al arte; se aplica en la ciencia, los negocios y la vida diaria. 

#### JavaScript y TypeScript

**JavaScript:** lenguaje de programación que se ejecuta, entre otros lugares, en el navegador y permite dar comportamiento a una página web.

**TypeScript:** extensión de JavaScript que permite describir qué tipo de datos esperamos usar y detectar muchos errores antes de abrir la página.

> JavaScript es el idioma que entiende el navegador. TypeScript nos ayuda a escribir ese idioma con señales adicionales. Por ejemplo, podemos declarar que una edad debe ser un número y que un nombre debe ser texto. Antes de ejecutar, TypeScript nos avisa si confundimos esas cosas.

```ts
const nombre: string = 'Ana'
const edad: number = 20
```

Aclara: TypeScript ayuda a detectar ciertos errores temprano; no elimina la necesidad de probar el programa.

#### React, interfaz y componente

**Interfaz de usuario:** parte de una aplicación con la que una persona ve, lee y actúa: textos, botones, formularios, tarjetas y mensajes.

**React:** biblioteca para construir interfaces de usuario a partir de piezas reutilizables.

**Componente:** pieza de interfaz con una responsabilidad concreta que podemos reutilizar y combinar con otras piezas.

> En vez de construir una página como un bloque enorme, React permite dividirla en piezas. Un encabezado, una tarjeta de proyecto y un botón pueden ser componentes. Así es más fácil cambiar, reutilizar y entender la interfaz.

Conecta con el portafolio: una tarjeta de proyecto aparece varias veces; cambia su contenido, pero conserva la misma estructura.

#### Vite

**Vite:** herramienta que crea la base de un proyecto web moderno, inicia un servidor local mientras desarrollamos y prepara una versión optimizada para publicar.

> Vite no es la página ni es React. Es el equipo de trabajo que nos permite empezar con una estructura lista, ver los cambios rápidamente y preparar los archivos que viajarán a internet.

### 17–23 min: del computador a internet

#### Node.js, npm y terminal

**Node.js:** entorno que permite ejecutar JavaScript y herramientas relacionadas fuera del navegador, desde la terminal.

**npm:** gestor de paquetes incluido con Node.js; descarga y organiza las dependencias que necesita el proyecto.

**Dependencia:** código ya construido por otra persona o equipo que el proyecto usa para funcionar. React es una dependencia.

**Terminal:** aplicación de texto que permite dar instrucciones al sistema operativo mediante comandos.

> Node no reemplaza al navegador. Lo usamos detrás de escena para ejecutar herramientas como npm y Vite. La terminal tampoco es una prueba de memoria: por ahora usaremos pocos comandos y siempre sabremos qué hacen antes de ejecutarlos.

Explica los comandos del día:

```bash
npm create vite@latest portafolio-programacion -- --template react-ts
cd portafolio-programacion
npm install
npm run dev
```

- El primero crea la base del proyecto.
- `cd` entra a la carpeta correcta.
- `npm install` descarga lo que el proyecto necesita.
- `npm run dev` inicia la versión local de la aplicación.

#### Servidor local

**Servidor local:** programa que muestra la aplicación en una dirección que solo funciona en el computador donde estamos trabajando, como `http://localhost:5173`.

> “Local” significa que la aplicación aún vive en mi computador. Podemos verla en el navegador, pero nadie más puede abrir ese enlace. Para compartirla debemos publicarla.

#### Git, GitHub, repositorio y commit

**Git:** sistema que registra los cambios de un proyecto a lo largo del tiempo.

**Repositorio:** carpeta de proyecto cuyo historial de cambios está gestionado por Git.

**Commit:** captura identificada de un conjunto de cambios que queremos guardar en ese historial.

**GitHub:** plataforma en internet donde podemos alojar repositorios Git y colaborar.

> Git funciona como el historial inteligente del proyecto. No guarda solo la última versión: permite saber qué cambió y cuándo. Un commit es un punto de guardado acompañado de una explicación. GitHub permite respaldar ese historial y compartirlo.

Da un ejemplo de mensaje útil: `Crea portada inicial del portafolio`. Compara con `cambios`, que no explica nada.

#### Vercel, despliegue y URL pública

**Vercel:** plataforma que puede construir y alojar una aplicación web conectada a un repositorio.

**Desplegar o publicar:** enviar una versión preparada de la aplicación a un servidor para que otras personas puedan abrirla desde internet.

**URL pública:** dirección web que cualquier persona con el enlace puede visitar.

> Cuando conectamos GitHub con Vercel, cada versión importante del repositorio puede convertirse en una página visible en internet. Esa URL es la diferencia entre “hice un ejercicio en mi computador” y “puedo mostrar mi trabajo”.

### 23–28 min: demostración narrada

Mientras haces la demostración, narra lo que ocurre:

> Acabo de crear la estructura del proyecto. Ahora entro a la carpeta correcta; si ejecuto comandos fuera de ella, no funcionarán como espero. Instalo las dependencias y enciendo el servidor local. Esta dirección solo la puede ver mi computador por ahora.

Después de cambiar un texto en `src/App.tsx`:

> Este archivo contiene un componente. Al guardar, React actualiza la interfaz que vemos. No editamos una imagen: editamos las instrucciones que producen la página.

Al mostrar GitHub y Vercel:

> Ahora guardamos una versión con Git, la respaldamos en GitHub y Vercel la convierte en una URL pública. Este será el ciclo que repetiremos durante el curso.

### 28–30 min: cierre

> En los próximos 30 minutos no vamos a intentar aprender todo React. Solo haremos una prueba de autonomía: abrir el proyecto, cambiar contenido y reconocer el camino entre código, navegador y publicación. Si algo falla, no significa que no sepan programar; significa que encontramos el siguiente problema que debemos aprender a resolver.

Pide que abran el diagnóstico y escriban una duda que quieran resolver durante el curso.

## Definiciones para repasar

| Término | Definición corta |
| --- | --- |
| Código fuente | Archivos que contienen las instrucciones del programa. |
| Aplicación web | Programa que se usa desde un navegador. |
| Frontend | Parte visible e interactiva que usa la persona. |
| Backend | Parte que procesa datos, reglas o servicios en un servidor. No es el foco inicial del curso. |
| Navegador | Programa que interpreta y muestra una aplicación web. |
| JavaScript | Lenguaje que da comportamiento a las páginas web. |
| TypeScript | JavaScript con tipos y comprobaciones adicionales durante el desarrollo. |
| React | Biblioteca para crear interfaces mediante componentes. |
| Componente | Pieza reutilizable de una interfaz con una responsabilidad concreta. |
| Vite | Herramienta para crear, ejecutar y construir el proyecto web. |
| Node.js | Entorno que ejecuta herramientas de JavaScript fuera del navegador. |
| npm | Herramienta para instalar y ejecutar paquetes y tareas del proyecto. |
| Dependencia | Paquete externo que el proyecto necesita para funcionar. |
| Terminal | Interfaz de texto para ejecutar comandos. |
| Servidor local | Programa que permite ver la aplicación en el propio computador durante el desarrollo. |
| Git | Sistema de control de versiones. |
| Repositorio | Proyecto con su historial de cambios, gestionado por Git. |
| Commit | Registro identificado de cambios realizados en el proyecto. |
| GitHub | Plataforma para alojar y compartir repositorios Git. |
| Vercel | Plataforma para publicar una aplicación web. |
| Despliegue | Proceso de hacer disponible una aplicación en internet. |
| URL pública | Enlace que permite visitar la aplicación publicada. |
| Portafolio | Colección organizada de proyectos que evidencia lo que una persona sabe hacer. |

## Errores de explicación que conviene evitar

- React no es un lenguaje: es una biblioteca; JavaScript y TypeScript son lenguajes.
- Vite no es una alternativa a React: Vite prepara y ejecuta el proyecto; React construye la interfaz.
- Git no es GitHub: Git registra cambios; GitHub puede alojar esos registros en internet.
- `localhost` no es una página publicada: nadie más puede abrir esa dirección.
- TypeScript no elimina todos los errores: ayuda a encontrar ciertos problemas antes de ejecutar.
- La IA puede proponer código, pero quien entrega debe poder explicarlo, probarlo y corregirlo.

## Preguntas de comprobación

Usa dos o tres antes de iniciar el ejercicio:

1. ¿Cuál es la diferencia entre TypeScript y React?
2. ¿Qué hace `npm install` y por qué se ejecuta dentro de la carpeta del proyecto?
3. ¿Qué significa que la aplicación se vea en `localhost`?
4. ¿Qué registra un commit?
5. ¿Para qué sirve conectar GitHub con Vercel?
6. Si una tarjeta de proyecto se repite varias veces, ¿por qué podría ser un componente?
