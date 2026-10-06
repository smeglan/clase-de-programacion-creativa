# Guía: leer una API REST

**Estás aquí:** [Inicio](../README.md) › [8. Proyecto final](README.md) › **Guía: leer una API REST**

Tu proyecto va a empezar con una colección de datos que no escribiste: alguien más la publicó en internet y tu [página](../glosario.md#html) la pide cuando se abre. Ese "alguien más" es una [API](../glosario.md#api), y la forma en que este curso la pide sigue el estilo [REST](../glosario.md#rest). Esta guía explica esa conversación completa, de principio a fin: qué le pides, qué te contestan, en qué formato llegan los datos y qué tiene que mostrar tu interfaz mientras espera.

No es teoría aparte del proyecto: cada sección corresponde a un requisito de la [guía del proyecto](proyecto.md) y a una etapa del [plan de una semana](proyecto.md#plan-sugerido-para-una-semana-de-clase). Al terminar, vas a poder seguir el camino de un [recurso](../glosario.md#recurso) desde la dirección que la pidió hasta la tarjeta que se dibuja en pantalla, y explicar cada tramo de ese camino.

> **¿Te frenó una palabra?** El [glosario del curso](../glosario.md#capitulo-9) explica [API](../glosario.md#api), [REST](../glosario.md#rest), [HTTP](../glosario.md#http), [JSON](../glosario.md#json), [endpoint](../glosario.md#endpoint), [recurso](../glosario.md#recurso) y [método HTTP](../glosario.md#metodo-http), una por una y con ejemplos.

## Objetivos

Al terminar esta guía deberías poder:

- Explicar con tus palabras qué es una API y qué quiere decir que una API siga el estilo REST.
- Distinguir la solicitud de la respuesta HTTP y leer un código de estado (`200`, `404`, `500`).
- Reconocer un [JSON](../glosario.md#json) y convertirlo al [tipo](../glosario.md#tipo) que usa tu aplicación.
- Explicar qué significan `GET`, `POST`, `PUT` y `DELETE`, y por qué este proyecto solo pide `GET`.
- Escribir una consulta de lectura con `fetch` y `await`, comprobando `response.ok` antes de leer el cuerpo.
- Nombrar los cuatro estados que la interfaz debe mostrar: cargando, éxito, error y vacío.
- Hacer las preguntas del contrato al docente antes de programar la conexión.
- Decidir qué datos vienen de la API y cuáles se guardan en `localStorage`, sin mezclarlos.

> **Cómo probar los ejemplos**
>
> Los ejemplos están escritos para un proyecto Vite con la [plantilla](../glosario.md#plantilla) `react-ts`, como el que describe el [README del módulo 03](../03-react-typescript/README.md). Van en archivos dentro de `src/features/<tu-tema>/`, y el que usa React se importa desde `App.tsx`.
>
> - **Con la API del docente:** usa la [URL](../glosario.md#url) que te compartió y cambia `URL_API` por esa dirección.
> - **Sin API todavía:** guarda una respuesta de ejemplo en `public/ejemplo.json` y úsala como dirección. Ese archivo es tu mesa de trabajo: cuando la API real esté disponible, solo cambias la constante.
>
> Abre las herramientas del navegador (`F12`) y mira la pestaña *Network* (Red): cada consulta que hace tu página aparece ahí con su método, su dirección, su código de estado y su cuerpo. Es la mejor forma de ver con los ojos lo que esta guía explica. Si algo falla, la misma pestaña dice en qué tramo se rompió la conversación.

**En esta guía:**

1. [El problema: los datos no pueden vivir en tu código](#1-el-problema-los-datos-no-pueden-vivir-en-tu-código)
2. [HTTP: una conversación con dos mensajes](#2-http-una-conversación-con-dos-mensajes)
3. [JSON: el formato en el que viajan los datos](#3-json-el-formato-en-el-que-viajan-los-datos)
4. [REST: el acuerdo que le pone nombre a las cosas](#4-rest-el-acuerdo-que-le-pone-nombre-a-las-cosas)
5. [Métodos HTTP: qué le estás pidiendo](#5-métodos-http-qué-le-estás-pidiendo)
6. [Tu primera consulta de lectura con `fetch`](#6-tu-primera-consulta-de-lectura-con-fetch)
7. [Los cuatro estados de la interfaz](#7-los-cuatro-estados-de-la-interfaz)
8. [El contrato con el docente y los límites del proyecto](#8-el-contrato-con-el-docente-y-los-límites-del-proyecto)
9. [Errores frecuentes](#9-errores-frecuentes)
10. [Chuleta rápida](#10-chuleta-rápida)
11. [Herramientas para profundizar](#11-herramientas-para-profundizar)
12. [Lo que viene después](#12-lo-que-viene-después)

- [Objetivos](#objetivos)

## 1. El problema: los datos no pueden vivir en tu código

Hasta ahora tus colecciones vivían adentro del [código](../glosario.md#codigo):

```ts
const libros: Libro[] = [
  { id: "1", titulo: "El Aleph", autor: "Borges", genero: "Cuentos", leido: false },
  { id: "2", titulo: "Cien años de soledad", autor: "García Márquez", genero: "Novela", leido: true },
];
```

Eso funciona, y lo sigues necesitando para los ejemplos pequeños. Pero tiene tres problemas en un proyecto real:

- **Quien conoce los datos mejor no eres tú.** Un catálogo de obras del club lo mantiene otra persona; si está escrito en tu archivo, cada cambio es un commit tuyo.
- **Copia y pega.** Si la misma colección aparece en dos componentes, ya tienes dos versiones que pueden diferir.
- **Crecen mal.** Ocho elementos entran en un archivo; cuatrocientos no.

La solución es mover los datos a un lugar que tenga dirección propia y pedirlos cuando la página los necesita. Ese lugar es una API, y el pedido se hace con una consulta [HTTP](../glosario.md#http). El resto de esta guía es ese pedido, abierto en canal.

```text
Antes                          Después
----                           ----
tu código                      la API
contiene los datos      →       contiene los datos
tu página los dibuja    →       tu página los pide y los dibuja
```

## 2. HTTP: una conversación con dos mensajes

[HTTP](../glosario.md#http) es el acuerdo con el que una página y una computadora ajena se piden cosas. Tiene una regla que ordena todo lo demás: **siempre hay dos mensajes y siempre van en ese orden**.

- **Solicitud** (*request*): lo que tu [programa](../glosario.md#programa) pide. Tiene dos piezas que te importan: el [método HTTP](../glosario.md#metodo-http) (qué quieres hacer) y la dirección (a quién se lo pides).
- **Respuesta** (*response*): lo que el otro lado contesta. Tiene dos piezas que te importan: el código de estado (¿cómo fue?) y el cuerpo (los datos, si los hay).

```text
Tu página                                              La API
   |                                                      |
   |  1. solicitud: GET /api/libros                       |
   | ---------------------------------------------------> |
   |                                                      |
   |  2. respuesta: 200 OK  +  cuerpo con los libros      |
   | <--------------------------------------------------- |
   |                                                      |
```

### El código de estado

Es un número de tres dígitos que resume el resultado. No hace falta conocerlos todos; con cinco alcanza para leer cualquier mensaje:

| Código | Qué significa | Qué hace tu interfaz |
|---|---|---|
| `200` | Todo salió bien y el cuerpo trae lo pedido. | Muestra los datos. |
| `400` | La solicitud estaba mal escrita (falta un parámetro, el cuerpo no se entiende). | Error: el problema es del cliente, revisa la URL o el cuerpo. |
| `404` | Esa dirección no existe en ese servidor. | Error: revisa la URL y el nombre del [endpoint](../glosario.md#endpoint). |
| `500` | El servidor recibió la petición y se rompió al responderla. | Error: el problema no es tuyo; reintentar después. |

> **La trampa más común de toda la guía:** `fetch` **no** se rechaza cuando el servidor contesta `404` o `500`. Para `fetch`, eso es una respuesta perfectamente válida: la falla de red es lo único que lanza un error. Por eso en la sección [6](#6-tu-primera-consulta-de-lectura-con-fetch) se comprueba `response.ok` a mano.

### HTTP no recuerda nada

HTTP es [sin estado](../glosario.md#http): cada solicitud llega sin memoria de la anterior. Si pediste el listado hace un segundo, la siguiente solicitud no sabe que lo pediste. Para este proyecto eso significa dos cosas: la colección inicial se pide de nuevo en cada carga de la página, y los cambios que haces sobre los elementos no viajan solos: o los guardas en `localStorage`, como pide el [proyecto](proyecto.md#4-persistencia-y-publicación), o se los envías a la API con un método de escritura, si el docente lo habilitó.

## 3. JSON: el formato en el que viajan los datos

El cuerpo de la respuesta llega como **texto**. No llega un [arreglo](../glosario.md#arreglo) de [objetos](../glosario.md#objeto): llega una cadena que representa uno. Ese formato se llama [JSON](../glosario.md#json) y es casi el mismo lenguaje que ya sabes leer:

```json
[
  { "id": "1", "titulo": "El Aleph", "autor": "Borges", "genero": "Cuentos", "leido": false },
  { "id": "2", "titulo": "Cien años de soledad", "autor": "García Márquez", "genero": "Novela", "leido": true }
]
```

Las reglas que importan, todas:

- los nombres de propiedad van entre comillas dobles;
- `true`, `false` y `null` se escriben en minúscula;
- no hay comentarios ni comas colgando al final;
- los [tipos](../glosario.md#tipo) que trae son solo texto, número, booleano, `null`, listas y objetos.

```ts
const texto = '{"id":"1","titulo":"El Aleph"}';
const dato = JSON.parse(texto);   // string  →  objeto

const deVuelta = JSON.stringify(dato);  // objeto  →  string
```

`JSON.parse` convierte texto en objeto y `JSON.stringify` hace el viaje de vuelta. Cuando tu programa dice `await respuesta.json()`, está pidiendo exactamente eso: que el texto del cuerpo se convierta en un [objeto](../glosario.md#objeto) de JavaScript.

### JSON no es tu tipo de TypeScript

Este es el punto donde conviene ir despacio. `JSON.parse` devuelve `any`, es decir: **no sabe qué hay adentro**. Tu [type](../glosario.md#type) `Libro` sí lo sabe, pero TypeScript no puede comprobarlo por su cuenta. Entre el JSON y tu tipo hay dos decisiones posibles:

- **Convertir sin comprobar:** `return datos as Libro[]`. Es corto y funciona si el contrato es confiable. Es lo que hace la mayoría del código de este curso.
- **Convertir comprobando:** pasar cada elemento por una función que verifica los campos. Es más largo y descubre respuestas rotas antes de que rompan la pantalla.

```ts
function esLibro(dato: unknown): dato is Libro {
  if (typeof dato !== "object" || dato === null) return false;
  const posible = dato as Record<string, unknown>;
  return (
    typeof posible.id === "string" &&
    typeof posible.titulo === "string" &&
    typeof posible.autor === "string" &&
    typeof posible.genero === "string" &&
    typeof posible.leido === "boolean"
  );
}

function aLibros(datos: unknown): Libro[] {
  if (!Array.isArray(datos)) {
    throw new Error("La respuesta no es una lista de libros");
  }
  return datos.filter(esLibro);
}
```

Elige una de las dos y quédate con ella. Lo que no se puede hacer es fingir que el texto ya era un `Libro[]`: eso compila perfecto y falla en el navegador, en la línea donde menos lo esperas.

## 4. REST: el acuerdo que le pone nombre a las cosas

[REST](../glosario.md#rest) no es un lenguaje ni una herramienta que se instala: es un conjunto de acuerdos para diseñar una API de manera predecible. Si tu API es REST, quien la mira por primera vez puede adivinar cómo pedir las cosas. Para el proyecto alcanzan cuatro acuerdos:

1. **Todo es un [recurso](../glosario.md#recurso).** Un libro, la colección de libros, un autor: cosas con nombre propio del problema que estás resolviendo.
2. **La dirección nombra al recurso.** Se usan sustantivos en plural, sin verbos: `/api/libros`, `/api/libros/1`. No `/api/traerLibros` ni `/api/libros/obtener`.
3. **El método dice la intención.** Leer, crear, reemplazar y borrar se expresan con el método, no con la dirección. La misma dirección `/api/libros` significa cosas distintas según si llega con `GET` o con `POST`.
4. **El código de estado cuenta cómo fue.** `200` si salió bien, `404` si el recurso no existe, `500` si el servidor se rompió. No hace falta leer un texto especial: el número ya informa.

```text
Dirección (el qué)        Método (la intención)      Código (el resultado)
-----------------         -------------------        ---------------------
GET    /api/libros        leer la colección    →      200 OK
GET    /api/libros/1      leer uno solo        →      200 OK  |  404
POST   /api/libros        crear uno            →      201 Created
DELETE /api/libros/1      borrar uno           →      200 OK  |  404
```

Un detalle que ayuda a entender la palabra: como HTTP no guarda nada entre una solicitud y la siguiente, REST describe el estado de un recurso **en cada respuesta**. Tu aplicación, en cambio, sí tiene estado: lo que cambia en la pantalla es el estado de React, y la API es de donde salen los datos que lo alimentan.

### No todo lo que parece REST lo es

Vas a encontrar APIs que mezclan verbos en la dirección o devuelven códigos distintos de los esperados. Esto está bien para el curso: la guía te pide que **descubras el formato real y lo expliques**, no que lo corrijas. Anota lo que encontraste y trabaja con lo que hay.

## 5. Métodos HTTP: qué le estás pidiendo

| Método | Significado | Uso en este proyecto |
|---|---|---|
| `GET` | Leer. No modifica nada. | **Obligatorio.** Es la consulta de lectura que trae la colección inicial. |
| `POST` | Crear un recurso nuevo. | Extensión, solo si el docente lo habilita y acuerdas el contrato de datos. |
| `PUT` | Reemplazar un recurso completo. | Fuera del alcance del núcleo. |
| `PATCH` | Modificar una parte de un recurso. | Fuera del alcance del núcleo. |
| `DELETE` | Borrar un recurso. | Fuera del alcance del núcleo. |

Dos reglas de diseño que conviene escuchar una vez:

- **`GET` no cambia nada.** Pedir el listado mil veces deja la API igual que al principio. Por eso es seguro de reintentar y por eso tu interfaz puede volver a pedirlo cuando quiera.
- **`GET` no lleva cuerpo.** No hay "datos del pedido" dentro de una solicitud `GET`: todo lo que necesitas saber está en la dirección. Si un día necesitas enviar datos, ese es `POST`.

El [requisito](../glosario.md#requisito) del proyecto es tajante sobre esto: la primera versión requiere **una consulta de lectura**, por ejemplo `GET /api/recursos`. El servidor no necesita cuentas, autenticación ni base de datos. La creación y los cambios van a `localStorage`, que es [persistencia](../glosario.md#localstorage) del navegador, no del servidor.

## 6. Tu primera consulta de lectura con `fetch`

`fetch` es la función del navegador que manda una solicitud y devuelve una promesa con la respuesta. El patrón completo, con las tres comprobaciones que importan:

```ts
// src/features/libros/services/api.ts
import type { Libro } from "../types";

const URL_API = import.meta.env.VITE_API_URL ?? "/ejemplo.json";

export async function obtenerLibros(): Promise<Libro[]> {
  // 1. la solicitud
  const respuesta = await fetch(URL_API);

  // 2. el código de estado
  if (!respuesta.ok) {
    throw new Error(`La API contestó ${respuesta.status}`);
  }

  // 3. el cuerpo, convertido al tipo de la aplicación
  const datos: unknown = await respuesta.json();
  return aLibros(datos);
}
```

Lee la función en voz alta, en orden: **pedí, miré cómo me fue, leí el cuerpo, lo convertí**. Cada paso tiene una responsabilidad distinta y cada uno falla de una manera distinta:

```text
fetch(URL)          →  puede fallar si no hay red (rechaza la promesa)
respuesta.ok        →  false si el código es 4xx o 5xx (hay que mirarlo)
respuesta.json()    →  devuelve `any`; si el cuerpo no es JSON, revienta ahí mismo
aLibros(...)        →  devuelve Libro[] o lanza un error comprensible
```

### Dónde vive esta función

Siguiendo la [estructura por funcionalidad](planeacion-y-arquitectura.md#7-organiza-carpetas-por-responsabilidad-y-funcionalidad) de la guía de planeación, el archivo va en `services/` junto al tipo que describe la respuesta:

```text
src/features/libros/
├── components/     # TarjetaLibro, ListaLibros
├── services/
│   └── api.ts      # obtenerLibros()
├── types.ts        # type Libro  y  la forma de la respuesta
└── utils.ts        # filtros u operaciones propias
```

`types.ts` es el lugar donde documentas **qué forma tiene la respuesta de la API**: el [modelo](../glosario.md#modelo) que escribiste en la planeación y el JSON que llega tienen que hablar el mismo idioma, y ese archivo es donde se anota el acuerdo.

## 7. Los cuatro estados de la interfaz

Una consulta de red tiene cuatro resultados posibles, y el [requisito](../glosario.md#requisito) del proyecto pide los cuatro explícitamente: indicador mientras espera, elementos cuando la respuesta sea correcta, mensaje con reintento si falla, y estado vacío si la respuesta trae cero elementos.

Para expresarlos sin inventar variables sueltas, usa un [tipo unión](../glosario.md#tipo-union) con una propiedad que nombra la fase: el valor solo puede estar en uno de los tres estados, y TypeScript te obliga a cubrir los tres.

```tsx
// src/features/libros/components/CatalogoLibros.tsx
import { useEffect, useState } from "react";
import type { Libro } from "../types";
import { obtenerLibros } from "../services/api";

type EstadoCarga =
  | { fase: "cargando" }
  | { fase: "exito"; libros: Libro[] }
  | { fase: "error"; mensaje: string };

export function CatalogoLibros() {
  const [estado, setEstado] = useState<EstadoCarga>({ fase: "cargando" });

  useEffect(() => {
    let vigente = true;

    obtenerLibros()
      .then((libros) => {
        if (vigente) setEstado({ fase: "exito", libros });
      })
      .catch((error: unknown) => {
        const mensaje = error instanceof Error ? error.message : "Error desconocido";
        if (vigente) setEstado({ fase: "error", mensaje });
      });

    return () => {
      vigente = false;
    };
  }, []);

  if (estado.fase === "cargando") return <p>Cargando catálogo…</p>;

  if (estado.fase === "error") {
    return (
      <div role="alert">
        <p>No se pudo cargar el catálogo: {estado.mensaje}</p>
        <button onClick={() => window.location.reload()}>Reintentar</button>
      </div>
    );
  }

  if (estado.libros.length === 0) {
    return <p>No hay libros en la colección todavía.</p>;
  }

  return (
    <ul>
      {estado.libros.map((libro) => (
        <li key={libro.id}>{libro.titulo} — {libro.autor}</li>
      ))}
    </ul>
  );
}
```

Este es el momento en que aparece `useEffect`, el tema que la [guía de React](../03-react-typescript/guia-react.md#20-lo-que-viene-después) dejó para después justamente porque trabaja con algo que **ocurre fuera de la pantalla**: pedir datos a otro lado. Tres detalles que se repiten en todo el curso:

- **La consulta se dispara una sola vez:** el `[]` del final evita que se repita en cada redibujo.
- **La bandera `vigente`** evita escribir estado de una consulta que la persona ya abandonó al navegar.
- **El renderizado condicional** cubre los cuatro casos antes de dibujar la lista feliz; el estado vacío va antes del `map`, no dentro.

Si tu API no está disponible, la misma estructura sirve: la función de `services/` lanza un error y la interfaz muestra el mensaje con reintentar. Eso es exactamente la [prueba](../glosario.md#prueba) que pide el proyecto de "simular una respuesta fallida".

## 8. El contrato con el docente y los límites del proyecto

Antes de escribir la primera línea de la sección anterior, necesitas cinco respuestas. Eso es el **contrato de datos**: el acuerdo mínimo para que tu código y la API hablen el mismo idioma.

```text
Contrato de la API del proyecto

1. URL completa de la consulta de lectura:
   ¿Es relativa (/api/libros) o absoluta (https://...)?
2. Método:   ¿GET?  ¿Hay otro habilitado?
3. Forma de la respuesta:
   ¿Llega el arreglo directo, o dentro de { datos: [...] }?
   ¿Cuáles son los campos y de qué tipo?
4. Códigos que devuelve:
   ¿Qué contesta si no hay elementos? ¿Si la URL está mal?
5. Escritura: ¿está habilitada o los cambios van a localStorage?
```

Con esas respuestas anotadas en el README, dos decisiones quedan resueltas de entrada:

- **La URL se configura, no se escribe.** En el proyecto va en una [variable de entorno](../glosario.md#variables-de-entorno) como `VITE_API_URL`, leída con `import.meta.env.VITE_API_URL`, y Vercel te la vuelve a pedir al [publicar](../05-publicacion/README.md). Así cambias de dirección sin tocar el [código](../glosario.md#codigo).
- **Las claves privadas no van en el frontend.** Todo lo que empieza con `VITE_` viaja visible en el código que se descarga en el navegador. Si el docente te da un token, pregunta cómo debe guardarse; la respuesta nunca es "en un archivo del repositorio".

Y una separación que conviene escribir en el README porque es la pregunta más probable en la presentación:

```text
Viene de la API                Se guarda en localStorage
------------------             --------------------------
la colección inicial           los elementos que creaste
(ocho o más registros)         los favoritos / estados modificados

se pide en cada carga          sobrevive a la recarga
```

Si mezclas las dos columnas, la próxima carga de la página pide la colección y tapa los cambios de la persona.

## 9. Errores frecuentes

1. **Olvidar `await`.** `const datos = fetch(url)` guarda una promesa, no los libros. El [error](../glosario.md#error) aparece después, cuando intentas hacer `map` sobre algo que no es un arreglo. Regla: todo lo que viene de `await` llega después, y el `await` va en la línea donde usas el resultado.
2. **Confiar en `fetch` para detectar errores.** Como viste en la sección [2](#2-http-una-conversación-con-dos-mensajes), un `404` llega como una respuesta normal. Sin `if (!respuesta.ok)` tu programa intenta leer `respuesta.json()` de un cuerpo que dice `Not Found` y revienta en un lugar que no tiene nada que ver con la red.
3. **Asumir que siempre hay elementos.** Si la respuesta puede traer `[]`, la lista vacía es un estado de la interfaz, no un caso raro. Compruébalo en la práctica: haz que la API devuelva un `[]` y mira qué dibuja tu pantalla.
4. **Escribir la URL en varios archivos.** Si la dirección aparece en el componente, en el servicio y en un archivo de configuración, el próximo cambio toca tres lugares. Una constante, un solo archivo.
5. **Creer que `localStorage` es la API.** Son dos orillas distintas: una vive en el navegador de esa persona, la otra vive en el servidor. Guarda en cada una lo que corresponde según el [contrato](#8-el-contrato-con-el-docente-y-los-límites-del-proyecto).
6. **Pegar la respuesta de ejemplo en JSX.** La [colección inicial se dibuja desde los datos](proyecto.md#1-interfaz-y-modelo-propios), no tarjeta por tarjeta. Si escribiste ocho elementos a mano en el JSX, la consulta no está haciendo su trabajo.

## 10. Chuleta rápida

**El flujo completo, de arriba para abajo:**

```text
URL  →  fetch()  →  respuesta.ok  →  respuesta.json()  →  convertir al tipo
                                                                  ↓
     interfaz  ←  estado (cargando | éxito | error | vacío)  ←  datos
```

**Métodos:**

| Verbo | Se lee como | Cambia datos |
|---|---|---|
| `GET` | "dame" | no |
| `POST` | "crea" | sí |
| `PUT` | "reemplaza" | sí |
| `DELETE` | "borra" | sí |

**Códigos que vas a ver en este proyecto:**

| Código | Frase | Acción |
|---|---|---|
| `200` | "acá está" | dibujar |
| `400` | "no te entendí" | revisar la solicitud |
| `404` | "no existe" | revisar la dirección |
| `500` | "me rompí" | reintentar más tarde |

**Las cuatro preguntas de la respuesta:**

1. ¿Pudo pedirse? (hubo red)
2. ¿Salió bien? (`response.ok`)
3. ¿Qué trae? (`response.json()` → convertir)
4. ¿Está vacío? (estado vacío)

## 11. Herramientas para profundizar

- **MDN, "Generalidades del protocolo HTTP"** (`developer.mozilla.org/es/docs/Web/HTTP/Overview`): la fuente oficial sobre solicitud y respuesta, métodos, códigos y por qué HTTP es sin estado. Es la sección [2](#2-http-una-conversación-con-dos-mensajes) explicada con más detalle y en español.
- **MDN, "Códigos de estado de respuesta HTTP"** (`developer.mozilla.org/es/docs/Web/HTTP/Status`): la lista completa, agrupada por rangos (2xx, 4xx, 5xx). Útil cuando tu API conteste con un código que no viste en la chuleta.
- **MDN, `fetch()`** (`developer.mozilla.org/es/docs/Web/API/Window/fetch`): la referencia del método, incluida la nota clave de esta guía: comprobar que la promesa se resuelve **y** que `Response.ok` sea `true`.
- **MDN, glosario "REST"** (`developer.mozilla.org/es/docs/Glossary/REST`): la definición corta, con la aclaración de que muchas APIs se llaman REST sin cumplir todas las reglas — justo lo de la sección [4](#4-rest-el-acuerdo-que-le-pone-nombre-a-las-cosas).
- **MDN, glosario "API"** (`developer.mozilla.org/es/docs/Glossary/API`): qué significa la sigla y por qué el mismo nombre sirve para describir tanto una interfaz de JavaScript como un conjunto de direcciones que se piden por HTTP.
- **MDN, glosario "JSON"** (`developer.mozilla.org/es/docs/Glossary/JSON`): qué puede y qué no puede representar JSON, y por qué no es idéntico a la sintaxis de JavaScript.
- **La pestaña *Network* del navegador:** no es una lectura, es el laboratorio. Ábrela con `F12`, carga tu página y mira cada solicitud: método, dirección, código, tiempo y cuerpo. Ninguna explicación reemplaza eso.

## 12. Lo que viene después

Esta guía te dio la conversación completa: **HTTP** es el canal de dos mensajes, **JSON** es el formato en que viajan los datos, **REST** es el acuerdo que nombra los recursos y le pone intención a cada solicitud, y `fetch` es cómo tu programa participa de esa conversación. Sobre eso se apoyan los cuatro estados de la interfaz y el contrato con el docente.

Lo que sigue es aplicarlo al [reto](proyecto.md): la etapa 3 del [plan](proyecto.md#plan-sugerido-para-una-semana-de-clase) es "estructura y datos — proyecto Vite, carpetas iniciales y colección cargada desde la API", y las [pruebas de aceptación](proyecto.md#pruebas-de-aceptación-mínimas) del proyecto incluyen "carga ocho o más elementos desde la API acordada" y "la interfaz informa claramente si está cargando, si ocurrió un error o si no hay elementos". Ambas se comprueban con lo que acabas de leer.

Antes de seguir, comprueba que puedes responder estas cuatro preguntas sin mirar el código:

1. ¿Por qué un `404` no hace fallar a `fetch`, y qué línea de tu código lo detecta igual?
2. ¿Qué tres cosas tiene una solicitud HTTP y qué dos tiene la respuesta?
3. ¿Por qué la colección inicial va en la API y tus favoritos van en `localStorage`?
4. Si la respuesta llega como `[]`, ¿en qué fase queda tu `EstadoCarga` y qué dibuja la pantalla?

Si las cuatro salen con tus propias palabras, la guía hizo su trabajo. Si alguna no, vuelve a la sección [2](#2-http-una-conversación-con-dos-mensajes) (solicitud y respuesta), a la [6](#6-tu-primera-consulta-de-lectura-con-fetch) (`fetch`) o a la [7](#7-los-cuatro-estados-de-la-interfaz) (estados) y vuelve a intentarlo.

---

**Anterior:** [Planear un proyecto y organizar su arquitectura](planeacion-y-arquitectura.md) · **Siguiente:** [Proyecto: una herramienta interactiva conectada a una API](proyecto.md)
