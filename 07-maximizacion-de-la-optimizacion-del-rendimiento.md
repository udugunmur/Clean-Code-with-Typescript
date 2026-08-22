# Parte 2: Pruebas y Calidad de Código

## Capítulo 7: Maximización de la Optimización del Rendimiento

### Sección: Introducción

En este capítulo, analizaremos diversas técnicas y estrategias para optimizar el rendimiento de las aplicaciones TypeScript. Aprenderás técnicas para identificar cuellos de botella, herramientas de creación de perfiles (*profiling*) e implementación de estrategias de optimización de código como minificación, *tree shaking*, carga diferida (*lazy loading*) y almacenamiento en caché (*caching*). El objetivo es dotarte de estrategias prácticas que se puedan aplicar en escenarios del mundo real para mejorar la eficiencia de las aplicaciones. La optimización del rendimiento es crucial no solo para mejorar la experiencia del usuario, sino también para reducir el consumo de recursos, lo que puede generar ahorros de costos significativos.

Aquí tienes lo que aprenderás como parte de este capítulo:

- Comprensión de la importancia de la optimización del rendimiento
- Identificación de cuellos de botella en el rendimiento
- Técnicas para mejorar el rendimiento

Al final de este capítulo, tendrás una sólida comprensión de cómo maximizar el rendimiento de las aplicaciones TypeScript, asegurando que tus proyectos no solo sean funcionales sino también altamente eficientes.

---

### Sección: Requisitos técnicos

Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones en la sección *Download the example code files* en el Prefacio de este libro.

Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

---

### Sección 1: Comprensión de la optimización del rendimiento

La optimización del rendimiento es una parte clave en la creación de aplicaciones TypeScript que sean fiables y escalables. A medida que tu base de código crece y tu aplicación maneja más funcionalidades, usuarios y datos, su rendimiento en el mundo real se vuelve tan importante como las características que ofrece. La optimización no se trata únicamente de velocidad; se trata de mantener tu aplicación con capacidad de respuesta, eficiente y rentable.

En esta sección, veremos por qué es importante la optimización del rendimiento, cómo afecta tanto a los usuarios como a los desarrolladores, y los principios fundamentales detrás de la construcción de aplicaciones TypeScript de alto rendimiento.

#### Por qué es importante la optimización del rendimiento

Cuando creas una aplicación TypeScript, tu objetivo principal es desarrollar algo que funcione de manera eficiente y brinde una experiencia fluida a los usuarios. Sin embargo, incluso si tu aplicación funciona correctamente, es posible que no esté optimizada para ofrecer una excelente experiencia de usuario. Una aplicación lenta puede frustrar a los usuarios, provocar altas tasas de rebote e incluso generar pérdidas de ingresos.

La optimización del rendimiento garantiza que tus aplicaciones TypeScript se ejecuten rápido, respondan con agilidad y utilicen menos recursos del sistema. Esto es especialmente importante a medida que las aplicaciones crecen en complejidad, manejan más usuarios y procesan grandes cantidades de datos.

Razones clave por las que la optimización del rendimiento es crucial:

- **Mejora la experiencia del usuario**: Los usuarios esperan que las aplicaciones se carguen rápidamente y respondan al instante. Por ejemplo, los datos de Google muestran que aumentar el tiempo de carga de la página de 1 segundo a 3 segundos incrementa la probabilidad de que los usuarios abandonen el sitio en un 32%. En un contexto de comercio electrónico, los estudios demuestran que incluso 100 ms de latencia adicional pueden reducir las tasas de conversión entre un 2% y un 7%.
- **Reduce el consumo de recursos**: Las aplicaciones optimizadas utilizan menos CPU, memoria y ancho de banda de red. Esto mejora la duración de la batería para los usuarios móviles, reduce los costos de alojamiento (*hosting*) para las empresas y permite que las aplicaciones escalen de manera eficiente sin necesidad de costosas actualizaciones de infraestructura.
- **Impulsa el posicionamiento en motores de búsqueda (SEO)**: Google y otros motores de búsqueda priorizan los sitios web rápidos. Si tu aplicación es lenta, es posible que no se posicione bien en los resultados de búsqueda.
- **Mejora la escalabilidad**: A medida que más usuarios acceden a tu aplicación, los problemas de rendimiento se vuelven más notorios. Una aplicación bien optimizada puede manejar a miles de usuarios simultáneamente sin ralentizarse ni colapsar.
- **Ahorra tiempo de desarrollo y mantenimiento**: Solucionar problemas de rendimiento de forma temprana evita fallos mayores a largo plazo y reduce la deuda técnica.

#### Causas comunes del bajo rendimiento en aplicaciones TypeScript

Algunas razones comunes que ralentizan las aplicaciones incluyen:

- **Demasiadas rerenderizaciones (*re-renders*)**: En frameworks como React, las rerenderizaciones innecesarias pueden degradar la fluidez de la aplicación.
- **Bucles y funciones recursivas ineficientes**: Bucles que realizan cálculos redundantes o iteran sobre grandes conjuntos de datos.
- **Tamaños de paquete (*bundle sizes*) excesivos**: Cargar demasiado código JavaScript inicial prolonga el tiempo de inicio.
- **Llamadas a API no optimizadas**: Realizar demasiadas peticiones de red o recuperar cantidades masivas de datos innecesarios.
- **Fugas de memoria (*Memory leaks*)**: No limpiar correctamente escuchadores de eventos (*event listeners*), variables o referencias provoca un consumo creciente de RAM.

#### Ejemplo de código: comparación de patrones de iteración

Para comprender cómo los patrones de codificación influyen tanto en el rendimiento como en la legibilidad, comparemos dos enfoques para sumar números en un arreglo.

Enfoque con bucle tradicional `for`:

```typescript
function sumWithForLoop(numbers: number[]): number { let total = 0; for (let i = 0; i < numbers.length; i++) { total += numbers[i]; } return total; } const nums = Array.from({ length: 1000000 }, (_, i) => i); console.time("For Loop Sum"); console.log(sumWithForLoop(nums)); console.timeEnd("For Loop Sum");
```

El bucle `for` realiza una iteración directa, accede a los elementos del arreglo y actualiza el acumulador. En muchos motores de JavaScript, este enfoque está sumamente optimizado y puede ser más rápido que los métodos de orden superior para grandes conjuntos de datos.

Enfoque utilizando la función `reduce()`:

```typescript
function sumWithReduce(numbers: number[]): number { return numbers.reduce((acc, num) => acc + num, 0); } console.time("Reduce Sum"); console.log(sumWithReduce(nums)); console.timeEnd("Reduce Sum");
```

El método `reduce()` proporciona una forma más declarativa y expresiva de realizar la misma operación en un estilo funcional.

#### Rendimiento frente a legibilidad

Aunque `reduce()` puede producir un código más limpio y expresivo, no es necesariamente más rápido. En escenarios donde el rendimiento es crítico, los bucles tradicionales `for` pueden superar a `reduce()` debido a la sobrecarga (*overhead*) que introduce la invocación de una función callback en cada iteración.

Recomendaciones:
- Usa un bucle `for` cuando el rendimiento sea crítico o cuando se requiera un control minucioso.
- Usa `reduce()` cuando se priorice la legibilidad, el estilo funcional y la expresividad.

#### Estrategias clave para la optimización del rendimiento

Las siguientes estrategias ofrecen los mayores beneficios para la mayoría de los proyectos:

- **División de código (*Code splitting*)**: Divide la aplicación en fragmentos (*chunks*) más pequeños que se cargan solo cuando son necesarios (por ejemplo, al navegar a una ruta específica), reduciendo el tiempo de carga inicial.
- **Minificación (*Minification*)**: Elimina caracteres innecesarios (como espacios en blanco y comentarios) para reducir el tamaño de los archivos.
- **Tree shaking**: Elimina el código no utilizado (*dead code*) durante el proceso de compilación para reducir el tamaño final del bundle.
- **Almacenamiento en caché (*Caching*)**: Evita repetir cálculos o solicitudes de red reutilizando resultados previamente calculados o descargados.

---

### Sección 2: Identificación de cuellos de botella en el rendimiento

Los cuellos de botella son áreas en tu aplicación que ralentizan su rendimiento general: tardan demasiado en ejecutarse, consumen demasiada memoria o renderizan más de lo necesario.

#### Uso de herramientas de creación de perfiles para medir el rendimiento

Las herramientas de creación de perfiles (*profiling*) proporcionan información detallada sobre cuánto tiempo consume cada función, cómo se utiliza la memoria y qué partes de la aplicación provocan ralentizaciones.

##### Chrome DevTools Performance Profiler

Para aplicaciones web, el generador de perfiles de rendimiento de Chrome DevTools es una de las herramientas más potentes y accesibles:
- Está integrado directamente en el navegador Chrome.
- Ofrece una línea de tiempo completa de la ejecución de JavaScript, renderizado, pintura (*paint*), diseño (*layout*), solicitudes de red y recolección de basura (*garbage collection*).
- El gráfico de llamas (*flame graph*) y la pila de llamadas permiten detectar tareas largas (*long tasks*) y funciones costosas con precisión milimétrica.

###### Demo simple de Chrome DevTools Performance Profiler

Crea un archivo llamado `perf-demo.html` y ábrelo en Chrome:

```html
<!DOCTYPE html> <html> <head> <title>DevTools Performance Demo</title> <style>body { font-family: sans-serif; padding: 2rem; } button { padding: 1rem 2rem; font-size: 1.2rem; margin: 0.5rem; }</style> </head> <body> <h1>Chrome DevTools Performance Demo</h1> <button id="slow">Run Very Slow Code (~5 sec block)</button> <button id="fast">Run Fast Code</button> <script> function wasteCpuTime() { const start = performance.now(); while (performance.now() - start < 5000) { /* spin */ } console.log("Done wasting 5 seconds"); } document.getElementById("slow").onclick = wasteCpuTime; document.getElementById("fast").onclick = () => console.log("Instant!"); </script> </body> </html>
```

Pasos para generar el perfil de rendimiento:
1. Abre DevTools (`F12` o clic derecho → Inspeccionar).
2. Ve a la pestaña **Performance**.
3. Haz clic en el botón de grabación (**Record** ●).
4. Haz clic inmediatamente en el botón **Run Very Slow Code** en tu aplicación (la interfaz se congelará durante unos 5 segundos).
5. Haz clic en **Stop** para detener la grabación.

*Figura 7.1 – Grabación de Chrome DevTools Performance mostrando una tarea larga en el hilo principal (~5 segundos) activada por la implementación lenta, ilustrando cómo el código bloqueante afecta la capacidad de respuesta y la latencia de interacción (INP)*

Análisis de la grabación:
- **Métricas clave**: El valor de *Interaction to Next Paint* (INP) es de 5099 ms, lo que indica una respuesta visual pésima (cualquier valor superior a 200 ms se considera deficiente).
- **Línea de tiempo**: La barra roja de ~5,099 ms y la barra amarilla continua muestran que el hilo principal estuvo completamente bloqueado por la ejecución de JavaScript de la función `wasteCpuTime`.

##### Webpack Bundle Analyzer

Si tu aplicación tarda en cargarse, puede deberse a un tamaño excesivo del bundle de JavaScript. Webpack Bundle Analyzer permite visualizar qué módulos y dependencias ocupan más espacio.

###### Demostración de Webpack Bundle Analyzer

1. **Crear el proyecto e instalar dependencias**:
```bash
mkdir bundle-demo && cd bundle-demo npm init -y npm install react react-dom lodash moment dayjs npm install --save-dev webpack webpack-cli webpack-bundle-analyzer typescript ts-loader @types/react @types/react-dom @types/lodash
```

2. **Crear `tsconfig.json`**:
```json
{ "compilerOptions": { "target": "es2020", "module": "esnext", "moduleResolution": "node", "jsx": "react-jsx", "strict": true, "esModuleInterop": true, "allowSyntheticDefaultImports": true, "skipLibCheck": true, "forceConsistentCasingInFileNames": true, "outDir": "./dist", "lib": ["dom", "dom.iterable", "es6"] }, "include": ["src"] }
```

3. **Crear `webpack.config.js`**:
```javascript
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer'); module.exports = { mode: 'production', entry: './src/index.tsx', module: { rules: [{ test: /\.tsx?$/, use: 'ts-loader', exclude: /node_modules/ }] }, resolve: { extensions: ['.tsx', '.ts', '.js'] }, plugins: [ new BundleAnalyzerPlugin() // ← this line opens the visualizer automatically ] };
```

4. **Crear el archivo fuente**:
```bash
mkdir -p src && touch src/index.tsx
```

5. **Añadir código a `src/index.tsx`**:
```typescript
import React from 'react'; import ReactDOM from 'react-dom/client'; import moment from 'moment'; // ~280 KB of pain import _ from 'lodash'; // ~70 KB more console.log('moment&lodashareHUGE:',moment,_); const root = document.createElement('div'); root.id = 'root'; document.body.appendChild(root); ReactDOM.createRoot(root).render( <div style={{ padding: '3rem', fontFamily: 'system-ui', lineHeight: 1.6 }}> <h1>Webpack Bundle Analyzer Demo </h1> <p>Open <strong>http://127.0.0.1:8888</strong> to see the treemap!</p> <p>You'll see <code>moment</code> and <code>lodash</code> eating almost the entire bundle.</p> </div> );
```

6. **Compilar y visualizar**:
```bash
npx webpack
```

El navegador abrirá automáticamente el mapa visual interactivo en `http://127.0.0.1:8888`:

*Figura 7.2 – Webpack Bundle Analyzer – la imagen del antes (salida real de nuestra demo)*

El mapa muestra que `moment` y `lodash` acaparan casi la totalidad del bundle, lo que demuestra visualmente la necesidad de sustituir librerías pesadas por alternativas más ligeras (como `dayjs`) o aplicar tree shaking.

##### Hooks de rendimiento de Node.js (para rendimiento en backend)

En el backend, puedes usar los hooks de rendimiento de Node.js para medir la duración de funciones críticas:

```typescript
import { performance } from "perf_hooks"; function slowFunction() { let total = 0; for (let i = 0; i < 1e7; i++) { total += i; } return total; } const start = performance.now(); slowFunction(); const end = performance.now(); console.log(`Execution time: ${end - start}ms`);
```

#### Detección de funciones lentas, rerenderizaciones excesivas y fugas de memoria

##### Optimización de funciones lentas evitando trabajo repetido

Código no optimizado (ordena el arreglo en cada llamada):
```typescript
function processData(data: number[]): number[] { // Sorting happens every time the function is called const sorted = data.sort((a, b) => a - b); return sorted.map(value => value * 2); }
```

Código optimizado (reutiliza el arreglo previamente ordenado):
```typescript
function processDataOptimized(sortedData: number[]): number[] { return sortedData.map(value => value * 2); } const data = [5, 3, 1, 4, 2]; const sortedData = [...data].sort((a, b) => a - b); // Reuse the sorted data instead of sorting repeatedly processDataOptimized(sortedData);
```

##### Detección de rerenderizaciones excesivas en React

Código no optimizado (provoca re-render en cada actualización de estado del padre):
```typescript
function MyComponent({ count }: { count: number }) { return <div>Count: {count}</div>; }
```

Versión optimizada utilizando `React.memo()`:
```typescript
import React from "react"; const MyComponent = React.memo(({ count }: { count: number }) => { return <div>Count: {count}</div>; });
```

`React.memo()` evita que el componente hijo se vuelva a renderizar a menos que sus propiedades cambien.

##### Detección de fugas de memoria (*Memory leaks*)

Mala práctica (no limpia el escuchador de eventos al desmontar):
```typescript
useEffect(() => { window.addEventListener("resize", () => console.log("Resized!")); }, []);
```

Versión optimizada con función de limpieza (*cleanup function*):
```typescript
useEffect(() => { const handleResize = () => console.log("Resized!"); window.addEventListener("resize", handleResize); return () => { window.removeEventListener("resize", handleResize); }; }, []);
```

*Figura 7.3 – La actualización de estado del componente padre desencadena rerenderizaciones innecesarias de los hijos, lo que genera trabajo de CPU desperdiciado en frameworks basados en componentes*

#### Estrategias para priorizar los esfuerzos de optimización

1. **Comenzar con la experiencia del usuario**: Soluciona primero los problemas más lentos y perceptibles para el usuario.
2. **Buscar victorias rápidas (*Quick wins*)**: Optimiza elementos de bajo esfuerzo y alto impacto.
3. **Enfocarse en áreas de alto impacto**: Optimiza funciones que se ejecutan frecuentemente en rutas críticas (*hot paths*).
4. **Reducir el tamaño del bundle**: Utiliza *tree shaking* y *lazy loading*.
5. **Monitorear y mejorar continuamente**: Realiza perfiles periódicos de tu aplicación.

---

### Sección 3: Técnicas para mejorar el rendimiento

#### Implementación de lazy loading y división de código (*Code splitting*)

- **Lazy loading (Carga diferida)**: Retrasa la carga de componentes específicos hasta que realmente se necesitan.
- **Code splitting (División de código)**: Divide los bundles grandes en fragmentos (*chunks*) más pequeños que se pueden cargar de forma independiente.

##### Cómo implementar lazy loading en React

Código antes de lazy loading (carga ansiosa / *eager*):
```typescript
import Dashboard from "./Dashboard"; import Settings from "./Settings"; function App() { return ( <div> <Dashboard /> <Settings /> </div> ); }
```

Código después de lazy loading (carga diferida con `React.lazy` y `Suspense`):
```typescript
import React, { Suspense, lazy } from "react"; const Dashboard = lazy(() => import("./Dashboard")); const Settings = lazy(() => import("./Settings")); function App() { return ( <div> <Suspense fallback={<div>Loading...</div>}> <Dashboard /> </Suspense> <Suspense fallback={<div>Loading...</div>}> <Settings /> </Suspense> </div> ); }
```

##### División de código (*Code splitting*)

Sin división de código:
```text
main.bundle.js → 3.5 MB
```

Con división de código:
```text
main.bundle.js → 800 KB dashboard.chunk.js → 600 KB settings.chunk.js → 400 KB reports.chunk.js → 700 KB
```

Habilitación en Webpack:
```javascript
module.exports = { optimization: { splitChunks: { chunks: "all", }, }, };
```

#### Aplicación de tree shaking para eliminar código no utilizado

*Tree shaking* es una técnica que elimina el código JavaScript no utilizado del bundle final.

Ejemplo de código no optimizado:
```typescript
// utils.ts export function add(a: number, b: number) { return a + b; } export function multiply(a: number, b: number) { return a * b; } // main.ts import { add } from "./utils"; console.log(add(2, 3));
```

Pasos para habilitar tree shaking con Webpack:
1. Usar módulos ES6 (`import`/`export`).
2. Configurar `"sideEffects": false` en `package.json`:
```json
{ "sideEffects": false }
```
3. Compilar en modo producción:
```bash
webpack --mode production
```

#### Aprovechamiento de mecanismos de almacenamiento en caché

Tipos de almacenamiento en caché:
- **Caché del navegador**: Almacena archivos estáticos.
- **Caché de respuestas de API**: Guarda resultados de peticiones para evitar solicitudes repetidas.
- **Memoización**: Almacena en memoria los resultados de funciones costosas.

Ejemplo de memoización en TypeScript:

```typescript
function memoize<T extends string | number>(fn: (arg: T) => number) { const cache: Record<string, number> = {}; return (arg: T) => { const key = String(arg); if (key in cache) { return cache[key]; } const result = fn(arg); cache[key] = result; return result; }; } const square = memoize((n: number) => n * n); console.log(square(4)); // Calculates and stores result console.log(square(4)); // Fetches from cache
```

#### Optimización de bucles, funciones recursivas y operaciones asíncronas

##### Optimización de bucles

Bucle `for` (predecible y de mínimo overhead):
```typescript
const numbers = [1, 2, 3, 4, 5]; let sum = 0; for (let i = 0; i < numbers.length; i++) { sum += numbers[i]; }
```

Método `reduce()` (declarativo y limpio):
```typescript
const numbers = [1, 2, 3, 4, 5]; const sum = numbers.reduce((acc, num) => acc + num, 0);
```

##### Optimización de funciones recursivas

Recursión básica (puede provocar desbordamiento de pila en valores grandes):
```typescript
function factorial(n: number): number { if (n <= 1) return 1; return n * factorial(n - 1); }
```

Recursión de cola (*tail recursion*):
```typescript
function factorial(n: number, acc = 1): number { if (n <= 1) return acc; return factorial(n - 1, n * acc); }
```

Enfoque iterativo (el más seguro para entradas grandes ya que utiliza espacio de pila constante):
```typescript
function factorialIterative(n: number): number { let acc = 1; for (let i = 2; i <= n; i++) { acc *= i; } return acc; }
```

##### Optimización de operaciones asíncronas

###### Debouncing de llamadas a API

Evita realizar llamadas a la API en cada pulsación de tecla ejecutando la acción solo cuando el usuario deja de escribir por un período determinado:

```typescript
function debounce<T extends (...args: any[]) => void>( fn: T, delay: number ) { let timer: ReturnType<typeof setTimeout>; return (...args: Parameters<T>) => { clearTimeout(timer); timer = setTimeout(() => fn(...args), delay); }; } const fetchResults = debounce((query: string) => { console.log("Fetching:", query); }, 500); fetchResults("Hello"); // Executes after 500ms if no further calls occur
```

Otras técnicas asíncronas avanzadas incluyen limitar la concurrencia con *pools* de promesas, agrupar peticiones relacionadas (*batching*) y cancelar solicitudes obsoletas.

---

### Sección: Resumen

En este capítulo, aprendiste varias técnicas de optimización del rendimiento para tus aplicaciones TypeScript. Cubrimos la carga diferida (*lazy loading*) y la división de código (*code splitting*) para reducir los tiempos de carga inicial, *tree shaking* para eliminar código no utilizado y mecanismos de almacenamiento en caché para acelerar la recuperación de datos. Finalmente, analizamos la optimización de bucles, funciones recursivas y operaciones asíncronas para mejorar el rendimiento general.

A lo largo de este capítulo, adquiriste una comprensión integral de cómo maximizar la eficiencia de las aplicaciones TypeScript: reconociste la importancia del rendimiento para la satisfacción del usuario y el ahorro de recursos, identificaste cuellos de botella utilizando herramientas de perfilado como Chrome DevTools, Webpack Bundle Analyzer y los hooks de rendimiento de Node.js, e implementaste técnicas prácticas de mejora.

Al aplicar estas estrategias, puedes crear aplicaciones TypeScript rápidas, reactivas y escalables. Continúa monitoreando y perfilando tu aplicación para abordar nuevos desafíos de rendimiento a medida que tu proyecto evoluciona.

En el próximo capítulo, nos adentraremos en **Dominando los Patrones de Diseño en TypeScript**.
