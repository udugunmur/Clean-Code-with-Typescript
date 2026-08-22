# Parte 3: TypeScript Avanzado y Aplicaciones en el Mundo Real

## Capítulo 12: TypeScript en Sistemas en Evolución

### Sección: Introducción

Hasta este punto del libro, has aprendido sobre las herramientas individuales que hacen que TypeScript sea eficaz en aplicaciones de producción. Has visto por qué los fallos tardíos son costosos, por qué detectar los errores a tiempo es crucial y cómo los tipos pueden hacer mucho más que simplemente anotar valores: pueden expresar intenciones, imponer estructuras y guiar la evolución de un sistema. También has visto cómo aplicar estas ideas en un flujo de trabajo ordenado y centrado en contratos (*contract-first*), donde el frontend y el backend avanzan en sincronía basándose en definiciones compartidas.

Ese enfoque funciona especialmente bien cuando los requisitos se comprenden claramente desde el principio y los equipos evolucionan el sistema de forma coordinada.

Este capítulo explora cómo se aplican estos mismos principios cuando los requisitos cambian, los sistemas crecen y las suposiciones iniciales comienzan a desviarse con el tiempo. En este escenario, el sistema ya funciona: se ha lanzado una prueba de concepto y los usuarios están satisfechos. Sin embargo, cuando los requisitos cambian, se incorporan más desarrolladores y las suposiciones divergen, es cuando los errores se vuelven fáciles de cometer y costosos de descubrir, porque el sistema parece "estable" justo hasta el momento en que falla en producción.

Por tanto, en lugar de introducir un framework nuevo o una característica sintáctica adicional, este capítulo sigue una narrativa concreta: un pequeño chatbot de soporte *full stack* que evoluciona bajo la presión del mundo real. Experimentaremos intencionalmente fallos en tiempo de ejecución para luego introducir progresivamente técnicas de TypeScript que trasladen esos fallos desde el navegador y producción hacia el tiempo de compilación (*build time*), donde son mucho más económicos y fáciles de corregir.

En este capítulo, cubriremos los siguientes temas principales:

- Comenzando con una prueba de concepto rápida
- Detección y manejo de la divergencia de contratos
- Haciendo explícito el contrato
- El problema que los tipos compartidos no resuelven por sí solos
- Protección del límite de JSON con una compuerta en tiempo de ejecución
- Escalado del sistema sin filtrar detalles del proveedor
- Haciendo explícitas las variantes con uniones discriminadas
- Múltiples endpoints sin conversiones de tipos frágiles
- La recompensa: mover los fallos a etapas más tempranas, de forma intencionada

---

### Sección: Requisitos técnicos

Todo el código de este capítulo está disponible como un espacio de trabajo ejecutable de Nx, y cada hito importante está etiquetado con tags de Git para que puedas situarte en el estado exacto analizado. Puedes descargar el proyecto de ejemplo y el código siguiendo las instrucciones en la sección *Download the example code files* en el Prefacio de este libro.

Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

Encontrarás etiquetas como `ch12-01-poc`, `ch12-02-contract-drift`, etc. No las trates como "versiones que debes copiar", sino como instantáneas dentro de una historia para experimentar cómo cada pequeña mejora transforma lo que el sistema facilita y lo que dificulta.

---

### Sección 1: Comenzando con una prueba de concepto rápida

Imagina que te piden construir un chatbot de soporte asistido por IA sencillo donde los usuarios escriben preguntas en el navegador, ven el historial de conversación y reciben respuestas de una API backend. El frontend envía el historial a un único endpoint, `POST /api/chat`, y el backend responde con un mensaje breve.

En esta etapa inicial, la velocidad prima sobre la exhaustividad. No diseñas un contrato perfecto desde el primer día; dejas que las suposiciones vivan en el código para ganar tracción rápida y aprender lo que los usuarios realmente necesitan.

En el backend, el manejador lee JSON sin tipar y devuelve una respuesta simple:

```typescript
app.post('/api/chat', (req, res) => {
  const messages = req.body.messages; // untyped JSON
  const last = messages?.[messages.length - 1];

  res.json({
    reply: `You said: ${last?.content ?? ''}`,
  });
});
```

En el frontend, la respuesta se consume como "cualquier cosa que devuelva el servidor", y los mensajes se almacenan como `any[]`:

```typescript
const [messages, setMessages] = useState<any[]>([]);

const data = await res.json();
setMessages([...updated, { role: 'assistant', content: data.reply }]);
```

El chatbot funciona y la demo es un éxito. Si descargas la etiqueta `ch12-01-poc` y ejecutas la app, todo parece correcto. Pero pregúntate: ¿dónde está documentado el contrato para este endpoint?

En esta versión, no está escrito en ninguna parte: vive únicamente en la memoria del desarrollador.

Esto parece inofensivo cuando una sola persona escribió ambas partes hace una semana, pero se vuelve extremadamente peligroso cuando el proyecto crece o se vuelve colaborativo, ya que la memoria humana no es un contrato escalable.

---

### Sección 2: Detección y manejo de la divergencia de contratos

Días después, surge un nuevo requisito: la respuesta ya no puede ser una simple cadena de texto (*string*). Debe convertirse en un objeto estructurado que contenga el mensaje del asistente, una lista de referencias y un conjunto de preguntas de seguimiento (*follow-ups*).

Actualizas el backend primero. El endpoint sigue devolviendo una propiedad llamada `reply`, pero ahora `reply` es un objeto en lugar de un string. Conceptualmente, el backend pasa de esto:

```json
{ "reply": "You said: hello" }
```

A una estructura como esta:

```json
{
  "reply": {
    "message": { "role": "assistant", "content": "You said: hello" },
    "references": [],
    "followUps": []
  }
}
```

Antes de abrir el navegador, compilas el frontend. **La compilación pasa sin errores.**

Esta es la parte más peligrosa del desarrollo: una compilación exitosa genera una falsa sensación de seguridad. Parece que TypeScript "aprobó" el cambio, pero en realidad no comprobó nada porque el frontend renunció al tipado en la frontera con `any[]` y `res.json()`.

Cuando refrescas la interfaz y envías un mensaje, React lanza un error en tiempo de ejecución (*runtime error*) al intentar renderizar un objeto donde esperaba un string. Este error, que debió haberse detectado de inmediato, surge tarde, en el navegador y bajo condiciones cercanas a producción (`ch12-02-contract-drift`).

---

### Sección 3: Haciendo explícito el contrato

La solución no consiste en exigir "desarrolladores más cuidadosos" ni en llenar la interfaz de sentencias condicionales dispersas, sino en transformar el acuerdo implícito en un contrato explícito mediante código compartido.

Creamos un módulo compartido (por ejemplo, `libs/chat-contract`) y definimos el contrato del dominio:

```typescript
export interface ChatMessage {
  role: 'user' | 'assistant';
  content: string;
}

export interface ChatRequest {
  messages: ChatMessage[];
}

export interface ChatReference {
  title: string;
  url: string;
}

export interface FollowUpQuestion {
  question: string;
}

export interface ChatReply {
  message: ChatMessage;
  references: ChatReference[];
  followUps: FollowUpQuestion[];
}

export interface ChatResponse {
  reply: ChatReply;
}
```

No estamos modelando el SDK de un proveedor externo (OpenAI o Anthropic), sino nuestro propio dominio de negocio: lo que nuestro producto necesita mostrar al usuario.

Ahora, tanto el frontend como el backend importan estos tipos compartidos.

En el frontend, los mensajes ya no se almacenan como `any[]`, sino con su tipo de dominio:

```typescript
const [messages, setMessages] = useState<ChatMessage[]>([]);
```

Y al invocar la API, tipamos la respuesta explícitamente:

```typescript
async function postChat(request: ChatRequest): Promise<ChatResponse> {
  const res = await fetch('/api/chat', {
    method: 'POST',
    headers: { 'content-type': 'application/json' },
    body: JSON.stringify(request),
  });

  const data = (await res.json()) as ChatResponse;
  return data;
}
```

Ahora, si intentas tratar `reply` como un string, TypeScript detiene la compilación al instante (`ch12-03-shared-contract`), bloqueando el error antes de abrir el navegador.

---

### Sección 4: El problema que los tipos compartidos no resuelven por sí solos

Tener tipos compartidos no garantiza inmunidad absoluta: el punto donde los datos entran al sistema a través de la red (en el backend con `req.body` y en el frontend con `res.json()`) sigue siendo JSON sin tipar.

Todo dato que cruza la red es `unknown` hasta que se valide su estructura en tiempo de ejecución. Los tipos de TypeScript desaparecen tras la compilación; si el backend devuelve un formato inesperado, un proxy altera la respuesta o un despliegue mezcla versiones, TypeScript no puede protegerte sin una validación en tiempo de ejecución.

---

### Sección 5: Protección del límite de JSON con una compuerta en tiempo de ejecución

Definimos una compuerta (*runtime gate*) para validar los datos antes de considerarlos de confianza.

Primero, creamos una función auxiliar para verificar objetos:

```typescript
export function isRecord(value: unknown): value is Record<string, unknown> {
  return typeof value === 'object'&&value!==null;
}
```

A continuación, implementamos guardas de tipo (*type guards*) para los objetos del dominio:

```typescript
import type { ChatMessage, ChatRequest, ChatResponse } from './chat-contract';

export function isChatMessage(value: unknown): value is ChatMessage {
  if (!isRecord(value)) return false;
  const role = value.role;
  return (
    (role === 'user' || role === 'assistant') &&
    typeof value.content === 'string'
  );
}

export function isChatRequest(value: unknown): value is ChatRequest {
  if (!isRecord(value)) return false;
  if (!Array.isArray(value.messages)) return false;
  return value.messages.every(isChatMessage);
}

export function isChatResponse(value: unknown): value is ChatResponse {
  if (!isRecord(value)) return false;
  if (!isRecord(value.reply)) return false;

  const reply = value.reply;

  if (!isRecord(reply.message)) return false;
  if (!isChatMessage(reply.message)) return false;

  if (!Array.isArray(reply.references)) return false;
  if (!Array.isArray(reply.followUps)) return false;

  return true;
}
```

En el backend, validamos `req.body` antes de procesarlo:

```typescript
import type { Request, Response } from 'express';
import type { ChatRequest, ChatResponse } from '@acme/chat-contract';
import { isChatRequest } from '@acme/chat-contract/guards';

app.post('/api/chat', (req: Request, res: Response) => {
  const body: unknown = req.body;

  if (!isChatRequest(body)) {
    res.status(400).json({ error: 'Invalid request body' });
    return;
  }

  const last = body.messages[body.messages.length - 1];

  const response: ChatResponse = {
    reply: {
      message: { role: 'assistant', content: `You said: ${last?.content ?? ''}` },
      references: [],
      followUps: [],
    },
  };

  res.json(response);
});
```

En el frontend, validamos la respuesta antes de consumirla:

```typescript
import type { ChatRequest, ChatResponse } from '@acme/chat-contract';
import { isChatResponse } from '@acme/chat-contract/guards';

async function postChat(request: ChatRequest): Promise<ChatResponse> {
  const res = await fetch('/api/chat', {
    method: 'POST',
    headers: { 'content-type': 'application/json' },
    body: JSON.stringify(request),
  });

  const data: unknown = await res.json();

  if (!isChatResponse(data)) {
    throw new Error('Server returned an invalid ChatResponse shape');
  }

  return data;
}
```

Esto garantiza un fallo rápido y controlado (*fail fast*) si la estructura recibida no coincide con el contrato (`ch12-04-runtime-gate`).

---

### Sección 6: Escalado del sistema sin filtrar detalles del proveedor

Cuando se requiere soportar múltiples proveedores de Modelos de Lenguaje (LLMs), los tipos compartidos deben describir tu dominio y nunca la estructura propietaria del proveedor externo.

Definimos una interfaz de proveedor que retorna tipos de nuestro dominio:

```typescript
import type { ChatMessage, ChatReply } from '@acme/chat-contract';

export interface LlmProvider {
  generateReply(messages: ChatMessage[]): Promise<ChatReply>;
}
```

Cada proveedor actúa como un adaptador (*Adapter*) que encapsula su SDK y transforma la salida al formato de nuestro dominio. Una fábrica (*Factory*) selecciona el proveedor adecuado en tiempo de ejecución:

```typescript
export function createProvider(name: string): LlmProvider {
  switch (name) {
    case 'provider-a':
      return new ProviderA();
    case 'provider-b':
      return new ProviderB();
    default:
      throw new Error(`Unknown provider: ${name}`);
  }
}
```

Esto aísla la volatilidad de los servicios externos detrás de una interfaz estable.

---

### Sección 7: Haciendo explícitas las variantes con uniones discriminadas

Cuando una respuesta puede incluir información adicional (como citas bibliográficas) de forma condicional, el uso indiscriminado de campos opcionales (`?`) genera ambigüedad.

En su lugar, modelamos las respuestas utilizando **uniones discriminadas** (*discriminated unions*):

```typescript
export interface Citation {
  sourceTitle: string;
  url: string;
}

export type ChatResponse =
  | { kind: 'answer'; reply: ChatReply }
  | { kind: 'answer-with-citations'; reply: ChatReply; citations: Citation[] };
```

En el cliente, evaluamos exhaustivamente cada variante con `switch`:

```typescript
switch (response.kind) {
  case 'answer':
    // render reply only
    break;

  case 'answer-with-citations':
    renderCitations(response.citations);
    break;

  default: {
    const _exhaustive: never = response;
    return _exhaustive;
  }
}
```

> **Comprobación exhaustiva con `never`**: Si en el futuro se añade una nueva variante (como `kind: 'rate-limited'`) y se olvida actualizar el `switch`, TypeScript provocará un error de compilación al no poder asignar el nuevo tipo a `never`, garantizando que ninguna variante quede sin manejar.

---

### Sección 8: Múltiples endpoints sin conversiones de tipos frágiles

A medida que crecen los endpoints, el uso repetido de conversiones forzadas (`as SomeType`) anula la seguridad de tipos.

Podemos definir un mapeo centralizado entre rutas y sus tipos de solicitud/respuesta mediante un cliente genérico:

```typescript
import type { ChatRequest, ChatResponse } from '@acme/chat-contract';

type ApiRoutes = {
  '/api/chat': {
    req: ChatRequest;
    res: ChatResponse;
  };
  '/api/health': {
    req: undefined;
    res: { ok: true };
  };
};

async function postJson<K extends keyof ApiRoutes>(
  path: K,
  body: ApiRoutes[K]['req']
): Promise<ApiRoutes[K]['res']> {
  const res = await fetch(path, {
    method: 'POST',
    headers: { 'content-type': 'application/json' },
    body: JSON.stringify(body),
  });

  return (await res.json()) as ApiRoutes[K]['res'];
}
```

El literal de ruta determina automáticamente los tipos requeridos en el cuerpo y en la respuesta devuelta, eliminando conversiones inseguras.

---

### Sección 9: La recompensa: mover los fallos a etapas más tempranas, de forma intencionada

El objetivo de un sistema de tipos riguroso no es la complejidad abstracta, sino hacer que los cambios incorrectos sean difíciles de enviar a producción.

Al sustituir las suposiciones invisibles por contratos explícitos, compuertas en tiempo de ejecución, límites desacoplados de proveedores y comprobaciones exhaustivas, convertimos fallos tardíos e inesperados en errores de compilación inmediatos.

---

### Sección: Resumen

En este capítulo, seguimos la evolución de un sistema TypeScript desde una prueba de concepto inicial hasta una arquitectura robusta capaz de adaptarse a cambios reales.

Aprendiste cómo introducir contratos compartidos para evitar la divergencia entre frontend y backend, cómo proteger las fronteras de red mediante compuertas en tiempo de ejecución con guardas de tipo, cómo desacoplar proveedores externos mediante interfaces estables, y cómo garantizar la exhaustividad de los estados del sistema mediante uniones discriminadas y clientes API tipados.

En el próximo capítulo, revisaremos **Desbloquea tus Beneficios Exclusivos** con recursos adicionales para continuar tu aprendizaje en TypeScript.
