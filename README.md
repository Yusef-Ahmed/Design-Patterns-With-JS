# **Design Patterns With JS**

## Table of Contents

### Creational Patterns

- [1. Singleton Pattern](#1--singleton-pattern)
- [2. Factory Pattern](#2--factory-pattern)
- [3. Abstract Factory Pattern](#3--abstract-factory-pattern)
- [4. Builder Pattern](#4--builder-pattern)
- [5. Prototype Pattern](#5--prototype-pattern)

### Structural Patterns

- [1. Adapter Pattern](#1--adapter-pattern-wrapper)
- [2. Bridge Pattern](#2--bridge-pattern)
- [3. Composite Pattern](#3--composite-pattern)
- [4. Decorator Pattern](#4--decorator-pattern)
- [5. Facade Pattern](#5--facade-pattern)
- [6. Flyweight Pattern](#6--flyweight-pattern)
- [7. Proxy Pattern](#7--proxy-pattern)

### Behavioral Patterns

- [1. Chain of Responsibility](#1--chain-of-responsibility)
- [2. Command Pattern](#2--command-pattern)
- [3. Interpreter Pattern](#3--interpreter-pattern)
- [4. Iterator Pattern](#4--iterator-pattern)
- [5. Mediator Pattern](#5--mediator-pattern)
- [6. Memento Pattern](#6--memento-pattern)
- [7. Observer Pattern](#7--observer-pattern)
- [8. State Pattern](#8--state-pattern)
- [9. Strategy Pattern](#9--strategy-pattern)
- [10. Template Pattern](#10--template-pattern)
- [11. Visitor Pattern](#11--visitor-pattern)

## **Creational Patterns**

Creational design patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code.

### **1- Singleton Pattern**

#### Description

Ensures that a class has only one instance and provides a global point of access to it.

#### Code

```js
class Singleton {
  constructor() {
    if (!Singleton.instance) {
      Singleton.instance = this;
    }
    this.name = "I am a Singleton instance!";
    return Singleton.instance;
  }

  getData() {
    return this.name;
  }

  setData(newName) {
    this.name = newName;
  }
}

const instance1 = new Singleton();
const instance2 = new Singleton();

instance1.setData("It's the same"):

console.log(instance2.getData()); // It's the same
```

#### Pros

- Saves memory by reusing the same instance.
- Useful for managing shared resources like configurations or database connections.

#### Cons

- Difficult to test due to global state.
- Can lead to hidden dependencies and tight coupling.

### **2- Factory Pattern**

#### Description

Provides an interface for creating objects but allows subclasses to alter the type of objects created.

#### Code

```js
class Car {
  constructor() {
    this.type = "Car";
  }
}

class Truck {
  constructor() {
    this.type = "Truck";
  }
}

class VehicleFactory {
  static createVehicle(vehicleType) {
    switch (vehicleType) {
      case "car":
        return new Car();
      case "truck":
        return new Truck();
      default:
        throw new Error("Unknown vehicle type");
    }
  }
}

const myCar = VehicleFactory.createVehicle("car");
console.log(myCar.type); // "Car"
```

#### Pros

- Simplifies object creation.
- Increases flexibility by decoupling object instantiation from implementation.

#### Cons

- Debugging can be harder due to abstraction.

### **3- Abstract Factory Pattern**

#### Description

Provides an interface for creating families of related objects without specifying concrete classes.

#### Code

```js
class Car {
  create() {
    return "Car created";
  }
}

class Truck {
  create() {
    return "Truck created";
  }
}

// Factory
class LandVehicleFactory {
  static createVehicle(vehicleType) {
    switch (vehicleType) {
      case "car":
        return new Car();
      case "truck":
        return new Truck();
      default:
        throw new Error("Unknown land vehicle type");
    }
  }
}

// Abstract Factory
class VehicleFactory {
  static getFactory(type) {
    switch (type) {
      case "land":
        return LandVehicleFactory;
      default:
        throw new Error("Unknown factory type");
    }
  }
}

const landFactory = VehicleFactory.getFactory("land");
const myCar = landFactory.createVehicle("car");

console.log(myCar.create()); // "Car created"
```

#### Pros

- Encapsulates object creation logic in one place.
- Helps maintain a scalable and extendable codebase.

#### Cons

- Can lead to an explosion of factory classes if not managed well.

### **4- Builder Pattern**

#### Description _[compare with bridge](#2--Bridge-Pattern)_

Separates object construction from its representation.

#### Code

```js
class Car {
  constructor() {
    this.engine = null;
    this.wheels = null;
  }
}

class CarBuilder {
  constructor() {
    this.car = new Car();
  }

  addEngine(engine) {
    this.car.engine = engine;
    return this;
  }

  addWheels(wheels) {
    this.car.wheels = wheels;
    return this;
  }

  build() {
    return this.car;
  }
}

const car = new CarBuilder().addEngine("V8").addWheels(4).build();
console.log(car); // { engine: 'V8', wheels: 4 }
```

#### Pros

- Improves readability and maintainability for complex objects.
- Provides more control over object creation.

#### Cons

- Can introduce unnecessary complexity for simple objects.

### **5- Prototype Pattern**

#### Description _[compare with Template](#10--Template-Pattern)_

Creates objects based on a template of an existing object.

#### Code

```js
const vehiclePrototype = {
  type: "Vehicle",
  getType() {
    return this.type;
  },
};

const car = Object.create(vehiclePrototype);
car.type = "Car";

console.log(car.getType()); // "Car"
console.log(vehiclePrototype.getType()); // "Vehicle"
```

#### Pros

- Faster object creation since it avoids calling constructors.
- Reduces memory usage by reusing object properties.

#### Cons

- Objects may share unintended properties, leading to unexpected side effects.
- Debugging can be tricky due to prototype chain complexity.

## Structural Patterns

Focus on composing objects and classes to form larger structures while keeping them flexible and efficient.

### **1- Adapter Pattern (Wrapper)**

#### Description

Allows two incompatible interfaces to work together by providing a wrapper that converts one interface into another.

#### Code

```js
class OldSystem {
  request() {
    return "Data from old system";
  }
}

class NewSystem {
  specificRequest() {
    return "Data from new system";
  }
}

class Adapter {
  constructor(newSystem) {
    this.newSystem = newSystem;
  }

  request() {
    return this.newSystem.specificRequest();
  }
}

const oldSystem = new OldSystem();
const newSystem = new NewSystem();
const adapter = new Adapter(newSystem);

// work the same
console.log(oldSystem.request());
console.log(adapter.request());
```

#### Pros

- Enables legacy code to work with modern systems.
- Helps integrate third-party libraries with incompatible APIs.

#### Cons

- Can introduce additional complexity.

### **2- Bridge Pattern**

#### Description _[compare with builder](#4--Builder-Pattern)_

Separates abstraction from implementation, allowing them to evolve independently.

#### Code

```js
// Abstraction
class Remote {
  constructor(device) {
    this.device = device;
  }

  pressPower() {
    return this.device.turnOn();
  }
}

// Implementation
class TV {
  turnOn() {
    return "TV is ON";
  }
}

class Radio {
  turnOn() {
    return "Radio is ON";
  }
}

// Using the bridge
const tvRemote = new Remote(new TV());
console.log(tvRemote.pressPower()); // TV is ON

const radioRemote = new Remote(new Radio());
console.log(radioRemote.pressPower()); // Radio is ON
```

#### Pros

- Increases flexibility by decoupling abstraction from implementation.
- Makes code easier to extend and modify.

#### Cons

- May add complexity if not needed.

### **3- Composite Pattern**

#### Description

Treats individual objects and compositions of objects uniformly.

#### Code

```js
class File {
  constructor(name) {
    this.name = name;
  }

  display() {
    console.log(this.name);
  }
}

class Folder {
  constructor(name) {
    this.name = name;
    this.children = [];
  }

  add(item) {
    this.children.push(item);
  }

  display() {
    console.log(this.name);
    this.children.forEach((child) => child.display());
  }
}

const file1 = new File("file1.txt");
const file2 = new File("file2.txt");
const folder = new Folder("Documents");

folder.add(file1);
folder.add(file2);
folder.display();
```

#### Pros

- Simplifies complex hierarchical structures.
- Makes it easy to treat individual and composite objects uniformly.

#### Cons

- Can make debugging more difficult due to nested structures.

### **4- Decorator Pattern**

#### Description

Adds functionality dynamically without modifying the original object.

#### Code

```js
class Coffee {
  cost() {
    return 5;
  }
}

class MilkDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }
  cost() {
    return this.coffee.cost() + 2;
  }
}

const basicCoffee = new Coffee();
const milkCoffee = new MilkDecorator(basicCoffee);
console.log(milkCoffee.cost()); // 7
```

#### Pros

- Allows extending functionality dynamically.
- Avoids modifying the original class.

#### Cons

- Can result in many small decorator classes, increasing complexity.

### **5- Facade Pattern**

#### Description

Provides a simplified interface to a complex system.

#### Code

```js
class CPU {
  start() {
    return "CPU started";
  }
}

class Memory {
  load() {
    return "Memory loaded";
  }
}

class ComputerFacade {
  constructor() {
    this.cpu = new CPU();
    this.memory = new Memory();
  }
  start() {
    return `${this.cpu.start()}, ${this.memory.load()}`;
  }
}

const computer = new ComputerFacade();
console.log(computer.start()); // CPU started, Memory loaded
```

#### Pros

- Simplifies interactions with complex subsystems.
- Reduces dependencies on internal system details.

#### Cons

- Can hide necessary details, making debugging harder.

### **6- Flyweight Pattern**

#### Description

Reduces memory usage by sharing objects instead of creating new ones.

#### Code

```js
class Car {
  constructor(model) {
    this.model = model;
  }
}

class CarFactory {
  constructor() {
    this.cars = {};
  }
  getCar(model) {
    if (!this.cars[model]) {
      this.cars[model] = new Car(model);
    }
    return this.cars[model];
  }
}

const factory = new CarFactory();
const car1 = factory.getCar("Tesla");
const car2 = factory.getCar("Tesla");
console.log(car1 === car2); // true (same shared instance)
```

#### Pros

- Saves memory by sharing common objects.
- Improves performance for large-scale applications.

#### Cons

- Increased complexity in managing shared objects.

### **7- Proxy Pattern**

#### Description

Controls access to an object, adding an extra layer of functionality.

#### Code

```js
class BankAccount {
  constructor(balance) {
    this.balance = balance;
  }
  withdraw(amount) {
    if (amount <= this.balance) {
      this.balance -= amount;
      return `Withdrawn: $${amount}`;
    }
    return "Insufficient funds";
  }
}

class BankProxy {
  constructor(account) {
    this.account = account;
  }
  withdraw(amount) {
    console.log(`Attempting to withdraw $${amount}...`);
    return this.account.withdraw(amount);
  }
}

const account = new BankAccount(100);
const proxy = new BankProxy(account);
console.log(proxy.withdraw(50)); // Attempting to withdraw $50... Withdrawn: $50
```

## Behavioral Patterns

How objects interact and communicate, they help manage responsibilities between objects, making the system more flexible and easier to maintain.

### **1- Chain of Responsibility**

#### Description

Passes a request along a chain of handlers until one handles it.

#### Code

```js
class Handler {
  setNext(handler) {
    this.next = handler;
    return handler;
  }
  handle(request) {
    if (this.next) return this.next.handle(request);
    return "Chain is over";
  }
}

class AuthHandler extends Handler {
  handle(request) {
    if (!request.user) return "Unauthorized";
    console.log("user is:", request.user);
    return super.handle(request);
  }
}

class LogHandler extends Handler {
  handle(request) {
    console.log("action:", request.action);
    return super.handle(request);
  }
}

const auth = new AuthHandler();
const logger = new LogHandler();
auth.setNext(logger);

console.log(auth.handle({ user: "admin", action: "delete" }));
```

#### Pros

- Decouples sender and handler logic.
- Easy to add/modify handlers.

#### Cons

- Harder to debug with many links in the chain.

### **2- Command Pattern**

#### Description

Encapsulates a request as an object.

#### Code

```js
class Light {
  turnOn() {
    return "Light is on";
  }
}

class TurnOnCommand {
  constructor(light) {
    this.light = light;
  }
  execute() {
    return this.light.turnOn();
  }
}

const light = new Light();
const command = new TurnOnCommand(light);
console.log(command.execute());
```

#### Pros

- Enables undo/redo and queuing.
- Fully decouples invoker from receiver.

#### Cons

- Introduces many small classes.

### **3- Interpreter Pattern**

#### Description

Evaluates language grammar or expressions.

#### Code

```js
class NumberExpression {
  constructor(value) {
    this.value = value;
  }
  interpret() {
    return this.value;
  }
}

class AddExpression {
  constructor(left, right) {
    this.left = left;
    this.right = right;
  }
  interpret() {
    return this.left.interpret() + this.right.interpret();
  }
}

const expr = new AddExpression(
  new NumberExpression(5),
  new NumberExpression(3)
);
console.log(expr.interpret()); // 8
```

#### Pros

- Easy to extend grammar.
- Good for DSLs or config rules.

#### Cons

- Can get complex for large grammars.

### **4- Iterator Pattern**

#### Description

Provides a way to access elements of a collection without exposing its structure.

#### Code

```js
class Iterator {
  constructor(items) {
    this.items = items;
    this.index = 0;
  }
  next() {
    return this.index < this.items.length
      ? { value: this.items[this.index++], done: false }
      : { done: true };
  }
}

const iter = new Iterator(["a", "b", "c"]);
console.log(iter.next()); // { value: 'a', done: false }
```

#### Pros

- Uniform way to traverse collections.
- Encapsulates iteration logic.

### **5- Mediator Pattern**

#### Description

Centralizes communication between components.

#### Code

```js
class ChatRoom {
  showMessage(user, message) {
    console.log(`${user}: ${message}`);
  }
}

class User {
  constructor(name, chatroom) {
    this.name = name;
    this.chatroom = chatroom;
  }
  send(message) {
    this.chatroom.showMessage(this.name, message);
  }
}

const room = new ChatRoom();
const alice = new User("Alice", room);
alice.send("Hello!");
```

#### Pros

- Reduces direct dependencies between components.

#### Cons

- Mediator can become a complex object.

### **6- Memento Pattern**

#### Description

Captures and restores an object's internal state.

#### Code

```js
class Editor {
  constructor() {
    this.content = "";
  }
  setContent(content) {
    this.content = content;
  }
  save() {
    return new Memento(this.content);
  }
  restore(memento) {
    this.content = memento.getContent();
  }
}

class Memento {
  constructor(content) {
    this.content = content;
  }
  getContent() {
    return this.content;
  }
}

const editor = new Editor();
editor.setContent("v1");
const saved = editor.save();
editor.setContent("v2");
editor.restore(saved);
console.log(editor.content); // v1
```

#### Pros

- Allows undo/redo without violating encapsulation.

### **7- Observer Pattern**

#### Description

Notifies multiple objects about changes to another object.

#### Code

```js
class Subject {
  constructor() {
    this.observers = [];
  }
  subscribe(fn) {
    this.observers.push(fn);
  }
  notify(data) {
    this.observers.forEach((fn) => fn(data));
  }
}

const subject = new Subject();
subject.subscribe((data) => console.log("First Received:", data));
subject.subscribe((data) => console.log("Second Received:", data));
subject.subscribe((data) => console.log("Third Received:", data));
subject.notify("Hello!");
```

#### Pros

- Promotes decoupling between emitter and subscribers.
- Great for real-time features.

### **8- State Pattern**

#### Description

Lets an object alter its behavior when its internal state changes.

#### Code

```js
class TrafficLight {
  setState(state) {
    this.state = state;
  }
  change() {
    return this.state.handle();
  }
}

class RedState {
  handle() {
    return "Stop";
  }
}

class GreenState {
  handle() {
    return "Go";
  }
}

const light = new TrafficLight();
light.setState(new GreenState());
console.log(light.change()); // Go
light.setState(new RedState());
console.log(light.change()); // Stop
```

#### Pros

- Organizes behavior by state.
- Avoids huge switch-case logic.

### **9- Strategy Pattern**

#### Description

Enables selecting an algorithm at runtime.

#### Code

```js
class Context {
  setStrategy(strategy) {
    this.strategy = strategy;
  }
  execute(a, b) {
    return this.strategy.calculate(a, b);
  }
}

class AddStrategy {
  calculate(a, b) {
    return a + b;
  }
}

class MultiplyStrategy {
  calculate(a, b) {
    return a * b;
  }
}

const context = new Context();
context.setStrategy(new AddStrategy());
console.log(context.execute(2, 3)); // 5
```

#### Pros

- Easy to switch between algorithms.
- Avoids hardcoded logic.

### **10- Template Pattern**

#### Description _[compare with Prototype](#5--Prototype-Pattern)_

Defines the skeleton of an algorithm but lets subclasses change parts.

#### Code

```js
class Meal {
  prepare() {
    this.getIngredients();
    this.cook();
    this.serve();
  }
  getIngredients() {}
  cook() {}
  serve() {
    console.log("Serving...");
  }
}

class Pasta extends Meal {
  getIngredients() {
    console.log("Getting pasta and sauce");
  }
  cook() {
    console.log("Cooking pasta");
  }
}

const dinner = new Pasta();
dinner.prepare();
```

#### Pros

- Promotes code reuse.
- Helps enforce a process flow.

### **11- Visitor Pattern**

#### Description

Lets you add operations to objects without changing their classes.

#### Code

```js
class Animal {
  accept(visitor) {
    visitor.visit(this);
  }
}

class Dog extends Animal {
  bark() {
    return "Woof!";
  }
}

class SoundVisitor {
  visit(animal) {
    if (animal instanceof Dog) {
      console.log(animal.bark());
    }
  }
}

const dog = new Dog();
dog.accept(new SoundVisitor());
```

#### Pros

- Adds functionality to object structure without modifying it.

#### Cons

- Can make object structure rigid.
