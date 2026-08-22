# Parte 2: Pruebas y Calidad de Código

## Capítulo 6: Manejo de Errores, Depuración y Mejores Prácticas de Seguridad

### Sección: Introducción

Desarrollar software robusto y confiable requiere dominar el arte del manejo de errores y la depuración. Estos son componentes críticos para construir aplicaciones TypeScript sólidas. Comprender cómo gestionar eficazmente los errores, aprovechar las herramientas de depuración e implementar las mejores prácticas de seguridad garantiza que las aplicaciones sean fiables, mantenibles y seguras.

En este capítulo, aprenderás técnicas y estrategias esenciales para manejar errores tanto síncronos como asíncronos en TypeScript. Los errores síncronos ocurren durante la ejecución de un bloque de código específico y se lanzan de inmediato, como los errores de sintaxis o los errores de referencia. Los errores asíncronos, por otro lado, ocurren fuera del flujo de ejecución principal —generalmente en callbacks, promesas o funciones async—, lo que los hace más difíciles de manejar.

Exploraremos el uso de diferentes tipos de errores y aprenderemos a utilizar herramientas de depuración para identificar y resolver problemas de manera eficiente. Además, cubriremos patrones comunes de error con sus soluciones, y prácticas esenciales de seguridad para proteger tus aplicaciones TypeScript.

En este capítulo, cubriremos lo siguiente:

- Aprendizaje de estrategias para el manejo de errores
- Manejo de errores síncronos en TypeScript
- Estrategias para lidiar con errores asíncronos (por ejemplo, promesas, async/await y try/catch)
- Uso efectivo de tipos de error
- Herramientas y técnicas de depuración
- Patrones comunes de error y soluciones
- Mejores prácticas de seguridad en TypeScript

Al final de este capítulo, estarás equipado con el conocimiento necesario para manejar errores con elegancia, depurar eficazmente e implementar medidas de seguridad para proteger tus aplicaciones.

---

### Sección: Requisitos técnicos

Para seguir este capítulo, necesitarás tener instaladas las siguientes herramientas en tu sistema:

- **Node.js (v16 o posterior)**: Requerido para ejecutar y compilar aplicaciones TypeScript.
- **TypeScript (v5 o posterior)**
- **Visual Studio Code (VS Code)**: Editor recomendado con soporte de depuración integrado.

Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones en la sección *Download the example code files* en el Prefacio de este libro. Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

El repositorio de GitHub contiene todos los proyectos de muestra, incluidos ejemplos para el manejo de errores, depuración con VS Code, mapas de origen (*source maps*), puntos de interrupción (*breakpoints*) e implementación de mejores prácticas de seguridad.

---

### Sección 1: Comprensión de los tipos y patrones de error

El manejo de errores es una parte crítica del desarrollo de aplicaciones TypeScript confiables. Antes de sumergirse en cómo manejar los errores, es esencial comprender primero los diferentes tipos de errores que puedes encontrar y reconocer los patrones comunes que pueden provocarlos. De esta manera, estarás mejor equipado para identificar, depurar y resolver problemas de manera eficiente.

#### Introducción a los tipos de error

Los errores en programación se clasifican generalmente en tres categorías principales: **errores de sintaxis**, **errores en tiempo de ejecución (*runtime errors*)** y **errores lógicos**. Estos tipos son comunes en la mayoría de los lenguajes de programación.

En TypeScript, surgen tipos de error adicionales debido a sus características únicas. Estos incluyen errores de tipo, errores de compilación y errores específicos de operaciones asíncronas o síncronas.

##### Errores de sintaxis

Los errores de sintaxis son equivocaciones en el código que violan las reglas del lenguaje TypeScript, como paréntesis no coincidentes o nombres de función no válidos. Estos errores impiden que tu código se compile, y el compilador de TypeScript los detecta durante el proceso de compilación.

Considera la siguiente línea de código:

```typescript
const x = 2; if (x > 5 { console.log("x is big");}
```

*Figura 6.1 — Editor VS Code resaltando un error de sintaxis con una línea ondulada roja bajo el código problemático, mostrando el tooltip: ')' expected ts(1005)*

En el código anterior, tenemos una condición que verifica si `x` es mayor que 5. Sin embargo, falta el paréntesis de cierre. El editor resalta el problema con una línea ondulada roja y muestra el mensaje de error:

```text
')' expected ts(1005)
```

Además, si revisas la terminal mientras ejecutas `npx tsc --w`, verás más detalles sobre el error:

*Figura 6.2 — Salida de terminal del compilador de TypeScript mostrando el error TS1005 por un paréntesis de cierre faltante en index.ts, incluyendo referencias de línea y detalles del analizador*

Para resolver los errores de sintaxis, revisa cuidadosamente los mensajes de error proporcionados por el compilador de TypeScript o tu editor.

Aquí está el código corregido:

```typescript
const x = 2; if (x > 5) { console.log("x is big"); }
```

*Figura 6.3 — Editor VS Code mostrando el código corregido con el paréntesis faltante añadido, sin líneas onduladas rojas*

*Figura 6.4 — Salida de la terminal después de corregir el error de sintaxis, mostrando una compilación exitosa con cero errores*

##### Errores de tipo

Los errores de tipo ocurren cuando se utiliza un valor de una manera que es incompatible con su tipo declarado o inferido. A diferencia de los errores de sintaxis, los errores de tipo no rompen la estructura de tu código, pero violan las reglas sobre cómo deben comportarse los valores. Uno de los mayores puntos fuertes de TypeScript es que detecta estos problemas durante la compilación.

Ejemplos comunes de errores de tipo y cómo resolverlos:

1. **Asignación de tipos incompatibles**:
```typescript
let num: number = 42; num = "Hello"; // Error: Type 'string' is not assignable to type 'number'.
```

Para solucionarlo, asigna un valor que coincida con el tipo declarado:
```typescript
num = 24; // Correctly assigning a number to a variable of type 'number'
```

2. **Llamar a un método que no existe en un tipo**:
```typescript
let user: string = "John"; user.push("Doe"); 
```

Dará como resultado:
```text
// Error: Property 'push' does not exist on type 'string'
```

Para solucionarlo, asegúrate de que la variable esté declarada con el tipo apropiado:
```typescript
let user: string[] = ["John"]; user.push("Doe"); // Correct usage
```

3. **Acceder a una propiedad en un valor que potencialmente es null o undefined**:
```typescript
interface User { firstName: string; lastName?: string; } // Simulated API call that may return a user or null function fetchUserFromAPI(): User | null { return Math.random() > 0.5 ? { name: "Rukee", lastName: "Doe" } : null; } const user = fetchUserFromAPI(); console.log(user.firstName); // Error: Object is possibly 'null'
```

Debes comprobar primero que el valor existe:
```typescript
if(user) { console.log(user.firstName); }
```

O usar encadenamiento opcional (*optional chaining*):
```typescript
console.log(user?.firstName);
```

Para resolver los errores de tipo eficazmente:
- Revisa cuidadosamente los mensajes del compilador o del editor.
- Asegúrate de que los tipos de variables, parámetros y retornos coincidan con los valores previstos.
- Utiliza protectores de tipo (*type guards* como `typeof`, `instanceof` o comprobaciones personalizadas) cuando trabajes con tipos unión o `unknown`.
- Habilita el modo estricto (`strict`) en tu configuración de TypeScript para obtener mayores garantías.

##### Errores en tiempo de ejecución (*Runtime errors*)

Los errores en tiempo de ejecución ocurren cuando tu código es sintácticamente correcto y pasa la verificación de tipos estática de TypeScript, pero surge un problema durante la ejecución del programa. Estos errores suelen estar relacionados con condiciones inesperadas en tiempo de ejecución más que con simples discrepancias de tipos.

Causas comunes de errores en tiempo de ejecución:
- Acceder a una propiedad en `undefined` o `null`.
- Asumir que las respuestas de API externas se ajustan a una estructura específica.
- Realizar operaciones sobre valores inesperados.
- Eludir la seguridad de tipos utilizando `any` o aserciones de tipo forzadas.

Veamos un ejemplo:

```typescript
type User = { name: string; }; function greet(user: User) { console.log(user.name.toUpperCase()); } // Simulating external or unsafe input greet(undefined as any); // Compiles, but throws at runtime
```

Aunque este código se compila, falla en tiempo de ejecución porque `undefined` no tiene una propiedad `name`.

###### Cómo identificar y resolver errores en tiempo de ejecución

A diferencia de los errores de sintaxis, los errores en tiempo de ejecución solo aparecen cuando ejecutas tu aplicación. Estos errores normalmente se registran en la consola y pueden hacer que el programa falle o se comporte de forma impredecible. Para resolverlos, deberás depurar tu código utilizando herramientas como puntos de interrupción (*breakpoints*), registros de consola (*console logs*) o depuradores.

##### Errores lógicos

Los errores lógicos ocurren cuando el código se ejecuta sin fallar pero produce resultados incorrectos debido a fallos en la lógica. Son los más difíciles de identificar porque ocurren cuando tu código no tiene errores de sintaxis ni de tiempo de ejecución, pero no produce el resultado esperado.

Ejemplos:

1. **Cálculo incorrecto de un valor**:
```typescript
let total = price - discount * quantity; // Incorrect calculation
```

El enfoque correcto es agrupar las operaciones adecuadamente:
```typescript
let total = (price - discount) * quantity; // Correct calculation
```

2. **Uso incorrecto de condicionales**:
```typescript
if (a = b) { // Incorrect use of assignment operator // Some code }
```

El enfoque correcto debe usar operadores de igualdad:
```typescript
if (a == b) { // Correct use of equality operator // Some code } // or if (a === b) { // Correct use of strict equality operator // Some code }
```

###### Cómo identificar y resolver errores lógicos

Para identificarlos, debes probar tu código minuciosamente y comparar la salida real con la esperada, examinando los valores límite (*boundary values*). Considera adoptar el Desarrollo Guiado por Pruebas (TDD) para definir los resultados deseados de antemano.

Ejemplo: Una función para calcular el promedio de un arreglo de números:

```typescript
function calculateAverage(numbers: number[]): number { let sum: number = 0; for (let i: number = 0; i <= numbers.length; i++) { sum += numbers[i]; } return sum / numbers.length; }
```

Al probar `calculateAverage([1, 2, 3, 4])`, se obtiene `NaN` en lugar de `2.5`. Para depurar, agregamos registros:

```typescript
function calculateAverage(numbers: number[]): number { let sum: number = 0; for (let i: number = 0; i <= numbers.length; i++) { console.log(`Index: ${i}, Value: ${numbers[i]}`); // Debugging sum += numbers[i]; } console.log(`Sum: ${sum}, Length: ${ ${numbers.length}`); return sum / numbers.length; } const averageResult = calculateAverage([1, 2, 3, 4]); console.log(averageResult,'this is average result')
```

*Figura 6.5 — Logs de depuración de una función TypeScript con un error lógico, mostrando un valor undefined en el índice 4 debido a una condición de bucle fuera de límites (off-by-one) al calcular el promedio de [1, 2, 3, 4]*

La lógica correcta debe usar `i < numbers.length` y manejar el caso extremo de un arreglo vacío:

```typescript
function calculateAverage(numbers: number[]): number { if (numbers.length === 0) return 0; // Handle edge case let sum: number = 0; for (let i: number = 0; i < numbers.length; i++) { sum += numbers[i]; } return sum / numbers.length; }
```

---

### Sección 2: Manejo de errores en TypeScript

El objetivo del manejo de errores es garantizar que los fallos no afecten negativamente la experiencia del usuario interrumpiendo la funcionalidad o impidiendo el uso adecuado de la aplicación.

#### Manejo de errores síncronos

En JavaScript y TypeScript, la ejecución de código suele ser síncrona, ejecutando cada declaración en secuencia. Si algo sale mal en el camino, puede romper todo el programa a menos que se maneje adecuadamente.

##### ¿Qué son los errores síncronos?

Ocurren durante la ejecución de código síncrono línea por línea.

Para demostrarlo, observemos una pequeña aplicación web:

*Figura 6.6 — Estructura HTML básica para una app calculadora de cuadrados, con campo de entrada numérica, botón Calculate Square y área de visualización de resultados*

Estructura HTML:

```html
<!DOCTYPE html> <html lang="en"> <head> <meta charset="UTF-8" /> <title>Synchronous Error Handling</title> </head> <body> <h2>Calculate the square of:</h2> <input type="text" id="numberInput" placeholder="Enter a number" /> <button onclick="handleCalculation()">Calculate Square</button> <p id="result"></p> <script src="script.js"></script> </body> </html>
```

Código TypeScript (`script.ts`):

```typescript
function calculateSquare(input: string): number { const num = parseFloat(input); if (isNaN(num)) { throw new Error("Please enter a valid number."); } return num * num; } function handleCalculation(): void { const inputValue = (document.getElementById("numberInput") as HTMLInputElement).value; const resultEl = document.getElementById("result") as HTMLParagraphElement; const result = calculateSquare(inputValue); resultEl.style.color = "green"; resultEl.textContent = `Square: ${result}`; }
```

*Figura 6.7 — Aplicación Calculate Square funcionando como se esperaba*

Si un usuario ingresa texto no numérico, la aplicación no muestra ningún resultado y se detiene:

*Figura 6.8 — Visualización de un mensaje de error en la consola del navegador por entrada no válida*

##### Solución del problema con try...catch

Podemos resolver el problema manejando el error con un bloque `try...catch`:

```typescript
function handleCalculation(): void { const inputValue = (document.getElementById("numberInput") as HTMLInputElement).value; const resultEl = document.getElementById("result") as HTMLParagraphElement; try { const result = calculateSquare(inputValue); resultEl.style.color = "green"; resultEl.textContent = `Square: ${result}`; } catch (error: unknown) { resultEl.style.color = "red"; if (error instanceof Error) { resultEl.textContent = error.message; } else { resultEl.textContent = "An unexpected error occurred."; } } }
```

*Figura 6.9 — Mostrando un mensaje de error claro como retroalimentación al usuario cuando se ingresa texto*

##### Validación de datos de entrada

Otra estrategia esencial es validar los datos antes de procesarlos:

```typescript
function handleCalculation(): void { const inputValue = (document.getElementById("numberInput") as HTMLInputElement).value; const resultEl = document.getElementById("result") as HTMLParagraphElement; // Input validation if (!inputValue) { resultEl.style.color = "red"; resultEl.textContent = "Input cannot be empty."; return; } if (!/^-?\d*\.?\d*$/.test(inputValue)) { resultEl.style.color = "red"; resultEl.textContent = "Please enter a valid number."; return; } // Try-catch for main logic try { const result = calculateSquare(inputValue); resultEl.style.color = "green"; resultEl.textContent = `Square: ${result}`; } catch (error: any) { resultEl.style.color = "red"; resultEl.textContent = error.message; } }
```

Comprobaciones añadidas:
- Comprobación de campo vacío:
```typescript
if (!inputValue) { resultEl.style.color = "red"; resultEl.textContent = "Input cannot be empty."; return; }
```
- Comprobación de formato con expresión regular:
```typescript
if (!/^-?\d*\.?\d*$/.test(inputValue)) { resultEl.style.color = "red"; resultEl.textContent = "Please enter a valid number."; return; }
```

#### Manejo de errores asíncronos

La programación asíncrona permite que las operaciones que toman tiempo (como peticiones de red o lectura de archivos) se ejecuten sin bloquear el hilo principal.

##### ¿Qué son los errores asíncronos?

Ocurren cuando una tarea diferida falla fuera del flujo principal de ejecución.

Ejemplo:
```typescript
function fetchData(url: string): Promise<void> { return fetch(url) .then(response => response.json()) .then(data => { console.log(data); }) .catch(error => { console.error("An error occurred:", error); }); } fetchData("https://api.example.com/data");
```

##### Estrategias para el manejo de errores asíncronos

###### 1. Manejo de errores con promesas

Se utilizan los métodos `.then()`, `.catch()` y `.finally()`:

```typescript
function fetchData(url: string): Promise<void> { return fetch(url) .then(response => { if (!response.ok) { throw new Error("Network response was not ok"); } return response.json(); }) .then(data => { console.log("Data fetched successfully:", data); }) .catch(error => { console.error("An error occurred:", error.message); }) .finally(() => { console.log("Fetch operation completed."); }); }
```

###### 2. Manejo de errores con async/await

Proporciona una sintaxis más limpia y secuencial utilizando bloques `try...catch`:

```typescript
async function fetchData(url: string): Promise<void> { try { const response = await fetch(url); if (!response.ok) { throw new Error("Network response was not ok"); } const data = await response.json(); console.log("Data fetched successfully:", data); } catch (error) { console.error("An error occurred:", error.message); } finally { console.log("Fetch operation completed."); } } fetchData("https://api.example.com/data");
```

###### ¿Por qué los errores en catch se tipan como `unknown` en TypeScript?

En JavaScript, cualquier valor puede ser lanzado con `throw`:

```typescript
throw new Error("Something went wrong");
```
```typescript
throw "Something went wrong"; throw 404; throw { code: 500, message: "Server error" };
```

Dado que dependencias externas o APIs pueden lanzar valores no estandarizados, TypeScript asigna el tipo `unknown` a la variable capturada en el bloque `catch`. Esto obliga a verificar el tipo antes de acceder a propiedades como `message`:

```typescript
catch (error: unknown) { if (error instanceof Error) { console.error("Error message:", error.message); } else { console.error("Unexpected error value:", error); } }
```

##### Mejores prácticas para un manejo robusto de errores asíncronos

- **Manejar siempre los errores**: No dejar promesas o funciones async sin capturar.
- **Usar finally para limpieza**: Para cerrar conexiones o liberar recursos.
- **Degradación elegante**: Proporcionar retroalimentación clara y evitar que la app colapse.
- **Registro centralizado de errores**: Utilizar servicios como Sentry o Datadog.
- **Tiempos de espera (*Timeouts*) y lógica de reintento**: Manejar fallos de red transitorios.

---

### Sección 3: Herramientas de depuración en TypeScript

Las herramientas más comunes para depurar en TypeScript incluyen:
- **Depuradores de editores (VS Code)**
- **DevTools del navegador**
- **Mapas de origen (*Source maps*)**
- **Puntos de interrupción (*Breakpoints*)**
- **Registros de consola (*Console logs*)**

#### Trabajo con source maps

Los mapas de origen (`.js.map`) vinculan el código JavaScript compilado con tus archivos fuente originales de TypeScript.

1. **Habilitar mapas de origen en `tsconfig.json`**:
```json
{ "compilerOptions": { "target": "ES6", "module": "commonjs", "outDir": "./dist", "sourceMap": true } }
```

2. **Crear archivo de ejemplo `index.ts`**:
```typescript
function greet(name: string): string { return `Hello, ${name}!`; } const message = greet("Alice"); console.log(message);
```

3. **Compilar el código**:
```bash
npx tsc
```

Genera en `dist/`:
```text
dist/ index.js index.js.map
```

4. **Inspeccionar `index.js`**:
```javascript
function greet(name) { return `Hello, ${name}!`; } const message = greet("Alice"); console.log(message); //# sourceMappingURL=index.js.map
```

*Figura 6.10 — Mostrando el contenido del archivo source map generado*

#### Uso del depurador de VS Code

##### Paso 1: Crear una configuración de lanzamiento (`launch.json`)

*Figura 6.11 — El icono de Run and Debug en la barra lateral de VS Code (Ctrl + Shift + D / Cmd + Shift + D)*

*Figura 6.12 — La opción create a launch.json file en la vista Run and Debug*

*Figura 6.13 — Selección de Node.js como entorno de depuración*

El archivo `launch.json` generado en `.vscode/` tendrá la siguiente estructura:

```json
{ // Use IntelliSense to learn about possible attributes. // Hover to view descriptions of existing attributes. // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387 "version": "0.2.0", "configurations": [ { "type": "node", "request": "launch", "name": "Launch Program", "skipFiles": ["<node_internals>/**"], "program": "${workspaceFolder}/Chapter6/source-maps/dist/index.js", "outFiles": ["${workspaceFolder}/**/*.js"] } ] }
```

##### Paso 2: Agregar un punto de interrupción (*breakpoint*)

Haz clic en el margen izquierdo junto al número de línea deseado para colocar un punto rojo.

*Figura 6.14 — Punto de interrupción añadido en la línea console.log(message) en index.ts*

##### Paso 3: Iniciar la depuración

Presiona `F5` o haz clic en el botón verde de inicio.

*Figura 6.15 — Interfaz de depuración de VS Code con panel de variables y controles de paso a paso (F10, F11, Shift+F11, F5)*

*Figura 6.16 — Uso avanzado de breakpoints*

*Figura 6.17 — Menú de breakpoints en VS Code mostrando Add Breakpoint, Add Conditional Breakpoint y Add Logpoint*

- **Puntos de interrupción de línea (*Line breakpoints*)**: Pausan en la línea especificada.
- **Puntos de interrupción condicionales (*Conditional breakpoints*)**: Pausan solo cuando se cumple una condición, por ejemplo:
```typescript
count > 10
```
- **Puntos de registro (*Logpoints*)**: Registran un mensaje en la consola sin detener la ejecución:
```text
Greeting value: {message}
```

Ejemplo de depuración de transformaciones:
```typescript
function processData(data: number[]): number[] { let processedData = data.map(num => num * 2); return processedData; } let result = processData([1, 2, 3]); console.log(result); // Expected output: [2, 4, 6]
```

#### Aprovechamiento de registros de consola (*Console logs*)

```typescript
function calculateDiscount(price: number, discount: number): number { console.log("Original price:", price); console.log("Discount percentage:", discount); let finalPrice = price - (price * (discount / 100)); console.log("Final price after discount:", finalPrice); return finalPrice; } calculateDiscount(100, 20);
```

Métodos avanzados:
- `console.error()`: Para mensajes de error resaltados en rojo.
- `console.warn()`: Para advertencias.
- `console.table()`: Para visualizar arreglos y objetos en formato tabular.

Buenas prácticas: retirar o minimizar los logs en entornos de producción y usarlos con moderación.

---

### Sección 4: Mejores prácticas de seguridad en TypeScript

#### Introducción a las prácticas de seguridad

La seguridad busca proteger las aplicaciones contra ataques maliciosos, fugas de datos y vulnerabilidades, rigiéndose por la **tríada CIA**:
- **Confidencialidad (*Confidentiality*)**: El acceso a los datos está restringido exclusivamente a personas autorizadas.
- **Integridad (*Integrity*)**: Se evitan modificaciones no autorizadas en los datos.
- **Disponibilidad (*Availability*)**: La aplicación permanece accesible y funcional cuando se necesita.

#### Validación de entradas y sanitización de datos

##### Validación de entradas

Comprueba que las entradas cumplan con el formato, tipo y restricciones esperadas:

```typescript
function validateEmail(email: string): boolean { const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/; return emailRegex.test(email); } if (!validateEmail(userInputEmail)) { throw new Error("Invalid email format."); }
```

##### Sanitización de datos

Limpia y filtra las entradas para eliminar caracteres peligrosos utilizados en ataques como XSS o inyección SQL:

```typescript
function sanitizeInput(input: string): string { return input.replace(/[<>"'();]/g, ""); // Removes potentially harmful characters } const sanitizedUserInput = sanitizeInput(userInput);
```

En entornos profesionales, se recomienda el uso de bibliotecas de validación de esquemas como **Zod** o **Joi**.

#### Técnicas de codificación segura

1. **Evitar el uso de `eval()`**: Puede ejecutar cadenas arbitrarias como código malicioso.
2. **Aprovechar el sistema de tipos de TypeScript**: Evitar `any` y utilizar tipos estrictos.
3. **Establecer valores predeterminados seguros**: Forzar HTTPS y políticas de contraseñas robustas.
4. **Patrón Fail-Fast**: Validar entradas inmediatamente al inicio de las funciones:

```typescript
function getUserData(id: number): string | null { if (typeof id !== 'number' || id <= 0) { return null; // Reject invalid inputs early } // Proceed with fetching user data }
```

5. **Consultas parametrizadas**: En bases de datos, utilizar siempre consultas parametrizadas para evitar ataques de inyección SQL.

#### Manejo de errores con la seguridad en mente

- **Mensajes de error genéricos para el usuario**: Evitar exponer trazas de pila (*stack traces*) o detalles de la base de datos al cliente.
- **Registro seguro**: Guardar los detalles técnicos en logs protegidos del servidor.

```typescript
try { // Code that might throw an error } catch (error) { console.error("An error occurred. Please try again later."); // User-friendly message logError(error); // Detailed logging in a secure location }
```

#### Gestión de datos sensibles

1. **Cifrado y hashing**: Cifrar datos en tránsito (HTTPS) y en reposo. Almacenar contraseñas siempre hasheadas (por ejemplo, con `bcrypt`).
2. **Variables de entorno para credenciales**: Almacenar claves secretas en archivos `.env`:

```text
API_KEY=your_secret_key_here
```

Acceso seguro en TypeScript:
```typescript
const apiKey = process.env.API_KEY;
```

3. **Control de acceso basado en roles (RBAC)**:

```typescript
function isAdmin(req, res, next) { if (req.user.role !== 'admin') { return res.status(403).send('Access Denied'); } next(); }
```

---

### Sección: Resumen

En este capítulo, exploramos los aspectos cruciales del manejo de errores, la depuración y las mejores prácticas de seguridad en TypeScript. Comenzamos comprendiendo varios tipos de errores (sintaxis, tiempo de ejecución y lógicos), sus patrones comunes y estrategias efectivas para solucionarlos.

Luego, profundizamos en el manejo de errores síncronos mediante bloques `try...catch` y validación de entradas, así como el manejo de operaciones asíncronas con promesas y la sintaxis `async/await`, entendiendo por qué TypeScript asigna `unknown` a los errores capturados.

A continuación, cubrimos herramientas y técnicas de depuración, centrándonos en el depurador de VS Code, el uso de mapas de origen (*source maps*), puntos de interrupción convencionales, condicionales y logpoints, junto con las utilidades de consola.

Finalmente, examinamos las mejores prácticas de seguridad en TypeScript, haciendo hincapié en la validación y sanitización de entradas, técnicas de codificación defensiva, manejo seguro de errores y la gestión y protección de datos sensibles.

En el próximo capítulo, cambiaremos nuestro enfoque hacia la **Maximización de la Optimización del Rendimiento** en aplicaciones TypeScript.
