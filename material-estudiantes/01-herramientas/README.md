# 1. Herramientas: escribir archivos y ejecutar programas

Referencias oficiales: [Node.js](https://nodejs.org/en/download) y [Getting Started de Vite](https://vite.dev/guide/).

Esta guía te explica de qué sirven las herramientas antes de usarlas, para que no sean "cajas negras". Al final deberías poder escribir un programa en un archivo, ejecutarlo con Node y entender dónde aparece cada cosa.

## Un programa es un archivo

La idea más importante de toda esta guía es esta:

> **Un programa es un archivo de texto con instrucciones ordenadas.**

Cuando programas, no estás "hablando" con la máquina en una caja mágica: estás **escribiendo un archivo** que puede abrirse, leerse, editarse, copiarse y guardarse a mano. Ese archivo dice, paso a paso y en un idioma exacto, qué queremos que la computadora haga.

Pensemos en una receta de cocina:

- La **receta escrita** es el archivo: una serie de instrucciones ordenadas.
- El **chef que la sigue** es el programa que ejecuta las instrucciones.
- Si la receta dice "agrega un poco de sal", una persona interpreta. Pero una computadora necesita precisión: *¿cuánta sal?* *¿cuándo?* *¿qué pasa si no hay sal?*

Por eso programar no es escribir código "para la máquina": es **planear y escribir archivos** con instrucciones lo suficientemente exactas para que un ejecutor las cumpla sin inventar nada.

Esto tiene una consecuencia práctica enorme: programar y ejecutar son dos acciones distintas.

- **Programar** = crear y modificar archivos (lo haces con el editor).
- **Ejecutar** = correr un archivo para que sus instrucciones se cumplan (lo haces con la terminal y con Node).

Cada vez que "corras" un programa, el archivo se vuelve a leer desde el inicio y sus instrucciones se ejecutan en orden. Modifica el archivo, vuelve a ejecutarlo, y el resultado cambia.

## Tres herramientas y sus roles

Para trabajar necesitas cinco piezas, y cada una tiene un trabajo específico. No son "la misma cosa disfrazada": cumplen roles diferentes.

| Herramienta | Su rol | En una sola frase |
|---|---|---|
| **Editor** (VS Code u otro) | escribir, leer y editar archivos | el taller donde se crea el código |
| **Terminal** | dar órdenes a tu sistema: navegar carpetas y ejecutar comandos | el centro de mando |
| **Node** | ejecutar archivos JavaScript fuera del navegador | el ejecutor de tus programas |
| **npm** | instalar y administrar bibliotecas para tus proyectos | el gestor de paquetes |
| **Vite** | crear, ejecutar y construir el proyecto web con React del curso | el que ejecuta la app React |

La frase que resume el flujo: **el editor escribe, la terminal ordena, Node ejecuta, npm consigue piezas y Vite ejecuta la app React.**

Una aclaración importante desde ahora: en este curso, cuando hablemos de "ejecutar la aplicación", quien la ejecuta de verdad es **Vite**. Node y npm preparan el terreno (instalan las piezas y lanzan el comando), pero quien compila tu código React, lo abre en el navegador y lo refresca automáticamente cuando lo editas es Vite. Lo verás en acción en la parte final de esta guía.

## Instalar y comprobar Node

Node trae consigo a npm, así que instalar Node nos da ambas cosas a la vez.

1. Descarga la versión **LTS** desde [nodejs.org](https://nodejs.org/en/download).
2. Instala con las opciones predeterminadas.
3. Cierra y vuelve a abrir la terminal (esto hace que el sistema "se entere" de la instalación).
4. Comprueba que todo quedó listo:

```bash
node --version
npm --version
```

Si aparecen dos números de versión, Node y npm están disponibles. Un número de versión es solo eso: la etiqueta de qué versión tienes instalada.

**Si la terminal dice algo como `node no se reconoce como un comando`**: el sistema no encuentra a Node en su ruta de comandos. Cierra la terminal y ábrela de nuevo; si el problema sigue, repite la instalación y reinicia. Si aún así no aparece, copia el mensaje exacto y consúltalo: los errores se resuelven con el texto del error, no adivinando.

## Tu primer programa con Node

Vamos a hacer lo más simple pero decisivo de todo el curso: escribir un programa en un archivo y ejecutarlo.

### 1. Crea un archivo

Abre tu editor y dentro de él abre una carpeta de práctica (por ejemplo, `practica-node`). Crea un archivo nuevo y nómbralo `hola.js`. La extensión `.js` le dice al sistema: *"esto es código JavaScript"*.

### 2. Escribe unas instrucciones

Dentro del archivo escribe:

```js
console.log("Hola, mundo");
console.log("Estoy corriendo JavaScript desde un archivo.");
```

`console.log(...)` es la forma de decirle al programa: *"muestra este valor en la consola"*. Aquí, "consola" es la terminal. Por ahora, `console.log` es tu ventana de salida: la manera de *ver* lo que tu programa hace.

### 3. Ejecuta el archivo

En la terminal, asegúrate de estar dentro de la carpeta donde creaste el archivo:

```bash
pwd        # ¿en qué carpeta estoy? (muestra tu ruta real)
cd ruta    # entrar a la carpeta correcta
ls         # lista los archivos (macOS/Linux/Git Bash)
dir        # lista los archivos (PowerShell)
```

Después corre el programa:

```bash
node hola.js
```

Node **abre el archivo**, lee sus instrucciones en orden y las ejecuta. El resultado aparece en la terminal:

```
Hola, mundo
Estoy corriendo JavaScript desde un archivo.
```

Ese es el ciclo completo y no va a cambiar durante todo el curso:

> **escribir archivo → `node archivo.js` → ver la salida**

### 4. Modifica y vuelve a ejecutar

Cambia el texto del archivo, agrega una línea, quita otra… y ejecuta `node hola.js` de nuevo. El resultado cambia. Puedes editar y volver a correr tantas veces quieras: programar es justamente ese ciclo de editar y probar.

**Si el sistema responde algo como *"Cannot find module 'hola.js'"***: Node está buscando el archivo y no lo encuentra. Casi siempre es porque la terminal está en otra carpeta (usa `pwd` para comprobarlo) o porque el nombre no coincide exactamente. El error mismo te está diciendo qué falta: lee la primera línea, no adivines.

**Si el programa falla con algo como `hola.js:2`**: es un error dentro del código. El número te dice la **línea exacta** donde Node encontró el problema. Ábrela, léela, corrígela y vuelve a ejecutar. Aprender a leer errores con número de línea es una de las habilidades más valiosas del curso.

> Cuando un programa se queda pegado y no termina, detenlo con `Ctrl + C`.

## JavaScript corre en dos lugares: Node y el navegador

JavaScript es originalmente el lenguaje del navegador: por eso, cuando abres un sitio web, su código corre dentro de la página. Pero JavaScript no vive solo ahí. **Node ejecuta el mismo lenguaje en tu terminal**, sin necesidad de navegador.

- **El navegador** ejecuta JavaScript dentro de una página.
- **Node** ejecuta JavaScript en tu computador, desde la terminal.

En este curso usamos los dos:

- **Node** para ejecutar tus programas de práctica (los guiones de las [Guías de JavaScript](../02-programacion-base/README.md#gu%C3%ADas-de-javascript-l%C3%B3gica-antes-de-los-tipos) usan exactamente este flujo: escribes un `.js` y lo ejecutas con `node archivo.js`; puedes incluso practicar desde el navegador en la consola con F12).
- **Node + Vite** para preparar, ejecutar y construir el proyecto web con React y TypeScript (lo explorarás más adelante en esta misma guía).

Nota: otra forma rápida de probar JavaScript sin guardar ningún archivo es abrir la consola del navegador (F12 → pestaña *Console*) y escribir directamente. Sirve para experimentar, pero para tener un programa de verdad necesitas un archivo: algo que se pueda guardar, modificar, reutilizar y compartir.

## npm: el gestor de bibliotecas

npm viene instalado junto con Node. Su trabajo es **instalar y administrar bibliotecas**: pedazos de código ya escritos que otros desarrolladores publican y que tu proyecto puede usar (React, Vite, y otros).

Cuando instalas las piezas de un proyecto, npm las descarga en una carpeta llamada `node_modules`. Esa carpeta es grande y no se guarda en Git (conviene regenerarla cuando se necesita), así que lo importante no es su contenido, sino poder recrearla con un solo comando:

```bash
npm install
```

Si descargas un proyecto de Git y le falta `node_modules`, ese comando lo reconstruye completa según lo que diga el archivo `package.json`. Ese archivo es la "hoja de vida" del proyecto: registra qué bibliotecas se usan y qué comandos son válidos para ejecutarlo.

## Comandos esenciales

| Comando | Qué hace |
|---|---|
| `node archivo.js` | ejecuta un programa JavaScript guardado en un archivo |
| `node --version` | muestra la versión de Node instalada |
| `npm --version` | muestra la versión de npm instalada |
| `npm install` | instala (o reinstala) las dependencias del `package.json` |
| `npm create vite@latest proyecto -- --template react-ts` | crea un proyecto web nuevo con Vite y la plantilla de React + TypeScript |
| `npm run dev` | inicia el servidor de desarrollo |
| `npm run build` | comprueba y construye la versión de producción |
| `npm run preview` | previsualiza la construcción |
| `Ctrl + C` | detiene el programa o servidor en marcha |

## Crear el proyecto web con Vite

Aquí encaja todo lo que ya sabes. Vite es la herramienta que **crea, ejecuta y construye** el proyecto web del curso (React + TypeScript); como todo llega a través de npm, el primer comando es precisamente crear el proyecto:

```bash
npm create vite@latest portafolio-programacion -- --template react-ts
```

Recomiendo usar el linter llamado **ESLint**, es ya un clasico y se usa bastante en empresas, pero eres libre de usar el que quieras, lo realmente importante es su funcionalidad, la cual es la de un software que analiza tu código fuente de forma automática para detectar errores, fallos potenciales y problemas de estilo antes de ejecutarlo.

```bash
cd portafolio-programacion
```

Lo normal seria que al crear la carpeta de tu proyecto este tambien te instale todas las dependencias en una carpeta que se llama **node_modules**, pero en caso de que te falte esta carpeta, ya sea porque clonaste el proyecto de un repositorio o cometiste algun error, el siguiente comando te puede ayudar a recuperarla:

```bash
npm install
```

Por ultimo, puedes correr el proyecto con este comando:

```bash
npm run dev
```

Aquí es donde **Vite ejecuta React**: compila tu código, lo sirve en el navegador y lo actualiza solo cada vez que editas un archivo del proyecto. Abre la dirección local que muestra la terminal y verás tu aplicación corriendo. Para detener el servidor, presiona `Ctrl + C`.

Algunas veces este comando puede cambiar segun la herramienta, el framework, criterio o deseo egoista de alguna demente, pero no te asustes, generalmente esta especificado dentro del archivo package.json, ahi encontraras un apartado que dice "scripts" y podras checkar que cosas se corren con el run. Por defecto `npm run dev` es casi lo mismo que correr `npm run vite`.

Con esto ya tienes la última pieza del flujo completo: escribes instrucciones en archivos (editor), las ejecutas con Node y su ecosistema (terminal + npm), y **Vite ejecuta la app React**. El mismo principio de la sección inicial —**un programa es un archivo**— sigue aplicando: React, TypeScript y Vite son, en el fondo, archivos organizados dentro del proyecto.

## Si algo falla

Antes de pedir ayuda o cambiar código, aplica siempre esta secuencia:

1. **Lee el error completo.** La primera línea dice dónde falló (archivo y línea) y qué esperaba el programa. No arregles a ciegas.
2. **Comprueba la carpeta.** Si el archivo "no existe", pregunta: ¿estoy en la carpeta correcta? (`pwd` responda.)
3. **Comprueba el nombre.** `hola.js` no es `hola´. Los nombres y las extensiones deben coincidir exactamente.
4. **Reinicia la terminal** si recién instalaste Node y el sistema no lo reconoce.
5. **Divide el problema.** Prueba un ejemplo pequeño que sí funcione (como `console.log("Hola")`) y agrega código de a poco.

Si aún no encuentras la causa, copia el mensaje de error completo y consulta esa información: un error bien descrito se soluciona mucho más rápido que una conjetura.