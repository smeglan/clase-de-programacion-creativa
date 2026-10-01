# Proyecto: mi portafolio personal

Crea un sitio web sencillo para presentarte y mostrar lo que has aprendido. Usa **Vite + React + TypeScript**. El objetivo es tener una primera versión clara y funcional; puedes mejorarla después.

## Requisitos mínimos

El portafolio debe incluir:

1. **Presentación:** tu nombre y una descripción breve (una o dos frases) sobre ti o lo que te interesa aprender.
2. **Contacto:** al menos una forma de contactarte, por ejemplo un correo electrónico o un enlace a un perfil público. No publiques datos personales que prefieras mantener privados.
3. **Tecnologías:** una lista de lenguajes y herramientas que estás aprendiendo o usaste, por ejemplo HTML, CSS, JavaScript, TypeScript, React y Vite. Escribe solo las que puedas reconocer y explicar.
4. **Proyectos:** al menos una tarjeta para el proyecto **Fila creativa** del [Taller de modelado y React del módulo 03](../03-react-typescript/taller-modelado-react.md). Incluye su nombre y una descripción breve. Cuando tengas el enlace, puedes añadir el repositorio o la versión publicada. Agrega otros proyectos solo si ya los tienes.
5. **Diseño legible:** organiza el contenido en secciones, usa títulos claros y comprueba que se pueda leer tanto en una pantalla angosta como en una computadora.

## Requisito de React y componentes

Construye el portafolio como una interfaz de **React**; no entregues una página HTML estática. Divide la interfaz en al menos estos tres componentes y úsalos desde `App`:

- `Presentacion`: muestra el nombre y la descripción.
- `ListaTecnologias`: recibe una lista por props y la presenta.
- `ListaProyectos` o `TarjetaProyecto`: muestra el proyecto Fila creativa a partir de datos.

Los componentes pueden estar en un mismo archivo al principio o en archivos separados. Lo importante es que cada uno tenga una responsabilidad clara, reciba los datos que necesita mediante props y se componga dentro de `App`.

## Alcance técnico

- Crea el proyecto con la plantilla React + TypeScript de Vite.
- Usa HTML semántico y CSS para estructurar y dar estilo a la página.
- Guarda tecnologías y proyectos en arreglos y muéstralos con `map` dentro de los componentes.
- No necesitas instalar librerías adicionales ni crear un formulario de contacto. Un enlace `mailto:` o a un perfil público es suficiente.

## Entrega

- El proyecto inicia con `npm install` y `npm run dev`.
- Revisaste la página en el navegador y corregiste los errores visibles.
- Incluiste un `README.md` que explica cómo iniciar el proyecto y qué partes construiste.
- Subiste el código siguiendo el [flujo de Git y GitHub del módulo 04](../04-git-github/README.md).
- Puedes explicar qué hacen tus componentes y cómo se muestran las listas. Sigue la [regla de oro del módulo 06](README.md#la-regla-de-oro): no subas código que no sabes qué hace.
