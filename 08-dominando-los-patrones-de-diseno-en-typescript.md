# Parte 2: Pruebas y Calidad de Código

## Capítulo 8: Dominando los Patrones de Diseño en TypeScript

### Sección: Introducción

En este capítulo, exploraremos los patrones de diseño y cómo se pueden implementar eficazmente utilizando TypeScript. Los patrones de diseño son soluciones estándar a problemas comunes en el diseño de software. Ayudan a los desarrolladores a crear código más flexible, reutilizable y fácil de mantener. Al dominar estos patrones, mejorarás tu capacidad para resolver problemas de arquitectura y elevarás la calidad de tu código.

Comprender los patrones de diseño implica conocer su propósito, su estructura y cuándo utilizarlos. Este capítulo desglosará varios patrones de diseño en tres categorías principales: **creacionales**, **estructurales** y **de comportamiento**. Cada categoría aborda diferentes aspectos del diseño de software, lo que te facilitará aplicar el patrón correcto en la situación adecuada.

Proporcionaremos explicaciones claras de cada patrón, ejemplos prácticos en TypeScript y discusiones sobre las ventajas y desventajas de cada enfoque. También cubriremos las mejores prácticas y consejos esenciales para ayudarte a evitar errores comunes. De esta manera, podrás tomar decisiones fundamentadas al aplicar estos patrones en tus proyectos.

En este capítulo, cubriremos los siguientes temas principales:

- Introducción a los patrones de diseño
- Patrones creacionales: Técnicas para la creación de objetos
- Patrones estructurales: Técnicas para la composición de clases y objetos
- Patrones de comportamiento: Definiendo la interacción y comunicación entre objetos
- Ejemplos prácticos: Aplicando patrones de diseño en escenarios del mundo real
- Ventajas y desventajas de los patrones de diseño
- Mejores prácticas para implementar patrones de diseño en TypeScript

Al final de este capítulo, tendrás una sólida comprensión de cómo utilizar patrones de diseño en TypeScript para crear mejores soluciones de software.

---

### Sección: Requisitos técnicos

Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones en la sección *Download the example code files* en el Prefacio de este libro.

Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

---

### Sección 1: Introducción a los patrones de diseño

Los patrones de diseño son soluciones a problemas comunes que los desarrolladores de software enfrentan al diseñar aplicaciones. Piensa en un patrón de diseño como un plano que te ayuda a resolver un problema recurrente en tu código. No son fragmentos específicos de código para copiar y pegar; en su lugar, proporcionan un enfoque general para resolver problemas en el diseño de software.

La idea de los patrones de diseño comenzó en otros campos, como la arquitectura tradicional, donde se utilizaban para resolver problemas de construcción habituales. En el desarrollo de software, el concepto fue popularizado por un grupo de autores conocido como el *Gang of Four* (GoF). Escribieron un libro célebre en 1994 llamado *Design Patterns: Elements of Reusable Object-Oriented Software*, que introdujo 23 patrones de diseño diferentes.

Aprender sobre patrones de diseño es importante porque proporciona un lenguaje común entre desarrolladores. Si alguien dice: "Usemos el patrón Singleton aquí", otros desarrolladores saben exactamente lo que significa sin necesidad de una larga explicación.

Aplicar estos patrones de manera efectiva mejora tu código en tres áreas clave:
- **Flexibilidad**: Puedes crear código adaptable a los cambios sin tener que reescribir grandes partes de la aplicación.
- **Reutilización**: Fomentan la escritura de código que se puede reutilizar en diferentes proyectos.
- **Mantenibilidad**: El código estructurado con patrones de diseño suele ser más fácil de leer, entender y actualizar.

A continuación, comenzaremos con los **patrones creacionales**, la primera gran categoría, para aprender a crear objetos de forma flexible, controlar la instanciación y mantener el código limpio desde el principio.

---

### Sección 2: Patrones creacionales: Técnicas para la creación de objetos

Crear objetos es una de las tareas más comunes en la programación. En ocasiones, la creación de un objeto es simple, pero en otros casos puede volverse compleja, especialmente si el objeto tiene muchas partes o debe seguir reglas específicas. Los patrones creacionales son técnicas que ayudan a crear objetos de forma inteligente y organizada.

En esta sección, analizaremos cuatro patrones creacionales populares: **Factory Method**, **Abstract Factory**, **Builder** y **Singleton**.

#### Factory Method: Simplificando la creación de objetos

El patrón Factory Method permite crear objetos sin especificar su clase exacta. En lugar de utilizar el operador `new` directamente (por ejemplo, `new PayPalProcessor()`), lo que acoplaría estrechamente tu código a una implementación específica, delegas la creación a un método de fábrica.

Este método decide qué clase instanciar en función de una lógica de entrada. Esta abstracción permite intercambiar o agregar nuevos métodos de pago más adelante (como Apple Pay) sin reescribir la lógica de la aplicación.

Ejemplo en TypeScript con procesadores de pago:

```typescript
// 1. The Interface: Defines what a payment processor must do
interface PaymentProcessor {
  process(amount: number): void;
}
 
// 2. Concrete Classes: The specific implementations
class PayPal implements PaymentProcessor {
  process(amount: number) {
    console.log(`Processing $${amount} via PayPal.`);
  }
}
 
class Stripe implements PaymentProcessor {
  process(amount: number) {
    console.log(`Processing $${amount} via Stripe Credit Card.`);
  }
}
 
// 3. The Factory: Decides which object to create
class PaymentFactory {
  static getProcessor(method: "paypal" | "stripe"): PaymentProcessor {
    if (method === "paypal") {
      return new PayPal();
    } else if (method === "stripe") {
      return new Stripe();
    }
    throw new Error("Unknown payment method");
  }
}
 
// Usage: The client code asks for a processor, unaware of the specific class logic
const processor = PaymentFactory.getProcessor("paypal");
processor.process(50); // Output: Processing $50 via PayPal.
```

La interfaz `PaymentProcessor` establece un contrato común que tanto `PayPal` como `Stripe` cumplen. El método `PaymentFactory.getProcessor()` actúa como un punto central de decisión.

#### Abstract Factory: Creando familias de objetos relacionados

El patrón Abstract Factory permite crear familias completas de objetos relacionados (por ejemplo, todos los componentes de interfaz de usuario para un tema claro o para un tema oscuro) sin que el código del cliente conozca las clases concretas involucradas.

Es un paso más allá del Factory Method: proporciona una "fábrica de fábricas" que garantiza que cada objeto creado pertenezca a la misma familia compatible.

##### Paso 1: Definir las interfaces de producto

```typescript
interface Button {
  render(): void;
}
 
interface Checkbox {
  toggle(): void;
}
```

##### Paso 2: Crear los productos concretos para cada tema

```typescript
// --- Family 1: Light Theme Components ---
class LightButton implements Button {
  render() {
    console.log("Rendering Light Button");
  }
}
 
class LightCheckbox implements Checkbox {
  toggle() {
    console.log("Toggling Light Checkbox");
  }
}
 
// --- Family 2: Dark Theme Components ---
class DarkButton implements Button {
  render() {
    console.log("Rendering Dark Button");
  }
}
 
class DarkCheckbox implements Checkbox {
  toggle() {
    console.log("Toggling Dark Checkbox");
  }
}
```

##### Paso 3: Definir la interfaz de la Abstract Factory

```typescript
interface ThemeFactory {
  createButton(): Button;
  createCheckbox(): Checkbox;
}
```

##### Paso 4: Implementar las fábricas concretas

```typescript
class LightThemeFactory implements ThemeFactory {
  createButton(): Button {
    return new LightButton();
  }
  createCheckbox(): Checkbox {
    return new LightCheckbox();
  }
}
 
class DarkThemeFactory implements ThemeFactory {
  createButton(): Button {
    return new DarkButton();
  }
  createCheckbox(): Checkbox {
    return new DarkCheckbox();
  }
}
```

##### Paso 5: Usar la fábrica (código del cliente)

```typescript
// Choose the theme once (could come from user settings, config, etc.)
const themeFactory: ThemeFactory = new DarkThemeFactory();
 
// Create a consistent family of components
const button = themeFactory.createButton();
const checkbox = themeFactory.createCheckbox();
 
button.render();   // Output: Rendering Dark Button
checkbox.toggle(); // Output: Toggling Dark Checkbox
```

#### Builder: Construyendo objetos complejos

El patrón Builder es ideal cuando necesitas construir un objeto complejo con muchas configuraciones posibles paso a paso, evitando constructores sobrecargados con múltiples parámetros.

##### Paso 1: Definir el objeto complejo (el producto)

```typescript
class Car {
  engine!: string;
  wheels!: number;
  color!: string;
  // Imagine many more properties here (sunroof, GPS, etc.)
}
```

##### Paso 2: Crear la clase Builder

```typescript
class CarBuilder {
  private car: Car;
 
  constructor() {
    this.car = new Car();
  }

// We will add the methods below inside this class...
}
```

##### Paso 3: Implementar métodos fluidos (*fluent methods*)

```typescript
// ... continue inside CarBuilder
setEngine(engine: string): CarBuilder {
  his.car.engine = engine;
    return this; // Returning 'this' enables method chaining
  }
 
  setWheels(wheels: number): CarBuilder {
    this.car.wheels = wheels;
    return this;
  }
```

##### Paso 4: Implementar el método build

```typescript
// ... continue inside CarBuilder 
build(): Car { 
return this.car; 
}
```

##### Paso 5: Usar el builder (código del cliente)

```typescript
const car = new CarBuilder()
  .setEngine("V8")
  .setWheels(4)
  .setColor("Red")
  .build(); // Finalizes the object
 
console.log(car); 
// Output: { engine: 'V8', wheels: 4, color: 'Red' }
```

##### El patrón Builder en TypeScript moderno

TypeScript moderno admite propiedades opcionales, tipos parciales y sintaxis de propagación de objetos (*spread syntax*). A menudo, los objetos complejos se pueden componer directamente:

```typescript
const request = {
  ...defaultConfig,
  ...userInput,
  metadata: { orderId }
};
```

Aun así, el patrón Builder sigue siendo fundamental cuando la construcción debe seguir una secuencia estricta, cuando se requiere validación en cada paso o en el desarrollo de APIs fluidas y SDKs.

#### Singleton: Una única instancia

El patrón Singleton garantiza que una clase tenga una única instancia a lo largo de todo el ciclo de vida de la aplicación y proporciona un punto de acceso global a ella.

Es útil para recursos compartidos como administradores de conexiones a bases de datos, servicios de logging o ajustes de configuración global.

##### Paso 1: Usar un constructor privado

```typescript
class DatabaseConnection {
  // This static property holds the ONE unique instance
  private static instance: DatabaseConnection;
 
  // Private constructor prevents usage of 'new DatabaseConnection()'
  private constructor() {
    console.log("Initializing Database Connection...");
  }
  // We'll add the access logic next...
}
```

##### Paso 2: Usar el método de acceso estático `getInstance()`

```typescript
// ... inside DatabaseConnection class
 
  public static getInstance(): DatabaseConnection {
    if (!DatabaseConnection.instance) {
      // If no instance exists, create one (Lazy Initialization)
      DatabaseConnection.instance = new DatabaseConnection();
    }
    // Return the existing instance
    return DatabaseConnection.instance;
  }
 
  // A generic method to simulate doing work
  public query(sql: string): void {
    console.log(`Executing query: ${sql}`);
  }
```

##### Paso 3: Verificación del Singleton

```typescript
// Client A asks for the database
const db1 = DatabaseConnection.getInstance();
db1.query("SELECT * FROM users");
 
// Client B asks for the database
const db2 = DatabaseConnection.getInstance();
db2.query("SELECT * FROM products");
 
// Verify they are actually the exact same object
console.log(db1 === db2); // Output: true
```

---

### Sección 3: Patrones estructurales: Técnicas para la composición de clases y objetos

Los patrones estructurales se centran en organizar las relaciones entre clases y objetos. Ayudan a construir código más fácil de gestionar simplificando la estructura o haciendo que diferentes partes del código colaboren eficazmente.

Aquí analizaremos cuatro patrones estructurales: **Adapter**, **Composite**, **Decorator** y **Facade**.

#### Adapter: Haciendo compatibles interfaces incompatibles

El patrón Adapter permite que dos interfaces incompatibles trabajen juntas. Funciona como un adaptador de viaje que conecta un enchufe a una toma de corriente de otro estándar.

##### Paso 1: El sistema antiguo (la expectativa)

```typescript
// The interface our application expects
interface OldPaymentSystem {
  makePayment(amount: number): void;
}
 
// The existing implementation we want to replace
class LegacyPayment implements OldPaymentSystem {
  makePayment(amount: number) {
    console.log(`Payment of $${amount} made using legacy system`);
  }
}
```

##### Paso 2: El nuevo sistema (la clase incompatible)

```typescript
// The new interface (incompatible with the old one)
interface NewPaymentSystem {
  processPayment(amount: number): void;
}
 
class ModernPayment implements NewPaymentSystem {
  processPayment(amount: number) {
    console.log(`Payment of $${amount} processed using modern system`);
  }
}
```

##### Paso 3: El adaptador (el puente)

```typescript
class PaymentAdapter implements OldPaymentSystem {
  private newPaymentSystem: NewPaymentSystem;
 
  constructor(newPaymentSystem: NewPaymentSystem) {
    this.newPaymentSystem = newPaymentSystem;
  }
 
  // The translation magic happens here
  makePayment(amount: number) {
    // We receive the call as 'makePayment'...
    // ...and forward it as 'processPayment'
    this.newPaymentSystem.processPayment(amount);
  }
}
```

##### Paso 4: Uso del adaptador

```typescript
// 1. Create the new service
const modernPayment = new ModernPayment();
 
// 2. Wrap it in the adapter
const adapter = new PaymentAdapter(modernPayment);
 
// 3. Use it! (The code thinks it's using the old system)
adapter.makePayment(100); 
// Output: Payment of $100 processed using modern system
```

#### Composite: Tratando objetos individuales y composiciones de manera uniforme

El patrón Composite permite tratar a los objetos individuales y a los grupos de objetos de manera uniforme, organizándolos en estructuras de árbol.

##### Paso 1: Definir la interfaz común

```typescript
interface Shape {
  draw(): void;
}
```

##### Paso 2: Crear los nodos hoja (*leaf nodes*)

```typescript
class Circle implements Shape {
  draw() {
    console.log("Drawing a Circle");
  }
}
 
class Rectangle implements Shape {
  draw() {
    console.log("Drawing a Rectangle");
  }
}
```

##### Paso 3: Crear el nodo compuesto (*Composite node*)

```typescript
class ShapeGroup implements Shape {
  // This array can hold Circles, Rectangles, or even other ShapeGroups!
  private shapes: Shape[] = [];
 
  addShape(shape: Shape) {
    this.shapes.push(shape);
  }
 
  // The magic happens here: delegating the task to children
  draw() {
    console.log("--- Starting Group Draw ---");
    this.shapes.forEach((shape) => shape.draw());
    console.log("--- Finished Group Draw ---");
  }
}
```

##### Paso 4: Uso de la estructura Composite

```typescript
const circle = new Circle();
const rectangle = new Rectangle();
const group = new ShapeGroup();
 
// Add individual shapes to the group
group.addShape(circle);
group.addShape(rectangle);
 
// Trigger the operation on the entire hierarchy at once
group.draw();
 
/* Output:
--- Starting Group Draw ---
Drawing a Circle
Drawing a Rectangle
--- Finished Group Draw ---
*/
```

#### Decorator: Añadiendo comportamiento sin alterar la estructura

El patrón Decorator permite agregar nuevas responsabilidades y comportamientos a un objeto dinámicamente envolviéndolo en una clase decoradora.

##### Paso 1: Definir la interfaz base y el componente base

```typescript
// The common interface
interface Coffee {
  getCost(): number;
  getDescription(): string;
}
 
// The base component
class BasicCoffee implements Coffee {
  getCost(): number {
    return 5;
  }
 
  getDescription(): string {
    return "Basic Coffee";
  }
}
```

##### Paso 2: Crear los decoradores

```typescript
class MilkDecorator implements Coffee {
  private coffee: Coffee;
 
  constructor(coffee: Coffee) {
    this.coffee = coffee;
  }
 
  getCost(): number {
    return this.coffee.getCost() + 2; // Add cost of milk
  }
 
  getDescription(): string {
    return this.coffee.getDescription() + ", Milk";
  }
}
```

##### Paso 3: Apilar decoradores (uso en el cliente)

```typescript
// 1. Start with a basic coffee
const coffee = new BasicCoffee();
 
// 2. Wrap it with Milk
const coffeeWithMilk = new MilkDecorator(coffee);
 
// 3. Wrap that result with Sugar
const fullCoffee = new SugarDecorator(coffeeWithMilk);
 
console.log(fullCoffee.getDescription()); 
// Output: Basic Coffee, Milk, Sugar
 
console.log(fullCoffee.getCost()); 
// Output: 8 (5 + 2 + 1)
```

#### Facade: Simplificando subsistemas complejos

El patrón Facade proporciona una interfaz simplificada y de alto nivel a un subsistema complejo con múltiples componentes interconectados.

##### Paso 1: Los subsistemas complejos

```typescript
class Amplifier {
  on() { console.log("Amplifier is on"); }
}
 
class Projector {
  on() { console.log("Projector is on"); }
}
 
class Lights {
  dim() { console.log("Lights are dimmed"); }
}
```

##### Paso 2: La fachada (*Facade*)

```typescript
class HomeTheaterFacade {
  private amp: Amplifier;
  private projector: Projector;
  private lights: Lights;
 
  constructor(amp: Amplifier, projector: Projector, lights: Lights) {
    this.amp = amp;
    this.projector = projector;
    this.lights = lights;
  }
 
  // The simplified interface
  watchMovie() {
    console.log("--- Get Ready for the Movie ---");
    this.lights.dim();
    this.projector.on();
    this.amp.on();
    console.log("--- Ready to watch! ---");
  }
}
```

##### Paso 3: Uso de la fachada

```typescript
// 1. Setup (usually done once in your app configuration)
const amp = new Amplifier();
const projector = new Projector();
const lights = new Lights();
 
// 2. Create the Facade
const homeTheater = new HomeTheaterFacade(amp, projector, lights);
 
// 3. One simple call triggers the complex sequence
homeTheater.watchMovie();
 
/* Output:
--- Get Ready for the Movie ---
Lights are dimmed
Projector is on
Amplifier is on
--- Ready to watch! ---
*/
```

---

### Sección 4: Patrones de comportamiento: Definiendo la interacción y comunicación entre objetos

Los patrones de comportamiento se centran en los algoritmos y la asignación de responsabilidades entre objetos.

Aquí cubriremos cuatro patrones clave: **Observer**, **Strategy**, **Command** e **Iterator**.

#### Observer: Notificando a objetos dependientes sobre cambios de estado

El patrón Observer permite que un objeto (el *Subject*) notifique automáticamente a múltiples objetos observadores (*Observers*) cuando cambia su estado interno.

##### Paso 1: Definir la interfaz Observer

```typescript
interface Observer {
  update(temperature: number): void;
}
```

##### Paso 2: Implementar el Subject (`WeatherStation`)

Parte A: Gestión de suscripciones:
```typescript
class WeatherStation {
  // A list to hold everyone listening to this station
  private observers: Observer[] = [];
  private temperature: number = 0;
 
  // 1. Subscribe: Add someone to the list
  addObserver(observer: Observer) {
    this.observers.push(observer);
  }
 
  // 2. Unsubscribe: Remove someone from the list
  removeObserver(observer: Observer) {
    this.observers = this.observers.filter((obs) => obs !== observer);
  }
  // ... We will add the update logic next
}
```

Parte B: Notificación de eventos:
```typescript
// ... continuing inside WeatherStation class
 
  // 3. The Trigger: Change data and tell everyone
  setTemperature(temp: number) {
    console.log(`\nNew Temperature measured: ${temp}°C`);
    this.temperature = temp;
    
    // The state changed, so we notify everyone immediately
    this.notifyObservers();
  }
 
  private notifyObservers() {
    // Loop through the list and update every single observer
    this.observers.forEach((observer) => observer.update(this.temperature));
  }
```

##### Paso 3: Crear observadores concretos

```typescript
class TemperatureDisplay implements Observer {
  update(temperature: number) {
    console.log(`Display: Current temp is ${temperature}°C`);
  }
}
 
class AlertSystem implements Observer {
  update(temperature: number) {
    if (temperature > 30) {
      console.log("Alert: Temperature is too high! Evacuate!");
    }
  }
}
```

##### Paso 4: Puesta en marcha

```typescript
const weatherStation = new WeatherStation();
const tempDisplay = new TemperatureDisplay();
const alertSystem = new AlertSystem();
 
// Subscribe the devices
weatherStation.addObserver(tempDisplay);
weatherStation.addObserver(alertSystem);
 
// Simulate weather changes
weatherStation.setTemperature(25); 
// Output: Display updates. (Alert stays silent)
 
weatherStation.setTemperature(35); 
// Output: Display updates AND Alert triggers!
```

#### Strategy: Algoritmos intercambiables en tiempo de ejecución

El patrón Strategy define una familia de algoritmos, encapsula cada uno de ellos y los hace intercambiables en tiempo de ejecución sin modificar el contexto que los utiliza.

##### Paso 1: Definir la interfaz Strategy

```typescript
interface PaymentStrategy {
  pay(amount: number): void;
}
```

##### Paso 2: Implementar estrategias concretas

```typescript
class PayPalPayment implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Paid $${amount} using PayPal.`);
  }
}
 
class CreditCardPayment implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Paid $${amount} using Credit Card.`);
  }
}
 
class BankTransferPayment implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Paid $${amount} using Bank Transfer.`);
  }
}
```

##### Paso 3: Crear el contexto (`PaymentContext`)

```typescript
class PaymentContext {
  // The context holds a reference to the INTERFACE, not a specific class
  private strategy!: PaymentStrategy; // Using '!' to assert it will be set
 
  setStrategy(strategy: PaymentStrategy) {
    this.strategy = strategy;
  }
 
  executePayment(amount: number) {
    if (!this.strategy) {
      console.log("No payment method selected!");
      return;
    }
    this.strategy.pay(amount);
  }
}
```

##### Paso 4: Cambiar estrategias en tiempo de ejecución

```typescript
const paymentContext = new PaymentContext();
 
// 1. User selects PayPal
paymentContext.setStrategy(new PayPalPayment());
paymentContext.executePayment(100); 
// Output: Paid $100 using PayPal.
 
// 2. User switches to Credit Card
paymentContext.setStrategy(new CreditCardPayment());
paymentContext.executePayment(200); 
// Output: Paid $200 using Credit Card.
```

#### Command: Encapsulando solicitudes como objetos para Undo y Logging

El patrón Command transforma una solicitud en un objeto independiente que contiene toda la información sobre la operación, lo que permite parametrizar clientes, encolar solicitudes y admitir operaciones reversibles (*Undo/Redo*).

##### Paso 1: El receptor (*Receiver*)

```typescript
class MusicPlayer {
  play(track: string) {
    console.log(`Now playing: ${track}`);
  }
 
  stop() {
    console.log("Music stopped.");
  }
}
```

##### Paso 2: La interfaz Command

```typescript
interface Command {
  execute(): void;
}
```

##### Paso 3: Comandos concretos

```typescript
class PlayMusicCommand implements Command {
  private player: MusicPlayer;
  private track: string;
 
  constructor(player: MusicPlayer, track: string) {
    this.player = player;
    this.track = track;
  }
 
  execute() {
    this.player.play(this.track);
  }
}
 
class StopMusicCommand implements Command {
  private player: MusicPlayer;
 
  constructor(player: MusicPlayer) {
    this.player = player;
  }
 
  execute() {
    this.player.stop();
  }
}
```

##### Paso 4: El invocador (*Invoker*)

```typescript
class SmartController {
  private command!: Command;
 
  setCommand(command: Command) {
    this.command = command;
  }
 
  pressButton() {
    console.log("Button pressed...");
    this.command.execute();
  }
}
```

##### Paso 5: Ensamblaje del sistema

```typescript
// 1. Set up the hardware
const player = new MusicPlayer();
 
// 2. Create commands
const playJazz = new PlayMusicCommand(player, "Smooth Jazz");
const stopMusic = new StopMusicCommand(player);
 
// 3. Set up the controller
const controller = new SmartController();
 
// 4. Load 'Play' and press
controller.setCommand(playJazz);
controller.pressButton(); 
// Output: Button pressed... Now playing: Smooth Jazz
 
// 5. Load 'Stop' and press
controller.setCommand(stopMusic);
controller.pressButton(); 
// Output: Button pressed... Music stopped.
```

#### Iterator: Accediendo a elementos de una colección sin exponer su estructura

El patrón Iterator proporciona una forma estándar de recorrer secuencialmente los elementos de una colección sin exponer su representación interna.

##### Paso 1: Definir la interfaz Iterator

```typescript
interface Iterator<T> {
  next(): T | null;
  hasNext(): boolean;
}
```

##### Paso 2: Crear la lógica del iterador

```typescript
class NumberIterator implements Iterator<number> {
  private collection: number[];
  private position: number = 0;
 
  constructor(collection: number[]) {
    this.collection = collection;
  }
 
  // Returns the current item and moves the pointer forward
  next(): number | null {
    if (this.hasNext()) {
      return this.collection[this.position++];
    }
    return null;
  }
 
  // Checks if we have reached the end of the list
  hasNext(): boolean {
    return this.position < this.collection.length;
  }
}
```

##### Paso 3: Crear la colección

```typescript
class NumberCollection {
  private numbers: number[] = [];
 
  addNumber(num: number) {
    this.numbers.push(num);
  }
 
  // Returns a fresh iterator starting at index 0
  createIterator(): NumberIterator {
    return new NumberIterator(this.numbers);
  }
}
```

##### Paso 4: Recorrido de la colección

```typescript
// 1. Populate the collection
const numbers = new NumberCollection();
numbers.addNumber(1);
numbers.addNumber(2);
numbers.addNumber(3);
 
// 2. Ask the collection for an iterator
const iterator = numbers.createIterator();
 
// 3. Loop through using the standard interface
while (iterator.hasNext()) {
  console.log(iterator.next());
}
 
// Output:
// 1
// 2
// 3
```

---

### Sección 5: Ejemplos prácticos: Aplicando patrones de diseño en escenarios del mundo real

#### Patrones creacionales: Factory Method en un sistema de notificaciones

```typescript
abstract class Notification {
  abstract send(message: string): void;
}
 
class EmailNotification extends Notification {
  send(message: string) {
    console.log(`Sending Email: ${message}`);
  }
}
 
class SMSNotification extends Notification {
  send(message: string) {
    console.log(`Sending SMS: ${message}`);
  }
}
```

Fábrica:
```typescript
class NotificationFactory {
  static getNotification(type: "email" | "sms"): Notification {
    if (type === "email") return new EmailNotification();
    if (type === "sms") return new SMSNotification();
    throw new Error("Unsupported notification type.");
  }
}
```

Uso:
```typescript
const alert = NotificationFactory.getNotification("email");
alert.send("Your report is ready!"); 
// Output: Sending Email: Your report is ready!
```

#### Patrones estructurales: Adapter para APIs de terceros

APIs incompatibles:
```typescript
interface WeatherService {
  getTemperature(): number;
}
 
class WeatherAPI1 {
  fetchTemp(): number { return 28; }
}
 
class WeatherAPI2 {
  getTemp(): number { return 30; }
}
```

Adaptadores:
```typescript
class WeatherAdapter1 implements WeatherService {
  constructor(private api: WeatherAPI1) {}
 
  getTemperature(): number {
    return this.api.fetchTemp();
  }
}
 
class WeatherAdapter2 implements WeatherService {
  constructor(private api: WeatherAPI2) {}
 
  getTemperature(): number {
    return this.api.getTemp();
  }
}
```

Uso unificado:
```typescript
const serviceA: WeatherService = new WeatherAdapter1(new WeatherAPI1());
console.log(`Temperature: ${serviceA.getTemperature()}°C`);
```

#### Patrones de comportamiento: Observer en una sala de chat en tiempo real

```typescript
class ChatRoom {
  private users: User[] = [];
 
  addUser(user: User) {
    this.users.push(user);
  }
 
  sendMessage(message: string) {
    // Notify every user in the list
    this.users.forEach((user) => user.notify(message));
  }
}
```

```typescript
interface User {
  notify(message: string): void;
}
 
class ChatUser implements User {
  constructor(private name: string) {}
 
  notify(message: string) {
    console.log(`${this.name} received: ${message}`);
  }
}
```

Uso:
```typescript
const room = new ChatRoom();
const user1 = new ChatUser("Alice");
const user2 = new ChatUser("Bob");
 
room.addUser(user1);
room.addUser(user2);
 
room.sendMessage("Hello, everyone!"); 
// Output: 
// Alice received: Hello, everyone!
// Bob received: Hello, everyone!
```

#### Patrones de comportamiento: Command para funcionalidad Undo en un editor de texto

```typescript
class TextDocument {
  private text: string = "";
 
  write(text: string) {
    this.text += text;
  }
 
  getText() {
    return this.text;
  }
 
  erase() {
    this.text = ""; // Simplified undo logic
  }
}
```

```typescript
interface Command {
  execute(): void;
  undo(): void;
}
 
class WriteCommand implements Command {
  constructor(private doc: TextDocument, private text: string) {}
 
  execute() {
    this.doc.write(this.text);
  }
 
  undo() {
    this.doc.erase();
  }
}
```

Uso:
```typescript
const doc = new TextDocument();
const command = new WriteCommand(doc, "Hello, World!");
 
// 1. Execute Command
command.execute();
console.log(doc.getText()); // Output: Hello, World!
 
// 2. Undo Command
command.undo();
console.log(doc.getText()); // Output: (empty string)
```

#### Cuándo y dónde es más útil cada patrón

- **Patrones creacionales**:
  - *Factory / Abstract Factory*: Flexibilidad en la instanciación sin acoplarse a clases concretas.
  - *Builder*: Creación de objetos con múltiples campos u opciones complejas.
  - *Singleton*: Gestión de recursos e instancias globales compartidas.
- **Patrones estructurales**:
  - *Adapter*: Integración con sistemas o APIs de interfaces incompatibles.
  - *Composite*: Estructuras jerárquicas y de árbol tratadas uniformemente.
  - *Decorator*: Extensión dinámica de funcionalidades en tiempo de ejecución.
  - *Facade*: Punto de entrada simplificado para subsistemas complejos.
- **Patrones de comportamiento**:
  - *Observer*: Sistemas de eventos y actualizaciones en tiempo real.
  - *Strategy*: Intercambio ágil de algoritmos en caliente.
  - *Command*: Encolado de tareas, auditoría y operaciones reversibles (*Undo/Redo*).
  - *Iterator*: Recorrido seguro de colecciones personalizadas.

---

### Sección 6: Ventajas y desventajas de los patrones de diseño

#### Ventajas

- **Soluciones estándar a problemas comunes**: Enfoques probados y consolidados en la industria.
- **Mejor organización del código**: Separación clara de responsabilidades.
- **Reutilización y flexibilidad**: Estructuras adaptables a requisitos cambiantes.
- **Vocabulario común**: Comunicación fluida entre miembros del equipo.
- **Facilidad de mantenimiento**: Menor probabilidad de introducir regresiones.

#### Desventajas

- **Complejidad innecesaria**: Introducir patrones complejos en aplicaciones sencillas puede resultar en sobreingeniería (*overengineering*).
- **Curva de aprendizaje**: Puede dificultar la comprensión inicial a desarrolladores noveles.
- **Impacto potencial en rendimiento**: Capas adicionales de abstracción e indirección.
- **Rigidez por mal uso**: Un patrón inadecuado (como abusar de Singleton) puede complicar las pruebas unitarias.

#### Búsqueda del equilibrio adecuado

- **Comienza simple**: Introduce un patrón solo si resuelve un problema claro y evidente.
- **Comprende el problema**: Basa la decisión en la necesidad real de la arquitectura.
- **Documenta la intención**: Deja constancia en el código de por qué se eligió dicho patrón.

---

### Sección 7: Mejores prácticas para implementar patrones de diseño en TypeScript

1. **Comprende el problema antes de elegir un patrón**: Analiza si la solución aporta valor antes de forzar un patrón.
2. **Mantén la simplicidad**: No utilices patrones para problemas que una función simple puede solucionar.
3. **Combina patrones cuando sea beneficioso**: Por ejemplo, un Factory para crear objetos que luego son decorados por un Decorator.
4. **Haz que tus patrones sean flexibles**: Utiliza interfaces y tipos genéricos.
5. **Prueba exhaustivamente**: Asegura con pruebas unitarias las interacciones complejas.
6. **Documenta el código**:

```typescript
// Using the Observer pattern to notify subscribers of stock price updates
class StockTicker { ... }
```

7. **Aprovecha las fortalezas de TypeScript**: Utiliza `this` como tipo de retorno para habilitar el encadenamiento fluido con seguridad de tipos:

```typescript
interface ICarBuilder {
  setEngine(engine: string): this;
  setWheels(wheels: number): this;
  build(): Car;
}
```

8. **Evita la obsesión por los patrones**: Evalúa siempre la relación costo-beneficio de cada abstracción.
9. **Refactoriza cuando sea necesario**: Los patrones no son inmutables; adapta o elimina patrones si los requisitos cambian.

---

### Sección: Resumen

En este capítulo, aprendiste cómo los patrones de diseño proporcionan soluciones probadas a desafíos comunes de diseño de software y cómo implementarlos eficazmente utilizando TypeScript. Exploramos en profundidad los patrones creacionales, estructurales y de comportamiento, analizando escenarios prácticos de aplicación en el mundo real junto con sus ventajas y desventajas.

Finalmente, revisamos las mejores prácticas para aplicar patrones de diseño con sensatez, aprovechando características fundamentales de TypeScript como interfaces, clases abstractas, tipos genéricos y retornos fluidos.

En el próximo capítulo, nos adentraremos en la **Comprensión de Características Avanzadas de TypeScript**.
