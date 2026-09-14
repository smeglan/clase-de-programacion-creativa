# Taller de modelado y React: Fila creativa

## Un solo proyecto, máximo 4 horas

Construye una interfaz gráfica para gestionar una fila de turnos. La aplicación permite registrar personas, ver quién sigue y atender en orden de llegada.

El proyecto enseña abstracción, objetos, arreglos, estado en React y el comportamiento de una **cola FIFO** sin intentar cubrir demasiadas estructuras a la vez.

> **FIFO** significa *First In, First Out*: la primera persona que entra a la fila es la primera que se atiende.

## Producto final esperado

Una página React publicada que tenga:

- formulario para registrar nombre y motivo del turno;
- tarjeta destacada con el siguiente turno;
- lista visual de personas en espera;
- botón para atender al siguiente turno;
- mensaje claro cuando no haya personas en la fila;
- un diseño simple, legible y propio.

## Lo que vas a modelar

Antes de crear componentes, define qué representa cada dato:

```ts
type Turno = {
  id: string;
  nombre: string;
  motivo: string;
};
```

Un `Turno` representa una solicitud de atención. La fila completa es un arreglo:

```ts
const turnosIniciales: Turno[] = [
  { id: "t-1", nombre: "Lina", motivo: "Pregunta sobre React" },
  { id: "t-2", nombre: "Tomás", motivo: "Error de instalación" },
];
```

La abstracción es intencional: no guardamos la historia completa de la persona, solo lo necesario para organizar la atención.

## Plan de trabajo: 4 horas

| Bloque | Tiempo | Meta |
|---|---:|---|
| 1. Modelo y pantalla estática | 45 min | Definir `Turno`, crear datos de ejemplo y dibujar la interfaz |
| 2. Formulario y estado | 60 min | Agregar turnos desde inputs controlados |
| 3. Comportamiento FIFO | 60 min | Atender el primer turno sin mutar el estado |
| 4. Pruebas, diseño y publicación | 75 min | Probar casos, mejorar estilos, documentar y publicar |

## Paso 1. Dibuja la interfaz antes de programar

En papel o en un comentario, decide dónde estarán:

```text
┌─────────────────────────────────────┐
│ Fila creativa                       │
│ [Nombre         ] [Motivo        ]  │
│ [Agregar turno]                     │
├─────────────────────────────────────┤
│ Siguiente: Lina                     │
│ Motivo: Pregunta sobre React         │
│ [Atender siguiente]                  │
├─────────────────────────────────────┤
│ En espera                            │
│ 1. Lina - Pregunta sobre React       │
│ 2. Tomás - Error de instalación      │
└─────────────────────────────────────┘
```

No busques un diseño perfecto. Busca que la relación entre datos y pantalla sea clara.

## Paso 2. Crea estado para la fila y el formulario

```tsx
import { useState } from "react";

type Turno = {
  id: string;
  nombre: string;
  motivo: string;
};

const turnosIniciales: Turno[] = [
  { id: "t-1", nombre: "Lina", motivo: "Pregunta sobre React" },
  { id: "t-2", nombre: "Tomás", motivo: "Error de instalación" },
];

function App() {
  const [turnos, setTurnos] = useState<Turno[]>(turnosIniciales);
  const [nombre, setNombre] = useState("");
  const [motivo, setMotivo] = useState("");

  // Agrega aquí las funciones del proyecto.
}
```

`turnos` es el estado principal. `nombre` y `motivo` son el estado temporal del formulario.

## Paso 3. Agrega un turno

Usa una cláusula de guarda para evitar turnos vacíos y crea un nuevo arreglo en lugar de modificar el actual.

```tsx
function agregarTurno() {
  const nombreLimpio = nombre.trim();
  const motivoLimpio = motivo.trim();

  if (!nombreLimpio || !motivoLimpio) {
    return;
  }

  const nuevoTurno: Turno = {
    id: crypto.randomUUID(),
    nombre: nombreLimpio,
    motivo: motivoLimpio,
  };

  setTurnos((filaActual) => [...filaActual, nuevoTurno]);
  setNombre("");
  setMotivo("");
}
```

El operador `...` crea una nueva lista. Esto importa porque React debe recibir un estado nuevo para representar el cambio.

## Paso 4. Atiende al siguiente turno

El primer elemento de la lista es el siguiente por atender. No uses `shift()` directamente sobre el estado, porque modifica el arreglo original.

```tsx
function atenderSiguiente() {
  setTurnos((filaActual) => filaActual.slice(1));
}
```

`slice(1)` crea una nueva lista desde el segundo elemento. De este modo respetamos FIFO y evitamos mutar el estado.

## Paso 5. Muestra la información en React

```tsx
const siguienteTurno = turnos[0];

return (
  <main>
    <h1>Fila creativa</h1>

    <label>
      Nombre
      <input value={nombre} onChange={(evento) => setNombre(evento.target.value)} />
    </label>

    <label>
      Motivo
      <input value={motivo} onChange={(evento) => setMotivo(evento.target.value)} />
    </label>

    <button onClick={agregarTurno}>Agregar turno</button>

    {siguienteTurno ? (
      <section>
        <h2>Siguiente: {siguienteTurno.nombre}</h2>
        <p>{siguienteTurno.motivo}</p>
        <button onClick={atenderSiguiente}>Atender siguiente</button>
      </section>
    ) : (
      <p>No hay personas en espera.</p>
    )}

    <ol>
      {turnos.map((turno) => (
        <li key={turno.id}>
          {turno.nombre} - {turno.motivo}
        </li>
      ))}
    </ol>
  </main>
);
```

## Casos que debes probar

| Caso | Resultado esperado |
|---|---|
| Agregar nombre y motivo válidos | El turno aparece al final de la lista |
| Intentar agregar un campo vacío | No se agrega un turno vacío |
| Atender con varias personas | Sale la primera y la segunda pasa a ser siguiente |
| Atender la última persona | Aparece el mensaje de fila vacía |
| Agregar después de vaciar la fila | El nuevo turno aparece como siguiente |

## Criterios de logro

- El tipo `Turno` representa datos relacionados y tiene nombres claros.
- La fila se modela como `Turno[]`.
- Agregar un turno usa un nuevo arreglo.
- Atender respeta FIFO.
- La interfaz muestra estados con y sin turnos.
- El estudiante puede explicar por qué el primer elemento es el siguiente.
- La aplicación está publicada y enlazada desde el portafolio.

## Extensiones opcionales

Solo haz estas extensiones si el núcleo ya funciona.

### A. Historial de atención - pila LIFO

Guarda los turnos atendidos en un historial y añade un botón para restaurar el último. Así aparece una pila: el último turno atendido es el primero que puede volver.

### B. Prioridad

Agrega una prioridad `normal` o `urgente`. Antes de hacerlo, explica cómo cambia la regla: ya no sería una cola FIFO pura, sino una cola de prioridad.

### C. Diseño creativo

Convierte la fila en sala de espera espacial, tablero de videojuego, atención de mascotas, cola de conciertos o laboratorio futurista. El diseño puede cambiar; el modelo y la regla FIFO deben seguir siendo comprensibles.

## Reflexión final

En la página del proyecto responde:

1. ¿Qué representa un `Turno` y qué información decidiste no guardar?
2. ¿Por qué la fila es un arreglo?
3. ¿Por qué `slice(1)` es preferible a mutar el estado con `shift()`?
4. ¿Qué pasaría si atendieras siempre al último elemento?
5. ¿Qué cambiaría si agregas prioridad?
