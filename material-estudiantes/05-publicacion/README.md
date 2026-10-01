# 5. Publicar en Vercel

**Estás aquí:** [Inicio](../README.md) › **5. Publicar en Vercel**

> **¿Te se frenó una palabra?** El [glosario del curso](../glosario.md#capitulo-6) explica build, deploy, dominio y hosting, una por una y con ejemplos.

Referencia oficial: [Vite on Vercel](https://vercel.com/docs/frameworks/frontend/vite).

## Antes de publicar

Comprueba localmente:

```bash
npm run build
```

Si la construcción termina sin errores, sube los cambios a [GitHub](../glosario.md#github).

## Publicación desde GitHub

1. Entra a [Vercel](../glosario.md#vercel) y crea un proyecto nuevo.
2. Importa el [repositorio](../glosario.md#repositorio) de GitHub.
3. Selecciona el proyecto Vite.
4. Usa `npm run build` como [comando](../glosario.md#comando) de construcción.
5. Usa `dist` como [carpeta](../glosario.md#carpeta) de salida si no aparece automáticamente.
6. Publica y copia la URL.

Cada cambio que subas al repositorio puede generar un nuevo despliegue. Revisa la URL después de publicar y prueba las funciones principales.

## Checklist de entrega

- [ ] La URL abre en una ventana privada.
- [ ] No hay errores visibles en la interfaz.
- [ ] Los botones y formularios funcionan.
- [ ] La aplicación se adapta razonablemente a una pantalla pequeña.
- [ ] El repositorio tiene un README breve.
- [ ] La URL está incluida en el [portafolio](../glosario.md#portafolio).

---

**Anterior:** [Git y GitHub](../04-git-github/README.md) · **Siguiente:** [Vibecoding responsable](../06-vibecoding/README.md)
