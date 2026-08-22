# Parte 1: Fundamentos de TypeScript

## Capítulo 2: Escritura de Funciones Limpias

### Sección: Introducción

Las funciones son los bloques de construcción de cualquier programa. Te permiten descomponer problemas complejos en tareas más pequeñas y sencillas, además de facilitar la reutilización del código, reduciendo la repetición. Sin embargo, no todas las funciones se crean de la misma manera: algunas son fáciles de leer, entender y mantener, mientras que otras son desordenadas, confusas y propensas a errores. ¿Cómo puedes escribir funciones que sean limpias y mantenibles? ¿Cuáles son las mejores prácticas y principios para diseñar y documentar funciones en TypeScript?

En este capítulo, aprenderás a escribir funciones limpias y mantenibles aplicando el **Principio de Responsabilidad Única (SRP)**, evitando efectos secundarios y utilizando firmas de funciones de manera efectiva. También descubrirás cómo elegir nombres descriptivos para las funciones y cómo documentar tu código utilizando **TSDoc** y **TypeDoc**. Al comprender las firmas de funciones, como los tipos de parámetros y de retorno, puedes mejorar tanto la estructura como la legibilidad de tus funciones. Además, exploraremos cómo utilizar la inferencia de tipos de TypeScript, los parámetros opcionales y los valores predeterminados para mejorar la claridad de tu código.

Al final de este capítulo, serás capaz de escribir funciones limpias y mantenibles, aplicar SRP, evitar efectos secundarios, comprender y utilizar firmas de funciones, elegir nombres descriptivos y documentar tus funciones con TSDoc y TypeDoc. Cubriremos los principios de las funciones limpias, las firmas de funciones y las mejores prácticas para el nombrado y la documentación.

Específicamente, este capítulo cubrirá:

- Aprendizaje de los principios de funciones limpias
- Comprensión de las firmas de funciones

---

### Sección: Requisitos técnicos

Antes de continuar, es fundamental contar con una comprensión básica de los fundamentos de TypeScript y su funcionamiento. Si eres nuevo en TypeScript o necesitas un repaso, se recomienda familiarizarse con sus conceptos fundamentales (ver Capítulo 1). Esto incluye entender la sintaxis de TypeScript, los tipos de datos y en qué se diferencia de JavaScript.

Para comenzar con TypeScript, asegúrate de tener configuradas las herramientas y el entorno necesarios:

- Un editor de código o IDE (como Visual Studio Code, IntelliJ o Sublime Text)
- Node.js instalado en tu máquina
- El compilador de TypeScript (`tsc`) instalado de forma global o local en tu proyecto
- Una comprensión básica de cómo crear y gestionar archivos TypeScript (`.ts`) y compilarlos en archivos JavaScript (`.js`)

Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones de la sección *Download the example code files* en el Prefacio de este libro. Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

---

### Sección 1: Aprendizaje de los principios de funciones limpias

Imagina trabajar en un proyecto de construcción masivo donde cada ladrillo se coloca al azar sobre el anterior, careciendo de una organización y un propósito claros. Dicha estructura sería frágil y propensa a derrumbarse bajo presión. Del mismo modo, las funciones escritas sin considerar los principios de código limpio pueden generar un código enredado que resulta difícil de entender, depurar y extender.

Las funciones limpias, por el contrario, son funciones bien estructuradas, concisas y fáciles de entender. Actúan como pilares de soporte colocados con precisión, ofreciendo varias ventajas:

- **Mayor legibilidad del código**: Una estructura clara y un nombrado descriptivo mejoran la comprensión tanto para ti como para futuros colaboradores.
- **Reducción de la complejidad**: Descomponer la lógica en funciones enfocadas evita la sobrecarga de código y simplifica el mantenimiento.
- **Reutilización mejorada**: Las funciones bien definidas pueden integrarse fácilmente en diferentes partes de tu aplicación.
- **Capacidad de prueba mejorada**: Las funciones más pequeñas y autónomas son más fáciles de aislar y probar a fondo.

Algunos de los principios para escribir funciones limpias son los siguientes:

1. Cada función debe tener una **única responsabilidad**, lo que significa que debe hacer una sola cosa y hacerla bien.
2. Cada función debe tener un **nombre claro y descriptivo** que refleje su propósito y comportamiento.
3. Cada función debe tener un **número mínimo y consistente de parámetros**, idealmente tres o menos, con cada parámetro bien definido y tipado. Si se necesitan más, considera agrupar parámetros relacionados en un objeto o dividir la función en partes más pequeñas.
4. Cada función debe tener un **valor de retorno claro y explícito** que esté bien definido y tipado.
5. Cada función debe **comportarse de manera predecible y consistente**, evitando efectos no deseados al mantener sus operaciones contenidas dentro de su propio alcance.
6. Cada función debe estar **documentada**, usando comentarios o anotaciones que expliquen su funcionalidad y uso.

Ahora que tenemos una visión general de las funciones limpias y sus beneficios, procedamos a examinar cada principio en detalle y veamos cómo se traducen en aplicaciones prácticas en tu código.

#### Nombrado de funciones

Elegir nombres apropiados para las funciones es esencial para escribir código legible y comprensible. Los buenos nombres de funciones mejoran la claridad y mantenibilidad del código y facilitan que los desarrolladores comprendan el propósito del código sin tener que profundizar en los detalles de su implementación. Aquí tienes algunas pautas para nombrar tus funciones de manera efectiva:

- **Usar camel case**: Comienza los nombres de funciones con una letra minúscula y usa letras mayúsculas para las palabras posteriores (por ejemplo, `calculateArea`, `isUserLoggedIn`). Esta convención se utiliza ampliamente en JavaScript/TypeScript y garantiza la coherencia con el nombrado de variables.
- **Ser descriptivo**: Utiliza nombres descriptivos que transmitan con precisión el propósito o la acción de la función. El nombre de una función debe proporcionar una indicación clara de lo que hace sin necesidad de leer su implementación. Además, evita el uso de abreviaturas o nombres excesivamente genéricos (por ejemplo, `doSomething`, `processStuff`).
- **Usar verbos para las acciones**: Comienza los nombres de funciones con un verbo para indicar la acción realizada por la función. Esto ayuda a distinguir las funciones de las variables u otras entidades en el código. Por ejemplo, utiliza nombres como `getUserData()` o `updateDatabase()` para indicar claramente las acciones realizadas por estas funciones.
- **Considerar el uso de prefijos**: Ciertos prefijos específicos pueden mejorar la claridad para determinados tipos de funciones; aquí tienes algunos ejemplos:
  - `is`: Para funciones booleanas (por ejemplo, `isValid()`, `isAuthorized()`)
  - `get`: Para funciones que recuperan datos (por ejemplo, `getUserName()`, `getProductDetails()`)
  - `set`: Para funciones que establecen o actualizan valores (por ejemplo, `setTheme()`, `setActiveUser()`)
- **Evitar nombres excesivamente largos**: Aunque la descriptividad es importante, los nombres de funciones demasiado largos pueden resultar engorrosos y dificultar la lectura. Busca un equilibrio entre claridad y concisión.
- **Ser consistente**: Adhiérete a convenciones de nombrado consistentes en toda tu base de código. La coherencia en el nombrado ayuda a mantener la legibilidad y facilita a los desarrolladores la comprensión y navegación por el proyecto. Si tu equipo sigue una convención específica, cúmplela para mantener la uniformidad.

Al seguir estas pautas, puedes asegurarte de que los nombres de tus funciones sean claros, descriptivos y útiles para cualquiera que lea o trabaje con tu código. Ahora que comprendemos la importancia de nombrar las funciones correctamente, exploraremos un principio fundamental conocido como el **Principio de Responsabilidad Única (SRP)**. Este principio enfatiza que una función debe tener un único propósito bien definido. Exploraremos cómo organizar tus funciones siguiendo los principios de SRP, logrando un código más enfocado y reutilizable.

#### Aplicación del Principio de Responsabilidad Única

El **Principio de Responsabilidad Única (SRP)** es uno de los principios más importantes de las funciones limpias. Establece que cada función debe tener una sola razón para cambiar, lo que significa que debe tener una única responsabilidad o una sola tarea que realizar. Esto hace que la función sea más enfocada, cohesiva y modular. También reduce la complejidad y el acoplamiento de la función, facilitando su comprensión, prueba y depuración.

Para aplicar el SRP a tus funciones, debes seguir estos pasos:

1. Identifica el propósito principal o meta de tu función y escríbelo en una sola oración.
2. Analiza tu función y observa si hace algo más aparte de su propósito principal. Si es así, considera extraer esas partes en funciones separadas.
3. Refactoriza tu función en múltiples funciones más pequeñas si es necesario. Asigna a cada nueva función un nombre claro y descriptivo que refleje su responsabilidad específica, como se describió en la sección anterior.
4. Revisa tu función y comprueba si aún sigue el SRP. Si no es así, repite los pasos anteriores hasta que lo cumpla.

A continuación, veamos un ejemplo de aplicación del SRP a una función.

Supongamos que tienes la siguiente función que calcula el precio total de un carrito de compras, aplica un descuento e imprime el recibo:

```typescript
type CartItem = { price: number; quantity: number; }; function checkout(cart: CartItem[], discount: number) { let total = 0; for (let item of cart) { total += item.price * item.quantity; } total = total * (1 - discount / 100); console.log("Your total is: $" + total.toFixed(2)); console.log("Thank you for shopping with us!"); }
```

Esta función tiene más de una responsabilidad: calcula el precio total, aplica el descuento e imprime el recibo. También tiene un nombre vago que no refleja su comportamiento. Para hacerla más limpia, podemos aplicar el SRP y extraer las diferentes responsabilidades en funciones separadas:

```typescript
function calculateTotal(cart: CartItem[]) { let total = 0; for (let item of cart) { total += item.price * item.quantity; } return total; } function applyDiscount(total: number, discount: number) { return total * (1 - discount / 100); } function printReceipt(total: number) { console.log("Your total is: $" + total.toFixed(2)); console.log("Thank you for shopping with us!"); } function checkout(cart: CartItem[], discount: number) { let total = calculateTotal(cart); total = applyDiscount(total, discount); printReceipt(total); }
```

Hemos mejorado nuestro código aplicando el principio SRP. Dividimos la función `checkout` en tres funciones separadas: `calculateTotal`, `applyDiscount` y `printReceipt`. Cada una de estas funciones tiene un nombre claro y descriptivo y una única responsabilidad, lo que facilita su comprensión y prueba individual.

Revisémoslas:

- La función `calculateTotal` itera a través de cada elemento en el carrito y calcula el precio total.
- La función `applyDiscount` toma el precio total y aplica un descuento.
- La función `printReceipt` imprime el total final y un mensaje de agradecimiento.

Finalmente, redefinimos la función `checkout` para usar estas funciones más pequeñas. Esta nueva función `checkout` es más concisa y legible. Delega tareas a las otras funciones, haciendo que el código sea más modular y comprobable. Este enfoque también hace que el código sea más fácil de mantener y extender, ya que los cambios en una función tienen menos probabilidades de impactar en las demás.

Es importante señalar que el SRP no es algo específico de TypeScript, sino un principio general de programación. Puede ayudarte a escribir código más limpio y fácil de mantener en cualquier lenguaje y paradigma de programación.

Ahora que dominamos el SRP, veamos otro aspecto crucial del código limpio: la gestión de efectos secundarios en nuestras funciones de TypeScript. Para hacer que nuestro código sea predecible y más fácil de probar, debemos comprender bien los efectos secundarios no deseados y saber cómo evitarlos. En la siguiente sección, descubriremos qué son los efectos secundarios, por qué son perjudiciales y cómo podemos evitarlos para garantizar la integridad del código.

#### Comprensión y prevención de efectos secundarios (malos) en funciones de TypeScript

Los efectos secundarios (*side effects*) son cualquier cambio que una función realiza en el estado del programa o en el entorno externo que se extienda más allá de su alcance local. Por ejemplo, una función que modifica una variable global, escribe en un archivo o imprime en la consola tiene efectos secundarios. Estos pueden hacer que el código sea impredecible y difícil de depurar, ya que pueden afectar a otras partes del programa de formas ocultas. Los efectos secundarios son especialmente problemáticos cuando se trata de concurrencia y paralelismo, donde múltiples tareas interactúan con recursos compartidos.

La concurrencia y el paralelismo se refieren a diferentes formas de manejar múltiples tareas, y ambos pueden ocasionar problemas cuando las funciones comparten recursos como variables globales. Aquí tienes una descripción general rápida de cada concepto:

- **Concurrencia** significa que múltiples tareas están en progreso aproximadamente al mismo tiempo, pero no necesariamente se ejecutan simultáneamente. Por ejemplo, las funciones asíncronas permiten que las tareas se superpongan, incluso si no ocurren en el mismo instante exacto.
- **Paralelismo** se refiere a tareas que se ejecutan simultáneamente, a menudo a través de múltiples hilos o procesadores. JavaScript en sí es monohilo (*single-threaded*), pero el paralelismo se puede lograr utilizando Web Workers, que permiten que las tareas se ejecuten en hilos separados.

Ahora veamos ejemplos de ambos conceptos. Examinaremos un ejemplo de concurrencia y luego uno de paralelismo.

##### Ejemplo de concurrencia

Imagina que tenemos una variable global compartida, `currentUser`, y una función que recupera datos de usuario y los actualiza:

```typescript
let currentUser: string = 'Alice'; async function fetchUserData(userId: string) { const response = await fetch(`https://api.example.com/users/${userId}`); const data = await response.json(); currentUser = data.name; // This updates the global variable }
```

Ahora imagina que llamamos a esta función dos veces, casi inmediatamente una tras otra (de forma concurrente).

Simulación de dos llamadas concurrentes a la API:

```typescript
fetchUserData('1'); fetchUserData('2');
```

En este ejemplo, si la función `fetchUserData` se llama dos veces de forma concurrente para dos IDs de usuario diferentes, ambas operaciones de búsqueda podrían completarse en momentos diferentes. Esto podría hacer que `currentUser` se establezca de manera impredecible según la respuesta de la API que finalice en último lugar. El estado global (`currentUser`) puede terminar en un estado inconsistente, dependiendo del tiempo de estas operaciones asíncronas. Ahora echemos un vistazo al paralelismo.

##### Ejemplo de paralelismo

Imagina que necesitamos procesar un array grande de números duplicando cada valor. En un entorno paralelo, diferentes partes del array podrían procesarse simultáneamente. Si bien TypeScript (y JavaScript) son monohilo, el paralelismo se puede lograr utilizando Web Workers para realizar estos cálculos.

Para simplificar, supongamos que usamos un worker para procesar fragmentos del array en paralelo, en lugar de hacerlo en el hilo principal. En una configuración con dos workers, cada uno procesa la mitad del array de forma independiente, de modo que ambas partes se procesan exactamente al mismo tiempo.

##### Por qué esto importa

Los efectos secundarios combinados con la concurrencia y el paralelismo pueden generar múltiples problemas en el código, especialmente con dependencias del estado global. A continuación, se detallan las principales preocupaciones:

- **Modificación de variables globales**: Actualizar variables globales, como hace `fetchUserData` con `currentUser`, puede introducir efectos secundarios que afecten a otras operaciones. En escenarios concurrentes, dichos cambios pueden dar como resultado un estado de programa impredecible o inconsistente.
- **Falta de modularidad y reutilización**: Cuando las funciones dependen de variables globales, quedan estrechamente acopladas a un estado de programa específico, lo que las hace menos modulares y más difíciles de reutilizar en otros lugares. Esta dependencia del estado global limita la versatilidad de `fetchUserData`.
- **Dificultades en las pruebas**: Las funciones con dependencias globales son más difíciles de probar de forma aislada, ya que requieren condiciones de estado específicas. Probar `fetchUserData` implicaría controlar la variable global `currentUser`, lo que dificultaría probar diferentes escenarios de forma independiente.
- **Salida no confiable**: Los efectos secundarios pueden hacer que las funciones produzcan diferentes salidas para las mismas entradas, según el estado global actual. Esta falta de previsibilidad hace que la depuración sea un desafío, especialmente con la concurrencia.
- **Problemas de concurrencia**: En el código concurrente, pueden ocurrir condiciones de carrera (*race conditions*) si múltiples tareas asíncronas modifican el mismo estado global. Por ejemplo, si dos llamadas a `fetchUserData` se superponen, el resultado puede depender del tiempo de finalización de cada llamada. Esto puede provocar errores o datos inconsistentes.

Al comprender y gestionar estos problemas potenciales, puedes escribir código más fiable, fácil de mantener y fácil de probar en TypeScript.

Ahora que hemos explorado los problemas potenciales de los efectos secundarios, centrémonos en las estrategias para evitarlos. Al aplicar estas técnicas, puedes escribir código más fiable, mantenible y comprobable.

##### Cómo evitar los efectos secundarios

Ahora que entendemos los problemas que pueden introducir los efectos secundarios, exploremos formas de evitarlos en nuestro código. Al aplicar las mejores prácticas, podemos hacer que nuestras funciones sean más predecibles, más fáciles de probar y menos propensas a errores. Aquí tienes algunas estrategias:

- **Funciones puras**: Procura escribir funciones que no cambien ni dependan de estados externos, como variables globales. Estas funciones siempre producen la misma salida para las mismas entradas, lo que las hace fiables y sencillas de probar.
- **Mantener los datos inmutables**: Al manipular datos, considera crear nuevas versiones de objetos o arrays en lugar de modificar directamente los existentes. Esto garantiza que los datos originales permanezcan sin cambios, evitando efectos secundarios no deseados en otras partes de la aplicación que dependan de esos datos.
- **Gestión de estado local**: Para situaciones que requieran gestión de estado, explora librerías diseñadas para este propósito. Estas herramientas proporcionan formas controladas y predecibles de manejar los cambios de estado dentro de tu código.

Al seguir las mejores prácticas, como evitar efectos secundarios, usar funciones puras y adoptar la inmutabilidad, puedes escribir código TypeScript que no solo sea más limpio y fácil de mantener, sino también más fácil de probar. En la siguiente sección, profundizaremos en la importancia de las firmas de funciones en el código TypeScript y cómo ayudan a garantizar la seguridad de tipos y la confiabilidad del código.

---

### Sección 2: Comprensión de las firmas de funciones

La firma de una función define la estructura y la información de tipos de una función, incluidos sus parámetros (entradas) y el tipo de retorno. Sirve como modelo de cómo debe definirse e invocarse una función. Esto es lo que normalmente incluye la firma de una función:

- **Tipos de parámetros**: Los tipos de parámetros que la función espera recibir. Estos tipos definen los datos que se pueden pasar a la función cuando se llama.
- **Tipo de retorno**: El tipo de valor que se espera que devuelva la función después de su ejecución. Esto especifica el tipo de datos del valor que producirá la función como salida.
- **Excepciones**: En algunos casos, una función puede lanzar excepciones. La firma puede indicar qué excepciones podrían lanzarse o devolverse.

Veamos el siguiente ejemplo simple:

```typescript
function calculateCircleArea(radius: number): number { return Math.PI * radius * radius; }
```

Desglosemos los componentes del código anterior para comprenderlo mejor:

- `calculateCircleArea` es el nombre de la función.
- `(radius: number)` especifica un único parámetro llamado `radius` de tipo `number`.
- `: number` indica que la función devuelve un valor de tipo `number`.

Las firmas de funciones son esenciales para la comprobación de tipos y para garantizar la seguridad de tipos en TypeScript. Proporcionan documentación clara de la interfaz de una función, facilitando la comprensión de cómo utilizarla correctamente. Además, las firmas de funciones permiten que el sistema de inferencia de tipos de TypeScript deduzca los tipos de los parámetros de la función y de los valores de retorno, lo que ayuda a detectar errores de forma temprana durante el desarrollo.

Ahora que dominamos la importancia de las firmas de funciones para garantizar la seguridad de tipos, estamos listos para llevar la documentación de nuestro código al siguiente nivel con **TypeDoc**. Esta potente herramienta nos ayuda a generar documentación completa y precisa para nuestros proyectos de TypeScript, facilitando que otros (¡y nosotros mismos!) comprendan nuestro código. Con TypeDoc y TSDoc, podemos crear documentación clara y concisa que ahorra tiempo y reduce confusiones.

#### Integración de TypeDoc para documentación completa en TypeScript

Como desarrolladores, no nos limitamos a escribir código para máquinas; creamos soluciones para otros desarrolladores. Un código claro, conciso y bien estructurado es crucial, pero igualmente importante es la documentación. Es la narrativa que acompaña a nuestro código, proporcionando un contexto esencial a quienes vienen después de nosotros. Escribir código limpio no se trata solo de estructura; se trata de una comunicación eficaz.

En el pasado, los desarrolladores dependían de herramientas como JSDoc para anotar su código con comentarios. Aunque resultaba útil, JSDoc tenía sus limitaciones, especialmente al trabajar con proyectos de TypeScript. Aquí entra **TSDoc**: la solución moderna para documentar código TypeScript. Cuando se combina con **TypeDoc**, se convierten en un dúo formidable para generar documentación completa sin esfuerzo.

En esta sección, demostraremos cómo usar TSDoc y TypeDoc para documentar tu código TypeScript. Exploremos cómo aplicar estas herramientas en un escenario del mundo real. Imagina construir una plataforma de comercio electrónico y necesitar una clase `ShoppingCart` robusta con documentación clara para asegurar que otros desarrolladores puedan usarla eficazmente. Definiremos la clase `ShoppingCart`, incluidas sus propiedades y métodos, y recorreremos el proceso de anotarla y documentarla usando TypeDoc. Este ejemplo te mostrará cómo aprovechar las características de TypeDoc para generar documentación completa para tu código TypeScript, mejorando la mantenibilidad y la colaboración.

Aquí está el código TypeScript para nuestro ejemplo de `ShoppingCart`:

```typescript
interface CartItem { price: number; quantity: number; } class ShoppingCart { private cartItems: CartItem[] = []; addItem(item: CartItem): void { this.cartItems.push(item); } calculateTotalPrice(): number { let total = 0; for (const item of this.cartItems) { total += item.price * item.quantity; } return total; } applyDiscount(discount: number): number { const total = this.calculateTotalPrice(); return total * (1 - discount / 100); } checkout(discount: number): number { const total = this.applyDiscount(discount); console.log("Your total is: $" + total.toFixed(2)); console.log("Thank you for shopping with us!"); return total; } }
```

El fragmento de código anterior servirá como nuestro punto de partida para explorar las anotaciones y la documentación de TypeDoc.

El código no está mal, pero podemos mejorar aún más la claridad anotándolo con TypeDoc. Por ejemplo, podríamos proporcionar aún más contexto para el método `applyDiscount`. ¿Qué representa `discount`? ¿Es un porcentaje o una cantidad fija? Al anotar nuestro código, podemos responder a estas preguntas directamente en el código fuente, facilitando que otros comprendan y utilicen nuestras funciones.

Si copias el código tal como está ahora y lo pegas en tu editor, notarás que al pasar el cursor sobre uno de los métodos se revela información limitada. Aunque TypeScript proporciona algunas sugerencias, como indicar que es un método y mostrar su tipo de retorno, carece de información detallada.

Consulta la siguiente captura de pantalla para mayor claridad:

*Figura 2.1 — Mostrando la retroalimentación de TypeScript al pasar el cursor sobre el método applyDiscount*

En el siguiente paso, comenzamos anotando el código con TSDoc.

Los comentarios de TSDoc comienzan con un comentario multilínea regular como se muestra aquí:

```typescript
/* ... */
```

Dentro del bloque de comentarios, puedes agregar etiquetas de TSDoc para documentar varios aspectos de tu código. Las etiquetas básicas incluyen las siguientes:

- **@param**: Describe un parámetro de una función o método. Aquí tienes un ejemplo:

```typescript
/** * Concatenates two strings. * @param {string} firstName - The first name. * @param {string} lastName - The last name. */ function getFullName(firstName: string, lastName: string): string { return `${firstName} ${lastName}`; }
```

- **@returns**: Describe el valor de retorno de una función o método. Aquí tienes un ejemplo:

```typescript
/** * @returns {Promise<User>} A promise that resolves to the user data. */ async function getUserData(userId: number): Promise<User> { const response = await fetch(`/api/users/${userId}`); return response.json(); }
```

- **@remarks**: Se utiliza para proporcionar observaciones o explicaciones adicionales sobre un método, clase u otro elemento de código. Es útil para compartir detalles de implementación, decisiones de diseño o cualquier otra información relevante. Consulta el siguiente ejemplo:

```typescript
/** * Converts a temperature from Celsius to Fahrenheit. * @remarks The formula used for conversion is (Celsius * 9/5) + 32. */ function convertCelsiusToFahrenheit(celsius: number): number { return (celsius * 9) / 5 + 32; }
```

- **@deprecated**: Indica que un método o clase ya no se recomienda para su uso. También proporciona información sobre enfoques alternativos o reemplazos y ayuda a los usuarios a realizar la transición a APIs más nuevas. Consulta el siguiente ejemplo:

```typescript
/** * Calculates the area of a rectangle. * @deprecated Use the `calculateRectangleArea` function instead. */ function getArea(width: number, height: number): number { return width * height; }
```

- **@link**: Crea hipervínculos a recursos externos o documentación relacionada. Es útil para hacer referencia a especificaciones, clases relacionadas o sitios web pertinentes. Aquí tienes un ejemplo: `{@link Title}`

Ahora, con esto en mente, recorramos el proceso de anotar nuestra clase `ShoppingCart` paso a paso:

##### 1. Anotando la interfaz CartItem

```typescript
/** * Interface representing an item in the shopping cart. */ interface CartItem { price: number; // The price of the item. quantity: number; // The quantity of the item. }
```

En el fragmento de código anterior, estamos anotando la interfaz `CartItem`. Un comentario de documentación (`/** ... */`) describe el propósito de la interfaz. Cada propiedad (`price` y `quantity`) tiene su tipo (`number`) y una breve descripción.

##### 2. Anotando la clase ShoppingCart

A continuación, anotamos la clase `ShoppingCart`. Hacemos uso de la etiqueta `@remark` para proporcionar información adicional sobre la clase:

```typescript
/** * Manages a shopping cart. * @remark The ShoppingCart class provides methods for adding items, calculating total prices, * and applying discounts. It is designed to be extensible and easy to use. */ class ShoppingCart { private cartItems: CartItem[] = []; // Array to store cart items. // ... rest of the class methods }
```

##### 3. Anotando el método addItem

Anotamos el método `addItem` con una descripción y una etiqueta `@param` para describir el parámetro que acepta. En este caso, indicamos que el parámetro `item` debe ajustarse a la interfaz `CartItem`:

```typescript
/** * Adds an item to the shopping cart. * * @param item The item to add (must conform to the CartItem interface). */ addItem(item: CartItem): void { this.cartItems.push(item); }
```

##### 4. Anotando el método calculateTotalPrice

Esta anotación es similar a la anterior; la diferencia clave es el uso de la etiqueta `@returns`, que se utiliza para describir el valor de retorno de una función o método:

```typescript
/** * Calculates the total price of all items in the cart. * @returns The total price. */ calculateTotalPrice(): number { let total = 0; for (const item of this.cartItems) { total += item.price * item.quantity; } return total; }
```

##### 5. Anotando el método applyDiscount

En el siguiente fragmento, anotamos el método `applyDiscount` con una descripción: una etiqueta `@param` para describir su parámetro (`discount`). Especificamos que el descuento es un porcentaje y brindamos información sobre cómo se realiza el cálculo. También usamos una etiqueta `@returns` para describir su valor de retorno:

```typescript
/** * Applies a discount to the total price. * @param discount The discount percentage (e.g., 10 for 10% off). * @returns The discounted total price. */ applyDiscount(discount: number): number { const total = this.calculateTotalPrice(); return total * (1 - discount / 100); }
```

##### 6. Anotando el método checkout

Finalmente, anotamos el método `checkout` con una descripción, una etiqueta `@param` para describir su parámetro y una etiqueta `@returns` para describir su valor de retorno:

```typescript
/** * Completes the checkout process. * @param discount The discount percentage to apply. * @returns The final total after applying the discount. */ checkout(discount: number): number { const total = this.applyDiscount(discount); console.log('Your total is: $' + total.toFixed(2)); console.log('Thank you for shopping with us!'); return total; }
```

¡Enhorabuena! Al seguir los pasos anteriores, hemos anotado con éxito nuestra clase `ShoppingCart` utilizando TSDoc. Ahora veamos la diferencia que esto supone. Como se muestra en la siguiente captura de pantalla, cuando paso el cursor sobre mi clase `ShoppingCart`, obtengo muchos más detalles. De un vistazo, puedo saber qué hace la clase `ShoppingCart` y hacerme una idea de qué métodos proporciona:

*Figura 2.2 — El carrito de compras muestra información sobre lo que hace la clase al pasar el cursor*

Los pasos que hemos implementado aquí brindan ventajas significativas. A medida que una base de código se expande, es fácil perder la pista de la funcionalidad de cada componente. Esto puede resultar especialmente abrumador para los recién llegados al proyecto. Sin embargo, al anotar nuestro código como lo hemos hecho, creamos una estructura autodocumentada y navegable que facilita la comprensión y agiliza el proceso de incorporación (*onboarding*).

Echemos también otro vistazo al método `applyDiscount` del que hablamos anteriormente. Cuando pasamos el cursor sobre el método `applyDiscount` (ver la siguiente captura de pantalla), observa lo que sucede:

*Figura 2.3 — Podemos saber con solo pasar el cursor sobre la función lo que representa discount*

Como se aprecia en la figura anterior, ahora podemos comprender mejor qué hace el método `applyDiscount` y qué representa el parámetro `discount`, gracias a las anotaciones de TSDoc. Esta claridad nos ayuda a comprender cómo se aplica el descuento.

Ahora que dominamos el arte de anotar nuestro código con TSDoc, ampliaremos aún más este conocimiento en la siguiente sección. Continuaremos usando el mismo ejemplo e integrándolo con TypeDoc. Esto nos permitirá generar páginas estáticas, transformando esencialmente nuestra base de código en una página web fácilmente navegable que sirva como documentación completa.

#### Creación de páginas de documentación estáticas con la integración de TypeDoc

En la sección anterior, añadimos comentarios a nuestro código utilizando TSDoc. Estos comentarios actúan como la base de la documentación, describiendo lo que hace nuestro código. Ahora, construyamos sobre esa base introduciendo TypeDoc. TypeDoc es un popular generador de documentación. Toma los comentarios que añadimos en la sección anterior (como los de la clase `ShoppingCart`) y los transforma en páginas web estáticas y fáciles de usar. En esencia, TypeDoc crea un sitio web de documentación en línea para tu base de código.

Esta sección te guiará a través de un proceso paso a paso para mostrarte cómo usar TypeDoc para lograr esto.

Hagamos precisamente eso:

1. Primero, instalaremos TypeDoc mediante el siguiente comando:

```bash
npm install --save-dev typedoc
```

2. A continuación, crearemos un archivo `typedoc.json` en la raíz de nuestro directorio y añadiremos el siguiente código:

```json
{ "theme": "default", "exclude": "node_modules/**" }
```

El archivo `typedoc.json` es donde mantenemos todas las configuraciones relacionadas con TypeDoc en nuestro proyecto. En este archivo, hemos agregado solo una configuración mínima por el momento. Repasemos qué hace cada línea:

- `theme: "default"`: Establece el tema de la documentación en el diseño estándar proporcionado por TypeDoc, ofreciendo una visualización limpia de la información.
- `exclude: "node_modules/**"`: Indica a TypeDoc que omita el procesamiento de archivos dentro del directorio `node_modules`, que normalmente contiene dependencias externas e información irrelevante para la documentación.

3. Ahora añadiremos un comando para activar el proceso de documentación. Agregaremos el siguiente script a nuestro archivo `package.json`:

```json
"scripts": { "docs": "npx typedoc ./index.ts" },
```

Desglosemos el comando anterior:

- `npx`: Esta es una utilidad que viene con npm. Te permite ejecutar paquetes instalados localmente en tu proyecto sin instalarlos globalmente.
- `typedoc`: Este es el comando real proporcionado por el paquete TypeDoc.
- `./index.ts`: Especifica el punto de entrada para generar la documentación. En tu caso, apunta al archivo `index.ts` de tu proyecto.

4. Ahora podemos ejecutar el script utilizando el siguiente comando:

```bash
npm run docs
```

El comando anterior debería generar una carpeta `docs` en tu directorio raíz. Consulta la siguiente figura para obtener más detalles:

*Figura 2.4 — Captura de pantalla que muestra una vista general del resultado tras ejecutar el script para generar documentación*

La figura anterior muestra el directorio `docs` generado en la raíz de nuestro proyecto. Este directorio alberga los archivos estáticos esenciales que componen nuestra documentación basada en la web. Ahora exploremos esta documentación accediendo a ella y viendo cómo aparece en un navegador web, lo que nos dará una vista de primera mano de la interfaz intuitiva que TypeDoc ha creado para nuestra base de código.

Arrastra el archivo `index.html` a tu navegador y deberías ver algo como esto:

*Figura 2.5 — Una vista general de index.html generado por TypeDoc*

Verás una descripción general de las clases/funciones/interfaces que has agregado, las cuales ahora están anotadas. Cuando haces clic en el carrito de compras, navegas a una página que contiene detalles de la clase `ShoppingCart`:

*Figura 2.6 — Una vista general de la clase ShoppingCart*

Puedes desplazarte hacia abajo para ver más detalles sobre la clase `ShoppingCart` o ver detalles sobre un método en particular. Puedes hacer clic en un método; por ejemplo, si hacemos clic en el método `applyDiscount`, deberíamos ver los siguientes detalles:

*Figura 2.7 — Una vista general del método applyDiscount*

Ahora que hemos aprendido a usar los comentarios de TSDoc y TypeDoc para mejorar la claridad de nuestro código y generar documentación fácil de usar, es esencial establecer algunas pautas para comentar nuestro código de manera efectiva. En la siguiente sección, exploraremos la importancia de lograr un equilibrio entre el uso de comentarios y la escritura de código limpio y legible.

#### Equilibrio entre comentarios y código limpio

Si bien el uso de comentarios puede mejorar enormemente tu código, es importante tener en cuenta que los comentarios y la documentación no son sustitutos de un código bueno y limpio. De hecho, algunos sostienen que el código bien escrito requiere menos comentarios. Aunque no existen reglas estrictas, adherirse a los principios discutidos en este capítulo —como el SRP, evitar efectos secundarios y usar un nombrado adecuado— te coloca en el camino correcto.

Un consejo útil que aprendí cuando comencé a programar y a explorar los principios del código limpio involucra lo que me gusta llamar la **"Prueba del Extraño"** (*Stranger Test*). Esencialmente, deberías preguntarte: si alguien que no supiera programar se sentara frente a tu computadora y mirara tu archivo, ¿podría adivinar a grandes rasgos qué hace ese código? Por ejemplo, una clase llamada `ShoppingCart` proporciona mucho más contexto que simplemente `Cart`. Esta prueba es una excelente manera de asegurarte de que tu código sea fácilmente comprensible, incluso para un extraño.

---

### Sección: Resumen

En este capítulo, profundizamos en los principios fundamentales para escribir funciones limpias. Exploramos la importancia de comprender las firmas de funciones y cómo contribuyen a la legibilidad y mantenibilidad del código. Además, enfatizamos la importancia de utilizar nombres de funciones claros y descriptivos, junto con una documentación adecuada, para mejorar la comprensión del código y la colaboración dentro de un equipo. Al dominar estos conceptos, los desarrolladores pueden producir código que no solo sea más fácil de entender, sino también más robusto y escalable.

De cara al futuro, en el próximo capítulo realizaremos la transición hacia la exploración de los conceptos de la **Programación Orientada a Objetos (POO)** con TypeScript. Nos adentraremos en la definición de clases, constructores y herencia, además de sumergirnos en interfaces, clases abstractas y el principio de composición sobre herencia. Estos temas proporcionarán una base sólida para construir estructuras de software complejas y flexibles en TypeScript.
