# Editor: VS Code (recomendado)

Referencia oficial: [Visual Studio Code](https://code.visualstudio.com/).

Esta guía explica el editor de texto del curso. No es una lección de atajos ni de personalización: es lo mínimo para crear, guardar y editar los archivos de tus programas sin que el editor sea una "caja negra".

## ¿Por qué un editor de texto?

En la [guía principal](README.md) la idea central es esta:

> **Un programa es un archivo.**

Si programas es porque **escribes archivos**: los creas, los lees, los modificas, los guardas y los vuelves a abrir. El editor de texto es la herramienta para hacer eso, igual que un cuaderno es el lugar donde se escribe una receta antes de que alguien la cocine.

Dos aclaraciones para que no haya sorpresas:

- Un editor de texto trabaja con **texto plano**: solo letras, números y símbolos, sin decorado ni formato. Cuando ves colores en VS Code son solo ayuda visual; el archivo sigue siendo texto. No uses un procesador de texto como Word o Google Docs para programar: agregan formato invisible que confunde al ejecutor.
- Cualquier editor de texto sirve para programar. **VS Code es el recomendado del curso** porque es gratuito, ligero, funciona igual en Windows y macOS, tiene terminal integrada y una tienda de extensiones, pero la habilidad que practicas aquí (crear y guardar un archivo) es la misma en todos.

## Instalar VS Code

1. Descarga el instalador para tu sistema desde [code.visualstudio.com](https://code.visualstudio.com/).
2. Instala con las opciones predeterminadas.
3. Abre VS Code. En Windows, durante la instalación conviene marcar la opción de **"Agregar a PATH"** (o "Abrir con Code") si aparece: así podrás abrir el editor desde la terminal. Si no la viste, no es problema, lo podrás hacer desde el menú.

Si ya tienes otro editor que te funciona, puedes seguirlo usando: las instrucciones del curso funcionan igual. VS Code es la opción recomendada, no una obligación.

## Primer vistazo a la interfaz

Cuando abres VS Code, las piezas más importantes son cuatro:

| Pieza | Dónde está | Su rol |
|---|---|---|
| **Explorador** | panel izquierdo | muestra los archivos y carpetas de tu proyecto |
| **Editor** | zona central | el espacio donde escribes el texto de los archivos |
| **Terminal integrada** | panel inferior | una terminal igual a la que usas desde tu sistema |
| **Extensiones** | ícono de la barra izquierda (cuadritos) | instalar herramientas extra para el editor |

Esto conecta con la tabla de roles de la [guía principal](README.md): el editor es **"el taller donde se crea el código"**. El panel izquierdo te muestra *qué archivos tienes*, el centro es *donde los escribes* y el inferior es la *terminal misma* que ya conoces, solo que dentro de la misma ventana.

## Abrir una carpeta y crear archivos

La forma de trabajar en VS Code es con **carpetas**, no con archivos sueltos. Cuando abres una carpeta, el explorador te muestra su contenido y tus archivos nuevos aparecen ahí.

1. Menú **File → Open Folder...** (o **Archivo → Abrir carpeta...**) y elige la carpeta de práctica, por ejemplo `practica-node`.
2. Con el explorador visible a la izquierda, crea un archivo nuevo con el ícono de "nuevo archivo" o arrastrando un archivo existente.
3. Nómbralo `hola.js`. La extensión `.js` le dice al editor (y a Node) qué tipo de archivo es: al escribir verás el resaltado de color de JavaScript.
4. Escribe dentro el código del [primer programa](README.md#tu-primer-programa-con-node) y guárdalo con `Ctrl + S`.

Esto es todo lo que necesitas para la parte de "escribir archivos". Si los archivos no te aparecen en el explorador, casi siempre es porque no abriste la carpeta correcta: el editor no "adivina" dónde está tu código, tú le dices con Open Folder.

Abrir la carpeta del proyecto tiene además una consecuencia para tu terminal: **la terminal integrada arranca dentro de la carpeta que abriste**. Si abres un archivo suelto en vez de la carpeta, el explorador queda vacío y la terminal arranca en otra carpeta (por ejemplo, la de tu usuario). Resultado típico: `node hola.js` dice que no encuentra el archivo… aunque el archivo exista. La carpeta que abres decide también dónde empieza tu terminal.

## La terminal

La terminal es el lugar donde le das órdenes a tu sistema: navegar carpetas y ejecutar programas. En la [guía principal](README.md) se resume como **"el centro de mando"**: ahí corren `node`, `npm` y `git`. Cada terminal trabaja dentro de **una carpeta actual** (la que ves con `pwd`); casi todos los errores "de la terminal" se explican porque esa carpeta no es la que crees.

### Abrir una terminal independiente

Puedes abrir una terminal sin entrar a VS Code:

- **Windows:** presiona la tecla de Windows, escribe `Terminal` y presiona Enter. La **Terminal de Windows** es la recomendada y viene incluida; también sirven escribir `PowerShell` o, si prefieres la clásica, `cmd`.
- **Alternativa Windows:** presiona `Win + R`, escribe `cmd` o `powershell` y presiona Enter.
- **macOS:** presiona `Cmd + Espacio`, escribe `Terminal` y presiona Enter.

Sirve cuando solo necesitas instalar algo, probar un comando suelto o comprobar que Node y npm quedaron bien instalados, sin abrir un proyecto completo.

### La terminal integrada en VS Code

La terminal que abres dentro de VS Code **es la misma** terminal del sistema, solo que alojada en la ventana del editor: no es otra herramienta, es el mismo "centro de mando" a un clic de tu código.

Para abrirla: menú **Terminal → New Terminal** (o el atajo habitual `Ctrl + ñ` / `` Ctrl + ` ``).

Lo primero que debes comprobar al abrirla es en qué carpeta arrancó:

```bash
pwd        # ¿en qué carpeta estoy? Consulta esto cada vez que algo "no aparece"
```

- Si abriste la carpeta del proyecto con **File → Open Folder**, la terminal integrada arranca **dentro de esa carpeta**: `node hola.js` y `npm` funcionan de inmediato.
- Si abriste un archivo suelto o iniciaste VS Code sin carpeta, la terminal arranca en otra parte (por ejemplo, tu carpeta de usuario) y no va a encontrar tus archivos. Solución: reabre la carpeta del proyecto, o muévete con `cd` (tema de la [guía principal](README.md#tu-primer-programa-con-node)).

Si instalaste Node antes de abrir VS Code, la terminal integrada ya lo conoce. Si no, cierra y vuelve a abrir VS Code para que tome la configuración del sistema.

La confusión más común al empezar es "mezclar" terminal y editor: escribir comandos donde va el código o esperar que el editor ejecute cosas. Recuerda la división del módulo:

- **Escritura de archivos** → editor (zona central).
- **Ejecución de comandos** → terminal (panel inferior).

Aunque estén en la misma ventana, cumplen roles distintos.

### PowerShell o Command Prompt

En Windows, la terminal trabaja con un **intérprete**: el programa que lee y ejecuta los comandos que escribes. Los dos más usados son:

| Intérprete | Cuándo | Lo que ves al inicio |
|---|---|---|
| **PowerShell** | el predeterminado recomendado | `PS C:\proyectos>` |
| **Command Prompt (cmd)** | el clásico; algunos lo prefieren por simple | `C:\proyectos>` |

Ambos ejecutan los comandos del curso (`node`, `npm`, `git`). PowerShell es el recomendado, pero `cmd` funciona igual si te resulta familiar. Lo importante no es el intérprete, sino que el ejecutor de tus programas (Node) sea el mismo.

Para cambiar de uno al otro:

- **En la terminal de VS Code:** presiona la flecha ▾ junto al ícono **+** del panel de terminal y elige *Command Prompt* o *PowerShell*.
- **En la Terminal de Windows:** presiona la flecha ▾ junto al ícono **+** y elige la opción.
- **Desde cualquier terminal:** escribe `cmd` y Enter para entrar a Command Prompt, o `powershell` y Enter para entrar a PowerShell. Para volver al anterior, escribe `exit` y Enter.

### Habilitar la ejecución de scripts en PowerShell

Si al correr algún comando aparece un error como *"La ejecución de scripts está deshabilitada en este sistema"*, PowerShell está bloqueando un archivo `.ps1`. Se puede ajustar la política para el usuario actual, permitiendo scripts locales y de fuentes de confianza:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Confirma cuando pregunte (escribe `S` o `Y` según el idioma). Esto no desactiva la seguridad por completo: solo permite scripts firmados remotamente y los que creas tú mismo. Y ojo: muchos comandos del curso (`node`, `npm`, `git`) son programas que funcionan igual aunque no cambies esta política; este arreglo es solo para el mensaje de bloqueo que aparece en algunos scripts auxiliares.

## Extensiones: pocas y con propósito

Las extensiones son paquetes que añaden funciones al editor. Son útiles, pero **no necesitas instalar decenas**: cada extensión es una pieza más que puede fallar o desactualizarse. Instala pocas, con un propósito claro.

En este curso, la recomendada es **ESLint** (la menciona la [guía principal](README.md#crear-el-proyecto-web-con-vite) al crear el proyecto Vite): analiza tu código automáticamente y detecta errores o problemas de estilo antes de ejecutarlo. Un formato de código opcional y cómodo es **Prettier**.

Para instalarla: abre el panel de Extensiones, busca el nombre y presiona **Install**. Cuando necesites verificar una extensión, revisa en el panel su nombre exacto y cuántas personas la usan antes de instalarla a ciegas.

## Si algo falla

Aplica siempre la secuencia de la [guía principal](README.md#si-algo-falla): lee el error completo, comprueba la carpeta y divide el problema. En el editor y la terminal, los fallos típicos son:

1. **"La terminal no reconoce `node`"**: probablemente instalaste Node antes de abrir VS Code o la terminal actual. Cierra y vuelve a abrir VS Code (o abre una terminal nueva) y prueba `node --version` de nuevo.
2. **"Cannot find module 'hola.js'" (o el archivo "no existe" pero sí lo escribiste)**: la terminal está en otra carpeta. Esto es lo que pasa cuando no abriste la carpeta del proyecto: el editor muestra tu archivo, pero la terminal integrada arrancó en otra parte (como tu carpeta de usuario) y Node busca el archivo *desde ahí*. Revisa con `pwd`; si no estás en la carpeta del proyecto, reábrela con **File → Open Folder** o muévete con `cd ruta`.
3. **"No veo mi archivo en el explorador"**: ¿abriste la carpeta correcta con File → Open Folder? No estás viendo un archivo suelto, estás viendo el contenido de la carpeta que elegiste.
4. **"La ejecución de scripts está deshabilitada en este sistema"**: PowerShell bloquea un script (ver la sección [Habilitar la ejecución de scripts en PowerShell](#habilitar-la-ejecuci%C3%B3n-de-scripts-en-powershell)). Como buena práctica, escribe el mensaje completo y verifica antes de ejecutar comandos de política por sugerencia de cualquier fuente.
5. **"El archivo se ve sin colores / como texto plano"**: revisa que la extensión del archivo sea la correcta (`.js`, `.ts`, `.tsx`). Sin esa pista, el editor no sabe qué lenguaje resaltar.

## Resumen del flujo

Con el editor claro, la frase del módulo cobra todo su sentido:

> **El editor escribe, la terminal ordena, Node ejecuta, npm consigue piezas y Vite ejecuta la app React.**

El editor es la primera pieza del ciclo: ahí se escribe el archivo, la terminal integrada lo ejecuta y el resultado aparece abajo. Una herramienta menos misteriosa, un paso más del recorrido.

Continúa con el [primer programa en Node](README.md#tu-primer-programa-con-node) donde vas a usar esto de inmediato.