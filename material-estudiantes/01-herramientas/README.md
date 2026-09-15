# 1. Herramientas: Node, npm, terminal y editor

Referencias oficiales: [Node.js](https://nodejs.org/en/download) y [Getting Started de Vite](https://vite.dev/guide/).

## Node.js

Node.js permite ejecutar herramientas de JavaScript fuera del navegador. En este curso lo necesitamos para ejecutar npm y Vite.

1. Descarga la versión LTS desde [nodejs.org](https://nodejs.org/en/download).
2. Instala con las opciones predeterminadas.
3. Cierra y vuelve a abrir la terminal.
4. Comprueba la instalación:

```bash
node --version
npm --version
```

Si aparece una versión en ambos casos, Node y npm están disponibles.

## Terminal

La terminal permite ejecutar comandos en una carpeta.

```bash
pwd        # muestra la carpeta actual en macOS/Linux/Git Bash
cd ruta    # entra en una carpeta
cd ..      # sube un nivel
ls         # lista archivos en macOS/Linux/Git Bash
dir        # lista archivos en PowerShell
```

No pegues comandos que no entiendas. Si algo falla, copia el mensaje de error y consúltalo.

## Editor

Puedes usar Visual Studio Code u otro editor. Debes poder abrir una carpeta completa, no solo un archivo.

## Crear el proyecto

```bash
npm create vite@latest portafolio-programacion -- --template react-ts
```

Recomiendo usar el linter llamado **ESLint**, es ya un clasico y se usa bastante en empresas, pero eres libre de usar el que quieras, lo realmente importante es su funcionalidad, la cual es la de un software que analiza tu código fuente de forma automática para detectar errores, fallos potenciales y problemas de estilo antes de ejecutarlo.

```bash
cd portafolio-programacion
```

Lo normal seria que al crear la carpeta de tu proyecto este tambien te instale todas las dependencias en una carpeta que se llama **node_modules**, pero en caso de que te falte esta carpeta, ya sea porque clonaste el proyecto de un repositorio o cometiste algun error, el siguiente comando te puede ayudar a recuperarla

```bash
npm install
```

Por ultimo, puedes correr el proyecto con este comando
```bash
npm run dev
```

Abre la dirección local que muestra la terminal. Para detener el servidor, presiona `Ctrl + C`.

Algunas veces este comando puede cambiar segun la herramienta, el framework, criterio o deseo egoista de alguna demente, pero no te asustes, generalmente esta especificado dentro del archivo package.json, ahi encontraras un apartado que dice "scripts" y podras checkar que cosas se corren con el run. Por defecto `npm run dev` es casi lo mismo que correr `npm run vite`.

## Comandos npm esenciales

```bash
npm install        # instala las dependencias del package.json
npm run dev        # inicia el servidor de desarrollo
npm run build      # comprueba y construye la versión de producción
npm run preview    # previsualiza la construcción
```
