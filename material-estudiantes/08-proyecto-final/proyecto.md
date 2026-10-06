# Proyecto final: una herramienta interactiva conectada a una API

Diseña y publica una aplicación web que ayude a una persona a organizar, explorar o decidir algo que te importe. Puede ser un catálogo cultural, una biblioteca personal, un planificador de estudio, una colección de recetas, un registro de plantas o una idea propia.

La aplicación usará **React + TypeScript** y cargará su colección inicial desde una API HTTP expuesta por el docente. Además, permitirá interactuar con los datos, conservar los cambios acordados para el curso y publicarse en internet.

Antes de programar, completa la [guía de planeación y arquitectura](planeacion-y-arquitectura.md). Este documento define el reto y los requisitos; la guía te ayuda a descubrir qué construir y cómo organizarlo.

## Requisitos del proyecto

### 1. Interfaz y modelo propios

- Define un tipo de datos pertinente a tu tema con `id` y al menos tres campos.
- Muestra al menos ocho elementos iniciales obtenidos de la API; no escribas las tarjetas una por una en JSX.
- Usa componentes reutilizables y HTML semántico.
- Haz que la interfaz se pueda leer y usar en una pantalla angosta y en una computadora.

### 2. Interacción

- Añade búsqueda por texto y un filtro por categoría o propiedad. Deben poder usarse al mismo tiempo.
- Incluye una acción sobre cada elemento, como marcar favorito, completar o cambiar su estado.
- Incluye un formulario para crear un elemento. Valida los campos obligatorios y da mensajes comprensibles.
- Muestra un resumen calculado que cambie con los datos, por ejemplo el total o cuántos están completos.
- Conserva en `localStorage` los cambios que la API no guarde (por ejemplo elementos creados y estados modificados). Al volver a cargar, la persona debe recuperar sus cambios.

### 3. Consumo de API

El docente proporcionará una URL de API y explicará el formato de respuesta. La interfaz debe solicitar la colección inicial con `fetch` o una herramienta aprobada por el curso y:

- mostrar un indicador mientras espera;
- mostrar los elementos cuando la respuesta sea correcta;
- mostrar un mensaje y permitir reintentar si la red falla o la respuesta no es válida;
- mostrar un estado vacío si la respuesta contiene cero elementos;
- convertir la respuesta recibida al tipo que usa la aplicación y explicar esa forma de datos.

Si todavía no tienes claro qué es una API o cómo se hace una consulta HTTP, lee primero la [guía: leer una API REST](guia-rest.md): explica solicitud y respuesta, JSON, REST y cada uno de estos puntos con código.

La primera versión requiere **una consulta de lectura** (por ejemplo `GET /api/recursos`). El servidor no necesita cuentas, autenticación ni base de datos avanzada. La creación y los cambios pueden guardarse localmente en el navegador. Si el docente también habilita escritura, puedes añadir `POST` como extensión, acordando antes el contrato de datos.

Si la API no está disponible durante el trabajo, desarrolla contra una respuesta de ejemplo documentada y deja clara la URL que se conectará. Antes de entregar, la integración debe probarse con la API del docente o con una alternativa que este apruebe.

### 4. Persistencia y publicación

- Guarda los cambios locales usando `localStorage` y comprueba que sobrevivan a una recarga.
- Publica el código en GitHub y la aplicación en Vercel.
- Configura la URL de la API según el procedimiento que indique el docente. No escribas claves privadas dentro del código del frontend.

## Alcance y límites

No necesitas cuentas de usuario, pagos, un servidor propio ni una base de datos. No instales una librería nueva sin poder explicar por qué hace falta. El objetivo es integrar una aplicación de curso con un servicio HTTP pequeño, no construir una plataforma grande.

Completa primero el núcleo descrito arriba. Solo después considera extensiones como editar/eliminar, ordenar, exportar JSON/CSV, publicar una operación `POST` si está habilitada o agregar una visualización sencilla.

## Qué vas a practicar

- entender un problema y traducirlo a historias de usuario y criterios comprobables;
- crear un modelo de datos y organizar el código por funcionalidad;
- usar React, TypeScript, componentes, props, estado, eventos y formularios;
- hacer una solicitud HTTP asíncrona y manejar carga, éxito, error y lista vacía;
- combinar `map`, `filter`, `find` o `reduce` cuando tengan sentido;
- validar entradas y persistir cambios en `localStorage`;
- publicar una aplicación y explicar sus decisiones;
- usar vibecoding responsable: consulta la [guía del módulo 06](../06-vibecoding/README.md) y comprende cada línea antes de subirla.

## Plan sugerido para una semana de clase

Ajusta el ritmo con el docente. Cada etapa debe terminar en algo visible y ejecutable.

| Etapa | Resultado |
|---|---|
| 1. Investigar y definir | Problema, persona usuaria, alcance y dudas de la API. |
| 2. Requisitos y diseño | Historias, criterios de aceptación, modelo de datos y boceto. |
| 3. Estructura y datos | Proyecto Vite, carpetas iniciales y colección cargada desde la API. |
| 4. Explorar | Tarjetas, búsqueda, filtro y resumen calculado. |
| 5. Interactuar | Formulario, acción por elemento y validaciones. |
| 6. Recuperarse y guardar | Estados de carga/error/vacío y persistencia con `localStorage`. |
| 7. Revisar y publicar | Pruebas de aceptación, README, GitHub y Vercel. |

Como referencia, reserva entre **12 y 18 horas** de trabajo. Si el tiempo efectivo es menor, acuerda con el docente recortar extensiones; conserva al menos la consulta GET y el manejo de carga/error.

## Pruebas de aceptación mínimas

- Una persona puede describir qué problema resuelve la aplicación y para quién.
- La aplicación carga ocho o más elementos desde la API acordada.
- La interfaz informa claramente si está cargando, si ocurrió un error o si no hay elementos.
- La búsqueda y el filtro pueden combinarse.
- Se puede crear un elemento con datos válidos y se avisa si faltan datos obligatorios.
- La acción por elemento actualiza la interfaz y el cambio persiste tras recargar.
- El resumen refleja la colección actual.
- La vista se puede usar en móvil y escritorio.
- El estudiante puede mostrar el flujo desde la respuesta HTTP hasta las tarjetas, y explicar qué hace cada parte que publica.

## README de entrega

El repositorio debe explicar:

- el problema, la persona usuaria y cómo ejecutar el proyecto;
- los requisitos principales y qué se dejó fuera;
- la estructura de carpetas elegida;
- el formato esperado de la respuesta de la API y cómo configurar su URL;
- qué datos se obtienen de la API y cuáles se guardan en `localStorage`;
- enlace a la aplicación publicada;
- una breve nota sobre el uso de IA, si aplica.

## Presentación final

En una demostración breve, presenta el problema, muestra la carga desde la API y recorre búsqueda, filtro y una interacción. Explica una decisión de arquitectura, cómo manejas los errores de red y qué aprendiste. Si algo no está terminado, explica qué funciona y cuál sería el siguiente paso.

---

**Anterior:** [Algoritmia aplicada](../07-algoritmia/README.md) _(opcional)_ · [Volver al índice del proyecto](README.md)