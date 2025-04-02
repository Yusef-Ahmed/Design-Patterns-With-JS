# **Design Patterns With JS**

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

#### Description

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

### Adapter Pattern (Wrapper)

Allows two incompatible interfaces to work together by providing a wrapper that converts one interface into another.

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

### Bridge Pattern

## Behavioral Patterns
