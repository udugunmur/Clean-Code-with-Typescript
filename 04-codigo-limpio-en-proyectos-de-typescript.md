# Parte 1: Fundamentos de TypeScript

## Capítulo 4: Código Limpio en Proyectos de TypeScript

### Sección: Introducción

En los capítulos anteriores, sentamos las bases para escribir código limpio en el contexto de TypeScript. Cubrimos la instalación y configuración de un proyecto TypeScript, discutimos los principios de las funciones limpias y del código limpio, profundizamos en la **Programación Orientada a Objetos (POO)** y exploramos cómo documentar nuestro código con **TypeDoc** y **TSDoc**.

En este capítulo, avanzaremos en estos conceptos y los integraremos en un proyecto TypeScript completo. Nuestro enfoque se centrará en la importancia de la organización del código, la modularización y la adhesión a los principios de código limpio para crear proyectos robustos y escalables. Examinaremos las estructuras de carpetas basadas en funcionalidades (*feature-based*) frente a las basadas en funciones o capas (*function-based*), y brindaremos orientación para seleccionar el enfoque más adecuado para tu proyecto. Además, exploraremos el sistema de módulos, los diferentes tipos y su funcionamiento. También profundizaremos en la gestión de dependencias, comparando varios sistemas como **npm**, **Yarn** y **pnpm**. Finalmente, cubriremos el *linting* y la configuración de comprobaciones personalizadas para garantizar la consistencia y calidad del código.

La configuración de TypeScript se trató en detalle en el Capítulo 1, y asumimos familiaridad con ella para este capítulo basado en proyectos.

Específicamente, cubriremos lo siguiente:

- Mejores prácticas para la estructura de carpetas
- Aprendizaje de sistemas de módulos
- Gestión de dependencias en proyectos de TypeScript
- Linting y formateo de código

Al final de este capítulo, habrás adquirido una comprensión integral de cómo estructurar y organizar tus proyectos de TypeScript de manera efectiva. Aprenderás a gestionar dependencias, configurar los ajustes de TypeScript e implementar herramientas para mantener una alta calidad de código.

---

### Sección: Requisitos técnicos

Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones de la sección *Download the example code files* en el Prefacio de este libro.

Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

---

### Sección 1: Mejores prácticas para la estructura de carpetas

Una estructura de carpetas bien organizada es la columna vertebral de un proyecto TypeScript mantenible y escalable. Es la base sobre la que se construye tu código, y una buena estructura puede marcar una gran diferencia. Imagina intentar encontrar un archivo específico en un proyecto sin una organización clara: ¡es como buscar una aguja en un pajar!

En cualquier proyecto TypeScript, una estructura de carpetas bien definida es esencial por varias razones:

- **Mejora la legibilidad del código**: Facilita a los desarrolladores la comprensión del proyecto de un vistazo, lo cual es crucial en entornos colaborativos donde múltiples miembros del equipo contribuyen a la base de código.
- **Optimiza la mantenibilidad**: Ayuda a gestionar la complejidad y garantiza que se puedan agregar nuevas funciones sin generar caos.
- **Favorece la escalabilidad**: Permite que el proyecto evolucione y se expanda sin requerir una reestructuración significativa.

Al implementar una estructura de carpetas clara y lógica, podrás navegar por tu proyecto con facilidad, encontrar archivos rápidamente y construir una base sólida para tu código.

En esta sección, exploraremos las mejores prácticas para organizar los archivos y directorios de tu proyecto con el fin de garantizar que tu proyecto TypeScript se mantenga mantenible y escalable a medida que crece. Primero, veamos cómo sería una estructura de carpetas estándar para proyectos TypeScript.

#### Estructura de carpetas estándar

En un proyecto TypeScript típico, ciertos directorios y archivos se utilizan comúnmente para organizar el código de una manera clara y mantenible. Si bien la estructura exacta puede variar según el framework y las herramientas utilizadas, los siguientes elementos se observan frecuentemente en proyectos modernos de TypeScript:

- **Directorio `src` (*source*)**: Contiene todos los archivos de código fuente. Aquí es donde organizas tus archivos TypeScript según sus roles y funcionalidades.
- **Directorio `dist` (*distribution*)**: Alberga la salida compilada. Una vez que el código TypeScript se transpila a JavaScript, los archivos se colocan aquí, listos para su despliegue.
- **Directorio `config`**: Almacena archivos de configuración. Esto incluye ajustes para herramientas como webpack, Babel o variables de entorno.
- **Directorio de pruebas (*Test*)**: En proyectos modernos de TypeScript y React, las pruebas a menudo se coubican (*colocated*) junto al código que prueban (por ejemplo, `Button.test.tsx` junto a `Button.tsx`). Este enfoque se alinea bien con el desarrollo guiado por componentes y es compatible con herramientas como Vite, Next.js, Jest y configuraciones de monorrepositorio como Nx o Turborepo. Algunos proyectos aún pueden utilizar un directorio de prueba separado, según la preferencia del equipo y las herramientas.
- **Archivo `index.ts`**: A menudo se utiliza un archivo `index.ts` en el nivel raíz para exportar módulos, proporcionando un punto de entrada único a los componentes de tu proyecto.

En la lista anterior, discutimos la estructura de carpetas estándar en un proyecto TypeScript típico. Si bien esta estructura es común, no dicta cómo deben organizarse los elementos dentro de estas carpetas, como componentes de UI o servicios. Para abordar esto, los desarrolladores suelen considerar dos enfoques principales para agrupar su código en carpetas: **organización basada en funcionalidades (*feature-based*)** y **organización basada en funciones o capas (*function-based*)**. En las siguientes secciones, profundizaremos en los detalles de estas dos estrategias.

Además de la organización basada en funcionalidades y en funciones, muchos proyectos modernos de TypeScript adoptan una **arquitectura basada en módulos** a medida que crecen en tamaño y complejidad. En este enfoque, el código se agrupa en módulos o paquetes independientes, cada uno de los cuales representa una capacidad de negocio diferenciada, como autenticación, facturación o notificaciones.

Este estilo se usa comúnmente en configuraciones de monorrepositorio con herramientas como Yarn workspaces, Nx o Turborepo, donde cada módulo puede tener su propia estructura interna, dependencias y límites de propiedad. La organización basada en módulos se vuelve especialmente valiosa a medida que los equipos escalan, permitiendo que diferentes partes del sistema evolucionen de forma independiente mientras mantienen contratos claros entre ellas.

En la práctica, la estructura del proyecto suele evolucionar con el tiempo. Los equipos pueden comenzar con un diseño simple basado en funciones o funcionalidades y realizar una transición gradual hacia un enfoque basado en módulos a medida que los requisitos comerciales se expanden y la propiedad del código se distribuye más.

#### Organización por funcionalidad frente a función

Tanto la organización basada en funcionalidades como la basada en funciones tienen sus ventajas e inconvenientes particulares, y comprender estas diferencias es crucial para construir una base de código escalable, fácil de mantener y eficiente.

##### Organización basada en funcionalidades (*Feature-based organization*)

En la organización basada en funcionalidades, los archivos y directorios se estructuran en torno a características específicas de la aplicación. Por ejemplo, podrías tener un directorio llamado `cart` que contenga todos los archivos relacionados con la funcionalidad del carrito de compras, incluido el componente del carrito, el servicio del carrito y el reductor del carrito.

**Ventajas:**
- **Cohesión**: Los componentes relacionados se agrupan juntos, lo que facilita la gestión de funcionalidades específicas.
- **Escalabilidad**: Se pueden agregar nuevas funcionalidades sin afectar otras partes del proyecto.
- **Mantenibilidad**: Los cambios relacionados con una característica son más fáciles porque la mayor parte del código relevante reside en un solo lugar.
- **Facilidad de prueba (*Testability*)**: Las pruebas pueden coubicarse con el código de la funcionalidad, lo que facilita la validación y evolución del comportamiento.
- **Experiencia del desarrollador (*DX*)**: Los desarrolladores pueden navegar por la base de código según la capacidad comercial en lugar de saltar entre múltiples capas.

**Desventajas:**
- **Duplicación**: Las funcionalidades comunes podrían duplicarse en diferentes carpetas de características.
- **Complejidad**: Encontrar utilidades compartidas puede volverse difícil.

##### Organización basada en funciones (*Function-based organization*)

En la organización basada en funciones, los archivos y directorios se organizan en torno a funciones o capas específicas de la aplicación, como componentes, servicios y utilidades. Este enfoque agrupa el código por lo que hace, en lugar de por la funcionalidad a la que sirve, similar a una caja de herramientas donde las herramientas se agrupan por tipo, independientemente del proyecto en el que se utilicen.

Por ejemplo, podrías tener un directorio llamado `components` que contenga todos los archivos de componentes, un directorio llamado `services` para todos los archivos de servicios y un directorio llamado `utils` para archivos de utilidades.

**Ventajas:**
- **Reutilización**: El código compartido está centralizado, reduciendo la duplicación.
- **Simplicidad**: Es más fácil encontrar y gestionar funcionalidades comunes.
- **Arquitectura en capas**: Fomenta una arquitectura en capas, donde cada capa tiene una responsabilidad específica y está separada de otras capas.

**Desventajas:**
- **Acoplamiento**: Puede resultar en un acoplamiento estrecho entre capas, dificultando el mantenimiento y la evolución de la aplicación.
- **Jerarquía de directorios plana**: Puede conducir a una jerarquía de directorios plana, lo que dificulta la localización de archivos relacionados con una característica específica.

##### Enfoque híbrido (común en la práctica)

En muchos proyectos de TypeScript del mundo real, los equipos utilizan un enfoque híbrido. La base de código se organiza por funcionalidad en el nivel superior y, dentro de cada funcionalidad, los archivos se agrupan por función o responsabilidad (como componentes, servicios o utilidades). Esto equilibra la cohesión de las funcionalidades con una estructura interna clara y escala bien a medida que crecen las aplicaciones y los equipos.

#### Ejemplos de ambos enfoques

Consideremos un proyecto de mini comercio electrónico de muestra. Así es como podrías organizarlo por funcionalidad o por función:

**Enfoque basado en funcionalidades (*Feature-based*):**

```text
src/ └── products/ ├── ProductList.ts ├── ProductDetail.ts └── ProductService.ts └── cart/ ├── Cart.ts └── CartService.ts └── users/ ├── UserList.ts └── UserService.ts
```

En esta estructura:
- El directorio `products/` contiene todos los archivos relacionados con la funcionalidad de productos, incluida la lista de productos, el detalle del producto y el servicio de productos.
- El directorio `cart/` maneja todos los archivos relacionados con el carrito de compras.
- El directorio `users/` contiene archivos relacionados con la gestión de usuarios.

**Enfoque basado en funciones (*Function-based*):**

```text
src/ └── components/ ├── ProductList.ts ├── ProductDetail.ts ├── Cart.ts └── UserList.ts └── services/ ├── ProductService.ts ├── CartService.ts └── UserService.ts
```

En esta estructura:
- El directorio `components/` alberga archivos para componentes de interfaz de usuario específicos (`ProductList.ts`, `ProductDetail.ts`, `Cart.ts`, `UserList.ts`).
- El directorio `services/` incluye archivos para servicios de backend (`ProductService.ts`, `CartService.ts`, `UserService.ts`). Estos archivos manejan la lógica de negocios, la obtención de datos y otras operaciones de backend para diferentes partes de la aplicación.

Esta organización permite un código centralizado y reutilizable dentro de cada área funcional, promueve la separación de preocupaciones y facilita el mantenimiento y la extensión de la aplicación.

**Enfoque híbrido:**

```text
src/ products/ components/ ProductList.ts ProductDetail.ts services/ ProductService.ts cart/ components/ Cart.ts services/ CartService.ts users/ components/ UserList.ts services/ UserService.ts
```

Esta estructura híbrida mantiene unido el código relacionado con cada funcionalidad, al tiempo que preserva una clara separación de responsabilidades dentro de cada característica.

Ahora que comprendemos el enfoque para organizar la estructura de nuestro proyecto y sus ventajas y desventajas, en la siguiente sección profundizaremos en los sistemas de módulos.

---

### Sección 2: Aprendizaje de sistemas de módulos

Los módulos son los bloques de construcción de un código TypeScript bien organizado. Te permiten agrupar funciones, variables, clases e interfaces relacionadas en unidades autónomas. Piensa en los módulos como cajones en una caja de herramientas: cada cajón contiene herramientas específicas que necesitas para una tarea particular. Al igual que las herramientas en un cajón tienen un propósito definido, los módulos encapsulan funcionalidades dentro de tu aplicación.

#### ¿Qué son los sistemas de módulos?

En TypeScript, un sistema de módulos es una forma de estructurar y organizar el código dividiéndolo en archivos y directorios separados, cada uno de los cuales contiene código relacionado. Esto ayuda a gestionar y mantener grandes bases de código manteniendo juntas las funcionalidades relacionadas y separando las no relacionadas.

Los sistemas de módulos mejoran la mantenibilidad y la reutilización del código de las siguientes maneras:

- **Encapsulamiento**: Mantener el código relacionado junto y evitar que interfiera con otras partes de la aplicación.
- **Reutilización**: Permitirte reutilizar código en diferentes partes de tu aplicación o incluso en diferentes proyectos.
- **Claridad**: Hacer que la base de código sea más fácil de entender y navegar agrupando lógicamente las funcionalidades relacionadas.

Al final de esta sección, comprenderás cómo implementar sistemas de módulos en TypeScript y cómo usarlos para mejorar la mantenibilidad y reutilización del código. Cubriremos los conceptos básicos de creación y uso de módulos, incluyendo cómo exportar e importar funcionalidades entre diferentes módulos.

A continuación, demostraremos cómo definir un módulo simple e integrarlo en otras partes de tu aplicación.

#### Definiendo un módulo

Para comenzar, definimos un módulo simple que contiene una clase `User`. Hacemos esto creando un archivo llamado `src/models/User.ts`, que encapsula su funcionalidad y mantiene el código organizado:

```typescript
// src/models/User.ts export class User { constructor(public id: number, public name: string) {} }
```

En este ejemplo, la clase `User` tiene un constructor que inicializa las propiedades `id` y `name`. La palabra clave `export` hace que la clase `User` sea accesible para otras partes de la aplicación al permitir que se importe donde sea necesario.

#### Encapsulamiento y gestión de alcance (*scope*)

Los módulos ayudan a gestionar el alcance encapsulando el código, asegurando que cada pieza de funcionalidad sea autónoma. Esto evita que las variables y funciones contaminen el ámbito global, evita posibles conflictos de nombres y mantiene el código limpio.

Por ejemplo, al importar la clase `User` a otro módulo, el encapsulamiento ayuda a gestionar esta relación:

```typescript
// src/services/UserService.ts import { User } from '../models/User'; export class UserService { getUser(id: number): User { // logic to retrieve user return new User(id, 'John Doe'); } }
```

Aquí, la clase `User` se importa al módulo `UserService`. El encapsulamiento garantiza que la clase `User` solo sea accesible donde se importa explícitamente, manteniendo una estructura y un alcance claros dentro de la aplicación.

Además, veamos cómo se aplica el encapsulamiento a las funciones de utilidad:

```typescript
// src/utils/MathUtils.ts export function add(a: number, b: number): number { return a + b; } export function subtract(a: number, b: number): number { return a - b; }
```

Al encapsular estas funciones dentro del módulo `MathUtils`, evitamos posibles conflictos y nos aseguramos de que puedan importarse selectivamente cuando sea necesario:

```typescript
// src/app.ts import { add, subtract } from './utils/MathUtils'; console.log(add(5, 3)); // Output: 8 console.log(subtract(5, 3)); // Output: 2
```

#### Reutilización: importando y usando módulos

Los módulos no solo encapsulan funcionalidad, sino que también promueven la reutilización al permitirte importar código existente en diferentes partes de tu aplicación. Esto reduce la duplicación y promueve el principio *Don't Repeat Yourself* (DRY).

Por ejemplo, podemos crear una utilidad de registro (*logging*) reutilizable:

```typescript
// src/utils/Logger.ts export function log(message: string): void { console.log(`[LOG]: ${message}`); } 
```

Esta utilidad de registro puede importarse luego en varios módulos de tu aplicación, mejorando la mantenibilidad y la coherencia:

```typescript
// src/components/ComponentA.ts import { log } from '../utils/Logger'; export function ComponentA() { log('ComponentA initialized'); }
```

#### Mantenibilidad

Al organizar el código en módulos, puedes gestionar y mantener tu base de código de manera más eficaz. Los cambios en un módulo no afectan a otros módulos, lo que facilita el seguimiento de errores y la implementación de nuevas funciones.

#### Escalabilidad

A medida que tu aplicación crece, los módulos facilitan la escalabilidad sin alterar la funcionalidad existente. Por ejemplo, puedes agregar un nuevo módulo para gestionar pedidos en una aplicación de comercio electrónico sin afectar los módulos `User` o `UserService`:

```typescript
// src/models/Order.ts export class Order { constructor(public orderId: number, public userId: number, public amount: number) {} }
```

Este nuevo módulo `Order` se puede agregar sin problemas, demostrando la escalabilidad del código modular.

#### Pruebas mejoradas

Los módulos facilitan la escritura de pruebas unitarias al permitirte probar piezas individuales de funcionalidad de forma aislada. Por ejemplo, puedes escribir pruebas unitarias para la clase `User` y el módulo `UserService` por separado, asegurándote de que cada pieza funcione según lo esperado:

```typescript
// src/tests/User.test.ts import { User } from '../models/User'; test('User class should create a user with id and name', () => { const user = new User(1, 'Alice'); expect(user.id).toBe(1); expect(user.name).toBe('Alice'); });
```

Al probar la clase `User` de forma aislada, puedes garantizar su funcionalidad sin verte afectado por otras partes de la aplicación.

Ahora que comprendemos qué es el sistema de módulos y cómo mejora nuestro código, exploremos los diversos tipos de sistemas de módulos utilizados en TypeScript/JavaScript. Nos centraremos principalmente en los dos más comunes: **Módulos ECMAScript (Módulos ES6)** y **CommonJS**. Además, haremos breves referencias a otros tipos de sistemas de módulos como AMD, UMD y SystemJS.

#### Módulos ES6

Introducidos en ES2015 (ECMAScript 6), los módulos ES6 proporcionan una forma estandarizada de organizar y compartir código mediante `import` y `export`.

Un beneficio clave de los módulos ES6 es el **tree shaking**, el proceso de eliminar el código no utilizado durante la fase de compilación. Cuando el código se estructura como módulos, los empaquetadores (*bundlers*) pueden analizar estáticamente qué exportaciones se usan realmente y eliminar el resto de forma segura. Sin módulos, el código a menudo se empaqueta de una manera que dificulta determinar qué se puede eliminar, lo que genera paquetes más grandes y menos eficientes.

Dado que los módulos ES6 se analizan estáticamente, herramientas como empaquetadores y compiladores pueden optimizar las aplicaciones con anticipación, lo que da como resultado tamaños de paquete más pequeños y un mejor rendimiento en tiempo de ejecución.

He aquí un ejemplo sencillo:

```typescript
// utils.ts (source module) export function formatDate(date: Date): string { // formatting logic return new Date(date).toDateString(); }
```

Y así es como se ve el módulo consumidor (`app.ts`):

```typescript
import { formatDate } from './utils'; const formattedDate = formatDate(new Date()); console.log(formattedDate);
```

Básicamente, en el código anterior, usamos la sentencia `import` para traer la función `formatDate` del módulo `utils`.

Ventajas de usar módulos ES6:
- **Importaciones estáticas**: Los módulos ES6 se analizan estáticamente, lo que permite a las herramientas y compiladores optimizar el código antes de la ejecución.
- **Soporte de navegador**: Son soportados de forma nativa en navegadores modernos.
- **Tree Shaking**: Facilita la eliminación de código no utilizado durante el proceso de compilación.

#### CommonJS

CommonJS es el sistema de módulos predeterminado en Node.js y se utiliza ampliamente en el desarrollo backend. Los módulos se cargan dinámicamente en tiempo de ejecución utilizando la función `require`.

Para exportar un módulo en CommonJS, a diferencia de los módulos ES6 que usan la palabra clave `export`, debes usar `module.exports`. Aquí tienes un ejemplo:

```javascript
module.exports.formatDate = function (date) { // formatting logic return new Date(date).toDateString(); };
```

En este código, definimos una función `formatDate` y la exportamos usando `module.exports`, haciéndola disponible para otros archivos. La función `formatDate` acepta un parámetro de fecha, aplica cierta lógica de formato y devuelve la fecha como una cadena.

Para usar esta función exportada en otro archivo, la importamos usando la función `require`:

```javascript
const utils = require('./utils'); // Require the entire utils module const formattedDate = utils.formatDate(new Date()); console.log(formattedDate);
```

En este ejemplo, usamos `require('./utils')` para cargar todo el módulo `utils`. El objeto `utils` contiene la función `formatDate` exportada, a la que llamamos para formatear la fecha actual. Finalmente, registramos la fecha formateada en la consola.

La naturaleza dinámica de los módulos CommonJS ofrece flexibilidad, pero dificulta el análisis y la optimización del código.

Otros sistemas de módulos, como **AMD**, **SystemJS** y **UMD**, se han utilizado en el pasado, pero no son tan frecuentes en el desarrollo moderno, por lo que no profundizaremos en ellos en este libro. En la siguiente sección, nos centraremos en la gestión de dependencias en proyectos TypeScript.

---

### Sección 3: Gestión de dependencias en proyectos de TypeScript

En el mundo del desarrollo de software, la creación de aplicaciones rara vez implica reinventar la rueda desde cero. Aprovechamos el poder de las bibliotecas y herramientas de código existentes; estas son tus **dependencias**. Las dependencias proporcionan funcionalidades y características esenciales que integras en tu proyecto, ahorrándote tiempo y esfuerzo.

¿Por qué son importantes las dependencias?
- **Desarrollo más rápido**: Al utilizar código preescrito y probado, no necesitas construir todo tú mismo. Esto acelera el desarrollo y te permite concentrarte en la lógica central de tu aplicación.
- **Calidad y consistencia del código**: Las bibliotecas consolidadas a menudo se adhieren a altos estándares de codificación y mejores prácticas, lo que mejora la calidad general y la mantenibilidad de tu proyecto.
- **Soporte de la comunidad**: Las dependencias a menudo cuentan con una comunidad activa que proporciona documentación, tutoriales y soporte fácilmente accesibles cuando encuentras problemas.

A medida que integras dependencias en tu proyecto TypeScript, es fundamental comprender que no todas las dependencias tienen el mismo propósito. Algunas se usan solo durante el desarrollo, mientras que otras son necesarias para la funcionalidad principal de tu aplicación en producción.

#### Tipos de dependencias

Existen dos tipos de dependencias en un proyecto TypeScript:

- **Dependencias de desarrollo (*devDependencies*)**: Son necesarias para compilar, probar y ejecutar tu proyecto durante el desarrollo, pero no se incluyen en la compilación final de producción. Los ejemplos incluyen frameworks de prueba (Jest), empaquetadores (webpack) y linters (ESLint).
- **Dependencias de producción (*dependencies*)**: Son esenciales para la funcionalidad principal de tu aplicación y se incluyen al desplegar tu proyecto en un entorno de producción. Los ejemplos incluyen frameworks web (React, Angular), bibliotecas de acceso a datos (Axios) y bibliotecas de componentes de interfaz de usuario (Material UI).

Dependencias comunes en proyectos de TypeScript:
- **Frameworks de testing**: Jest, Mocha, Jasmine, Vitest
- **Empaquetadores (*Bundlers*)**: webpack, Rollup, Parcel
- **Linters**: ESLint, TSLint
- **Frameworks web**: React, Angular, Vue.js
- **Bibliotecas de acceso a datos**: Axios, Fetch API
- **Bibliotecas de componentes UI**: Material UI, Ant Design, PrimeNG

En la siguiente sección, examinaremos más de cerca cómo se gestionan estas dependencias utilizando gestores de dependencias en Node.js.

#### Gestores de dependencias en Node.js

Los gestores de dependencias son herramientas que automatizan el proceso de instalación, actualización, configuración y gestión de las librerías y paquetes de los que depende un proyecto. Garantizan que se instalen las versiones correctas de las dependencias y ayudan a resolver conflictos entre diferentes paquetes.

En el ecosistema de Node.js, los gestores de dependencias más comunes son **npm**, **Yarn** y **pnpm**. Cada uno tiene su propio conjunto de comandos para gestionar dependencias, pero todos comparten algunos comunes.

##### Descripción general de npm, Yarn y pnpm

- **npm (Node Package Manager)**: El gestor de paquetes predeterminado incluido con Node.js. Ofrece un amplio repositorio de paquetes públicos y privados.
  - *Instalación*: Viene preinstalado con Node.js.
  - *Comandos básicos*:
    - `npm install <package-name>`: Instala un paquete
    - `npm uninstall <package-name>`: Desinstala un paquete
    - `npm list`: Lista las dependencias instaladas
- **Yarn**: Un gestor de dependencias rápido, fiable y seguro, a menudo considerado una alternativa a npm.
  - *Instalación*: `npm install -g yarn` (instalación global)
  - *Comandos básicos*:
    - `yarn add <package-name>`: Instala un paquete
    - `yarn remove <package-name>`: Desinstala un paquete
    - `yarn list`: Lista las dependencias instaladas
- **pnpm**: Un gestor de dependencias relativamente nuevo que se centra en el rendimiento y la eficiencia.
  - *Instalación*: `npm install -g pnpm` (instalación global)
  - *Comandos básicos*:
    - `pnpm add <package-name>`: Instala un paquete
    - `pnpm remove <package-name>`: Desinstala un paquete
    - `pnpm list`: Lista las dependencias instaladas

##### Comparación de npm, Yarn y pnpm

| Aspecto | npm | Yarn | pnpm |
| :--- | :--- | :--- | :--- |
| **Por defecto con Node.js** | Sí | No | No |
| **Instalación** | Viene empaquetado con Node.js | Instalado vía npm | Instalado vía npm |
| **Almacenamiento de dependencias** | `node_modules` plano | `node_modules` plano | Almacén direccionable por contenido con symlinks |
| **Uso de espacio en disco** | Moderado | Moderado | Bajo (almacén compartido entre proyectos) |
| **Velocidad de instalación** | Buena | Más rápida que versiones antiguas de npm | Muy rápida |
| **Archivo de bloqueo (*Lockfile*)** | `package-lock.json` | `yarn.lock` | `pnpm-lock.yaml` |
| **Deduplicación de dependencias** | Deduplicación parcial | Mejor que npm | Estricta y eficiente |
| **Soporte para monorrepos** | Básico | Bueno | Excelente |
| **Curva de aprendizaje** | Baja | Baja | Media |
| **Más adecuado para** | Principiantes, proyectos simples | Equipos que buscan estabilidad y velocidad | Proyectos grandes, monorrepos, equipos enfocados en rendimiento |

*Tabla 4.1 — Comparación de npm, Yarn y pnpm*

¿Cómo elegir un gestor de dependencias? Las tres opciones son ampliamente utilizadas y ofrecen funcionalidades similares. npm es la opción predeterminada si estás comenzando. Yarn es conocido por su velocidad y confiabilidad, mientras que pnpm se enfoca en optimizaciones de rendimiento. Considera las necesidades y preferencias de tu proyecto al realizar una selección.

#### Comprobación de dependencias desactualizadas

La actualización periódica de las dependencias garantiza que tu proyecto se beneficie de las últimas funciones, mejoras de rendimiento y parches de seguridad. También ayuda a mantener la compatibilidad con otros paquetes y herramientas. Cada gestor de dependencias tiene su propia herramienta integrada para comprobar paquetes desactualizados:

- **npm**: `npm outdated`
- **Yarn**: `yarn outdated`
- **pnpm**: `pnpm outdated`

Estos comandos escanearán el archivo `package.json` de tu proyecto e identificarán las dependencias con versiones más nuevas disponibles en el registro:

*Figura 4.1 — El resultado del comando npm outdated*

Desglosemos las columnas mostradas en el resultado de `npm outdated`:
- **Package**: Enumera las dependencias del proyecto, incluyendo `react` y `react-dom`.
- **Current**: Muestra la versión actualmente instalada de cada paquete.
- **Wanted**: Muestra la versión deseada, que es la versión estable más reciente compatible con el proyecto.
- **Latest**: Indica la versión más reciente disponible, que puede incluir cambios importantes (*breaking changes*).
- **Depended by**: Revela las bibliotecas o módulos dentro de la aplicación que dependen de estas dependencias.

#### Actualización de dependencias

Al trabajar en un proyecto, es importante gestionar y actualizar las distintas librerías y paquetes de los que depende tu proyecto. Para gestionar esto, la mayoría de las dependencias siguen un sistema de control de versiones llamado **Versionado Semántico (SemVer)**.

Este sistema utiliza un formato de `major.minor.patch` para indicar la importancia de los cambios en una nueva versión:

- **Major**: Introduce cambios importantes e incompatibles (*breaking changes*) que pueden requerir modificaciones de código en tu proyecto.
- **Minor**: Añade nuevas características o funcionalidades manteniendo la compatibilidad con versiones anteriores.
- **Patch**: Corrige errores o vulnerabilidades de seguridad sin introducir cambios importantes.

Es especialmente importante tener precaución al actualizar a una versión *Major*. Antes de actualizar, es una buena práctica revisar las notas de la versión, probar los cambios en un entorno controlado y actualizar el código dependiente según sea necesario para evitar problemas imprevistos.

Comandos para actualizar dependencias en los diferentes gestores:

- **npm**:
  - `npm update <package-name>`: Actualiza un paquete específico a la última versión compatible (según el rango SemVer en `package.json`).
  - `npm update`: Actualiza todas las dependencias instaladas a sus versiones compatibles más recientes.
  - `npm update <package-name>@<version>`: Actualiza un paquete a una versión específica.
- **Yarn**:
  - `yarn upgrade <package-name>`: Actualiza un paquete específico a la última versión compatible.
  - `yarn upgrade`: Actualiza todas las dependencias instaladas a sus versiones compatibles más recientes.
  - `yarn upgrade <package-name>@<version>`: Actualiza un paquete a una versión específica.
  - `yarn upgrade-interactive`: Actualiza todos los paquetes, solicitando confirmación antes de proceder (útil para gestionar posibles *breaking changes*).
- **pnpm**:
  - `pnpm update <package-name>`: Actualiza un paquete específico a la última versión compatible.
  - `pnpm update`: Actualiza todas las dependencias instaladas a sus versiones compatibles más recientes.
  - `pnpm update <package-name>@<version>`: Actualiza un paquete a una versión específica.
  - `pnpm update --interactive`: Actualiza todos los paquetes, solicitando confirmación interactiva antes de proceder.

##### Estrategias para actualizar dependencias de forma segura

1. **Copia de seguridad y control de versiones**: Asegúrate de que tu proyecto esté bajo control de versiones mediante un sistema como Git. Crea copias de seguridad o ramas de prueba antes de realizar actualizaciones.
2. **Actualizaciones incrementales**: Actualiza una dependencia a la vez. Prueba a fondo después de cada actualización para identificar cualquier problema de forma temprana.
3. **Revisar registros de cambios y notas de versión**: Examina los *changelogs* y notas de versión de las dependencias que estás actualizando para comprender el impacto potencial.
4. **Pruebas**: Ejecuta tu suite de pruebas automatizadas después de cada actualización para detectar regresiones o problemas introducidos por la nueva versión.

---

### Sección 4: Linting y formateo de código

Piensa en un proyecto de software como la construcción de un edificio. A medida que se agregan nuevas funciones, la base de código crece. Sin embargo, al igual que los ladrillos mal colocados pueden provocar debilidades estructurales, el formato inconsistente, la falta de mejores prácticas y los errores no detectados pueden debilitar tu base de código. Con el tiempo, esto hace que el proyecto sea más difícil de leer, depurar y mantener.

Para evitar estos problemas, los desarrolladores utilizan herramientas de **linting** y **formateo de código**. El *linting* actúa como un inspector de calidad, identificando posibles errores y haciendo cumplir los estándares de codificación. El formateo de código garantiza un estilo visual uniforme, mejorando la legibilidad y la mantenibilidad.

#### Configuración de ESLint

En esta sección, veremos cómo configurar ESLint en un proyecto TypeScript:

##### Paso 1 — Instalar ESLint y los plugins necesarios

Ejecuta el siguiente comando para instalar ESLint junto con los paquetes necesarios para TypeScript:

```bash
npm install eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin --save-dev
```

Estos paquetes permiten que ESLint comprenda la sintaxis de TypeScript y aplique las reglas de linting recomendadas.

##### Paso 2 — Inicializar la configuración de ESLint

Crea un archivo de configuración de ESLint ejecutando el siguiente comando y respondiendo a las preguntas:

```bash
npx eslint –init
```

*Figura 4.2 — Mostrando las preguntas para inicializar ESLint en nuestro proyecto*

Resumen de las opciones seleccionadas en el asistente interactivo:
1. *How would you like to use ESLint?*: Se selecciona **To check syntax, find problems, and enforce code style**.
2. *What type of modules does your project use?*: Se selecciona **JavaScript modules (import/export)**.
3. *Which framework does your project use?*: Se selecciona **None of these** (proyecto TypeScript puro).
4. *Does your project use TypeScript?*: Se responde **Yes**.
5. *Where does your code run?*: Se selecciona **Browser**.
6. *Would you like to install them now?*: Se responde **Yes**.
7. *Which package manager do you want to use?*: Se selecciona **npm**.

El comando anterior creará un archivo `eslint.config.mjs` en el directorio raíz de tu proyecto:

*Figura 4.3 — El archivo eslint.config.mjs generado*

##### Paso 3 — Probar ESLint con un archivo de código de muestra

Crea un archivo llamado `index.ts` con el siguiente código que contiene un error común:

```typescript
function greet(name: string) { console.log("Hello" + naame + "!"); }
```

*Figura 4.4 — index.js con el código de la función greet defectuosa*

Para comprobar si ESLint detecta el problema, ejecuta el siguiente comando:

```bash
npx eslint *.ts
```

*Figura 4.5 — Errores encontrados por ESLint*

¡Felicidades! ESLint ha detectado con éxito el error. Tu proyecto ahora cuenta con linting básico.

Para obtener más detalles sobre reglas y comprobaciones adicionales, consulta la documentación oficial: [https://eslint.org/docs/latest/use/getting-started](https://eslint.org/docs/latest/use/getting-started).

#### Configuración de Prettier

Prettier es una popular herramienta de formateo de código que ayuda a mantener tu código limpio, legible y consistente. Formatea automáticamente tu código de acuerdo con un conjunto de reglas y preferencias, ahorrándote tiempo y esfuerzo.

Pasos para configurar Prettier en tu proyecto:

1. **Instalar Prettier**: Instala Prettier junto con los complementos necesarios para la compatibilidad con ESLint:

```bash
npm install prettier eslint-plugin-prettier eslint-config-prettier --save-dev
```

2. **Crear la configuración de Prettier**: Crea un archivo `.prettierrc` para definir tus reglas de formato:

```json
{ "semi": true, "singleQuote": true, "trailingComma": "all", "printWidth": 80 }
```

3. **Integrar Prettier con ESLint**: Actualiza `eslint.config.mjs` para incluir la integración con Prettier:

```javascript
export default defineConfig([ { files: ["**/*.{js,mjs,cjs,ts,mts,cts}"], plugins: { js }, extends: ["js/recommended"] }, { files: ["**/*.{js,mjs,cjs,ts,mts,cts}"], languageOptions: { globals: globals.browser } }, tseslint.configs.recommended, prettier.configs.recommended, ]);
```

4. **Formatear tu código**: Ejecuta Prettier para formatear tu código:

```bash
npx prettier --write *.ts
```

Al ejecutar este comando, tu código se formateará según las reglas establecidas (por ejemplo, reemplazando comillas dobles por comillas simples y ajustando los espacios).

---

### Sección: Resumen

En este capítulo, exploramos las prácticas clave para escribir código limpio, mantenible y escalable en proyectos de TypeScript. Comenzamos analizando las mejores prácticas para estructurar carpetas, enfatizando la importancia de organizar los proyectos de manera que faciliten la navegación y el mantenimiento a largo plazo. También profundizamos en los sistemas de módulos, destacando su papel en la mejora de la reutilización y la mantenibilidad del código. Además, examinamos cómo gestionar las dependencias de manera eficaz, cubriendo gestores de paquetes, estrategias para actualizar dependencias en proyectos de gran escala y el uso del versionado semántico (SemVer) para garantizar la compatibilidad. Para mantener la calidad y la consistencia del código, introdujimos herramientas de linting y formateo como ESLint, TSLint y Prettier. Al aplicar estas técnicas, puedes asegurarte de que tu base de código TypeScript se mantenga eficiente y fácil de trabajar a medida que tu proyecto crece.

En el próximo capítulo, cambiaremos nuestro enfoque hacia garantizar la calidad y confiabilidad de tu código mediante el establecimiento de una sólida estrategia de pruebas, cubriendo temas esenciales como pruebas unitarias, pruebas de integración y el **Desarrollo Guiado por Pruebas (TDD)**.
