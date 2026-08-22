# Parte 1: Fundamentos de TypeScript

## Capítulo 1: Introducción a TypeScript

### Sección: Introducción

En el mundo en constante evolución del desarrollo web, **TypeScript** se ha consolidado como una herramienta poderosa para crear código robusto, mantenible y libre de errores. En este capítulo, exploraremos los beneficios de usar TypeScript en el desarrollo web moderno y proporcionaremos ejemplos prácticos de cómo puede emplearse para mejorar la calidad, legibilidad y mantenibilidad del código.

Comenzaremos analizando las razones para utilizar TypeScript e identificando los escenarios en los que resulta más eficaz. A continuación, proporcionaremos una guía paso a paso para configurar un proyecto básico de TypeScript e instalar la herramienta. Después, nos sumergiremos en los tipos fundamentales de TypeScript, incluidos strings, numbers y booleans, y ofreceremos ejemplos prácticos de cómo utilizarlos en escenarios del mundo real. También exploraremos tipos más complejos, como arrays y tuplas, y analizaremos cómo manipular datos con ellos en TypeScript.

Además, cubriremos el uso de enums para una mejor legibilidad del código. Finalmente, introduciremos tipos avanzados, como los tipos de unión (*union types*) y los tipos de intersección (*intersection types*), explicando cómo pueden utilizarse para crear tipos dinámicos y adaptables.

Como parte de nuestra cobertura integral, revisaremos los aspectos más cruciales del archivo `tsconfig.json`, proporcionando una comprensión clara de sus opciones de configuración.

Al final de este capítulo, tendrás una sólida comprensión de TypeScript y podrás utilizarlo para escribir código más eficiente y fácil de mantener.

En este capítulo, cubriremos los siguientes temas principales:

- ¿Por qué TypeScript?
- Primeros pasos con TypeScript
- Comprensión de los tipos básicos
- Trabajo con tipos complejos
- Dominando los tipos avanzados
- Configuración de `tsconfig.json`

---

> [!NOTE]
> **Su compra incluye una copia gratuita en PDF + paquete de código**
> 
> Su compra incluye una copia en PDF sin DRM de este libro, el paquete de código y extras exclusivos adicionales. Consulte la sección de beneficios gratuitos con su libro en el Prefacio para desbloquearlos al instante y maximizar su aprendizaje.

---

### Sección: Requisitos técnicos

Para maximizar tu aprovechamiento de este capítulo y participar activamente en los ejercicios prácticos, asegúrate de contar con los siguientes requisitos técnicos:

- Una comprensión básica del lenguaje de programación **JavaScript**.
- **Node.js y npm**: Node.js debe estar instalado en tu sistema para ejecutar el compilador de TypeScript (`tsc`). Puedes descargar e instalar Node.js desde el sitio web oficial: [https://nodejs.org/en](https://nodejs.org/en).
- Un editor de texto o un entorno de desarrollo integrado (IDE) compatible con TypeScript. Recomendamos utilizar **Visual Studio Code (VS Code)**, un editor de código gratuito y de código abierto desarrollado por Microsoft. Puedes descargar VS Code aquí: [https://code.visualstudio.com/](https://code.visualstudio.com/). También puedes utilizar CodeSandbox para experimentar con TypeScript ([https://codesandbox.io/s/vanilla-typescript-vanilla-ts](https://codesandbox.io/s/vanilla-typescript-vanilla-ts)).
- **tsc**: TypeScript es un superconjunto de JavaScript que añade tipado estático opcional. `tsc` convierte el código TypeScript en código JavaScript ejecutable en cualquier entorno JavaScript. Puedes instalar `tsc` mediante `npm install -g typescript`.
- **Repositorio de código**: Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones de la sección *Download the example code files* en el Prefacio de este libro. Los archivos de código de este capítulo están incluidos en el paquete de código descargable. Para clonar el repositorio mediante HTTPS, utiliza el siguiente comando:

```bash
git clone https://github.com/Rukeeo1/Clean-Code-in-TypeScript.git
```

Con los prerrequisitos técnicos listos, estás preparado para sumergirte en los conceptos fundamentales de TypeScript. Pero antes de comenzar, es importante comprender por qué TypeScript se ha convertido en una opción tan popular entre los desarrolladores. En la siguiente sección, exploraremos las principales ventajas de usar TypeScript y cómo optimiza el proceso de desarrollo.

---

### Sección 1: ¿Por qué TypeScript?

JavaScript fue diseñado originalmente como un lenguaje de scripting ligero para pequeñas interacciones en el navegador, tales como gestionar clics en botones, validar formularios o añadir comportamientos sencillos a la interfaz de usuario. Durante muchos años, nunca se pretendió que gestionara aplicaciones grandes y de larga duración.

A medida que las aplicaciones web se volvieron más ricas y complejas, una mayor cantidad de lógica se trasladó al navegador y a los entornos de JavaScript del lado del servidor. Las bases de código pasaron de unos pocos scripts a cientos de archivos interconectados mantenidos por grandes equipos. Este crecimiento expuso varios desafíos: errores en tiempo de ejecución causados por datos inesperados, dificultad para comprender y refactorizar el código de forma segura, y un soporte limitado de herramientas para razonar sobre sistemas de gran escala.

En el centro de estos problemas existía una limitación fundamental: JavaScript no proporciona ninguna forma integrada de verificar la corrección del código antes de que se ejecute. Con frecuencia, los errores solo se descubren en tiempo de ejecución, a veces mucho después del despliegue.

TypeScript se creó para solucionar esta deficiencia. Al añadir tipado estático opcional y mejores herramientas a JavaScript, TypeScript permite a los desarrolladores detectar errores de manera temprana, razonar sobre el código de forma más eficaz y escalar aplicaciones con mayor confianza.

#### Ventajas del uso de TypeScript

La característica principal de TypeScript es su sistema de tipos estático, el cual permite a los desarrolladores definir tipos para variables, parámetros de funciones, valores de retorno, clases y espacios de nombres. No solo nos beneficiamos de una mayor legibilidad del código al definir explícitamente estos tipos, sino que también se reducen los errores en tiempo de ejecución y se detectan posibles fallos durante la fase de desarrollo. Las aplicaciones del mundo real se benefician de una menor cantidad de errores inesperados en runtime, lo que da como resultado un software más fiable y estable.

En entornos de desarrollo colaborativo, las anotaciones de tipo de TypeScript sirven como documentación para los desarrolladores que trabajan en la misma base de código. Las definiciones de tipos aclaran la entrada y salida esperadas de las funciones, reduciendo confusiones y mejorando la comprensión del código. A medida que los proyectos crecen, mantener y extender el código se vuelve más manejable gracias a la capa adicional de información que proporciona TypeScript.

Al refactorizar o modificar código existente, TypeScript ofrece una red de seguridad al identificar los lugares de la base de código que deben actualizarse debido a cambios en tipos o firmas. Esto minimiza el riesgo de introducir errores durante la refactorización y permite a los desarrolladores realizar modificaciones sin temor a romper la funcionalidad existente.

La integración de TypeScript con herramientas de desarrollo modernas, como VS Code, proporciona IntelliSense para autocompletado de código, sugerencias inteligentes y verificación de errores en tiempo real. Estas funciones permiten una codificación más rápida, reducen la carga cognitiva y mejoran la productividad de los desarrolladores.

#### Aplicaciones de TypeScript en el mundo real

TypeScript cuenta con una amplia adopción en los frameworks y librerías frontend más populares, tales como **Angular**, **React** y **Vue.js**. Al añadir verificación de tipos estática a estos frameworks, TypeScript ayuda a los desarrolladores a construir aplicaciones más fiables y mantenibles al crear interfaces de usuario complejas, gestionar el estado y manejar el flujo de datos. El sistema de tipos de TypeScript proporciona estructura y detecta errores en etapas tempranas del proceso de desarrollo, haciendo que las aplicaciones frontend sean más escalables y fáciles de mantener.

TypeScript no se limita al desarrollo frontend. También está ganando un fuerte impulso en el desarrollo backend utilizando tecnologías como **Node.js** y **Deno**. La capacidad de definir tipos e interfaces hace que trabajar con APIs y bases de datos sea más intuitivo y resistente a errores.

Las anotaciones de tipo de TypeScript garantizan que las solicitudes y respuestas se ajusten a estructuras definidas al construir middlewares o APIs RESTful con frameworks como **Express.js**, lo que resulta en un código menos propenso a errores y una documentación de API mejorada. Las librerías y módulos de terceros también se benefician del sistema de tipos de TypeScript, disponiendo de definiciones de tipos para librerías populares en **DefinitelyTyped**. El repositorio DefinitelyTyped proporciona definiciones de tipo para librerías populares, acortando la brecha entre JavaScript no tipado y proyectos TypeScript seguros en tipos. Puedes encontrar el repositorio aquí: [https://github.com/DefinitelyTyped](https://github.com/DefinitelyTyped).

Ahora que hemos explorado TypeScript, su propósito y los beneficios que aporta, la siguiente sección te guiará a través del proceso de configuración de un proyecto TypeScript, ofreciendo un enfoque paso a paso para una implementación fluida.

---

### Sección 2: Primeros pasos con TypeScript: instalación y configuración del proyecto

Antes de profundizar en las complejidades de TypeScript, establezcamos una base sólida instalando las herramientas esenciales y configurando la estructura básica de un proyecto.

Comenzaremos instalando `tsc`. Esto se puede lograr ejecutando el siguiente comando en tu terminal (ya sea la terminal integrada en VS Code o la predeterminada de tu sistema operativo):

```bash
npm install -g typescript
```

Esto instalará `tsc` de forma global, permitiéndote acceder a él desde cualquier ubicación de tu sistema.

¡Enhorabuena! Has instalado TypeScript con éxito. Ahora podemos continuar con la configuración de nuestro primer proyecto TypeScript.

Para hacerlo, sigue estos pasos:

1. Comienza creando un directorio para tu proyecto. Esta será la carpeta donde se almacenarán todos los archivos del proyecto. Puedes llamarla `clean-code-with-typescript` o elegir un nombre diferente si lo prefieres. Así es como se hace en VS Code:
   - Abre VS Code.
   - Haz clic en **File** en la barra de menú y luego selecciona **Open Folder**.
   - En el cuadro de diálogo que aparece, navega hasta la ubicación donde deseas crear el nuevo directorio.
   - Haz clic en **New Folder** en la parte inferior del cuadro de diálogo.
   - Introduce `clean-code-with-typescript` (o el nombre de tu preferencia) como nombre de la nueva carpeta.
   - Haz clic en **Create** y luego en **OK**.

2. Ejecuta el siguiente comando para inicializar un proyecto npm nuevo y vacío en el directorio de tu proyecto (en la terminal integrada de VS Code):

```bash
npm init -y
```

Este comando genera un archivo `package.json` con la configuración predeterminada. El flag `-y` selecciona automáticamente los valores por defecto, omitiendo el proceso interactivo de configuración. Puedes personalizar estos ajustes según sea necesario editando el archivo `package.json` posteriormente. Tu directorio debería verse como en la Figura 1.1:

*Figura 1.1 — Archivo package.json generado y estructura del directorio del proyecto*

3. Instala TypeScript como dependencia de desarrollo en tu proyecto con el siguiente comando:

```bash
npm install --save-dev typescript
```

Si instalaste TypeScript globalmente en tu máquina, es posible omitir este paso si estás trabajando en algo básico, pero es excelente hacerlo en proyectos donde colaboras con otros, asegurando que todos dispongan de las mismas versiones.

4. Crea un archivo `tsconfig.json` utilizando el siguiente comando:

```bash
npm tsc --init
```

El archivo `tsconfig.json` contiene la configuración para tu proyecto TypeScript. El comando anterior generará un archivo similar al mostrado en la siguiente figura:

*Figura 1.2 — Archivo tsconfig.json generado con la configuración predeterminada*

5. Crea tu primer archivo TypeScript y nómbralo `index.ts`. Abre un editor de texto y añade el siguiente contenido:

```typescript
console.log("Hello, TypeScript!");
```

6. Para compilar tu código TypeScript a JavaScript, ejecuta el siguiente comando en la terminal:

```bash
npx tsc
```

Esto generará un archivo JavaScript llamado `index.js` en el mismo directorio, el cual puede ejecutarse con Node.js:

```bash
node index.js
```

Observarás el mensaje `"Hello, TypeScript!"` impreso en la consola.

¡Felicidades! Has instalado TypeScript con éxito y has configurado un proyecto TypeScript fundamental. La siguiente figura muestra una vista general del resultado final:

*Figura 1.3 — Resultado de crear y compilar el primer archivo TypeScript (index.ts)*

¡Fantástico! Hemos creado nuestro primer proyecto TypeScript y compilado el código a JavaScript. Ahora continuaremos nuestra aventura. Antes de avanzar, hablemos del archivo `tsconfig.json`. Aunque lo exploraremos en profundidad más adelante, resulta útil comprender algunas de sus configuraciones predeterminadas en este momento.

Desglosemos las opciones de configuración en el archivo `tsconfig.json`:

- **target**: Especifica la versión de ECMAScript a la que TypeScript debe compilar. En este caso, el objetivo es **ES5**, la quinta versión principal de JavaScript lanzada en 2009. Esto significa que TypeScript generará código JavaScript compatible con la mayoría de los navegadores web e instalaciones de Node.js.
- **module**: Especifica el formato de módulos para el código JavaScript generado. En este caso, el formato está configurado como `commonjs`, que es el formato de módulos utilizado por Node.js.
- **strict**: Activa el modo estricto, un conjunto de reglas que ayuda a mejorar la seguridad y mantenibilidad del código TypeScript. Por ejemplo, el modo estricto evitará que utilices variables no declaradas o que no cierres paréntesis correctamente.
- **esModuleInterop**: Permite la compatibilidad con características modernas de JavaScript, como las palabras clave `import` y `export`. Esta opción es crucial para que TypeScript funcione con librerías JavaScript que utilizan estas características más nuevas.
- **skipLibCheck**: Indica a TypeScript que omita la verificación de tipos de los archivos de definición de librerías de TypeScript (`lib.d.ts`). Esta es una opción beneficiosa si estás empleando una versión personalizada de la librería de TypeScript. Acelera los tiempos de compilación, especialmente en proyectos grandes con muchas dependencias. `skipLibCheck` debe usarse con precaución, ya que puede ocultar errores dentro de la propia librería de TypeScript o en su interacción con tu código.
- **allowJs**: Permite el uso de archivos JavaScript dentro de tu proyecto TypeScript. Resulta muy útil si necesitas incluir código escrito en JavaScript o si deseas migrar gradualmente un proyecto a TypeScript.

Ahora que nuestro proyecto TypeScript está configurado, profundicemos más. En la siguiente sección, exploraremos los tipos básicos en TypeScript y comprenderemos cómo utilizarlos.

---

### Sección 3: Comprensión de los tipos básicos en TypeScript

En TypeScript, los tipos básicos actúan como los bloques de construcción para crear representaciones de datos significativas. Establecen un entendimiento compartido entre tú, el desarrollador, y el compilador de TypeScript, garantizando claridad y precisión en tu código.

A lo largo de esta sección, comprenderás los tipos básicos en TypeScript, cómo utilizarlos de manera efectiva en tu código y descubrirás ejemplos prácticos y casos de uso para cada uno de ellos.

#### ¿Qué son los tipos básicos en TypeScript?

En TypeScript, los tipos básicos se utilizan para representar los tipos de datos más fundamentales del lenguaje:

- **string**: Representa una secuencia de caracteres.
- **number**: Representa un valor numérico.
- **boolean**: Representa un valor lógico que puede ser `true` o `false`.
- **null**: Representa un valor nulo.
- **undefined**: Representa un valor indefinido.
- **symbol**: Representa un identificador único.

#### Cómo usar los tipos básicos en TypeScript

Para usar los tipos básicos en TypeScript, puedes declarar una variable con un tipo específico. A continuación se muestran algunos ejemplos:

**Tipo String:**

```typescript
let fullName: string = "John Doe";
```

- *Ejemplo práctico / caso de uso*: Utilizado para representar datos de texto, como nombres, direcciones y mensajes.

**Tipo Number:**

```typescript
let age: number = 30;
```

- *Ejemplo práctico / caso de uso*: Utilizado para representar datos numéricos, como edades, precios y cantidades.

**Tipo Boolean:**

```typescript
let isStudent: boolean = true;
```

- *Ejemplo práctico / caso de uso*: Utilizado para representar datos lógicos, como si un usuario ha iniciado sesión o no.

**Tipo Null:**

```typescript
let nothing: null = null;
```

- *Ejemplo práctico / caso de uso*: Utilizado para representar la ausencia de un valor.

**Tipo Undefined:**

```typescript
let something: undefined = undefined;
```

- *Ejemplo práctico / caso de uso*: Utilizado para representar una variable a la que no se le ha asignado un valor.

**Tipo Symbol:**

```typescript
let id: symbol = Symbol("id");
```

- *Ejemplo práctico / caso de uso*: Utilizado para representar un identificador único, como claves de objetos.

Acabamos de listar los tipos básicos comunes en TypeScript. Si seleccionas una de estas variables, como la variable `fullName`, e intentas reasignarla a un tipo diferente que no coincida con el tipo esperado, TypeScript mostrará un error. Por ejemplo, en la siguiente figura, hemos intentado reasignar la variable `fullName` a un número en la línea 4:

*Figura 1.4 — La línea roja ondulada debajo de la variable fullName indica un error de TypeScript*

Si observas detenidamente la línea 3, donde ocurre la reasignación, notarás la línea roja. Si pasas el cursor sobre la línea roja, verás cuál es el problema:

```text
Error: Type 'number' is not assignable to type 'string'.
```

Consulta la siguiente figura para mayor claridad:

*Figura 1.5 — Detalles del error al pasar el cursor sobre la variable fullName*

Aunque este ejemplo pueda parecer trivial, las implicaciones son cruciales en escenarios del mundo real. Imagina construir una aplicación financiera en JavaScript puro donde los tipos de datos precisos son esenciales para las operaciones matemáticas. En estos escenarios, la verificación de tipos de TypeScript detecta posibles errores de tipo de forma temprana, reduciendo el riesgo de fallos, como operaciones aritméticas no deseadas entre tipos incompatibles (por ejemplo, un string y un number).

#### Arrays

Piensa en una lista de compras, una secuencia de elementos que necesitas comprar. En TypeScript, los arrays proporcionan una funcionalidad similar, actuando como colecciones ordenadas de valores. Al igual que los elementos de tu lista, cada elemento dentro de un array puede ser accedido mediante su posición de índice.

En el siguiente ejemplo, declaramos un array `shoppingList` con una anotación de tipo que especifica que debe contener strings. Inicializamos el array con algunos elementos iniciales: `"Apples"`, `"Bananas"` y `"Milk"`:

```typescript
const shoppingList: string[] = ["Apples", "Bananas", "Milk"];
```

Si intentamos añadir un número (`123`) al array `shoppingList` utilizando el método `push`, obtenemos un error de TypeScript:

```text
Argument of type 'number' is not assignable to parameter of type 'string'.
```

En una sección anterior (sobre tipos e interfaces), discutimos los objetos y tipos. Como ejemplo, definimos el tipo `Person`:

```typescript
type Person = { name: string; age: number; greet: () => void; };
```

Ahora, combinemos este tipo `Person` con un array para crear una lista de personas, cada una representada por un objeto. Por ejemplo, incluiremos a dos personas, Alice y Bob:

```typescript
const persons: Person[] = [ { name: "Alice", age: 30, greet() { console.log( `Hello, my name is ${this.name} and I'm ${this.age} years old.` ); }, }, { name: "Bob", age: 25, greet() { console.log( `Hello, my name is ${this.name} and I'm ${this.age} years old.`); }, }, ];
```

En este ejemplo, ya estamos ensamblando estos bloques de construcción para crear código más robusto y seguro en tipos. Si añadieras un valor al array `persons` que no cumpliera con la estructura definida por el tipo `Person`, TypeScript generaría un error.

Como demostramos anteriormente, continuamos aprovechando los beneficios de la inferencia de tipos. Por ejemplo, si intentamos añadir un nuevo objeto person a nuestra lista, TypeScript proporciona una sugerencia útil en nuestro editor de código sobre los tipos de propiedades esperados. Observa la siguiente figura:

*Figura 1.9 — Editor de código mostrando sugerencias de tipos para las propiedades esperadas*

#### Inferencia de tipos

En los ejemplos de tipos básicos que acabamos de ver, especificamos explícitamente el tipo de la variable. Sin embargo, el sistema de tipos de TypeScript puede inferir tipos automáticamente. Por ejemplo, si usamos la variable `fullName` nuevamente sin especificar su tipo de forma explícita, TypeScript seguirá advirtiéndonos si intentamos reasignar la variable a un valor de tipo numérico:

*Figura 1.6 — TypeScript infiriendo tipos, incluso cuando no se declaran explícitamente*

Ahora que hemos explorado los fundamentos de los tipos en TypeScript, profundicemos en tipos más complejos en la siguiente sección.

---

### Sección 4: Trabajo con tipos complejos

En la sección anterior, analizamos los tipos básicos en TypeScript. En el ecosistema de TypeScript, la representación de datos se extiende más allá de tipos simples como strings y numbers. Esta sección profundiza en objetos, arrays, tuplas y enums, lo que te permitirá estructurar y representar colecciones de datos más complejas y mejorar la legibilidad del código.

Comenzaremos con los objetos.

#### Objetos

Los objetos en TypeScript te permiten encapsular datos y funcionalidades relacionadas en una sola entidad. Puedes definir propiedades y métodos de objetos, proporcionando una estructura clara a tu código. En el siguiente fragmento de código, hemos creado un objeto `person`:

```typescript
const person: { name: string; age: number; greet: () => void } = { name: "Alice", age: 30, greet() { console.log( `Hello, my name is ${this.name} and I'm ${this.age} years old.` ); }, };
```

Hemos especificado la estructura esperada de nuestro objeto (el tipo esperado). Veamos qué sucede cuando eliminamos una de las propiedades. En el fragmento de código de la siguiente figura, se ha omitido la propiedad `age`:

*Figura 1.7 — Error de TypeScript cuando el objeto contiene detalles incompletos*

Notarás la línea roja debajo de la variable `person`. Si pasamos el cursor sobre esa línea, veremos más detalles sobre el problema. Para una mejor comprensión, consulta la siguiente figura:

*Figura 1.8 — TypeScript muestra los detalles del error al pasar el cursor sobre el objeto person*

TypeScript vuelve a resultar sumamente útil, resaltando los errores y proporcionando retroalimentación inmediata.

#### Tipos / Interfaces

En el ejemplo anterior, definimos la estructura de un objeto (tipo) en la misma línea que la declaración de nuestra variable. Sin embargo, para mejorar la reutilización y la organización, resulta ventajoso extraer la definición del tipo en un bloque separado:

```typescript
type Person = { name: string; age: number; greet: () => void }
```

Y luego podemos usar el tipo definido con el objeto de la siguiente manera:

```typescript
const person: Person = { name: "Alice", age: 25, greet() { console.log( `Hello, my name is ${this.name} and I'm ${this.age} years old.`); }, };
```

Este enfoque es beneficioso porque permite la reutilización de código. Puedes combinar tipos para formar tipos aún más complejos y reutilizarlos en diferentes partes de tu aplicación.

Las interfaces en TypeScript cumplen un propósito similar a los tipos pero con capacidades adicionales, permitiéndote definir contratos para la forma y el comportamiento de los objetos. Profundizaremos en las interfaces más adelante, explorando sus particularidades y aplicaciones prácticas.

#### Tuplas

Las tuplas son similares a los arrays, pero tienen una longitud fija y predefinida, así como tipos específicos para cada elemento.

Imagina modelar un producto con ID, nombre y precio:

```typescript
type Product = [number, string, number]; const product: Product = [123, "T-Shirt", 19.99];
```

Puedes acceder a esto como a un array normal; por ejemplo, si quisiera ver el precio (tercer elemento del array o índice 2), podría escribir algo como esto:

```typescript
console.log(product[2]);
```

Verás en tu consola o terminal que la salida es la siguiente:

```text
19.99;
```

Pero si declarara una variable diferente, por ejemplo `invalidProducts`, y especificara su tipo como `Product`:

```typescript
const invalidProduct: Product = [123, "T-Shirt"];
```

El código anterior genera un error de tipo. Consulta la siguiente figura para más detalles:

*Figura 1.10 — Editor de código mostrando sugerencias de tipos para las propiedades esperadas*

El mensaje de error indica lo siguiente:

```text
Type '[number, string]' is not assignable to type 'Product'. Source has 2 element(s) but target requires 3.
```

#### Enums

Los enums permiten definir constantes con nombre para un conjunto de valores relacionados con una lista clara y predefinida de opciones. Esta estructura hace que el código sea más consistente y reduce la posibilidad de valores no válidos.

Comencemos con un ejemplo simple para demostrar cómo funcionan los enums:

```typescript
enum Direction { Up, Down, Left, Right } let playerDirection: Direction = Direction.Up; console.log("Player is facing:", playerDirection); Output: Player is facing: 0 (Up)
```

En este ejemplo, definimos un enum `Direction` con cuatro miembros: `Up`, `Down`, `Left` y `Right`. Por defecto, a `Up` se le asigna el valor `0`, a `Down` el `1`, a `Left` el `2` y a `Right` el `3`. Luego asignamos `Direction.Up` a una variable, `playerDirection`, y mostramos su valor en consola, lo que imprime `0`, indicando que el jugador mira hacia arriba.

También puedes asignar valores explícitos a los miembros del enum:

```typescript
enum ErrorCode { NotFound = 404, Unauthorized = 401, InternalServerError = 500 } let error: ErrorCode = ErrorCode.NotFound; console.log("Error code:", error); // Output: Error code: 404 (NotFound)
```

En este ejemplo, definimos un enum `ErrorCode` con tres miembros: `NotFound`, `Unauthorized` e `InternalServerError`. Asignamos códigos de estado HTTP personalizados a cada miembro. Cuando asignamos `ErrorCode.NotFound` a la variable `error`, esta contiene el valor `404`.

Los enums son especialmente útiles al definir funciones que operan sobre un conjunto fijo de valores. Aquí hay un ejemplo:

```typescript
enum DayOfWeek { Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday } function isWeekend(day: DayOfWeek): boolean { return day === DayOfWeek.Saturday || day === DayOfWeek.Sunday; } let today: DayOfWeek = DayOfWeek.Saturday; console.log("Is today a weekend?", isWeekend(today));
```

En este ejemplo, definimos un enum `DayOfWeek` que representa los días de la semana. Luego definimos una función `isWeekend` que toma un parámetro `DayOfWeek` y retorna `true` si es fin de semana (sábado o domingo), y `false` en caso contrario. Finalmente, llamamos a la función con `DayOfWeek.Saturday`, lo cual imprime `true`.

Los enums en TypeScript proporcionan una forma poderosa de representar conjuntos fijos de valores. Al utilizar enums de manera efectiva, puedes hacer que tu código sea más expresivo y autodocumentado, logrando una mayor calidad general del código. Experimenta con enums en tus proyectos TypeScript para ver cómo pueden simplificar tu proceso de desarrollo.

En esta sección, hemos ampliado nuestras habilidades de TypeScript explorando objetos, arrays, tuplas y enums, mejorando nuestra capacidad para manejar datos complejos. A continuación, exploraremos tipos avanzados como uniones, intersecciones, genéricos y condicionales, perfeccionando aún más nuestras capacidades de programación.

---

### Sección 5: Dominando los tipos avanzados

En esta sección, profundizarás en el sistema de tipos avanzado de TypeScript. Aprenderás sobre características potentes como los tipos de unión (*union types*), tipos de intersección (*intersection types*), genéricos y tipos condicionales (*conditional types*). Estos tipos avanzados te permiten crear definiciones de tipos más flexibles, dinámicas y adaptables en tu código TypeScript. Al dominar estos conceptos, estarás preparado para abordar escenarios complejos y escribir código más robusto y fácil de mantener. Comencemos.

#### Tipos de Unión: El poder de OR

Los tipos de unión te permiten definir un tipo que puede contener valores de múltiples tipos. Esta flexibilidad es particularmente útil cuando una función o variable puede aceptar diferentes tipos de valores. Imagina representar una entrada de usuario que podría ser un string o un number:

```typescript
type Input = string | number; function processInput(value: Input) { if (typeof value === "string") { console.log(value.toUpperCase()); } else { console.log(value.toFixed(2), "number"); } }
```

En el código anterior, hemos definido un tipo llamado `Input` que puede ser un string o un number.

Tenemos una función que acepta esta entrada y realiza una operación según el valor pasado a la función.

Llamemos a la función `processInput` con un string:

```typescript
processInput("hello");
```

La llamada anterior imprime en consola la cadena en mayúsculas: `"HELLO"`.

Llamemos ahora a la función `processInput` con un número:

```typescript
processInput(3.14121);
```

La llamada anterior imprime el resultado con dos decimales: `3.14`.

Los tipos de unión permiten un manejo flexible de datos y previenen errores en tiempo de ejecución al garantizar que el valor asignado coincida con uno de los tipos permitidos.

#### Tipos de Intersección: Fusionando fortalezas

En TypeScript, los tipos de intersección proporcionan una forma de combinar las fortalezas de múltiples tipos en un único tipo cohesivo. Imagina combinar las propiedades y métodos de diferentes fuentes para crear un tipo más expresivo. Exploremos este concepto usando un ejemplo relacionado con los empleados de una organización.

Supongamos que estás construyendo una aplicación sencilla para gestionar los empleados de una organización. Cada empleado tiene propiedades específicas como nombre, edad y un número de credencial único. Además, pueden saludar a otros con un mensaje personalizado.

Inicialmente, podrías definir un tipo `Employee` directamente, incluyendo todas las propiedades necesarias:

```typescript
type Employee = { name: string; age: number; greet: () => void; badgeNumber: number; };
```

Sin embargo, si miras de cerca, algunas de estas propiedades te resultarán familiares. Las propiedades `name`, `age` y `greet` son esencialmente las mismas que las de un tipo `Person`. En lugar de duplicar código, creemos una solución más elegante.

Crearemos un tipo `EmployeeBase` que contenga únicamente la propiedad `badgeNumber`:

```typescript
type EmployeeBase = { badgeNumber: number; };
```

Ahora ocurre la magia. Combinaremos el tipo `Person` existente con nuestro tipo `EmployeeBase` utilizando un tipo de intersección:

```typescript
type Employee = EmployeeBase&Person
```

El tipo `Employee` resultante hereda todas las propiedades tanto del tipo `Person` como de `EmployeeBase`. ¡Es como fusionar dos mundos en uno!

Definamos a un empleado llamado John:

```typescript
const john: Employee = { name: "John", age: 30, greet() { console.log( `Hello, my name is ${this.name} and I'm ${this.age} years old.`, ); }, badgeNumber: 2342342, }
```

Ahora, John es un empleado completo con todas las propiedades necesarias.

Al usar tipos de intersección, evitamos la redundancia y mantenemos nuestro código DRY (*don't repeat yourself*). Expresamos las relaciones entre tipos con mayor precisión, haciendo que nuestro código sea más limpio y fácil de mantener.

#### Genéricos: Una herramienta, muchos tipos

Los genéricos en TypeScript te permiten crear funciones, clases e interfaces que pueden funcionar con diferentes tipos de datos. Proporcionan flexibilidad y reutilización al permitir que los tipos se especifiquen dinámicamente. Veamos algunas aplicaciones de los genéricos.

##### Aplicaciones de los genéricos

Uno de los usos principales de los genéricos en TypeScript es hacer que las funciones sean más flexibles y reutilizables. Los genéricos permiten que las funciones manejen múltiples tipos sin necesidad de reescribir la misma función para cada tipo. Exploremos esto con un ejemplo.

Considera una función que invierte un array. Deseas que esta función funcione con cualquier tipo de array (por ejemplo, string o numbers) sin tener que reescribir la función para cada uno. Así es como puedes lograrlo con genéricos:

```typescript
function reverseArray<T>(array: T[]): T[] { return array.reverse(); }
```

Expliquemos el bloque de código anterior:

- **Declaración de función**: Declaramos una función llamada `reverseArray`. Dentro de los paréntesis angulares, usamos `<T>` para declarar un tipo genérico, `T`. Esto significa que la función puede operar con cualquier tipo de array.
- **Implementación de la función**: La función toma un array de tipo `T` como parámetro. `T` podría representar un string, number, boolean, etc. Invierte el array mediante el método `reverse()` y devuelve el array invertido.

Ahora, pongamos a prueba nuestra función. ¿Cómo lo hacemos? Observa la siguiente figura:

*Figura 1.11 — Error de TypeScript en la función genérica reverseArray*

Revisemos cada parte del código en detalle para ver qué está sucediendo.

En primer lugar, invocamos la función `reverseArray` y especificamos su tipo, `T`, como `number`.

Luego, pasamos un array mixto de números y strings. Si inspeccionas con atención, observarás la línea roja debajo de `"some random string"`. Veamos cuál es el error en la siguiente figura:

*Figura 1.12 — Detalles del error de la función genérica reverseArray*

Como puedes ver, estamos especificando dinámicamente cuál debe ser el tipo del array.

Podrías especificar que la función `reverseArray` acepte un array de strings de la siguiente manera:

```typescript
reverseArray<string>(['hello', 'world'])
```

Definir explícitamente el tipo (como hemos hecho en este caso con `string`) ofrece varias ventajas. Asegura que solo se puedan pasar arrays de strings a la función, lo que evita posibles errores si, por ejemplo, se incluye por error un número. Además, especificar el tipo mejora la legibilidad del código, dejando claro de inmediato a otros desarrolladores (o a ti mismo en el futuro) qué tipo de datos debe procesar la función.

En esencia, los genéricos nos permiten escribir una sola función que funciona con diferentes tipos de arrays, haciendo que nuestro código sea más flexible y reutilizable. No tenemos que escribir funciones separadas para arrays de números, strings o cualquier otro tipo; la misma función funciona para todos.

En este punto, probablemente te estés preguntando qué sucede si no pasamos el tipo. Si no pasamos un tipo, TypeScript lo infiere a partir de los valores (un array en este caso). Para tener una imagen clara, consulta la siguiente figura:

*Figura 1.13 — Demostración de la inferencia de tipos de TypeScript con la función reverseArray*

En el ejemplo anterior, no hemos pasado el tipo explícitamente y, sin embargo, TypeScript detecta a partir del array suministrado cuál debe ser el tipo: en este caso, un array de números. Ahora que comprendemos cómo funcionan las funciones genéricas, veamos cómo usar genéricos con clases.

#### Clases genéricas: Estructuras de objetos flexibles

Ilustraremos esto utilizando una pila (*stack*). Imagina construir una clase `Stack` que pueda almacenar elementos de cualquier tipo:

```typescript
class Stack<T> { private items: T[] = []; push(item: T): void { this.items.push(item); } pop(): T | undefined { return this.items.pop(); } }
```

Repasemos el código y veamos qué hace:

- **Clase genérica**: Declaramos una clase llamada `Stack` con un tipo genérico, `<T>`. Este tipo `T` actúa como un marcador de posición, permitiendo que la clase trabaje con cualquier tipo de datos.
- **Almacenamiento adaptable**: Internamente, `Stack` usa un array para almacenar elementos, pero estos elementos pueden ser del tipo `T`. Así, `stringStack` puede contener strings y `numberStack` puede contener numbers, todo dentro de la misma clase `Stack`.
- **Métodos push y pop**: La clase proporciona métodos como `push` para añadir elementos y `pop` para eliminarlos, ambos funcionando a la perfección con el tipo genérico `T`.

Ahora pongamos en uso nuestra clase genérica `Stack`:

```typescript
const stringStack = new Stack<string>(); stringStack.push("hello"); stringStack.push("world"); const numberStack = new Stack<number>(); numberStack.push(10); numberStack.push(20);
```

El código anterior demuestra cómo usar la clase genérica `Stack` con diferentes tipos de datos:

- `const stringStack = new Stack<string>();`: Aquí estamos creando una nueva instancia de la clase `Stack` que contendrá valores de tipo string. La sintaxis `<string>` es cómo especificamos el tipo para esta instancia particular de `Stack`.
- `stringStack.push("hello");` y `stringStack.push("world");`: Aquí usamos el método `push` de la clase `Stack` para añadir los strings `"hello"` y `"world"` a `stringStack`.
- `const numberStack = new Stack<number>();`: De forma similar a `stringStack`, creamos `numberStack` que contendrá valores numéricos.
- `numberStack.push(10);` y `numberStack.push(20);`: Aquí añadimos los números `10` y `20` a `numberStack` utilizando el método `push`.

En este fragmento de código hemos creado dos pilas: una para strings y otra para números. Luego hemos añadido elementos a cada pila usando el método `push`. Esto demuestra la flexibilidad y reutilización de la clase genérica `Stack`, ya que puede manejar pilas de cualquier tipo.

A continuación, veamos el uso de genéricos con interfaces.

#### Interfaces genéricas

En este ejemplo, crearemos una interfaz genérica `Car` que permite que una de sus propiedades sea de un tipo dinámico. Utilizaremos una variable de tipo para lograr esta flexibilidad. Así es como puedes definir la interfaz genérica `Car`:

```typescript
interface Car<T> { make: string; model: string; year: number; data: T; // Dynamic property that can hold any type }
```

Creemos algunos objetos de automóvil utilizando esta interfaz genérica:

**Ejemplo 1 – Automóvil con datos de tipo string:**

```typescript
const stringCar: Car<string> = { make: "Toyota", model: "Camry", year: 2022, data: "Some string data", };
```

**Ejemplo 2 – Automóvil con datos numéricos:**

```typescript
const numberCar: Car<number> = { make: "Tesla", model: "Model 3", year: 2023, data: 42, };
```

**Ejemplo 3 – Automóvil con información de garantía:**

```typescript
interface WarrantyInfo { warrantyType: string; coverageMonths: number; expirationDate: Date; } const warrantyCar: Car<WarrantyInfo> = { make: "Chevrolet", model: "Cruze", year: 2021, data: { warrantyType: "Powertrain", coverageMonths: 36, expirationDate: new Date("2024-02-28"), }, };
```

En este caso, `warrantyCar` representa un Chevrolet Cruze 2021. La propiedad `data` contiene información relacionada con la garantía, como el tipo de garantía, la duración de la cobertura y la fecha de vencimiento.

Al usar genéricos, hemos hecho que nuestra interfaz `Car` sea adaptable a varios tipos de datos, incluidas pólizas de seguro, garantías y más. TypeScript garantiza la seguridad de tipos, permitiéndonos crear objetos de automóviles sumamente versátiles.

---

### Sección 6: Comprensión de los tipos condicionales en TypeScript

Los tipos condicionales en TypeScript nos permiten crear tipos que se adaptan en función de otros tipos. Son como camaleones para nuestro sistema de tipos. Esto te permite crear definiciones de tipos más flexibles y adaptables en tu código.

La idea básica es esta: defines un tipo con una condición. Si la condición es verdadera, el tipo se resuelve en un tipo; si la condición es falsa, se resuelve en un tipo diferente.

La sintaxis básica de un tipo condicional es la siguiente:

```typescript
type MyConditionalType<T> = T extends U ? X : Y;
```

Desglosemos el código:

- **T**: Es un marcador de posición para cualquier tipo de datos.
- **extends U**: Comprueba si `T` es del mismo tipo que `U` (o un subtipo de `U`).
- **X**: Si la comprobación es verdadera, el tipo se convierte en `X`.
- **Y**: Si la comprobación es falsa, el tipo se convierte en `Y`.

Los tipos condicionales se utilizan habitualmente en escenarios donde el tipo resultante depende de las características del tipo de entrada.

Veamos un ejemplo.

Supongamos que tenemos un tipo `Animal` que puede ser `'cat'` o `'dog'`:

```typescript
type Animal = 'cat' | 'dog';
```

Ahora creamos un tipo `Sound` que representa el sonido que hace cada animal. Si es un gato, el sonido es `'meow'`; si es un perro, el sonido es `'woof'`:

```typescript
type Sound<T extends Animal> = T extends 'cat' ? 'meow' : 'woof';
```

Probemos nuestro tipo `Sound`:

```typescript
let sound1: Sound<'cat'>; // sound1 is 'meow' (because 'cat' extends 'cat') let sound2: Sound<'dog'>; // sound2 is 'woof' (because 'dog' doesn't extend 'ca ')
```

En este ejemplo, `Sound` es un tipo condicional. Comprueba si el tipo `Animal` (`T`) es `'cat'`. Si lo es, el sonido se convierte en `'meow'`. De lo contrario, se convierte en `'woof'`.

En la siguiente sección, revisaremos el archivo `tsconfig.json`, qué es y cómo impacta en tu proyecto.

---

### Sección 7: Configuración de tsconfig.json

En esta sección, exploraremos los aspectos críticos de `tsconfig.json`, el archivo de configuración para proyectos TypeScript. Explicaremos su importancia y cómo controla el comportamiento del proyecto. Aprenderás a personalizar las opciones del compilador para optimizar tu proceso de compilación y verificación de tipos. A medida que tu proyecto crezca, te proporcionaremos consejos para mantener y expandir `tsconfig.json`.

#### ¿Qué es exactamente tsconfig.json?

`tsconfig.json` es un archivo que configura tu proyecto TypeScript. Le indica al compilador de TypeScript qué archivos utilizar y cómo convertirlos a JavaScript. Crear un archivo `tsconfig.json` es sencillo. Solo ejecuta `tsc --init` en la carpeta principal de tu proyecto. Esto creará un archivo `tsconfig.json` con la configuración predeterminada. Luego puedes modificar estos ajustes según sea necesario.

#### Profundizando en compilerOptions

En tu archivo `tsconfig.json`, la sección `"compilerOptions"` proporciona un amplio control sobre el proceso de compilación. Para establecer una base sólida para tu proyecto, priorizamos las configuraciones esenciales del compilador y explicamos cómo afecta cada una a tu trabajo. Desglosaremos esto en secciones para facilitar su comprensión:

##### Opciones esenciales del compilador

Estas opciones influyen directamente en la calidad y compatibilidad del código generado. Incluyen las siguientes:

- **target**: Define la versión objetivo de ECMAScript para el JavaScript compilado (por ejemplo, `es5`, `es2017` o `esnext`). Esto afecta directamente la compatibilidad con navegadores y las características disponibles en el código generado.
- **strict**: Activa reglas de verificación de tipos más estrictas para mejorar la calidad del código. Este modo aplica comprobaciones más rigurosas para detectar posibles errores en una etapa temprana del desarrollo.
- **sourceMap**: Genera mapas de origen que asignan el código JavaScript compilado nuevamente al código fuente original de TypeScript. Esto es crucial para una depuración más sencilla, permitiéndote recorrer el código en el depurador y ver las líneas originales de TypeScript en lugar del JavaScript compilado.

##### Configuración del sistema de módulos

Estas opciones determinan cómo se definen, cargan y resuelven los módulos en tu proyecto:

- **module**: Determina el sistema de módulos utilizado en tu proyecto (por ejemplo, `commonjs`, `amd` o `systemjs`). Esto influye en cómo se definen y cargan los módulos, afectando la estructura del proyecto y la gestión de dependencias.
- **moduleResolution**: Configura cómo resuelve el compilador las importaciones de módulos. Esta opción especifica la estrategia para localizar y empaquetar dependencias dentro de tu proyecto.

##### Comprobación de tipos

Estas opciones aplican una seguridad de tipos y verificación de nulos más estricta:

- **noImplicitAny**: Evita que a las variables y parámetros se les asigne implícitamente el tipo `any`. Esto impone un tipado explícito, mejora la mantenibilidad del código y reduce el riesgo de errores en tiempo de ejecución.
- **strictNullChecks**: Habilita una comprobación de nulos más estricta, diferenciando entre referencias que aceptan valores nulos y las que no. Esto ayuda a detectar posibles errores de referencia nula desde el principio.
- **esModuleInterop**: Facilita la compatibilidad con módulos CommonJS al usar módulos ES. Esto permite una integración más fluida de diferentes tipos de módulos dentro de tu proyecto.

##### Opciones avanzadas del compilador

Más allá de las configuraciones esenciales, TypeScript ofrece opciones avanzadas para manejar sintaxis específicas y proporcionar un mayor control sobre el proceso de compilación. Estas opciones no siempre son necesarias para todos los proyectos, pero son esenciales cuando se trabaja con configuraciones más complejas, como la integración de librerías externas, el uso de JSX con React o la gestión de bases de código grandes:

- **lib**: Incluye archivos de librería adicionales que contienen definiciones de tipos para objetos y funcionalidades integradas. Esto amplía los tipos disponibles que puedes usar en tu proyecto.
- **declaration**: Genera los correspondientes archivos de declaración de tipos `.d.ts` para módulos o librerías externas utilizados en tu proyecto. Esto ayuda en la verificación de tipos y el autocompletado para los desarrolladores que utilizan dichos módulos.

##### Opciones de construcción (*Build options*)

Estas opciones ayudan a gestionar el proceso de compilación y organizar la estructura del proyecto:

- **compilerOptions.paths**: Permite definir rutas de resolución de módulos personalizadas, simplificando declaraciones de importación largas o repetitivas.
- **compilerOptions.types**: Permite incluir o excluir definiciones de tipos específicas, lo que permite controlar el alcance de tipos ambientales disponible en el proyecto. Esto puede resultar útil para gestionar dependencias y evitar posibles conflictos.
- **compilerOptions.baseUrl**: Establece el directorio base para resolver las importaciones de módulos. Puede ser útil para organizar proyectos con estructuras de carpetas complejas y garantizar que los módulos se ubiquen correctamente.
- **compilerOptions.outDir**: Especifica el directorio de salida para los archivos JavaScript compilados. Esto ayuda a organizar los artefactos de compilación y separa el código compilado del código fuente.

##### Otras opciones del compilador a considerar

Estas opciones proporcionan funcionalidades adicionales y advertencias para un código más limpio:

- **allowJs**: Permite la compilación de archivos JavaScript planos junto con archivos TypeScript dentro del mismo proyecto.
- **noUnusedLocals**: Advierte sobre variables locales no utilizadas, ayudando a identificar posibles oportunidades de limpieza de código.
- **noUnusedParameters**: Advierte sobre parámetros de funciones no utilizados, lo que promueve un código más limpio.
- **preserveConstEnums**: Mantiene el comportamiento de `const enum` de versiones anteriores de TypeScript, si es necesario por compatibilidad.

Ten en cuenta que la configuración de tu archivo `tsconfig.json` variará según las necesidades y preferencias específicas de tu proyecto. Al familiarizarte con las opciones disponibles y su impacto en el proceso de compilación y el comportamiento del código, puedes adaptar `tsconfig.json` para mejorar tu experiencia de desarrollo con TypeScript.

En la siguiente sección, discutiremos y revisaremos algunas pautas altamente recomendadas para gestionar eficazmente tu archivo de configuración `tsconfig.json`.

#### Mejores prácticas para el mantenimiento de tsconfig.json

La gestión eficaz de tu archivo `tsconfig.json` es esencial para mantener un proyecto TypeScript bien estructurado y extensible. A continuación se presentan algunos consejos prácticos para garantizar que tu archivo `tsconfig.json` se mantenga organizado y funcional:

1. **Centrarse en lo esencial**: Prioriza las opciones principales para mayor claridad y facilidad de mantenimiento. No sobrecargues tu configuración con ajustes innecesarios.
2. **Explicar configuraciones no obvias**: Utiliza comentarios claros para explicar configuraciones que no se explican por sí mismas de inmediato. Esto facilita futuras referencias y comprensión.
3. **Aprovechar `extends`**: Considera extender desde configuraciones base para heredar configuraciones comunes en múltiples proyectos. Esto promueve la coherencia y permite anulaciones específicas del proyecto cuando sea necesario.
4. **Validar con herramientas**: Utiliza linters o validadores dedicados para asegurarte de que tu archivo `tsconfig.json` sea sintácticamente correcto y cumpla con las mejores prácticas. Siguiendo estas prácticas, garantizarás que tu archivo `tsconfig.json` se mantenga limpio, eficiente y escalable, contribuyendo a un proyecto TypeScript bien estructurado y fácil de mantener.

---

### Sección: Resumen

Este capítulo te ha proporcionado habilidades esenciales para un desarrollo eficaz en TypeScript. Has aprendido las razones para usar TypeScript, incluidos sus beneficios y escenarios de uso. Cubrimos el proceso práctico de configuración, incluida la instalación y la configuración básica del proyecto. El capítulo profundizó en los tipos fundamentales, ayudándote a dominar el trabajo con strings, numbers, booleans y el tipo `any`. Exploraste el trabajo con tipos complejos como arrays, tuplas y enums. Además, el capítulo introdujo conceptos avanzados como tipos de unión e intersección, alias e interfaces, genéricos y tipos condicionales. Como extra, abordamos brevemente la configuración de `tsconfig.json` y sus mejores prácticas.

El siguiente capítulo se centra en escribir funciones limpias y mantenibles en TypeScript. Profundizarás en el Principio de Responsabilidad Única (SRP), firmas de funciones, tipos de parámetros, tipos de retorno, parámetros opcionales y predeterminados, y mejores prácticas para el nombrado de funciones y documentación usando JSDoc y TypeScript. Este capítulo mejorará aún más tu dominio de TypeScript, capacitándote para producir código de alta calidad con claridad y facilidad.
