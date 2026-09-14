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
cd portafolio-programacion
npm install
npm run dev
```

Abre la dirección local que muestra la terminal. Para detener el servidor, presiona `Ctrl + C`.

## Comandos npm esenciales

```bash
npm install        # instala las dependencias del package.json
npm run dev        # inicia el servidor de desarrollo
npm run build      # comprueba y construye la versión de producción
npm run preview    # previsualiza la construcción
```
