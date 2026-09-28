# 1. Herramientas: escribir archivos y ejecutar programas

Referencias oficiales: [Node.js](https://nodejs.org/en/download) y [Visual Studio Code](https://code.visualstudio.com/).

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

## Las herramientas y sus roles

Para trabajar necesitas cuatro piezas, y cada una tiene un trabajo específico. No son "la misma cosa disfrazada": cumplen roles diferentes.

| Herramienta | Su rol | En una sola frase |
|---|---|---|
| **Editor** ([VS Code u otro](editor-vscode.md)) | escribir, leer y editar archivos | el taller donde se crea el código |
| **Terminal** | dar órdenes a tu sistema: navegar carpetas y ejecutar comandos | el centro de mando |
| **Node** | ejecutar archivos JavaScript fuera del navegador | el ejecutor de tus programas |
| **npm** | instalar y administrar bibliotecas | el gestor de paquetes |

La frase que resume el flujo: **el editor escribe, la terminal ordena, Node ejecuta y npm consigue piezas.**

Y una aclaración que evita mucha confusión, porque es la pregunta más frecuente del curso:

> **Durante la primera mitad del curso solo usas Node y npm.** Los ejercicios de las guías se ejecutan con `node archivo.js` (o `npx tsx archivo.ts`), en archivos sueltos, sin proyecto, sin servidor y sin configuración extra. Las otras piezas del curso —el proyecto web con React, la herramienta que lo construye y la publicación en la web— aparecen más adelante, y las explicaremos cuando lleguen. Si ahora te pierdes pensando en frameworks, servidores o comandos que no reconoces, no estás perdiendo nada: todavía no llegamos a eso.

## El editor: VS Code

Cuando escribes un programa, tu única herramienta de creación es el **editor de texto**: el lugar donde el código cobra forma de archivo. En este curso usamos **VS Code** (gratuito y recomendado), pero cualquier editor de texto sirve; lo importante es la habilidad de crear, guardar y modificar archivos, no la marca.

```text
editor escribe → terminal ordena → node ejecuta
```

No lo veas como una lección de configuración: es lo mínimo para dejar de "adivinar" dónde se escribe el código. La guía [Editor: VS Code](editor-vscode.md) explica qué es un editor, cómo instalarlo, la relación entre el explorador de archivos y su terminal integrada, cómo abrir una terminal independiente, la diferencia entre PowerShell y Command Prompt, y las extensiones que conviene usar (como ESLint). Antes de seguir con la instalación de Node, vale la pena tener el editor a la vista: tu primer programa quedará a un clic de distancia.

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

### 3. Qué es `console.log`

`console.log(...)` es la forma de decirle al programa: *"muestra este valor en la consola"*. Aquí, "consola" es la terminal. Por ahora, `console.log` es tu ventana de salida: la manera de *ver* lo que tu programa hace.

Conviene entenderlo en tres partes, porque evita media hora de confusión:

- **`console`** es un objeto que ya existe en el entorno donde corres el programa. No lo escribiste tú: te lo dio Node (o el navegador). Es el mismo tipo de cosa que cuando más adelante modelemos objetos en el curso.
- **`log`** es un método de ese objeto. El punto significa "llama a este método de este objeto", igual que `producto.descripcion()` cuando veamos clases.
- **`(...)`** es el valor que quieres ver. Puedes encerrar varios valores separados por comas.

Qué se puede imprimir y cómo:

| Quieres mostrar... | Se escribe | Notas |
|---|---|---|
| Un texto | `console.log("Hola")` | el texto va **entre comillas** |
| Un número | `console.log(42)` | los números van **sin** comillas |
| Una decisión | `console.log(42 > 10)` | imprime `true` o `false` |
| El valor de una variable | `console.log(edad)` | **sin** comillas, o imprimirías el texto `edad` |
| Una lista de valores | `console.log([10, 20, 30])` | muestra todos sus elementos |

El error más común de quien empieza desde cero, y la causa de casi todas las dudas del primer día:

```js
console.log(Hola);   // ReferenceError: Hola is not defined
```

Sin comillas, `Hola` no es un texto: es el nombre de una variable que no existe. Al revés también cambia el resultado: `console.log("42")` muestra el texto `42`, y `console.log(42)` muestra el número `42`.

Una nota de estilo: **en las guías de este curso todos los ejemplos usan `console.log` porque hay que ver qué hace el programa.** En una aplicación terminada no se imprime todo, solo lo que ayuda a entender qué está pasando.

La sección [Tu primer programa: hola mundo y qué es `console.log`](../02-programacion-base/README.md#tu-primer-programa-hola-mundo-y-qu%C3%A9-es-consolelog) del módulo 02 amplía esto con más ejemplos y ejercicios.

### 4. Ejecuta el archivo

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

### 5. Modifica y vuelve a ejecutar

Cambia el texto del archivo, agrega una línea, quita otra… y ejecuta `node hola.js` de nuevo. El resultado cambia. Puedes editar y volver a correr tantas veces quieras: programar es justamente ese ciclo de editar y probar.

**Si el sistema responde algo como *"Cannot find module 'hola.js'"***: Node está buscando el archivo y no lo encuentra. Casi siempre es porque la terminal está en otra carpeta (usa `pwd` para comprobarlo) o porque el nombre no coincide exactamente. El error mismo te está diciendo qué falta: lee la primera línea, no adivines.

**Si el programa falla con algo como `hola.js:2`**: es un error dentro del código. El número te dice la **línea exacta** donde Node encontró el problema. Ábrela, léela, corrígela y vuelve a ejecutar. Aprender a leer errores con número de línea es una de las habilidades más valiosas del curso.

> Cuando un programa se queda pegado y no termina, detenlo con `Ctrl + C`.

## JavaScript corre en dos lugares: Node y el navegador

JavaScript es originalmente el lenguaje del navegador: por eso, cuando abres un sitio web, su código corre dentro de la página. Pero JavaScript no vive solo ahí. **Node ejecuta el mismo lenguaje en tu terminal**, sin necesidad de navegador.

- **El navegador** ejecuta JavaScript dentro de una página.
- **Node** ejecuta JavaScript en tu computador, desde la terminal.

En este curso usamos los dos:

- **Node** para ejecutar tus programas de práctica: escribes un archivo y lo corres con `node archivo.js`. Las [Guías de JavaScript](../02-programacion-base/README.md#gu%C3%ADas-de-javascript-l%C3%B3gica-antes-de-los-tipos) usan exactamente este flujo, y también puedes practicar desde la consola del navegador con F12.
- **El navegador**, más adelante, cuando lleguemos a las aplicaciones de React: ahí el código corre dentro de la página.

Nota: otra forma rápida de probar JavaScript sin guardar ningún archivo es abrir la consola del navegador (F12 → pestaña *Console*) y escribir directamente. Sirve para experimentar, pero para tener un programa de verdad necesitas un archivo: algo que se pueda guardar, modificar, reutilizar y compartir.

## npm: el gestor de bibliotecas

npm viene instalado junto con Node. Su trabajo es **instalar y administrar bibliotecas**: pedazos de código ya escritos que otros desarrolladores publican y que tu proyecto puede usar (React, Vite, y otros).

Cuando instalas las piezas de un proyecto, npm las descarga en una carpeta llamada `node_modules`. Esa carpeta es grande y no se guarda en Git (conviene regenerarla cuando se necesita), así que lo importante no es su contenido, sino poder recrearla con un solo comando:

```bash
npm install
```

Si descargas un proyecto de Git y le falta `node_modules`, ese comando lo reconstruye completa según lo que diga el archivo `package.json`. Ese archivo es la "hoja de vida" del proyecto: registra qué bibliotecas se usan y qué comandos son válidos para ejecutarlo.

## Comandos esenciales

Estos son los comandos que vas a necesitar durante la primera mitad del curso. No hay más:

| Comando | Qué hace |
|---|---|
| `node archivo.js` | ejecuta un programa JavaScript guardado en un archivo |
| `node --version` | muestra la versión de Node instalada |
| `npx tsx archivo.ts` | ejecuta un archivo TypeScript sin instalar nada más |
| `npm --version` | muestra la versión de npm instalada |
| `npm install` | instala (o reinstala) las dependencias del `package.json` |
| `Ctrl + C` | detiene el programa que está corriendo en la terminal |

Si un comando no aparece en esta tabla, no lo necesitas todavía.

## Para más adelante: el proyecto web

En algún momento del curso (no ahora) vamos a construir una aplicación de verdad: una página que se abre en el navegador y que se publica en una URL. Para eso vamos a usar **Vite**, que es la herramienta que prepara el proyecto y lo ejecuta mientras lo escribes.

Lo único que necesitas saber por ahora es esto:

> El proyecto web es **otra cosa**, y vive aparte de los ejercicios del curso. En los ejercicios trabajas con archivos sueltos y `node`; en el proyecto web trabajas dentro de una carpeta de proyecto con un comando que la enciende. No son lo mismo, y no hace falta que entiendas el segundo para hacer bien el primero.

Si quieres ir adelantando, aquí abajo está el resumen. Vuelve a esta sección cuando lleguemos al [módulo 03](../03-react-typescript/README.md).

### Crear el proyecto

Vite llega a través de npm, así que el primer comando es crear el proyecto:

```bash
npm create vite@latest portafolio-programacion -- --template react-ts
```

Después entras en la carpeta creada:

```bash
cd portafolio-programacion
```

Lo normal es que al crear la carpeta se instalen también las dependencias en una subcarpeta llamada **node_modules**. Si te falta esa carpeta (porque clonaste un repositorio o algo salió mal), el comando que la reconstruye es:

```bash
npm install
```

La plantilla viene con **ESLint**, un analizador que revisa tu código automáticamente y detecta errores y problemas de estilo antes de que corras el programa. Si más adelante quieres añadir Prettier u otra herramienta, hazlo con criterio: cada extensión es una pieza más que puede fallar o desactualizarse.

### Encender el proyecto

```bash
npm run dev
```

Este es el único comando del proyecto que necesitas de memoria. Aquí es donde **Vite ejecuta tu aplicación**: lee tu código, lo muestra en el navegador y lo actualiza solo cada vez que guardas un archivo. Abre la dirección local que aparece en la terminal. Para detenerlo, `Ctrl + C`.

El comando puede cambiar según la herramienta, pero no tienes que memorizar nada: está escrito en el archivo `package.json`, en el apartado **"scripts"**. Ahí ves todo lo que se puede correr con `npm run`.

### Lo esencial del módulo

Con esto ya tienes lo que necesitas para la primera mitad del curso: **escribes archivos, los ejecutas con Node y guardas versiones con git**. El proyecto web que acabas de crear es tu portafolio; volveremos a él en la última parte del curso, cuando le agreguemos las aplicaciones de React.

## Si algo falla

Antes de pedir ayuda o cambiar código, aplica siempre esta secuencia:

1. **Lee el error completo.** La primera línea dice dónde falló (archivo y línea) y qué esperaba el programa. No arregles a ciegas.
2. **Comprueba la carpeta.** Si el archivo "no existe", pregunta: ¿estoy en la carpeta correcta? (`pwd` responda.)
3. **Comprueba el nombre.** `hola.js` no es `hola´. Los nombres y las extensiones deben coincidir exactamente.
4. **Reinicia la terminal** si recién instalaste Node y el sistema no lo reconoce.
5. **Divide el problema.** Prueba un ejemplo pequeño que sí funcione (como `console.log("Hola")`) y agrega código de a poco.

Si aún no encuentras la causa, copia el mensaje de error completo y consulta esa información: un error bien descrito se soluciona mucho más rápido que una conjetura.