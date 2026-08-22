# Parte 3: TypeScript Avanzado y Aplicaciones en el Mundo Real

## Capítulo 9: Comprensión de Características Avanzadas de TypeScript

### Sección: Introducción

En este capítulo, exploraremos algunas de las características más complejas de TypeScript. Estas características te ayudan a escribir un código mejor, más fácil de mantener, depurar y comprender. Desglosaremos estos conceptos en términos sencillos con ejemplos prácticos para garantizar su total claridad.

TypeScript ofrece numerosas herramientas y opciones que te permiten crear aplicaciones flexibles y potentes. Al dominar estas funciones avanzadas, adquirirás las habilidades necesarias para abordar con confianza proyectos más grandes y desafiantes, mejorando al mismo tiempo tu experiencia general en el desarrollo de software.

En este capítulo, cubriremos los siguientes temas principales:

- Explorando los genéricos
- Introducción a los tipos avanzados
- Comprensión de los decoradores
- Creación de tipos mapeados
- Uso de tipos condicionales
- Trabajo con tipos de utilidad

Al final de este capítulo, tendrás una sólida comprensión de estas características avanzadas de TypeScript y de cómo aplicarlas eficazmente en proyectos del mundo real. Estas habilidades te prepararán para crear aplicaciones escalables, robustas y de alta calidad con total soltura.

---

### Sección: Requisitos técnicos

Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones en la sección *Download the example code files* en el Prefacio de este libro.

Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

---

### Sección 1: Explorando los genéricos

Los genéricos son una característica poderosa de TypeScript que te permite crear componentes flexibles y reutilizables. Te permiten escribir funciones, clases e interfaces capaces de trabajar con diferentes tipos de datos manteniendo la seguridad de tipos (*type safety*). En otras palabras, no tienes que escribir el mismo código varias veces para distintos tipos.

Los genéricos son especialmente útiles cuando no conoces el tipo exacto de datos que manejará tu función o clase, pero aun así deseas garantizar la seguridad de tipos en tiempo de compilación.

Razones clave para utilizar genéricos:

- **Reutilización**: Te permiten escribir código una sola vez y usarlo para cualquier tipo.
- **Seguridad de tipos**: Garantizan el uso de los tipos correctos, reduciendo drásticamente los errores en tiempo de ejecución.
- **Flexibilidad**: Se adaptan a diferentes tipos sin sacrificar la claridad del código.

#### Genéricos en acción

Los genéricos se definen utilizando corchetes angulares (`<>`) y un tipo marcador de posición, como `T`. La `T` representa un parámetro de tipo, que se reemplazará con un tipo concreto cuando se utilice el código.

Ejemplo de una función genérica:

```typescript
function wrapInArray<T>(value: T): T[] {
  return [value];
}
 
// Using the generic function
const numberArray = wrapInArray(42); // T is inferred as number
const stringArray = wrapInArray("Hello"); // T is inferred as string
```

En este ejemplo, la función `wrapInArray` puede funcionar con cualquier tipo (`number`, `string`, etc.), y TypeScript infiere el tipo de `T` a partir del argumento proporcionado.

#### Clases genéricas

Los genéricos también se pueden utilizar en clases para que funcionen con diferentes tipos. Esto es especialmente útil cuando necesitas crear un contenedor que almacene un conjunto de elementos sin duplicar la lógica para cada tipo de dato.

Ejemplo de una clase genérica `Storage`:

```typescript
class Storage<T> {
  private items: T[] = [];
 
  addItem(item: T): void {
    this.items.push(item);
  }
 
  getItems(): T[] {
    return this.items;
  }
}
 
// Using the generic class
const stringStorage = new Storage<string>();
stringStorage.addItem("Item 1");
stringStorage.addItem("Item 2");
 
const numberStorage = new Storage<number>();
numberStorage.addItem(100);
numberStorage.addItem(200);
```

#### Interfaces genéricas

Podemos aplicar genéricos a interfaces para hacerlas más flexibles al definir la estructura de un objeto cuyos tipos internos puedan variar:

```typescript
interface Pair<T, U> {
  first: T;
  second: U;
}
 
const coordinate: Pair<number, number> = { first: 10, second: 20 };
const nameAndAge: Pair<string, number> = { first: "Alice", second: 30 };
```

#### Restricciones genéricas (*Generic constraints*)

Puedes restringir los tipos que un genérico puede aceptar utilizando la palabra clave `extends`. Esto es necesario cuando tu código necesita acceder a propiedades específicas del tipo genérico (por ejemplo, asegurar que el tipo tenga una propiedad `.length`):

```typescript
function logLength<T extends { length: number }>(item: T): void {
  console.log(item.length);
}
 
// Valid usage
logLength("Hello"); // T is string
logLength([1, 2, 3]); // T is array
 
// Invalid usage
// logLength(42); // Error: number does not have a length property
```

#### Ventajas de los genéricos

- Permiten la parametrización de tipos preservando las relaciones entre entradas y salidas.
- Evitan la duplicación de código sin recurrir a `any` o abstracciones débiles.
- Proporcionan flexibilidad con máxima precisión, habilitando el autocompletado y el refactor seguro del IDE.

---

### Sección 2: Introducción a los tipos avanzados

Los tipos avanzados en TypeScript te permiten definir estructuras de datos complejas combinando y manipulando tipos existentes.

#### Tipos de unión (*Union types*)

Permiten que una variable contenga múltiples tipos posibles usando el símbolo de barra vertical (`|`):

```typescript
function formatValue(value: string | number): string {
  return `Value: ${value}`;
}
 
// Usage
console.log(formatValue("Hello")); // Works with string
console.log(formatValue(42)); // Works with number
```

#### Tipos de intersección (*Intersection types*)

Combinan múltiples tipos en uno solo usando el símbolo de ampersand (`&`):

```typescript
interface Person {
  name: string;
}
 
interface Employee {
  id: number;
}
 
type EmployeeDetails = Person&Employee
 
const employee: EmployeeDetails = {
  name: "Alice",
  id: 101,
};
```

#### Alias de tipo (*Type aliases*)

Permiten asignar un nombre personalizado a un tipo para simplificar estructuras complejas:

```typescript
type Coordinate = { x: number; y: number };
 
const point: Coordinate = { x: 10, y: 20 };
```

#### Tipos literales (*Literal types*)

Especifican valores exactos que una variable puede tener:

```typescript
type Direction = "up" | "down" | "left" | "right";
 
function move(direction: Direction): void {
  console.log(`Moving ${direction}`);
}
 
// Usage
move("up"); // Valid
// move("forward"); // Error: "forward" is not assignable to type "Direction"
```

#### Tipos anulables (*Nullable types*)

Manejan valores que pueden ser `null` o `undefined`:

```typescript
function greet(name: string | null): string {
  return name ? `Hello, ${name}` : "Hello, stranger";
}
 
// Usage
console.log(greet("Alice")); // Hello, Alice
console.log(greet(null)); // Hello, stranger
```

#### Tipos mapeados (*Mapped types*)

Transforman tipos existentes aplicando operaciones sobre sus propiedades:

```typescript
type ReadonlyType<T> = {
  readonly [K in keyof T]: T[K];
};
 
interface Person {
  name: string;
  age: number;
}
 
const person: ReadonlyType<Person> = {
  name: "John",
  age: 30,
};
 
// person.name = "Doe"; // Error: Cannot assign to 'name' because it is a read-only property
```

#### Firmas de índice (*Index signatures*)

Definen la forma de objetos con claves dinámicas:

```typescript
interface StringDictionary {
  [key: string]: string;
}
 
const dictionary: StringDictionary = {
  hello: "world",
  goodbye: "everyone",
};
```

#### Tipos condicionales (*Conditional types*)

Permiten seleccionar tipos basados en condiciones:

```typescript
type IsString<T> = T extends string ? true : false;
 
type Test1 = IsString<string>; // true
type Test2 = IsString<number>; // false
```

---

### Sección 3: Comprensión de los decoradores

Los decoradores en TypeScript son funciones especiales que permiten modificar o extender el comportamiento de clases, métodos, propiedades o parámetros mediante anotaciones (`@`). Son ampliamente utilizados en frameworks como Angular y NestJS para inyección de dependencias, metadatos y enrutamiento.

#### Configuración de decoradores

Para habilitar decoradores en tu proyecto TypeScript, debes activar el flag `experimentalDecorators` en `tsconfig.json`:

```json
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```

#### Tipos de decoradores

##### Decoradores de clase (*Class decorators*)

Modifican o añaden funcionalidad a toda una clase:

```typescript
function LogClass(constructor: Function) {
  console.log(`Class ${constructor.name} has been created.`);
}
 
@LogClass
class User {
  constructor(public name: string) {}
}
 
// Output: Class User has been created.
```

##### Decoradores de método (*Method decorators*)

Se aplican a un método específico para alterar su comportamiento o interceptar llamadas:

```typescript
function LogMethod(target: Object, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Method ${propertyKey} called with args: ${args}`);
    return originalMethod.apply(this, args);
  };
}
 
class Calculator {
  @LogMethod
  add(a: number, b: number): number {
    return a + b;
  }
}
 
const calc = new Calculator();
console.log(calc.add(2, 3)); 
// Output: Method add called with args: 2,3
//         5
```

##### Decoradores de propiedad (*Property decorators*)

Se utilizan para añadir metadatos o funcionalidad a una propiedad de clase:

```typescript
function LogProperty(target: Object, propertyKey: string) {
  console.log(`Property ${propertyKey} was accessed.`);
}
 
class User {
  @LogProperty
  name: string = "John Doe";
}
 
// Output: Property name was accessed.
```

##### Decoradores de parámetro (*Parameter decorators*)

Se aplican a los parámetros de un método para registrar metadatos:

```typescript
function LogParameter(target: Object, propertyKey: string, parameterIndex: number) {
  console.log(`Parameter in ${propertyKey} at index ${parameterIndex} was accessed.`);
}
 
class Greeter {
  greet(@LogParameter message: string) {
    console.log(message);
  }
}
 
const greeter = new Greeter();
greeter.greet("Hello!");
// Output: Parameter in greet at index 0 was accessed.
//         Hello!
```

#### Beneficios de los decoradores

- **Reutilización de código**: La lógica transversal se abstrae en decoradores reutilizables.
- **Separación de responsabilidades**: Mantiene la lógica de negocio separada de tareas secundarias como logging, validación o autorización.
- **Legibilidad**: Proporcionan una sintaxis declarativa y limpia.

---

### Sección 4: Creación de tipos mapeados

Los tipos mapeados permiten crear nuevos tipos transformando las propiedades de tipos existentes mediante la iteración sobre sus claves con el operador `keyof`.

Estructura básica:

```typescript
type MappedType<T> = {
  [Key in keyof T]: Transformation;
};
```

Donde:
- `T` es el tipo de entrada.
- `keyof T` obtiene todas las claves del tipo `T`.
- `Key in keyof T` itera sobre cada propiedad de `T`.
- `Transformation` define el nuevo tipo o modificador para cada propiedad.

#### Ejemplos de tipos mapeados

##### Hacer todas las propiedades opcionales

```typescript
type MakeOptional<T> = {
  [Key in keyof T]?: T[Key];
};
 
// Example
type User = {
  name: string;
  age: number;
};
 
type OptionalUser = MakeOptional<User>;
/* 
OptionalUser is:
{
  name?: string;
  age?: number;
}
*/
```

##### Hacer todas las propiedades de solo lectura

```typescript
type MakeReadOnly<T> = {
  readonly [Key in keyof T]: T[Key];
};
 
// Example
type User = {
  name: string;
  age: number;
};
 
type ReadOnlyUser = MakeReadOnly<User>;
/*
ReadOnlyUser is:
{
  readonly name: string;
  readonly age: number;
}
*/
```

##### Cambiar el tipo de las propiedades

```typescript
type ConvertToString<T> = {
  [Key in keyof T]: string;
};
 
// Example
type User = {
  name: string;
  age: number;
};
 
type StringifiedUser = ConvertToString<User>;
/*
StringifiedUser is:
{
  name: string;
  age: string;
}
*/
```

##### Mapeo con tipos condicionales

```typescript
type TransformNumbersToStrings<T> = {
  [Key in keyof T]: T[Key] extends number ? string : T[Key];
};
 
// Example
type User = {
  name: string;
  age: number;
  isActive: boolean;
};
 
type TransformedUser = TransformNumbersToStrings<User>;
/*
TransformedUser is:
{
  name: string;
  age: string; // Transformed because it's a number
  isActive: boolean;
}
*/
```

#### Tipos mapeados integrados en TypeScript

- `Partial<T>`: Hace opcionales todas las propiedades.
- `Required<T>`: Hace obligatorias todas las propiedades.
- `Readonly<T>`: Hace de solo lectura todas las propiedades.
- `Record<K, T>`: Crea un tipo con claves de tipo `K` y valores de tipo `T`.
- `Pick<T, K>`: Selecciona un subconjunto de propiedades `K` de `T`.
- `Omit<T, K>`: Omite un subconjunto de propiedades `K` de `T`.

---

### Sección 5: Uso de tipos condicionales

Los tipos condicionales eligen uno de dos tipos posibles en función de una prueba de relación de subtipos:

```typescript
T extends U ? X : Y;
```

Si `T` es asignable a `U`, el tipo resultante es `X`; de lo contrario, es `Y`.

#### Cómo funcionan los tipos condicionales

Ejemplo básico:
```typescript
type IsString<T> = T extends string ? "Yes, it's a string" : "No, it's not a string";
 
// Example usage
type Test1 = IsString<string>; // "Yes, it's a string"
type Test2 = IsString<number>; // "No, it's not a string"
```

Inferencia de tipos con `infer`:
```typescript
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
 
// Example usage
type MyFunction = () => number;
type Result = ReturnType<MyFunction>; // number
```

#### Casos de uso comunes

Filtrar tipos en uniones:
```typescript
type ExcludeType<T, U> = T extends U ? never : T;
// If T is assignable to U, remove it (never); otherwise keep T

// Example usage
type AllTypes = string | number | boolean;
type Result = ExcludeType<AllTypes, boolean>; // string | number
```

Manejo de tipos anulables (`NonNullable`):
```typescript
type NonNullable<T> = T extends null | undefined ? never : T;
 
// Example usage
type MaybeNullable = string | null | undefined;
type Result = NonNullable<MaybeNullable>; // string
```

Creación de utilidades flexibles:
```typescript
type IsArray<T> = T extends any[] ? true : false;
 
// Example usage
type Test1 = IsArray<number[]>; // true
type Test2 = IsArray<string>; // false
```

#### Combinación con otras características

Tipos condicionales con tipos mapeados:
```typescript
type OptionalIfString<T> = {
  [K in keyof T]: T[K] extends string ? T[K] | undefined : T[K];
};
 
// Example usage
type User = {
  name: string;
  age: number;
};
 
type ModifiedUser = OptionalIfString<User>;
/*
ModifiedUser is:
{
  name?: string; // because it's a string
  age: number;   // unchanged
}
*/
```

Tipos condicionales con literales de plantilla (*template literals*):
```typescript
type Status<T extends number> = T extends 200 ? "Success" : "Error";
 
// Example usage
type Result1 = Status<200>; // "Success"
type Result2 = Status<404>; // "Error"
```

#### Tipos condicionales integrados en TypeScript

- `Exclude<T, U>`: Elimina de `T` los tipos asignables a `U`.
- `Extract<T, U>`: Extrae de `T` los tipos asignables a `U`.
- `NonNullable<T>`: Elimina `null` y `undefined` de `T`.
- `ReturnType<T>`: Extrae el tipo de retorno de una función.
- `InstanceType<T>`: Extrae el tipo de instancia de una clase.

---

### Sección 6: Trabajo con tipos de utilidad

TypeScript proporciona utilidades globales predefinidas para manipular tipos de forma concisa sin escribir lógica personalizada desde cero.

#### Principales tipos de utilidad

##### `Partial<T>`
Convierte todas las propiedades en opcionales:

```typescript
type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

```typescript
interface User {
  name: string;
  age: number;
}
 
type PartialUser = Partial<User>;
 
// Now this is valid:
const user: PartialUser = {
  name: "Alice",
};
```

##### `Required<T>`
Convierte todas las propiedades en obligatorias:

```typescript
type Required<T> = {
  [P in keyof T]-?: T[P];
};
```

```typescript
interface User {
  name?: string;
  age?: number;
}
 
type FullUser = Required<User>;
 
// This will throw an error if `name` or `age` is missing:
const user: FullUser = {
  name: "Alice",
  age: 25,
};
```

##### `Readonly<T>`
Convierte todas las propiedades en solo lectura:

```typescript
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
```

```typescript
interface User {
  name: string;
  age: number;
}
 
type ReadonlyUser = Readonly<User>;
 
const user: ReadonlyUser = {
  name: "Alice",
  age: 25,
};
 
// This will throw an error:
user.name = "Bob";
```

##### `Pick<T, K>`
Construye un tipo seleccionando un subconjunto de propiedades `K`:

```typescript
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

```typescript
interface User {
  name: string;
  age: number;
  email: string;
}
 
type UserSummary = Pick<User, "name" | "email">;
 
const summary: UserSummary = {
  name: "Alice",
  email: "alice@example.com",
};
```

##### `Omit<T, K>`
Construye un tipo excluyendo un subconjunto de propiedades `K`:

```typescript
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
```

```typescript
interface User {
  name: string;
  age: number;
  email: string;
}
 
type UserWithoutEmail = Omit<User, "email">;
 
const user: UserWithoutEmail = {
  name: "Alice",
  age: 25,
};
```

##### `Record<K, T>`
Construye un tipo de objeto cuyas claves son `K` y cuyos valores son `T`:

```typescript
type Record<K extends keyof any, T> = {
  [P in K]: T;
};
```

```typescript
type Roles = "admin" | "user" | "guest";
 
type Permissions = Record<Roles, boolean>;
 
const permissions: Permissions = {
  admin: true,
  user: false,
  guest: false,
};
```

##### `Exclude<T, U>`
Resta tipos de una unión `T` que coincidan con `U`:

```typescript
type Exclude<T, U> = T extends U ? never : T;
```

```typescript
type AllRoles = "admin" | "user" | "guest";
type NonAdminRoles = Exclude<AllRoles, "admin">;
 
// NonAdminRoles = "user" | "guest"
```

##### `Extract<T, U>`
Conserva solo los tipos de una unión `T` que coincidan con `U`:

```typescript
type Extract<T, U> = T extends U ? T : never;
```

```typescript
type AllRoles = "admin" | "user" | "guest";
type AdminRole = Extract<AllRoles, "admin">;
 
// AdminRole = "admin"
```

##### `ReturnType<T>`
Extrae el tipo devuelto por una función:

```typescript
type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;
```

```typescript
function getUser() {
  return {
    name: "Alice",
    age: 25,
  };
}
 
type UserType = ReturnType<typeof getUser>;
 
// UserType = { name: string; age: number; }
```

#### Mejores prácticas con tipos de utilidad

- **Reutilización**: Evita duplicar transformaciones comunes y aprovecha las utilidades integradas.
- **Composición**: Combina múltiples tipos de utilidad para generar definiciones robustas.
- **Pruebas**: Verifica que los tipos transformados se comporten exactamente como se espera en casos extremos.

---

### Sección: Resumen

En este capítulo, exploramos conceptos avanzados de TypeScript como genéricos, tipos avanzados, decoradores, tipos mapeados, tipos condicionales y tipos de utilidad. Estas herramientas permiten a los desarrolladores escribir código más flexible, reutilizable y seguro frente a errores de tipos. Al dominar estas características, puedes gestionar proyectos complejos de TypeScript con total confianza y elevar la calidad arquitectónica de tus aplicaciones.

En el próximo capítulo, aprenderás cómo aplicar TypeScript en escenarios de desarrollo web del mundo real, cubriendo la integración con React, Node.js, Express, arquitecturas full stack y herramientas modernas como Webpack y Nx.
