# Guía: planear un proyecto y organizar su arquitectura

Antes de abrir el editor, dedica un rato a entender el problema y acordar qué significa terminar. Planear no es predecir cada detalle: es reducir dudas importantes y tener un primer paso claro. Puedes volver a cambiar las decisiones cuando aprendas algo nuevo.

## 1. Entiende a quién ayudas y qué necesita

Empieza conversando con una persona que podría usar la aplicación. No le preguntes solo “¿qué funciones quieres?”. Pregunta por la situación actual:

- ¿Qué intentas hacer?
- ¿Qué parte te toma tiempo o te confunde?
- ¿Cómo lo resuelves ahora?
- ¿Qué información necesitas para tomar una decisión?
- ¿Qué sería una mejora útil para ti?

Anota ejemplos y palabras que la persona usa. Distingue lo que observaste de lo que estás suponiendo. Si no puedes hablar con una persona usuaria, declara tus supuestos y valida la idea con el docente o un compañero.

## 2. Escribe el problema en una frase

Usa esta forma como borrador:

```text
[Persona] necesita [necesidad] porque [situación o dificultad observada].
```

Ejemplo:

```text
El club de lectura necesita encontrar rápidamente los libros pendientes
porque ahora organiza sus reuniones con una lista larga sin filtros.
```

No elijas la tecnología todavía. Primero verifica que el problema sea claro y que tu aplicación pueda ayudar con una parte concreta.

## 3. Convierte la necesidad en requisitos

Un requisito describe algo que la persona necesita poder hacer o una condición que la aplicación debe cumplir. Escribe requisitos comprobables; evita frases vagas como “que sea bonita” o “que sea fácil”.

### Historias de usuario

Puedes expresarlos así:

```text
Como [tipo de persona], quiero [acción], para [beneficio].
```

Ejemplos:

- Como integrante del club, quiero ver los libros disponibles para elegir la próxima lectura.
- Como integrante del club, quiero filtrar por género para encontrar opciones rápidamente.

### Criterios de aceptación

Añade una condición que puedas revisar para cada historia. Puedes usar “Dado / Cuando / Entonces”:

```text
Dado que hay libros de varios géneros,
cuando elijo “Ciencia ficción”,
entonces veo solo los libros de ese género.
```

Los criterios deben poder demostrarse en el navegador. Define primero entre tres y cinco historias para mantener el alcance pequeño.

### Requisitos funcionales y de calidad

- **Funcionales:** acciones y resultados, como buscar, filtrar, guardar o cambiar un estado.
- **De calidad:** cómo debe funcionar la experiencia, como ser legible en móvil, mostrar errores comprensibles y no perder datos al recargar.

## 4. Define el alcance: núcleo y después

Haz dos listas:

- **Debe tener:** lo necesario para resolver el problema y cumplir los criterios de aceptación.
- **Podría tener:** mejoras que solo harás si el núcleo ya funciona.

Escribe también algo que explícitamente dejarás fuera. Por ejemplo: “No habrá cuentas de usuario ni pagos”. Esto protege el tiempo y evita que el proyecto se vuelva demasiado grande.

## 5. Modela el dominio con palabras sencillas

El **dominio** es el tema y las reglas del problema: por ejemplo, administrar libros para un club. Haz una lista de sustantivos (libro, reunión, integrante) y verbos (buscar, proponer, marcar como leído). Elige nombres que coincidan con las palabras del usuario.

Define los datos que realmente necesita tu versión inicial:

```ts
type Libro = {
  id: string;
  titulo: string;
  autor: string;
  genero: string;
  leido: boolean;
};
```

No modeles todos los detalles del mundo real. Modela lo que necesitan tus requisitos actuales.

### Una idea de DDD, sin aplicar todo DDD

Domain-Driven Design (DDD) propone que el diseño del software se apoye en el entendimiento del problema y en un vocabulario compartido. Para este proyecto tomaremos solo dos ideas útiles:

- **Lenguaje compartido:** nombres del código, interfaz y conversaciones deben significar lo mismo. Si el usuario dice “lectura pendiente”, evita inventar nombres técnicos diferentes sin razón.
- **Límite del dominio:** este proyecto trata de organizar lecturas del club. Funciones ajenas a esa necesidad quedan fuera por ahora.

DDD completo incluye técnicas y patrones para dominios complejos. No necesitas implementar agregados, microservicios ni capas sofisticadas para este curso. La documentación de Microsoft recomienda no cargar una aplicación CRUD sencilla con patrones complejos; Martin Fowler también destaca que DDD resulta especialmente útil en dominios complejos. Aquí usamos sus ideas como ayuda para pensar, no como una arquitectura empresarial.

## 6. Dibuja el recorrido principal y la pantalla

Escribe el recorrido en pasos, por ejemplo:

```text
La persona abre la lista
→ los libros se cargan
→ busca o filtra
→ abre una tarjeta
→ marca un libro como leído
→ ve el cambio reflejado
```

Haz un boceto sencillo con las zonas principales. Puedes dibujarlo a mano: encabezado, búsqueda/filtros, lista, acción y estados de carga/error. Un boceto sirve para conversar; no tiene que ser un diseño final.

## 7. Organiza carpetas por responsabilidad y funcionalidad

No existe una estructura única que debas copiar. Para una aplicación pequeña, empieza con una estructura que puedas recorrer. Agrupa junto lo que cambia por la misma funcionalidad; deja las piezas compartidas en un lugar común.

```text
src/
├── app/
│   └── App.tsx             # organiza la pantalla principal
├── features/
│   └── libros/
│       ├── components/     # TarjetaLibro, ListaLibros
│       ├── services/       # llamadas HTTP relacionadas con libros
│       ├── types.ts        # tipo Libro y respuesta de la API
│       └── utils.ts        # filtros u operaciones propias de libros
├── shared/
│   ├── components/         # componentes reutilizados en varias funciones
│   └── styles/             # estilos globales
└── main.tsx                # punto de entrada de React
```

Empieza más pequeño si todavía no necesitas todas las carpetas. Por ejemplo, no crees `utils`, `services` o `shared` vacíos para cumplir un dibujo. Si solo una función usa un componente, puede vivir dentro de esa función. Muévelo a `shared` cuando realmente se reutilice.

Esta organización orientada a funcionalidades tiene afinidad con la idea de DDD de agrupar código según el problema, pero **no convierte automáticamente** tu aplicación en DDD. Su propósito práctico es que puedas encontrar la interfaz, los datos y las operaciones de “libros” en un lugar entendible.

## 8. Decide cómo comprobar que funciona

Por cada requisito, anota cómo lo demostrarás. Ejemplo:

| Requisito | Comprobación |
|---|---|
| Se cargan libros desde la API | Abrir la página y ver las tarjetas; simular o provocar una respuesta fallida y comprobar el mensaje. |
| Se puede filtrar por género | Elegir un género y confirmar que los demás libros no aparecen. |
| El estado leído se conserva | Marcar un libro, recargar y confirmar que el estado sigue igual. |

Incluye el caso vacío y los errores de red, no solo el camino feliz.

## Plantilla de planeación

Antes de programar, completa este resumen en el README del proyecto:

```text
Nombre:
Persona usuaria:
Problema observado o supuesto:
Mi solución en una frase:
Tres requisitos funcionales:
Dos criterios de aceptación:
Requisitos de calidad:
Qué queda fuera:
Datos principales:
Recorrido principal:
Riesgos o dudas:
Cómo comprobaré que funciona:
```

No esperes a que el plan sea perfecto. Cuando una persona pruebe una primera versión, actualiza los requisitos según lo que aprendas.

## Fuentes para profundizar

- [Domain-Driven Design, de Martin Fowler](https://martinfowler.com/bliki/DomainDrivenDesign.html): vocabulario compartido y diseño centrado en el dominio.
- [Diseñar un microservicio orientado a DDD, Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/ddd-oriented-microservice): explica límites y señala que las reglas avanzadas de DDD se justifican sobre todo en problemas complejos.
- [Separar componentes en archivos, React](https://react.dev/learn/importing-and-exporting-components): cuándo separar componentes para facilitar lectura y reutilización.