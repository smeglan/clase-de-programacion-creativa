# 5. Publicar en Vercel

Referencia oficial: [Vite on Vercel](https://vercel.com/docs/frameworks/frontend/vite).

## Antes de publicar

Comprueba localmente:

```bash
npm run build
```

Si la construcción termina sin errores, sube los cambios a GitHub.

## Publicación desde GitHub

1. Entra a Vercel y crea un proyecto nuevo.
2. Importa el repositorio de GitHub.
3. Selecciona el proyecto Vite.
4. Usa `npm run build` como comando de construcción.
5. Usa `dist` como carpeta de salida si no aparece automáticamente.
6. Publica y copia la URL.

Cada cambio que subas al repositorio puede generar un nuevo despliegue. Revisa la URL después de publicar y prueba las funciones principales.

## Checklist de entrega

- [ ] La URL abre en una ventana privada.
- [ ] No hay errores visibles en la interfaz.
- [ ] Los botones y formularios funcionan.
- [ ] La aplicación se adapta razonablemente a una pantalla pequeña.
- [ ] El repositorio tiene un README breve.
- [ ] La URL está incluida en el portafolio.
