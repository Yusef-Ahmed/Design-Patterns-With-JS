# Design Patterns With JS

## Creational Patterns

### Singleton Pattern

Ensures that a class has only one instance and provides a global point of access to it.

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

### Factory Pattern

Provides an interface for creating objects but allows subclasses to alter the type of objects created.

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

### Abstract Factory Pattern

Provides an interface for creating families of related objects without specifying concrete classes.

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

### Builder Pattern

Separates object construction from its representation.

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

### Prototype Pattern

Creates objects based on a template of an existing object.

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

## Behavioral Patterns
