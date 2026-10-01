# 4. Git y GitHub

**Estás aquí:** [Inicio](../README.md) › **4. Git y GitHub**

> **¿Te se frenó una palabra?** El [glosario del curso](../glosario.md#capitulo-5) explica commits, ramas y conflictos, una por una y con ejemplos.

Para ampliar: [Pro Git](https://git-scm.com/book/en/v2) y [Getting started with GitHub](https://docs.github.com/en/get-started/learning-to-code/getting-started-with-git).

Git guarda versiones de tu proyecto. [GitHub](../glosario.md#github) permite alojar ese [repositorio](../glosario.md#repositorio) y compartirlo.

## Configuración inicial

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@example.com"
```

## Flujo básico

```bash
git status
git add .
git commit -m "Describe el cambio"
git push
```

- `status` muestra qué cambió.
- `add` prepara los cambios.
- `commit` guarda una versión local.
- `push` sube los commits a GitHub.

## Primer repositorio

Este es el flujo que usaremos para subir un proyecto nuevo: primero creas el repositorio en GitHub, lo clonas en tu computadora y luego copias dentro los archivos del proyecto. Al crear el repositorio en GitHub, déjalo vacío (sin README, licencia ni `.gitignore`) para evitar conflictos al combinar historiales.

En GitHub, copia la dirección HTTPS del repositorio y úsala aquí:

```bash
git clone https://github.com/tu-usuario/nombre-del-repositorio.git
```

El comando crea una carpeta llamada `nombre-del-repositorio`. Copia o mueve los archivos y carpetas de tu proyecto dentro de ella. Luego abre la terminal en esa carpeta:

```bash
cd nombre-del-repositorio
git status
```

`git status` permite revisar que estás en el repositorio correcto y ver los archivos nuevos. Prepara los archivos, crea el primer commit y súbelo:

```bash
git add .
git commit -m "Agrega el proyecto inicial"
git push
```

`git add .` prepara los archivos de la carpeta actual y sus subcarpetas. Antes de ejecutarlo, revisa que no estés incluyendo contraseñas, claves, tokens ni archivos `.env` privados. Como clonaste el repositorio, Git ya conoce el destino de `git push`.

Si ya empezaste el proyecto en una carpeta que no está dentro de un repositorio clonado, puedes usar `git init` y conectar un remoto siguiendo las instrucciones de GitHub.

## Comandos de consulta

```bash
git log --oneline
git diff
git remote -v
```

Haz commits pequeños y frecuentes. Un buen mensaje explica qué cambió, por ejemplo: `Agrega filtro de productos`.

## Regla de seguridad

Nunca subas contraseñas, claves, tokens ni archivos `.env` con información privada.

**NOTA:** Recomiendo instalar la [consola](../glosario.md#consola) de [Git](https://git-scm.com), no solo una extensión del editor: la [terminal](../glosario.md#terminal) de Git funciona también sin VS Code. Si no sabes qué es una extensión, lo explica la [guía del editor](../01-herramientas/editor-vscode.md) del módulo 01.

---

**Anterior:** [Taller de modelado](../03-react-typescript/taller-modelado-react.md) · **Siguiente:** [Publicar en Vercel](../05-publicacion/README.md)
