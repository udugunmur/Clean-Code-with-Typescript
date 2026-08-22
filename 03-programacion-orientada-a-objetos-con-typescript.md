# Parte 1: Fundamentos de TypeScript

## Capítulo 3: Programación Orientada a Objetos con TypeScript

### Sección: Introducción

Dominar la **Programación Orientada a Objetos (POO)** es clave para crear aplicaciones robustas. TypeScript, con su tipado estático y características de POO, proporciona una forma moderna de implementar estos principios. En este capítulo, nos sumergiremos en la POO con TypeScript, explicando conceptos fundamentales y prácticas avanzadas para un diseño de software eficaz.

Exploraremos clases, herencia, interfaces y clases abstractas para comprender cómo estructurar el código en favor de la reutilización, la flexibilidad y la simplicidad. También analizaremos la composición sobre la herencia, un principio de diseño para lograr bases de código más manejables. A través de ejemplos claros, aprenderás cómo y por qué utilizar estas técnicas para aplicarlas con confianza en tus proyectos.

Este capítulo cubre los siguientes temas principales:

- Comprensión del concepto de POO
- Clases, herencia y cadenas de prototipos para diseños robustos y flexibles
- Encapsulamiento para garantizar la privacidad de los datos y el acceso controlado
- Polimorfismo en la POO con TypeScript
- Interfaces en POO: definiendo contratos y asegurando la coherencia
- Composición sobre herencia para el desarrollo ágil

Al finalizar, tendrás una base sólida en POO con TypeScript, lo que te preparará para diseñar e implementar código eficiente y escalable. Las prácticas y principios analizados aquí serán invaluables para perfeccionar proyectos existentes o comenzar nuevos proyectos en el campo en constante cambio de la ingeniería de software.

---

### Sección: Requisitos técnicos

La configuración y los requisitos técnicos para este capítulo son similares a los de los capítulos anteriores. Tener instalados TypeScript y Node.js será suficiente para comenzar.

Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones de la sección *Download the example code files* en el Prefacio de este libro.

Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

---

### Sección 1: ¿Qué es la programación orientada a objetos?

En esta sección, comprenderemos qué es la POO y construiremos una base sobre cómo se puede aplicar.

Una definición común de la POO es que se trata de un paradigma de programación. Un paradigma de programación es un conjunto de principios que definen cómo se escriben, estructuran y ejecutan los programas. Puedes pensar en él como un enfoque o filosofía general para organizar el código.

Desde el punto de vista de una clasificación ampliamente aceptada, la mayoría de los paradigmas de programación se dividen en dos categorías amplias: **imperativo** y **declarativo**.

En la **programación imperativa**, describes cómo un programa debe realizar sus tareas, paso a paso, cambiando explícitamente el estado del programa a lo largo del tiempo. Tanto la programación procedimental como la programación orientada a objetos pertenecen a esta categoría. La programación procedimental organiza el código en funciones o rutinas, mientras que la POO organiza el código en objetos que combinan datos y comportamiento.

En la **programación declarativa**, describes qué debe lograr el programa en lugar de cómo hacerlo. El sistema subyacente determina los detalles de ejecución. Los ejemplos incluyen consultas SQL y estilos de programación funcional.

La POO, como paradigma imperativo, fomenta el modelado de software como una colección de objetos interactivos. Cada objeto representa un concepto del mundo real, encapsulando estado (propiedades) y comportamiento (métodos). Este enfoque ayuda a que los sistemas complejos sean más fáciles de entender, extender y mantener mediante una definición clara de responsabilidades e interacciones.

Con esta base establecida, a continuación revisaremos los objetos y las clases en TypeScript, cómo definirlos, trabajar con sus propiedades y utilizar sus métodos en la práctica.

#### Objetos en TypeScript

Los objetos en TypeScript son colecciones de pares clave-valor que proporcionan una forma estructurada de representar datos y comportamientos. Son bloques de construcción fundamentales tanto en JavaScript como en TypeScript, permitiéndote agrupar información relacionada y crear estructuras de datos más complejas.

En TypeScript, los objetos pueden representar entidades del mundo real o conceptos abstractos, facilitando el modelado de datos de una manera que se alinee con las necesidades de tu aplicación. Al agrupar propiedades y métodos relacionados dentro de un objeto, puedes encapsular la funcionalidad y mantener una estructura de código clara y organizada.

#### Literales de objetos

Un literal de objeto es la forma más sencilla y directa de crear un objeto en TypeScript. Es un método para definir un objeto enumerando sus propiedades y sus valores correspondientes entre llaves (`{}`). Este enfoque se denomina literal porque estás definiendo literalmente el objeto en el lugar, sin la necesidad de una función constructora o una clase.

##### Ejemplo: el objeto pastel (*cake object*)

En la Figura 3.1, hemos creado un objeto pastel de muestra utilizando un literal de objeto; examinémoslo:

*Figura 3.1 — Un objeto pastel mostrando pares clave-valor*

En el objeto pastel, `flavor`, `size` e `icing` son claves, y sus valores correspondientes son `'chocolate'`, `'medium'` y `'vanilla'`, respectivamente. Este ejemplo ilustra cómo los literales de objeto se pueden usar para representar una entidad del mundo real —en este caso, un pastel— agrupando propiedades relacionadas en una sola unidad cohesiva.

Al comprender los literales de objeto, puedes comenzar a crear y manipular objetos de manera eficiente en TypeScript, sentando las bases para conceptos más avanzados como clases, interfaces y POO.

En la siguiente sección, exploraremos las clases de TypeScript y aprenderemos a usarlas para crear objetos.

#### Clases en TypeScript

En la sección anterior, vimos cómo crear objetos usando literales de objeto. En JavaScript moderno (ES6 en adelante) y, por extensión, en TypeScript, las clases proporcionan una forma más estructurada de crear objetos e implementar principios de POO.

Definimos las clases como un modelo o plantilla (*blueprint*) para crear objetos. Definen las propiedades y métodos que compartirán todos los objetos de la clase dada. Esto promueve la reutilización del código y la coherencia, haciendo que tu código sea más fácil de entender y mantener.

Aquí tienes una guía paso a paso sobre cómo crear una clase en TypeScript:

1. Para crear una clase en TypeScript, usas la palabra clave `class` seguida del nombre de la clase y llaves `{}`, de la siguiente manera:

```typescript
class Cake { 
  // Define properties and methods here 
}
```

Esto crea una plantilla para objetos `Cake`.

2. Dentro de las llaves de la clase, puedes definir propiedades que representan los datos que contendrá un objeto de esa clase. Podemos definir propiedades para `flavor`, `size` e `icing`:

```typescript
class Cake {
  flavor: string;
  size: string;
  icing: string;
}
```

3. Ahora, definamos métodos para hornear, decorar y servir el pastel:

```typescript
class Cake {
  flavor: string;
  size: string;
  icing: string;
 
  bake() {
    console.log("Cake is baking in the oven!");
  }
 
  decorate() {
    console.log("Adding frosting and sprinkles!");
  }
 
  serve() {
    console.log("Slicing and serving the delicious cake!");
  }
}
```

Estos métodos definen las acciones que se pueden realizar sobre un objeto `Cake`. A continuación, agreguemos un constructor para inicializar las propiedades del pastel al crear una instancia.

Sin inicializar estas propiedades, el compilador de TypeScript producirá un error similar al siguiente:

```text
Property 'flavor' has no initializer and is not definitely assigned in the constructor.
```

Para resolver esto, definiremos un constructor que tome argumentos para `flavor`, `size` e `icing`, y asigne estos valores a las propiedades correspondientes de la clase:

```typescript
class Cake {
  flavor: string;
 
  size: string;
 
  icing: string;
 
  constructor(flavor: string, size: string, icing: string) {
    this.flavor = flavor;
 
    this.size = size;
 
    this.icing = icing;
  }
 
  bake() {
    console.log('Cake is baking in the oven!');
  }
 
  decorate() {
    console.log('Adding frosting and sprinkles!');
  }
 
  serve() {
    console.log('Slicing and serving the delicious cake!');
  }
}
```

En el código anterior, hemos añadido un constructor.

4. A continuación, podemos usar esta clase para crear nuevos objetos (instancias). Para hacer esto, usamos la palabra clave `new` para instanciar objetos `Cake`:

```typescript
const chocolateCake = new Cake("chocolate", "medium", "chocolate"); 
const redVelvetCake = new Cake("red velvet", "large", "cream cheese");6.
```

5. Podemos llamar a los métodos en los objetos `Cake` que acabamos de crear:

```typescript
chocolateCake.bake(); // Output: "Cake is baking in the oven!" 
redVelvetCake.decorate(); // Output: "Adding frosting and sprinkles!" 
chocolateCake.serve(); // Output: "Slicing and serving the delicious cake!"
```

Ahora sabemos cómo crear una clase en TypeScript y cómo podemos crear instancias de ella (objetos), permitiendo la separación de responsabilidades y la reutilización de código. A continuación, aprendamos sobre métodos y propiedades estáticas en TypeScript.

#### Métodos y propiedades estáticas en TypeScript

En la sección anterior, cuando creamos nuestra clase `Cake`, pasamos valores para las propiedades en los puntos en los que inicializábamos los objetos de la clase. Estas propiedades, como `flavor`, `size` e `icing`, son únicas para cada objeto. Pero hay ocasiones en las que deseas propiedades y métodos que sean peculiares de la clase en sí (padre) y no de las instancias (objetos creados a partir de la clase).

Aquí es donde entran en juego las propiedades y métodos estáticos. Los miembros estáticos pertenecen a la propia clase `Cake`. Puedes acceder a ellos directamente usando el nombre de la clase (por ejemplo, `Cake`), sin necesidad de crear un objeto primero. En el siguiente ejemplo, extenderemos nuestra clase `Cake` para usar algunas propiedades y métodos estáticos:

```typescript
class Cake {
  flavor: string;
  size: string;
  icing: string;
 
  // Static property
  static totalCakesBaked: number = 0;
 
  constructor(flavor: string, size: string, icing: string) {
    this.flavor = flavor;
    this.size = size;
    this.icing = icing;
    Cake.numberOfCakes++; // Increment the number of cakes each time a new cake is created
  }

  // Static method
  static getTotalCakesBaked () {
    console.log(`Baked ${this.numberOfCakes} cakes in total.`);
  }
 
  // the other methods and properties…
}
```

En este ejemplo, `totalCakesBaked` es una propiedad estática que realiza un seguimiento del número total de objetos `Cake` creados. Cada vez que se instancia un objeto `Cake`, `totalCakesBaked` se incrementa en 1 en el constructor (`totalCakesBaked++`).

El método estático `getTotalCakesBaked` se puede llamar directamente en la clase `Cake` para registrar el recuento acumulado de pasteles horneados. Ten en cuenta que dentro de los métodos estáticos, `this` se refiere a la clase en sí y no a una instancia de la clase. Esto significa que se puede acceder a las propiedades estáticas usando `this` dentro de los métodos estáticos.

Puedes llamar al método estático de la siguiente manera:

```typescript
Cake.getTotalCakesBaked();
```

Los métodos y propiedades estáticas son herramientas poderosas para definir comportamientos y datos que pertenecen a la clase misma, no a instancias individuales. Son especialmente útiles para funciones de utilidad, contadores o cualquier estado o comportamiento compartido. Al comprender y utilizar miembros estáticos de manera efectiva, puedes escribir código más estructurado y fácil de mantener.

En esta sección, hemos revisado cómo crear e instanciar clases en TypeScript, incluida la definición de propiedades y métodos. También hemos explorado propiedades y métodos estáticos. Con esta base en objetos, clases y propiedades de TypeScript, es momento de profundizar en algunos conceptos fundamentales de la POO.

Antes de pasar a la siguiente sección, donde exploraremos la herencia y la cadena de prototipos, un concepto clave en la POO de TypeScript, tomémonos un momento para revisar qué sucede entre bastidores cuando definimos una clase en JavaScript o TypeScript.

#### Clases en TypeScript: entre bastidores

Habiendo aprendido la sintaxis para crear clases en TypeScript, es crucial mirar detrás de escena para comprender la mecánica subyacente. Definimos una clase así:

```typescript
class MyClass {}
```

Luego, TypeScript/JavaScript realiza algunas transformaciones y lo convierte en algo como esto:

```javascript
function MyClass() {}
```

Esta función se conoce como constructor. Para ilustrarlo, creemos un archivo `dummy-class.ts`. En este archivo, definiremos una clase simple `Dog` con un método, `bark`:

```typescript
class Dog {
  bark() {
    console.log('Woof!');
  }
}
```

Cuando ejecutamos el comando `npx tsc dummy-class.ts`, TypeScript genera la versión de JavaScript del código, que podemos verificar en `dummy-class.js`.

Aquí está el código JavaScript generado:

```javascript
var Dog = /** @class */ (function () {
    function Dog() {
    }
    Dog.prototype.bark = function () {
        console.log('Woof!');
    };
    return Dog;
}());
```

Desglosemos el código generado:

- **Expresión de Función Invocada Inmediatamente (IIFE)**:

```javascript
var Dog = /** @class */ (function () {
```

Esta parte define una IIFE. Una IIFE es una función que se ejecuta tan pronto como se define. El propósito aquí es crear un ámbito privado para la clase `Dog`.

- **Función constructora**:

```javascript
function Dog() {
}
```

Dentro de la IIFE, se define la función `Dog`. Esta función actúa como el constructor para la clase `Dog`. Cuando creas una nueva instancia de `Dog` usando `new Dog()`, se llama a esta función para inicializar el objeto.

- **Método de prototipo**:

```javascript
Dog.prototype.bark = function () {
        console.log('Woof!');
    };
```

El método `bark` se define en el prototipo del constructor `Dog`. Esto significa que todas las instancias de `Dog` compartirán el mismo método `bark`. Cuando llamas a `dogInstance.bark()`, busca el método en la cadena de prototipos y encuentra esta definición.

- **Retorno del constructor**:

```javascript
return Dog;
}());
```

La IIFE devuelve la función constructora `Dog`. Luego, la función `Dog` se asigna a la variable `Dog`, haciéndola accesible en el ámbito externo.

La palabra clave `class` en TypeScript es azúcar sintáctico (*syntactic sugar*). Proporciona una forma más intuitiva de definir plantillas de objetos en comparación con las funciones puras de JavaScript. Esto hace que tu código sea más legible y fácil de mantener, especialmente para aquellos familiarizados con los conceptos de POO.

Ahora que entendemos el "secreto" detrás de las clases, estamos bien equipados para explorar la herencia y la cadena de prototipos en la siguiente sección. Estos conceptos son fundamentales para la programación orientada a objetos en TypeScript.

---

### Sección 2: Herencia y cadenas de prototipos

La herencia es una piedra angular de la POO, que te permite crear nuevas clases (subclases) que heredan propiedades y comportamientos de clases existentes (superclases). Esto promueve la reutilización del código y reduce la redundancia en tus aplicaciones TypeScript.

TypeScript, al ser un superconjunto de JavaScript, utiliza la herencia prototípica entre bastidores. La herencia prototípica difiere del modelo de herencia clásica utilizado en lenguajes como Java o C++. En la herencia prototípica, los objetos heredan directamente de otros objetos. Sin embargo, TypeScript proporciona una sintaxis basada en clases que abstrae la naturaleza prototípica de la herencia, haciéndola más familiar para los desarrolladores que provienen de lenguajes de POO clásica. Exploremos estos dos modelos para comprender mejor sus diferencias.

#### Herencia clásica

La herencia clásica se utiliza en lenguajes como Java y C++. Aquí, la jerarquía de herencia es más rígida. Una subclase hereda de una superclase y tiene un lugar fijo en el árbol de herencia. En la herencia clásica, tenemos lo siguiente:

- **Subclase y superclase**: Una subclase es una versión especializada de su superclase, heredando todas sus propiedades y comportamientos.
- **Jerarquía**: La estructura de herencia es una jerarquía fija, lo que significa que las clases se definen y las relaciones se establecen en el momento de la creación de la clase.

Por ejemplo, en Java, podrías definir clases como esta:

```java
class Animal {
  void makeSound() {
    System.out.println("The animal makes a sound");
  }
}
 
class Dog extends Animal {
  void makeSound() {
    System.out.println("The dog barks");
  }
}
```

En el ejemplo anterior, `Dog` es una subclase de `Animal` y sobrescribe el método `makeSound`.

#### Herencia prototípica

La herencia prototípica es un concepto del entorno de ejecución (*runtime*) de JavaScript. Debido a que TypeScript se compila a JavaScript y no introduce un nuevo runtime o modelo de herencia, toda la herencia en TypeScript se basa en última instancia en la herencia prototípica de JavaScript.

Este modelo de herencia es más flexible y dinámico. En la herencia prototípica, tenemos lo siguiente:

- **Herencia directa**: Los objetos heredan directamente de otros objetos.
- **Cadena de prototipos**: Los objetos tienen un prototipo, que es otro objeto del cual heredan propiedades. Esto crea una cadena de herencia.
- **Relaciones dinámicas**: Las relaciones entre objetos se pueden cambiar dinámicamente en tiempo de ejecución.

En TypeScript, podrías definir clases de la siguiente manera:

```typescript
class Animal {
  makeSound() {
    console.log('The animal makes a sound');
  }
}
 
class Dog extends Animal {
  makeSound() {
    console.log('The dog barks');
  }
}
```

Bajo el capó, TypeScript transforma estas definiciones de clase en funciones y prototipos de JavaScript, habilitando la herencia prototípica:

```javascript
function Animal() {}
Animal.prototype.makeSound = function () {
  console.log('The animal makes a sound');
};
 
function Dog() {}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.makeSound = function () {
  console.log('The dog barks');
};
```

En este ejemplo, el prototipo de la función constructora `Dog` se establece en un objeto creado a partir de `Animal.prototype`, estableciendo la cadena de prototipos. El método `makeSound` en `Dog.prototype` sobrescribe el de `Animal.prototype`.

#### Diferencias clave

Al comparar la herencia clásica con la herencia prototípica, existen varias distinciones importantes a tener en cuenta. A continuación, resumimos las diferencias clave entre estos dos modelos de herencia:

- **Jerarquía**:
  - *Herencia clásica*: Jerarquía fija y rígida. Las clases se definen una vez y sus relaciones no cambian.
  - *Herencia prototípica*: Flexible y dinámica. Los objetos pueden heredar de otros objetos y estas relaciones pueden cambiar en tiempo de ejecución.
- **Modelo de herencia**:
  - *Herencia clásica*: Clases y subclases.
  - *Herencia prototípica*: Objetos y prototipos.
- **Sobrescritura de métodos**:
  - *Herencia clásica*: Los métodos de la subclase sobrescriben los métodos de la superclase.
  - *Herencia prototípica*: Los métodos en el prototipo de un objeto hijo sobrescriben los métodos en el prototipo de un objeto padre.

Comprender las diferencias entre la herencia clásica y la prototípica ayuda a aprovechar eficazmente la herencia en tus aplicaciones TypeScript. Si bien las clases de TypeScript proporcionan una sintaxis familiar para aquellos con antecedentes de herencia clásica, es crucial recordar que en última instancia se compilan en el modelo de herencia prototípica de JavaScript. Este conocimiento te permite escribir código más flexible, dinámico y fácil de mantener. Por lo tanto, en la siguiente subsección, examinaremos los prototipos en TypeScript.

#### Comprensión de los prototipos en TypeScript

Para entender los prototipos en TypeScript, resulta útil comprender primero cómo funcionan en JavaScript. TypeScript se ejecuta sobre JavaScript y utiliza el modelo de objetos de JavaScript en tiempo de ejecución, por lo que la forma en que se comportan los prototipos es la misma que en JavaScript.

En JavaScript, todo es un objeto, incluidas las funciones. Cada objeto tiene una referencia interna `[[Prototype]]` que apunta a otro objeto, llamado su prototipo. Ese prototipo puede tener a su vez otro prototipo, formando lo que se conoce como la cadena de prototipos (*prototype chain*).

Cuando accedes a una propiedad en un objeto, JavaScript busca primero esa propiedad en el objeto mismo. Si no la encuentra allí, continúa buscando hacia arriba en la cadena de prototipos hasta que encuentra la propiedad o llega al final de la cadena.

TypeScript trabaja con este mismo sistema de prototipos. Añade información de tipos sobre el comportamiento de JavaScript, ayudándote a comprender y validar cómo se relacionan los objetos entre sí en tiempo de desarrollo, mientras que la búsqueda real de propiedades y el comportamiento de herencia son gestionados por JavaScript en tiempo de ejecución.

Veamos un ejemplo en las siguientes capturas de pantalla para más detalles:

*Figura 3.2: La propiedad [[Prototype]] de los objetos en la consola del navegador*

En la Figura 3.2, hemos creado un objeto pastel simple en nuestra consola del navegador. Si registras la variable `cake` en la consola, verás los detalles del objeto. Al mirar de cerca, puedes ver la propiedad `[[Prototype]]` del objeto. Cada objeto que creas tiene esta propiedad. Al hacer clic en la flecha, puedes ver más detalles, como se muestra en la Figura 3.3:

*Figura 3.3 — Mostrando los detalles de la propiedad [[Prototype]] de un objeto*

Si te fijas, incluso la propiedad `[[Prototype]]` tiene su propio `__proto__`. Si haces clic en la flecha de esa propiedad `__proto__` para ver más detalles, encuentras que también tiene un `__proto__`, que esta vez es `null`. Para mayor claridad, consulta la Figura 3.4:

*Figura 3.4 — Mostrando la cadena de prototipos de un objeto hasta null*

Ahora que hemos aprendido sobre la cadena de prototipos, veamos cómo podemos configurarla nosotros mismos.

#### Configuración de la cadena de prototipos

En esta sección, veremos las formas en que puedes configurar la cadena de prototipos en TypeScript.

##### Uso de Object.create para configurar la cadena de prototipos

`Object.create()` es un método utilizado para crear un nuevo objeto y establecer su prototipo en un objeto existente. Esto puede resultar útil cuando deseas crear un objeto con un prototipo específico. Por ejemplo, `let obj = Object.create(protoObj);` crea un nuevo objeto, `obj`, con su prototipo establecido en `protoObj`.

Veamos el siguiente ejemplo para más detalles:

```typescript
const cakePrototype = {
  bake() {
    console.log('The cake is baking.');
  },
};
 
const chocolateCake = Object.create(cakePrototype);
chocolateCake.flavor = 'Chocolate';
chocolateCake.bake();
```

En este ejemplo, `chocolateCake` es un objeto que hereda de `cakePrototype`. Esto significa que `chocolateCake` puede usar el método `bake` definido en `cakePrototype`, demostrando la herencia prototípica básica.

##### Herencia de clases ES6 con extends y super

Con la introducción de ES6, TypeScript proporcionó una sintaxis más familiar para la herencia usando clases, que se alinea más estrechamente con los patrones de programación clásica.

En ES6, podemos crear una clase y usar la palabra clave `extends` para crear una subclase. La subclase hereda todos los métodos y propiedades de la superclase. La palabra clave `super` se utiliza para llamar al constructor de la superclase y acceder a las propiedades y métodos de la misma. Aquí tienes un ejemplo:

```typescript
// Define the Oven class with a bake() method
class Oven {
  bake() {
    console.log('The oven is baking.');
  }
}
 
// Define the Cake class that extends Oven
class Cake extends Oven {
  flavor: string;
  size: string;
  icing: string;
 
  // Constructor for the Cake class
  constructor(flavor: string, size: string, icing: string) {
    super(); // Calls the constructor of the Oven class
    this.flavor = flavor;
    this.size = size;
    this.icing = icing;
  }
 
  // Additional methods specific to Cake
  decorate() {
    console.log(
      `Decorating the ${this.size} ${this.flavor} cake with ${this.icing} icing.`
    );
  }
 
  serve() {
    console.log(`Serving the delicious ${this.flavor} cake!`);
  }
}
 
// Example usage
const chocolateCake = new Cake('chocolate', 'large', 'chocolate ganache');
chocolateCake.bake(); // Inherited method from Oven
chocolateCake.decorate();
chocolateCake.serve();
```

Resumamos el código anterior para hacerlo más claro:

1. Definimos una nueva clase, `Oven`, con un método `bake` que simula la funcionalidad de horneado.
2. La clase `Cake` ahora hereda de `Oven` mediante el uso de `extends`.
3. Dentro del constructor de la clase `Cake`, llamamos a `super()` para garantizar que la inicialización adecuada se herede de `Oven`.
4. Definimos propiedades para `flavor`, `size` e `icing` específicas de los objetos `Cake`.
5. Agregamos los métodos `decorate()` y `serve()` a la clase `Cake`, demostrando acciones típicas que podrían realizarse en un pastel después de hornearlo.
6. Al crear un objeto `Cake` (por ejemplo, `chocolateCake`), este hereda el método `bake` de `Oven` y puede usar sus propios métodos, `decorate()` y `serve()`.

La configuración anterior utiliza la herencia de clases para modelar una relación del mundo real en la que un pastel es un tipo específico de producto horneado que utiliza un horno para hornearse, demostrando cómo un enfoque orientado a objetos puede modelar eficazmente jerarquías y comportamientos del mundo real en el diseño de software.

En esta sección, hemos examinado la herencia y las cadenas de prototipos. Hemos aprendido sobre los prototipos en TypeScript y cómo forman la columna vertebral del modelo de objetos de TypeScript. Hemos visto la herencia basada en prototipos, una forma de crear objetos que heredan propiedades y métodos de otros objetos. Hemos visto cómo usar `Object.create` para configurar la cadena de prototipos, un mecanismo para que los objetos hereden características unos de otros. Y hemos revisado la herencia de clases de ES6 con las palabras clave `extends` y `super`, que permiten una sintaxis más intuitiva para trabajar con prototipos y herencia.

En la siguiente sección, hablaremos sobre el encapsulamiento.

---

### Sección 3: Encapsulamiento: protegiendo la receta de tu pastel

El encapsulamiento es un principio fundamental de la POO que implica agrupar datos (propiedades) y los métodos (operaciones) que actúan sobre esos datos dentro de una sola unidad, típicamente una clase. Este concepto garantiza que la representación interna de un objeto quede oculta del exterior, permitiendo el acceso únicamente a través de una interfaz bien definida.

Piensa en el encapsulamiento como la protección de la receta secreta de tu pastel. Del mismo modo que no querrías que nadie altere la masa de tu pastel sin permiso, en programación, deseas asegurarte de que ciertos datos solo sean accesibles para partes específicas de tu código. Esto promueve la privacidad de los datos y el acceso controlado, asegurando la integridad y el correcto funcionamiento de tus objetos.

En esta sección, exploraremos la importancia de la privacidad de los datos y cómo el encapsulamiento ayuda a lograrla. Profundizaremos en implementaciones prácticas, incluidos los *getters* y *setters*, que actúan como guardianes de los datos de tu clase. Al comprender y aplicar el encapsulamiento, podrás crear código más seguro y fácil de mantener.

#### Getters y setters: los guardianes de tus datos

Los *getters* y *setters* son métodos especiales que proporcionan un acceso controlado a las propiedades de una clase.

- **Getters**: Estos métodos te permiten recuperar el valor de una propiedad de manera controlada. Potencialmente puedes realizar lógica adicional antes de devolver el valor, como formateo o validación.
- **Setters**: Estos métodos te permiten modificar el valor de una propiedad de manera controlada. Puedes utilizarlos para aplicar reglas específicas o validación de datos antes de asignar un nuevo valor.

Así es como puedes usar *getters* y *setters* en la clase `Cake`:

```typescript
class Cake {
  constructor(flavor, size, icing) {
    this._flavor = flavor;
    this._size = size;
    this._icing = icing;
  }
  get flavor() {
    // Getter for flavor property
    return this._flavor.toUpperCase();
  }
  set flavor(newFlavor) {
    // Setter for flavor property
    if (newFlavor.length < 3) {
      throw new Error('Flavor must be at least 3 characters long!');
    }
    this._flavor = newFlavor;
  }
 
  // other methods(decorate, serve etc).
}
const chocolateCake  = new Cake("chocolate", "medium", "vanilla");
console.log(chocolateCake .flavor); // Output: "CHOCOLATE"
 
chocolateCake.flavor = "ic"; // Error: Flavor must be at least 3 characters!
```

A continuación se muestra un desglose de lo que hicimos en el ejemplo anterior:

- Hicimos que la propiedad `_flavor` fuera privada utilizando la convención del prefijo de guion bajo (`_`).
- El *getter* `flavor` devuelve el sabor en mayúsculas.
- El *setter* `flavor` valida la longitud del nuevo sabor antes de asignarlo.

En conclusión, los *getters* y *setters* son herramientas poderosas en TypeScript que te permiten controlar cómo se accede y se modifican tus datos. Proporcionan una capa de abstracción sobre tus datos, permitiéndote aplicar reglas específicas y ejecutar lógica adicional. En nuestro ejemplo de la clase `Cake`, usamos un *getter* para formatear la propiedad `flavor` y un *setter* para validar la longitud de un nuevo sabor antes de asignarlo. Esto garantiza que nuestros objetos `Cake` siempre tengan datos válidos y formateados correctamente.

En la siguiente subsección, examinaremos los modificadores de acceso, otro elemento que forma parte del encapsulamiento.

#### Modificadores de acceso: controlando el acceso a tus datos

Los modificadores de acceso en TypeScript son palabras clave que determinan la visibilidad y accesibilidad de los miembros de una clase (propiedades y métodos). Ayudan a aplicar el encapsulamiento al restringir el acceso a ciertas partes de tu código:

- **public**: El modificador predeterminado. Los miembros con `public` se pueden acceder desde cualquier lugar, tanto dentro como fuera de la clase.
- **private**: Los miembros con `private` solo se pueden acceder dentro de la propia clase. Esto evita que el código externo modifique directamente estos miembros.
- **protected**: Los miembros con `protected` se pueden acceder dentro de la clase y sus subclases. Esto permite la herencia mientras sigue restringiendo el acceso externo.

Aquí tienes un ejemplo de cómo se utilizarían los modificadores de acceso, siguiendo con el ejemplo del pastel:

```typescript
class Cake { 
  private _flavor: string;
  public size: string;
  protected icing: string;
  
  constructor(flavor: string, size: string, icing: string) { 
    this._flavor = flavor; 
    this.size = size; 
    this.icing = icing; 
  } 
  
  get flavor() { 
    return this._flavor.toUpperCase(); 
  } 
  
  set flavor(newFlavor: string) { 
    if (newFlavor.length < 3) { 
      throw new Error('Flavor must be at least 3 characters long!'); 
    } 
    this._flavor = newFlavor; 
  } 
}
```

En el código anterior, hicimos que la propiedad `_flavor` fuera privada, lo que significa que no se puede acceder directamente desde fuera de la clase. La propiedad `size` es pública, por lo que se puede acceder desde cualquier lugar. La propiedad `icing` está protegida, lo que significa que se puede acceder dentro de la clase `Cake` y en cualquier subclase que extienda `Cake`.

Ahora veamos qué sucede cuando intentamos acceder a las propiedades:

```typescript
const chocolateCake = new Cake('chocolate', 'medium', 'vanilla');
console.log(chocolateCake.size); // Accessible
console.log(chocolateCake.flavor); // Accessible
// chocolateCake.icing = 'strawberry'; // Error: 'icing' is protected
// chocolateCake._flavor = 'strawberry'; // Error: '_flavor' is private
```

En conclusión, los modificadores de acceso son herramientas esenciales en TypeScript para controlar la visibilidad y accesibilidad de los miembros de la clase, mejorando el encapsulamiento y manteniendo la integridad del código.

En la siguiente sección, profundizaremos en el concepto de polimorfismo. Este es otro concepto fundamental en POO que permite que los objetos adopten muchas formas, mejorando aún más la flexibilidad y la reutilización de nuestro código.

---

### Sección 4: Polimorfismo en la POO con TypeScript

El polimorfismo, que significa "muchas formas", permite que objetos de diferentes clases respondan a la misma llamada de método de maneras únicas, haciendo que el código sea más flexible y adaptable. Por ejemplo, un método `draw()` definido en una clase base `Shape` puede ser personalizado por subclases como `Circle`, `Rectangle` y `Triangle`. Llamar a `draw()` en cada objeto ejecutará la versión apropiada para su tipo, permitiendo diferentes comportamientos a partir de una única llamada a método.

Existen dos formas principales de lograr el polimorfismo en TypeScript: **sobrescritura de métodos (*method overriding*)** e **interfaces**. Veamos estos dos enfoques con más detalle.

#### Sobrescritura de métodos (*Method overriding*)

La sobrescritura de métodos es un aspecto clave del polimorfismo en TypeScript. Te permite definir un método en una clase base y luego sobrescribirlo en clases hijas para proporcionar una implementación más específica. Esto te permite escribir código más genérico que puede funcionar con diferentes clases.

Para implementar la sobrescritura de métodos en TypeScript, sigue estos pasos:

1. Define una clase base con un método que tenga una implementación general.
2. Crea clases hijas que hereden de la clase base.
3. Sobrescribe el método en la clase hija para proporcionar una implementación más específica.

Veamos el ejemplo:

```typescript
class PaymentMethod {
  processPayment(amount: number) {
    // This method will be overridden in each subclass
    console.log(`Processing default payment: ${amount}`)
  }
}
 
class CreditCard extends PaymentMethod {
  processPayment(amount: number) {
    console.log(`Processing credit card payment for amount: ${amount}`);
  }
}
 
class DebitCard extends PaymentMethod {
  processPayment(amount: number) {
    console.log(`Processing debit card payment for amount: ${amount}`);
  }
}
 
let paymentMethods: PaymentMethod[] = [new CreditCard(), new DebitCard()];
 
paymentMethods.forEach((paymentMethod) => paymentMethod.processPayment(100));
```

En este ejemplo, cuando llamamos a `processPayment` en cada elemento del array `paymentMethods`, se ejecuta el método sobrescrito en cada subclase (`CreditCard` y `DebitCard`) en lugar del método de la clase base. Esto demuestra el polimorfismo en acción, permitiendo que cada subclase responda de manera diferente a la misma llamada de método.

Ahora veamos el mismo ejemplo, pero con interfaces.

#### Interfaces

Las interfaces en TypeScript son una forma muy potente de definir contratos dentro de tu código. Ofrecen una manera de definir un tipo mediante la forma (*shape*) de los datos. De este modo, las interfaces nos brindan la oportunidad de interactuar con varias clases de la misma manera siempre que implementen la misma interfaz.

Veamos cómo podemos implementar el ejemplo de la sección anterior usando interfaces:

```typescript
interface PaymentMethod {
processPayment(amount: number): void;
}

class CreditCard implements PaymentMethod {
processPayment(amount: number) {
console.log(`Processing credit card payment for amount: ${amount}`);
}
}

class DebitCard implements PaymentMethod {
processPayment(amount: number) {
console.log(`Processing debit card payment for amount: ${amount}`);
}
}

let paymentMethods: PaymentMethod[] = [new CreditCard(), new DebitCard()];

paymentMethods.forEach((paymentMethod) => paymentMethod.processPayment(100));
```

En este ejemplo, `CreditCard` y `DebitCard` son clases separadas que implementan la interfaz `PaymentMethod`. No están relacionadas por herencia, pero pueden usarse indistintamente porque implementan la misma interfaz.

#### Interfaces frente a clases: cuándo usar cada una

Llegados a este punto, has visto cómo tanto las interfaces como las clases habilitan el polimorfismo en TypeScript. La pregunta importante ahora no es qué son, sino cuándo usar una sobre la otra.

Una **interfaz** es más adecuada cuando deseas definir un contrato, una forma común o una API que diferentes implementaciones puedan seguir. Las interfaces funcionan especialmente bien cuando múltiples clases no relacionadas necesitan exponer el mismo comportamiento. Debido a que las interfaces solo existen en tiempo de compilación, proporcionan flexibilidad sin introducir acoplamiento en tiempo de ejecución.

Utiliza una interfaz cuando desees:

- Definir una forma común o API
- Permitir que múltiples clases no relacionadas sigan el mismo contrato
- Habilitar el polimorfismo sin forzar la herencia
- Mantener las implementaciones débilmente acopladas y fáciles de reemplazar

Una **clase**, por otro lado, es apropiada cuando necesitas tanto estructura como comportamiento. Las clases te permiten compartir detalles de implementación, definir constructores y usar herencia y sobrescritura de métodos. También existen en tiempo de ejecución, lo que las hace adecuadas cuando los objetos necesitan ser instanciados y portar comportamiento.

Utiliza una clase en las siguientes situaciones:

- Necesitas comportamiento compartido o implementación reutilizable
- Deseas usar herencia y sobrescritura de métodos
- Requieres características de tiempo de ejecución como constructores o estado interno
- Estás creando objetos que serán instanciados directamente

En la práctica, un patrón común y eficaz en TypeScript es utilizar interfaces para definir el comportamiento y clases para implementar dicho comportamiento. Este enfoque mantiene tu código flexible, comprobable y más fácil de mantener a medida que crece tu aplicación.

Con esta comprensión del polimorfismo y la abstracción, ahora estamos listos para explorar otra decisión de diseño clave: cuándo usar composición y cuándo tiene sentido la herencia.

---

### Sección 5: Composición sobre herencia para el desarrollo ágil

La herencia y la composición son conceptos fundamentales en POO que definen relaciones entre clases. La herencia establece una jerarquía "padre-hijo", donde la clase hija hereda propiedades y comportamientos del padre. La composición, por otro lado, describe cómo se construye un objeto a partir de otros objetos.

La herencia es útil cuando existe una relación clara de tipo "es-un" (*is-a*) entre dos clases. Por ejemplo, un perro es un tipo de animal. Comparten características y comportamientos comunes (como respirar y moverse) que se pueden definir en la clase `Animal` y ser heredados por `Dog`. Esto promueve la reutilización del código y reduce la redundancia.

Revisemos el siguiente código para más detalles:

```typescript
class Animal {
  breathe() {
    console.log('Breathing');
  }
}
 
class Dog extends Animal {
  bark() {
    console.log('Barking');
  }
}
 
let dog = new Dog();
dog.breathe(); // Outputs: 'Breathing'
dog.bark(); // Outputs: 'Barking'
```

En el ejemplo anterior, `Dog` hereda de `Animal`, adquiriendo su comportamiento y agregando comportamientos específicos propios, como ladrar.

La composición, por otro lado, es útil cuando un objeto está compuesto por otros objetos. Por ejemplo, una banda musical está compuesta por un cantante, un guitarrista y un baterista. En este caso, tiene más sentido usar composición, donde la banda "tiene-un" (*has-a*) cantante, un guitarrista y un baterista.

Como regla general, **preferimos la composición sobre la herencia**, ya que es más flexible. Conduce a una arquitectura débilmente acoplada alineada con el principio de responsabilidad única.

Consulta el siguiente ejemplo para más detalles:

```typescript
class Band {
  singer: Singer;
  guitarist: Guitarist;
  drummer: Drummer;
 
  constructor() {
    this.singer = new Singer();
    this.guitarist = new Guitarist();
    this.drummer = new Drummer();
  }
 
  perform() {
    this.singer.sing();
    this.guitarist.playGuitar();
    this.drummer.playDrums();
  }
}
 
let band = new Band();
band.perform(); // Outputs: 'Singer is singing', 'Guitarist is playing guitar', 'Drummer is playing drums'
```

En el ejemplo que acabamos de considerar, `Band` no hereda funcionalidades de `Singer`, `Guitarist` o `Drummer`. En su lugar, `Band` tiene un `Singer`, un `Guitarist` y un `Drummer`. Compone estos objetos y les delega funcionalidades específicas (cantar, tocar la guitarra, tocar la batería). Esto permite una mayor flexibilidad.

Puedes cambiar fácilmente la composición de la banda: tal vez tenga un corista de respaldo o un teclista. La composición permite esta flexibilidad sin modificar la funcionalidad central de `Band`. Cada objeto se puede extender o modificar de forma independiente sin afectar a los demás.

Al comprender y aplicar estos conceptos, puedes escribir código TypeScript más flexible y fácil de mantener. Recuerda: si bien la herencia puede ser útil en algunos casos, la composición a menudo puede ser un enfoque más potente y flexible.

---

### Sección: Resumen

Este capítulo proporcionó una comprensión profunda de los conceptos de POO aplicados en JavaScript moderno y TypeScript. Comenzamos con una introducción a la POO, su importancia y su comparación con otros paradigmas. Luego exploramos objetos y métodos, seguido de una inmersión profunda en las clases, incluida su creación, instanciación y características clave como constructores, la palabra clave `this` y propiedades estáticas. A lo largo del capítulo, examinamos cómo TypeScript amplía el modelo de POO de JavaScript agregando seguridad de tipos y mejores herramientas para desarrolladores.

El capítulo también cubrió la herencia y las cadenas de prototipos, el encapsulamiento para la privacidad de los datos y el polimorfismo. Concluimos con una discusión sobre composición frente a herencia, proporcionando orientación sobre cuándo usar cada una y los beneficios de la composición. El conocimiento adquirido permitirá escribir código TypeScript más limpio y fácil de mantener.

En el próximo capítulo, aprovecharemos el conocimiento acumulado hasta ahora. Nuestro enfoque estará en su aplicación práctica dentro del marco de un proyecto de TypeScript. Nos embarcaremos en la construcción de un proyecto de muestra, lo que nos brindará una oportunidad práctica para implementar lo aprendido. Esto implicará organizar y modularizar el código y garantizar la adhesión a los principios que hemos discutido previamente. Este capítulo promete ser un viaje enriquecedor desde la comprensión teórica hasta la implementación práctica.
