# 6. Vibecoding responsable

**Estás aquí:** [Inicio](../README.md) › **6. Vibecoding responsable**

> **¿Te se frenó una palabra?** El [glosario del curso](../glosario.md#capitulo-7) explica prompt, modelo de lenguaje y alucinación, una por una y con ejemplos.

Vibecoding es desarrollar con apoyo de una herramienta de inteligencia artificial. La IA puede ayudarte a pensar, explicar, escribir o corregir código; tú sigues tomando las decisiones y aprendiendo cómo funciona tu proyecto.

En este módulo aplicarás estas ideas al [proyecto de portafolio personal](proyecto-portafolio.md).

## La regla de oro

> **Nunca subas, publiques ni entregues código que no sabes qué hace.**

Antes de integrar una propuesta, léela y pruébala. Si encuentras una parte que no puedes explicar, pídele a la IA que la explique con palabras sencillas y un ejemplo, o consulta al docente. Adapta el código y confirma que entiendes cómo encaja en tu proyecto. Esta regla también aplica al código que escribiste tú.

## Cómo escribir buenos prompts

Un buen prompt da información suficiente para recibir ayuda concreta. Incluye:

1. **Contexto:** qué estás construyendo y con qué herramientas.
2. **Objetivo:** qué quieres que ocurra; describe el comportamiento, no solo “hazlo bonito”.
3. **Código o datos relevantes:** comparte solo la parte necesaria y elimina contraseñas, tokens y datos personales.
4. **Restricciones:** por ejemplo, “usa React y TypeScript”, “no agregues librerías” o “no cambies otros componentes”.
5. **Formato de respuesta:** pide una explicación, pasos o un cambio pequeño que puedas revisar.

### Ejemplo

```text
Estoy creando un portafolio con Vite, React y TypeScript.
En la sección de proyectos quiero mostrar tarjetas a partir de un arreglo llamado proyectos.
Cada proyecto tiene id, nombre y descripcion.
Ayúdame a crear un componente pequeño que reciba un proyecto y muestre esos datos.
No agregues librerías. Primero explícame la idea y luego muestra el código necesario.
```

## Recomendaciones para trabajar con IA

- **Pide una tarea a la vez.** Empieza por una explicación o un cambio pequeño; luego revisa el resultado antes de continuar.
- **Incluye el error completo y el contexto.** Di qué esperabas que pasara, qué pasó y qué intentaste. No pegues claves ni información privada.
- **Pide que explique las decisiones.** Puedes solicitar que explique cada parte nueva y qué archivos modifica.
- **Revisa la respuesta.** La IA puede equivocarse, inventar funciones o usar herramientas que no necesitas. Comprueba lo que dice en tu proyecto y en la documentación cuando sea necesario.
- **Prueba los casos importantes.** Revisa el resultado en el navegador y prueba también qué ocurre si falta un dato o la lista está vacía.
- **Haz los cambios tú y conserva tu voz.** Ajusta textos, colores y estructura para que el proyecto te represente.
- **Cuenta cómo usaste la IA.** Anota qué pediste, qué aprovechaste, qué modificaste y qué aprendiste.
- **No compartas datos privados.** No pegues contraseñas, claves, tokens, archivos `.env` ni información personal de otras personas.

## Ciclo de trabajo

1. **Define:** explica el problema, el [contexto](../glosario.md#contexto) y las restricciones.
2. **Pide:** solicita una solución pequeña.
3. **Inspecciona:** lee el código y pregunta por las partes que no entiendes.
4. **Prueba:** comprueba el resultado y casos importantes.
5. **Corrige:** adapta la propuesta a tu proyecto.
6. **Explica:** cuenta qué cambiaste y por qué antes de subirlo.

## Lo que debes documentar

- el prompt o una síntesis de lo que pediste;
- qué idea o respuesta aprovechaste;
- qué cambios hiciste;
- qué probaste;
- qué aprendiste o qué error resolviste.

---

**Anterior:** [Publicar en Vercel](../05-publicacion/README.md) · **Siguiente:** [Algoritmia aplicada](../07-algoritmia/README.md) _(opcional)_