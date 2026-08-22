# Parte 2: Pruebas y Calidad de Código

## Capítulo 5: Pruebas y Desarrollo Guiado por Pruebas (TDD)

### Sección: Introducción

En este capítulo, nos centraremos en construir una estrategia de pruebas integral para proyectos de TypeScript. Las pruebas juegan un papel vital en el desarrollo de software, asegurando que tu código permanezca fiable, mantenible y con un buen rendimiento. Al finalizar este capítulo, obtendrás una comprensión sólida de varios tipos de pruebas y cómo implementarlas de manera efectiva en TypeScript. Además, exploraremos el **Desarrollo Guiado por Pruebas (TDD)**, una metodología que enfatiza la escritura de pruebas antes de codificar, ayudando a mejorar tanto el diseño como la calidad de tus aplicaciones.

En este capítulo, cubriremos los siguientes temas principales:

- Comprensión de los fundamentos del testing
- Comprensión de los niveles de pruebas en el software
- Pruebas unitarias en TypeScript
- Pruebas de integración en TypeScript
- Introducción a TDD y sus beneficios

---

### Sección: Requisitos técnicos

Para seguir los ejemplos de este capítulo sobre pruebas y TDD, necesitarás tener instalados en tu máquina **Node.js versión 20.0.0 o posterior** y **TypeScript versión 5.0 o posterior**. Las versiones recientes de herramientas de prueba modernas, como Vitest, requieren Node.js 20 o superior; el uso de una versión anterior puede generar advertencias de instalación o errores en tiempo de ejecución.

Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones de la sección *Download the example code files* en el Prefacio de este libro.

Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

---

### Sección 1: Comprensión de los fundamentos del testing

En esta sección, exploraremos los fundamentos de las pruebas en el desarrollo de software. Aprenderás sobre la importancia del testing, los diferentes niveles de pruebas y sus respectivos objetivos. Este conocimiento proporcionará una base sólida para el resto del capítulo, donde profundizaremos en estrategias de prueba más específicas en TypeScript.

El desarrollo de software a menudo implica la creación de sistemas complejos con funcionalidades intrincadas. Sin pruebas adecuadas, es fácil que problemas imprevistos pasen desapercibidos. Las pruebas te ayudan a detectar estos problemas antes de que lleguen a producción, ahorrando tiempo y recursos a largo plazo. Además, las pruebas bien escritas contribuyen a la mantenibilidad del código, ya que sirven como documentación del comportamiento previsto de tu aplicación.

Veamos un ejemplo simple de una prueba de función en TypeScript:

```typescript
function add(a, b) {
  return a + b;
}
 
console.log('test should run here');
// Test
console.assert(add(2, 3) === 5, 'add(2, 3) should return 5');
```

En este ejemplo, tenemos una función básica `add` que toma dos argumentos, los suma y devuelve el resultado. La prueba utiliza `console.assert` para verificar si la función `add` se comporta como se espera. En este caso, esperamos que `add(2, 3)` devuelva `5`. Si la condición es verdadera, la prueba pasa silenciosamente. Sin embargo, si la condición es falsa, la aserción falla y se lanzará un mensaje de error, alertándonos sobre un problema potencial en el comportamiento de la función.

Por ejemplo, supongamos que modificamos la prueba de la siguiente manera:

```typescript
console.assert(add(2, 3) === 11, 'add(2, 3) should return 11');
```

La sentencia `console.assert` ahora comprueba si `add(2, 3)` es igual a `11`, lo cual es intencionalmente incorrecto. Cuando ejecutamos este código, arroja un error porque el resultado real de `add(2, 3)` es `5`, no `11`. La consola mostrará el siguiente mensaje:

```text
Assertion failed: add(2, 3) should return 11
```

En este contexto, el error es significativo porque resalta cómo las pruebas pueden detectar problemas inesperados o suposiciones incorrectas en una etapa temprana del proceso de desarrollo. Al crear intencionalmente una prueba fallida, podemos ver cómo funciona el mecanismo `console.assert` para señalar errores. En escenarios del mundo real, pruebas similares ayudan a garantizar que los cambios en el código no introduzcan errores ni regresiones. Este sencillo ejemplo demuestra cómo las pruebas pueden actuar como redes de seguridad, alertando a los desarrolladores sobre posibles problemas antes de que afecten a los usuarios finales.

---

### Sección 2: Comprensión de los niveles de pruebas en el software

En esta sección, profundizaremos en los diversos niveles de pruebas en el desarrollo de software. Al comprender estos niveles, podrás implementar una estrategia de prueba integral que garantice la confiabilidad y el rendimiento de tus aplicaciones.

Las pruebas de software son un proceso de múltiples capas diseñado para detectar defectos en diferentes etapas del desarrollo. Los niveles principales de pruebas son los siguientes:

1. **Pruebas unitarias (*Unit testing*)**
2. **Pruebas de integración (*Integration testing*)**
3. **Pruebas de sistema (*System testing*)**
4. **Pruebas de aceptación (*Acceptance testing*)**

Cada nivel aborda necesidades de prueba específicas y juega un papel crucial para garantizar la calidad del software. Si bien los cuatro niveles son importantes, en este capítulo nos centraremos en las **pruebas unitarias** y las **pruebas de integración**. Estas dos son fundamentales para detectar problemas tempranamente en el proceso de desarrollo y garantizar que los componentes individuales, así como sus interacciones, funcionen correctamente.

---

### Sección 3: Pruebas unitarias en TypeScript

Las pruebas unitarias son el primer nivel de pruebas en el desarrollo de software, donde se prueban componentes individuales del software para validar que cada unidad funcione como se espera. Una unidad es la pieza más pequeña de código que se puede probar de forma aislada. Dependiendo del software y la arquitectura, una unidad puede ser una función, un método dentro de una clase, una clase misma o incluso un pequeño módulo. La idea clave es que una unidad debe poder probarse de forma independiente sin depender de sistemas o componentes externos.

Este nivel de pruebas garantiza que los componentes individuales del software sean confiables y funcionen correctamente, sentando las bases para niveles más altos de pruebas, como las pruebas de integración y de sistema.

Algunos beneficios de las pruebas unitarias incluyen los siguientes:
- **Detección temprana de errores**
- **Depuración y mantenimiento simplificados**
- **Calidad y fiabilidad del código mejoradas**
- **Refactorización y actualizaciones facilitadas**
- **Mayor confianza del desarrollador**

#### Terminología común utilizada en pruebas unitarias

A medida que profundizamos en las pruebas unitarias, es esencial comprender los términos clave con los que te encontrarás con frecuencia:

- **Caso de prueba (*Test case*)**
- **Aserción (*Assertion*)**
- **Suite de pruebas (*Test suite*)**
- **Fixture de prueba (*Test fixture*)**
- **Mock**
- **Stub**
- **Spy**

Exploremos estos conceptos:

##### Caso de prueba (*Test case*)

Un caso de prueba es un escenario en el que compruebas (pruebas) una unidad específica de tu código (como una función) con un conjunto particular de entradas (variables) para verificar si el código se comporta como se espera en diversas condiciones.

Por ejemplo, supongamos que tenemos una función que suma dos números:

```typescript
function add (a: number, b: number): number { 
  return a + b; 
}
```

Ahora, creamos dos casos de prueba para verificar el comportamiento de la función `add`:

```typescript
test('adds 2 + 2 to equal 4', () => { 
// ....
}); 
test('adds -1 + 1 to equal 0', () => { 
  // ....
});
```

Aquí hemos escrito dos casos de prueba. Básicamente, declaramos lo que intentamos verificar. En el primer caso de prueba, decimos que queremos verificar que cuando se suman dos números, la función debe devolver 4. En el segundo, intentamos verificar que también funciona bien incluso con números negativos: -1 + 1 debería ser 0.

##### Aserción (*Assertion*)

Una aserción es una declaración utilizada en las pruebas para comprobar si una condición particular es verdadera. Es una sentencia que verifica si el código produce el resultado deseado.

Continuando con el ejemplo anterior, agreguemos algunas aserciones a nuestros casos de prueba:

```typescript
test('adds 2 + 2 to equal 4', () => {
  expect(add(2, 2)).toBe(4); // Assertion
});
test('adds -1 + 1 to equal 0', () => {
  expect(add(-1, 1)).toBe(0); // Assertion
});
```

La función `expect` se utiliza para afirmar si el resultado coincide con la salida esperada.

##### Mock

Imagina que tu código interactúa con una base de datos para recuperar información del usuario o realiza una llamada a la API. En una prueba unitaria, deseas evitar interacciones reales con dichos sistemas externos para garantizar que tus pruebas sean confiables y repetibles. Aquí es donde entra en juego el *mocking*.

Un objeto mock actúa como un sustituto de un objeto real con el que interactúa tu código. Replica el comportamiento del objeto real de manera controlada, lo que te permite aislar la unidad bajo prueba y evitar dependencias externas que puedan influir en los resultados.

Examinemos el siguiente ejemplo:

```typescript
async function getUserInfo(userId: string, databaseService: any): Promise<User> {
  const userInfo = await databaseService.getUserById(userId);
  return userInfo;
}
```

En esta función, `databaseService` es una dependencia externa. Durante las pruebas unitarias, existen dos enfoques principales para simular su comportamiento:

###### Enfoque 1: Mocks manuales (Inyección de dependencias)

En este patrón, defines un objeto mock localmente y lo pasas directamente a tu función como argumento:

1. **Importar la función que deseas probar**:
```typescript
import { getUserInfo } from '../src/getUserInfo';
```

2. **Crear un objeto mock**:
```typescript
const mockDatabaseService = { 
  getUserById: jest.fn().mockResolvedValue({ 
    id: "123", 
    name: "John Doe", 
    email: "john@example.com", 
  }), 
};
```

3. **Pasar el mock a la función**:
```typescript
test("fetches user information from the database", async () => { 
  const userId = "123"; 
 
  // We pass our fake mockDatabaseService directly into the function here
  const userInfo = await getUserInfo(userId, mockDatabaseService); 
  
  // Assertions
  expect(userInfo.name).toBe("John Doe");
  expect(mockDatabaseService.getUserById).toHaveBeenCalledWith(userId); 
});
```

###### Enfoque 2: Mocking de módulos importados

En muchos escenarios del mundo real, tu función no recibirá un servicio como argumento; en su lugar, importará ese servicio directamente en el archivo. Para probar esto, debes indicarle al ejecutor de pruebas que intercepte esa importación y reemplace el archivo real con una versión simulada:

1. **Importar los módulos e inicializar el mock**:
```typescript
// Import the actual module being mocked AND the function being tested
// Import the actual module AND the function being tested
import databaseService from '../src/databaseService';
import { getUserInfo } from '../src/getUserInfo';
 
// Tell Jest to replace the real file with this mock implementation
jest.mock('../src/databaseService', () => ({
  getUserById: jest.fn()
}));
```

2. **Definir el comportamiento del mock y ejecutar la prueba**:
```typescript
test("uses a module mock to fetch data", async () => {
  // Define the return value specifically for this test
  (databaseService.getUserById as jest.Mock).mockResolvedValue({ 
    name: "Jane Doe" 
  });
  
  // Call the function (it imports databaseService internally)
  const userInfo = await getUserInfo("123");
  
  expect(userInfo.name).toBe("Jane Doe");
});
```

3. **Verificar la interacción**:
```typescript
// Verify that the internal service was called with the right ID
  expect(databaseService.getUserById).toHaveBeenCalledWith("123");
```

#### Ejecutores de pruebas (*Test runners*)

Piensa en un ejecutor de pruebas como el director de tu orquesta de pruebas. Automatiza la ejecución de toda tu suite de pruebas de manera eficiente.

Opciones populares en el ecosistema de TypeScript:
- **Jest**: Conocido por su facilidad de uso, completas funciones como pruebas de instantáneas (*snapshot testing*) y mocking, y un amplio soporte integrado para TypeScript.
- **Mocha**: Una opción flexible y ligera que proporciona una base sólida para construir marcos de prueba personalizados.
- **Cypress**: Principalmente conocido por pruebas de extremo a extremo (E2E), también puede manejar escenarios de pruebas unitarias en aplicaciones TypeScript con componentes de interfaz de usuario.
- **Vitest**: Muy popular por su integración con la herramienta de construcción Vite. Ofrece una experiencia de prueba rápida y eficiente, aprovechando las características modernas de JavaScript y proporcionando compatibilidad perfecta con TypeScript.

##### Elección del ejecutor de pruebas adecuado

Para elegir el ejecutor de pruebas ideal, considera factores como:
- **Facilidad de uso**: ¿Con qué rapidez puedes comenzar y escribir pruebas?
- **Funcionalidades ofrecidas**: ¿Cuenta con características como pruebas de instantáneas, mocking o cobertura de código?
- **Soporte de la comunidad**: ¿Existe una comunidad amplia y activa para obtener ayuda y recursos?
- **Complejidad de tus necesidades de prueba**: ¿Necesitas un ejecutor básico o una opción más personalizable?

Vite y Vitest ofrecen una ventaja clave para proyectos que ya utilizan Vite para la compilación, garantizando un flujo de trabajo optimizado con ejecución rápida de pruebas y soporte nativo para TypeScript.

#### Configuración de Vitest en un proyecto TypeScript con Node.js

A continuación, veremos el proceso paso a paso para configurar Vitest en un proyecto TypeScript con Node.js:

1. **Inicializar tu proyecto**:
```bash
mkdir vitest-demo
cd vitest-demo
npm init -y
```

2. **Instalar TypeScript y Vitest**:
```bash
npm install typescript vitest ts-node @types/node --save-dev
```

3. **Configurar TypeScript**:
Inicializa una configuración básica de TypeScript ejecutando:
```bash
npx tsc -init
```

Asegúrate de que `tsconfig.json` incluya al menos las siguientes opciones:
```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "CommonJS",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "types": ["node", "vitest/globals"]
  }
}
```

4. **Crear la configuración de Vitest**:
Crea un archivo `vitest.config.ts` en la raíz de tu proyecto:
```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config';
 
export default defineConfig({
  test: {
    globals: true,
    environment: 'node', // Running tests in a Node.js environment
  },
});
```

Asegúrate también de agregar lo siguiente a tu `package.json`:
```json
"type": "module"
```

5. **Escribir una prueba unitaria**:
Supongamos que tienes un módulo llamado `square.ts`:
```typescript
// square.ts
export function square(n: number): number {
  return n * n;
}
```

Crea un archivo de prueba llamado `square.test.ts`:
```typescript
// square.test.ts
import { square } from './square.js';
 
test('calculates the square of 3 correctly', () => {
  expect(square(3)).toBe(9);
});
 
test('calculates the square of -4 correctly', () => {
  expect(square(-4)).toBe(16);});
```

6. **Agregar un script de prueba a `package.json`**:
```json
{
  "scripts": {
  }
}
```

7. **Ejecutar las pruebas**:
```bash
npm run test
```

*Figura 5.1 — Ejecución exitosa de pruebas unitarias usando el ejecutor Vitest*

Desglose de la función y pruebas:

```typescript
export function square(n: number): number {
  return n * n;
}
```

```typescript
// square.test.ts
import { square } from './square.js';
 
test('calculates the square of 3 correctly', () => {
  expect(square(3)).toBe(9);
});
 
test('calculates the square of -4 correctly', () => {
  expect(square(-4)).toBe(16);
});

import { square } from './square';
```

Analicemos lo que hicimos en cada prueba:

```typescript
test('calculates the square of 3 correctly', () => {
  expect(square(3)).toBe(9);
});
```

- `test`: Es la función que ejecuta un caso de prueba. Recibe una descripción textual y una función con la lógica de la prueba.
- `'calculates the square of 3 correctly'`: Es la descripción de lo que se verifica.
- `() => { ... }`: Función de flecha que contiene la lógica.
- `expect(square(3))`: Pasa el resultado de evaluar `square(3)` a `expect`.
- `toBe(9)`: Comparador (*matcher*) que valida si el valor resultante es igual a 9.

Segunda prueba para números negativos:

```typescript
test('calculates the square of -4 correctly', () => {
  expect(square(-4)).toBe(16);
});
```

Estas pruebas unitarias garantizan que la función `square` funcione correctamente tanto para números positivos como negativos.

---

### Sección 4: Pruebas de integración en TypeScript

Las pruebas de integración son un aspecto crucial del ciclo de vida del desarrollo de software. En aplicaciones basadas en TypeScript, desempeñan un papel complementario junto con la verificación de tipos estática. Mientras que TypeScript ayuda a detectar errores de tipos en tiempo de compilación, las pruebas de integración garantizan que los componentes funcionen e interactúen correctamente en tiempo de ejecución.

#### ¿Qué son las pruebas de integración?

Las pruebas de integración son un tipo de pruebas en las que las unidades individuales de una aplicación se combinan y se prueban como un grupo. El propósito principal es exponer fallos en la interacción entre las unidades integradas.

#### ¿Por qué pruebas de integración?

Las pruebas de integración son esenciales por varias razones:
- **Identifican problemas de interfaz entre módulos**: Aseguran que los componentes funcionen juntos según lo esperado.
- **Verifican el flujo de datos**: Garantizan que la salida de un módulo se pase correctamente como entrada a otro.
- **Detectan problemas con sistemas externos**: Validan la interacción con bases de datos, APIs y servicios de terceros.
- **Descubren comportamientos inesperados**: Ofrecen una evaluación más realista de la funcionalidad de la aplicación en su entorno previsto.

#### Tipos de pruebas de integración

Las pruebas de integración se pueden realizar utilizando diferentes enfoques:

##### Enfoque Big Bang

El enfoque *Big Bang* implica integrar todos los módulos de una aplicación a la vez y luego realizar las pruebas.

- **Ventajas**: Simple de implementar, sin necesidad de stubs o drivers; adecuado para sistemas pequeños con módulos fuertemente interdependientes.
- **Desventajas**: Difícil aislar el módulo que causa un fallo; requiere que todos los módulos estén terminados antes de comenzar las pruebas.

##### Enfoque incremental

El enfoque incremental implica integrar y probar dos módulos a la vez. Se divide en tres tipos:

###### Enfoque descendente (*Top-down*)

Las pruebas comienzan desde la parte superior de la jerarquía de módulos (los "padres") y avanzan hacia la parte inferior (los "hijos"). Se utilizan **stubs** para simular módulos de nivel inferior aún no desarrollados. Un *stub* es una implementación simplificada y temporal que devuelve respuestas fijas.

Ejemplo de un stub en código:

```typescript
// The 'TaxCalculator' isn't built yet, so we create this Stub
const taxServiceStub = {
  calculateTax: (amount: number) => {
    return 10.00; // A "canned" response regardless of the input
  }
};
 
// We can now test the Checkout logic even without the real Tax engine
const total = checkout(100.00, taxServiceStub);
console.log(total); // Should be 110.00
```

- **Ventajas**: Detección temprana de problemas de diseño de alto nivel; permite retroalimentación temprana del usuario.
- **Desventajas**: Requiere muchos stubs complejos.

###### Enfoque ascendente (*Bottom-up*)

Las pruebas comienzan desde la parte inferior de la jerarquía y avanzan hacia arriba. Se utilizan **drivers** para simular el comportamiento de módulos superiores no implementados. Un *driver* es un script o módulo temporal que invoca componentes de nivel inferior y les suministra datos de prueba.

Ejemplo de un driver en código:

```typescript
// The real Database module we want to test
import { saveToDatabase } from './dbModule.js'; 
 
// The Driver: A temporary script to "drive" data into the module
const testDriver = async () => {
  console.log("Driver starting: Testing Database Save...");
  const result = await saveToDatabase({ id: 1, title: 'Test' });
  
  if (result.success) {
    console.log("Success: The bottom-level module works!");
  }
};
 
testDriver();
```

- **Ventajas**: Fácil aislamiento de fallos; no requiere stubs.
- **Desventajas**: La creación de drivers puede complicar las pruebas; los problemas de lógica de alto nivel se detectan tarde.

###### Enfoque sándwich/híbrido

Combina los enfoques descendente y ascendente para aprovechar las ventajas de ambos métodos.

- **Ventajas**: Cobertura integral desde ambos extremos; reduce la cantidad total de stubs y drivers.
- **Desventajas**: Complejo de gestionar y coordinar.

#### Pruebas de integración manuales frente a automatizadas

##### Pruebas manuales
- **Ventajas**: Flexibilidad, capacidad para pruebas exploratorias y evaluación de usabilidad y experiencia de usuario.
- **Desventajas**: Consumen mucho tiempo, son propensas a errores humanos e inconsistencias, y son difíciles de escalar.
- **Cuándo usarlas**: En etapas iniciales de desarrollo rápido, pruebas exploratorias y evaluación de diseño de UI/UX.

##### Pruebas automatizadas
- **Ventajas**: Alta velocidad y eficiencia en pipelines de CI/CD, consistencia y precisión, reutilización de scripts y alta escalabilidad.
- **Desventajas**: Costo inicial de configuración y desarrollo de scripts, sobrecarga de mantenimiento y cobertura limitada a escenarios predefinidos.
- **Cuándo usarlas**: Pruebas de regresión, entornos de Integración y Despliegue Continuo (CI/CD) y aplicaciones complejas a gran escala.

#### Ejemplo práctico de pruebas de integración

Construiremos una aplicación de blog sencilla con operaciones CRUD y pruebas de integración utilizando **Vitest** y **Supertest**:

1. **Inicializar el proyecto**:
```bash
mkdir blog-app
cd blog-app
npm init –y
```

Agrega `"type": "module"` en `package.json` para habilitar módulos ESM.

2. **Instalar dependencias**:
```bash
npm install express 
npm install --save-dev typescript @types/node @types/express vitest supertest @types/supertest
```

3. **Configurar TypeScript**:
Crea un archivo `tsconfig.json`:
```json
{
"compilerOptions": {
"target": "ESNext",
"module": "NodeNext",
"moduleResolution": "NodeNext",
"strict": true,
"esModuleInterop": true,
"skipLibCheck": true,
"outDir": "./dist",
"verbatimModuleSyntax": true
},
"include": ["src/**/*", "tests/**/*"]
}
```

4. **Crear la estructura del proyecto**:
```bash
mkdir src src/models src/services src/controllers src/routes tests
```

5. **Crear el modelo (`src/models/post.ts`)**:
```typescript
export interface Post { 
  id: number; 
  title: string; 
  content: string; 
} 
 
export let posts: Post[] = []; 
let currentId = 1; 
 
// A helper to handle ID generation internally
export const getNextId = () => currentId++;
```

6. **Crear la capa de servicio (`src/services/postService.ts`)**:
```typescript
// Important: We use the .js extension for the relative import
import { type Post, posts, getNextId } from '../models/post.js'; 
 
export const getPosts = (): Post[] => posts; 
 
export const getPostById = (id: number) => posts.find(p => p.id === id); 
 
export const createPost = (title: string, content: string): Post => { 
  const newPost = { id: getNextId(), title, content }; 
  posts.push(newPost); 
  return newPost; 
}; 
 
export const updatePost = (id: number, title: string, content: string) => { 
  const post = getPostById(id); 
  if (post) { 
    post.title = title; 
    post.content = content; 
    return post; 
  } 
  return null; 
}; 
 
export const deletePost = (id: number) => { 
  const index = posts.findIndex(p => p.id === id); 
  if (index !== -1) posts.splice(index, 1); 
};
```

7. **Implementar el controlador (`src/controllers/postController.ts`)**:
```typescript
import type { Request, Response } from 'express'; 
import { getPosts, getPostById, createPost, updatePost, deletePost } from '../services/postService.js'; 
 
export const getAllPosts = (req: Request, res: Response) => {
  res.json(getPosts());
};
 
export const getPost = (req: Request, res: Response) => { 
  const post = getPostById(Number(req.params.id)); 
  post ? res.json(post) : res.status(404).send('Post not found'); 
}; 
 
export const createNewPost = (req: Request, res: Response) => { 
  const { title, content } = req.body; 
  res.status(201).json(createPost(title, content)); 
}; 
 
export const updateExistingPost = (req: Request, res: Response) => { 
  const post = updatePost(Number(req.params.id), req.body.title, req.body.content); 
  post ? res.json(post) : res.status(404).send('Post not found'); 
}; 
 
export const deleteExistingPost = (req: Request, res: Response) => { 
  deletePost(Number(req.params.id)); 
  res.status(204).send(); 
};
```

8. **Configurar las rutas (`src/routes/postRoutes.ts`)**:
```typescript
import { Router } from 'express'; 
import { getAllPosts, getPost, createNewPost, updateExistingPost, deleteExistingPost } from '../controllers/postController.js'; 
 
const router = Router(); 
router.get('/posts', getAllPosts); 
router.get('/posts/:id', getPost); 
router.post('/posts', createNewPost); 
router.put('/posts/:id', updateExistingPost); 
router.delete('/posts/:id', deleteExistingPost); 
 
export default router;
```

9. **Inicializar la aplicación Express (`src/index.ts`)**:
```typescript
import postRoutes from './routes/postRoutes.js'; 
 
const app = express(); 
 
// Middleware to parse JSON request bodies
app.use(express.json()); 
 
// Connect our blog routes to the application
app.use(postRoutes); 
 
export default app;
```

10. **Añadir las pruebas de integración (`tests/app.test.ts`)**:

```typescript
import { describe, it, expect } from 'vitest';
import request from 'supertest';
import app from '../src/index.js';
 
describe('Blog Post API Integration', () => {
  let postId: number;
 
  it('should create and then retrieve a blog post', async () => {
    const createRes = await request(app)
      .post('/posts')
      .send({ title: 'My Post', content: 'Integration Testing' });
 
    expect(createRes.statusCode).toBe(201);
    postId = createRes.body.id;
// ... The GET request verification will be added here

  })

// ... Additional test cases for Update and Delete will follow
})
```

Verificación de la obtención del post:
```typescript
const getRes = await request(app).get(`/posts/${postId}`);  
expect(getRes.statusCode).toBe(200);  
expect(getRes.body.title).toBe('My Post');
```

Actualización de un post existente:
```typescript
it('should update and delete the post', async () => {
    const updateRes = await request(app)
      .put(`/posts/${postId}`)
      .send({ title: 'Updated', content: 'Updated content' });
 
    expect(updateRes.statusCode).toBe(200);
    expect(updateRes.body.title).toBe('Updated');
expect(res.body.content).toBe('This content has been modified.');
  });
```

Eliminación del post y verificación:
```typescript
it('should delete a blog post and verify it is gone', async () => {
    // 1. Send the DELETE request
    const deleteRes = await request(app).delete(`/posts/${postId}`);
    expect(deleteRes.statusCode).toBe(204); // 204 signifies "No Content" (Success)
 
    // 2. Attempt to GET the deleted post to confirm it no longer exists
    const getRes = await request(app).get(`/posts/${postId}`);
 
    // The API should now return a 404 Not Found
    expect(getRes.statusCode).toBe(404);
  });
```

---

### Sección 5: Introducción a TDD y sus beneficios

El **Desarrollo Guiado por Pruebas (TDD)** invierte el proceso de codificación habitual. En lugar de escribir el código primero y las pruebas después, TDD se centra en escribir las pruebas antes del código real.

TDD sigue un ciclo de tres pasos: **Rojo, Verde y Refactorizar (*Red, Green, Refactor*)**:
1. **Red (Rojo)**: Escribir una prueba que defina el comportamiento deseado. La prueba fallará inicialmente porque la funcionalidad aún no existe.
2. **Green (Verde)**: Escribir la cantidad mínima de código necesaria para que la prueba pase.
3. **Refactor (Refactorizar)**: Mejorar y limpiar el código sin cambiar su comportamiento externo, asegurando que las pruebas sigan pasando.

#### Mejores prácticas para TDD en TypeScript

1. **Escribir nombres y descripciones de pruebas efectivos**:
```typescript
describe('Calculator', () => {
    it('correctly adds two numbers', () => {
        const result = Calculator.add(2, 3);
        expect(result).toBe(5);
    });
});
```

2. **Mantener las pruebas simples y enfocadas**:
```typescript
describe('Calculator', () => {
    it('correctly adds two numbers', () => {
        const result = Calculator.add(2, 3);
        expect(result).toBe(5);
    });
 
    it('correctly subtracts two numbers', () => {
        const result = Calculator.subtract(5, 2);
        expect(result).toBe(3);
    });
});
```

3. **Usar técnicas de mocking y stubbing**:
```typescript
import axios from 'axios';
jest.mock('axios');
 
describe('ApiService', () => {
    it('fetches data from the API', async () => {
        const data = { data: { results: [1, 2, 3] } };
        axios.get.mockResolvedValue(data);
 
        const result = await ApiService.fetchData();
        expect(result).toEqual(data.data.results);
    });
});
```

4. **Usar el patrón AAA (*Arrange-Act-Assert*)**:
```typescript
describe('Calculator', () => {
    it('correctly adds two numbers', () => {
        // Arrange
        const num1 = 2;
        const num2 = 3;
 
        // Act
        const result = Calculator.add(num1, num2);
 
        // Assert
        expect(result).toBe(5);
    });
});
```

5. **Evitar la interdependencia entre pruebas**: Cada prueba debe ejecutarse de forma independiente y no depender del estado de otras.
6. **Probar casos extremos (*Edge Cases*)**: Incluir pruebas para entradas inválidas, estados vacíos y valores límite.
7. **Mantener las pruebas DRY (*Don't Repeat Yourself*)**: Utilizar funciones auxiliares y hooks (`beforeEach`/`afterEach`) para evitar duplicaciones innecesarias en los tests.

---

### Sección: Resumen

En este capítulo, cubrimos aspectos esenciales de la construcción y prueba de una aplicación TypeScript. Comenzamos configurando un proyecto TypeScript con herramientas de prueba modernas como Vitest, Jest y Supertest.

Luego nos enfocamos en escribir pruebas unitarias y de integración utilizando una aplicación Express como ejemplo práctico de operaciones CRUD, demostrando las mejores prácticas para estructurar capas de modelos, servicios, controladores y rutas.

Finalmente, exploramos el Desarrollo Guiado por Pruebas (TDD), comprendiendo el ciclo Red-Green-Refactor y los patrones de diseño de pruebas que garantizan que el código sea robusto, confiable y fácil de mantener.

En el próximo capítulo, exploraremos el **Manejo de Errores, Depuración y Mejores Prácticas de Seguridad** en TypeScript, aprendiendo técnicas para detectar y controlar fallos tempranamente y proteger nuestras aplicaciones en producción.
