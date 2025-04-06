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

#### Description *[compare with bridge](#2--Bridge-Pattern)*

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

#### Description

Creates objects based on a template of an existing object.

#### Code

```js
const vehiclePrototype = {
  type: "Vehicle",
  getType() {
    return this.type;
  }
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

#### Description *[compare with builder](#4--Builder-Pattern)*

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
    return 'TV is ON';
  }
}

class Radio {
  turnOn() {
    return 'Radio is ON';
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
    this.children.forEach(child => child.display());
  }
}

const file1 = new File('file1.txt');
const file2 = new File('file2.txt');
const folder = new Folder('Documents');

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
    return 'CPU started';
  }
}

class Memory {
  load() {
    return 'Memory loaded';
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
const car1 = factory.getCar('Tesla');
const car2 = factory.getCar('Tesla');
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
    return 'Insufficient funds';
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
    if (!request.user) return 'Unauthorized';
    console.log("user is:", request.user);
    return super.handle(request);
  }
}

class LogHandler extends Handler {
  handle(request) {
    console.log('action:', request.action);
    return super.handle(request);
  }
}

const auth = new AuthHandler();
const logger = new LogHandler();
auth.setNext(logger);

console.log(auth.handle({ user: 'admin', action: 'delete' }));
```

#### Pros

- Decouples sender and handler logic.
- Easy to add/modify handlers.

#### Cons

- Harder to debug with many links in the chain.
