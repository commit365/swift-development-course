# Lesson 3: Object-Oriented Programming in Swift

## Understanding Classes, Objects, and Inheritance in Swift

Object-oriented programming (OOP) is a programming paradigm that uses "objects" to represent data and methods. Swift is a fully object-oriented language, allowing you to create and manage classes and objects effectively. In this lesson, we will cover the basics of classes, objects, and inheritance in Swift.

### Classes and Objects

1. **Classes**:
   - A class is a blueprint for creating objects. It defines properties (attributes) and methods (functions) that the objects created from the class can use.
   - To define a class in Swift, use the `class` keyword.
   ```swift
   class Car {
       var make: String
       var model: String
       var year: Int

       // Initializer
       init(make: String, model: String, year: Int) {
           self.make = make
           self.model = model
           self.year = year
       }

       // Method
       func displayInfo() {
           print("Car: \(year) \(make) \(model)")
       }
   }
   ```

2. **Creating Objects**:
   - Once you have defined a class, you can create instances (objects) of that class.
   ```swift
   let myCar = Car(make: "Toyota", model: "Camry", year: 2020)
   myCar.displayInfo() // Output: Car: 2020 Toyota Camry
   ```

### Inheritance

Inheritance allows one class to inherit the properties and methods of another class. This promotes code reuse and establishes a hierarchical relationship between classes.

1. **Base Class and Subclass**:
   - A base class (or parent class) is the class being inherited from, while a subclass (or child class) is the class that inherits from the base class.
   ```swift
   class Vehicle {
       var wheels: Int

       init(wheels: Int) {
           self.wheels = wheels
       }

       func displayWheels() {
           print("This vehicle has \(wheels) wheels.")
       }
   }

   class Motorcycle: Vehicle {
       var hasSidecar: Bool

       init(wheels: Int, hasSidecar: Bool) {
           self.hasSidecar = hasSidecar
           super.init(wheels: wheels) // Call the initializer of the base class
       }

       func displayInfo() {
           displayWheels()
           print("Has sidecar: \(hasSidecar)")
       }
   }

   let myMotorcycle = Motorcycle(wheels: 2, hasSidecar: false)
   myMotorcycle.displayInfo()
   // Output: This vehicle has 2 wheels.
   // Has sidecar: false
   ```

## Protocols and Extensions in Swift for Code Organization

Swift provides powerful features like protocols and extensions that help organize and structure your code effectively.

### Protocols

A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or functionality. Classes, structures, and enumerations can adopt protocols to provide specific implementations.

1. **Defining a Protocol**:
   - You can define a protocol using the `protocol` keyword.
   ```swift
   protocol Drivable {
       var speed: Double { get set }
       func accelerate()
       func brake()
   }
   ```

2. **Adopting a Protocol**:
   - A class can adopt a protocol by implementing its requirements.
   ```swift
   class Bicycle: Drivable {
       var speed: Double = 0.0

       func accelerate() {
           speed += 5.0
           print("Bicycle speed: \(speed) km/h")
       }

       func brake() {
           speed = max(0, speed - 5.0)
           print("Bicycle speed: \(speed) km/h")
       }
   }

   let myBicycle = Bicycle()
   myBicycle.accelerate() // Output: Bicycle speed: 5.0 km/h
   myBicycle.brake()      // Output: Bicycle speed: 0.0 km/h
   ```

### Extensions

Extensions allow you to add new functionality to existing classes, structures, enumerations, or protocols without modifying their original implementation. This is useful for organizing code and adding features in a modular way.

1. **Adding Methods with Extensions**:
   - You can extend a class to add new methods.
   ```swift
   extension Car {
       func startEngine() {
           print("\(make) \(model)'s engine started.")
       }
   }

   myCar.startEngine() // Output: Toyota Camry's engine started.
   ```

2. **Adding Computed Properties**:
   - You can also add computed properties through extensions.
   ```swift
   extension Car {
       var description: String {
           return "\(year) \(make) \(model)"
       }
   }

   print(myCar.description) // Output: 2020 Toyota Camry
   ```

## Conclusion

In this lesson, we explored the fundamentals of object-oriented programming in Swift, including classes, objects, and inheritance. We also learned about protocols and extensions, which help organize and structure code effectively. Understanding these concepts is vital for developing robust and maintainable applications in Swift. In the next lesson, we will delve into user interface design using UIKit, where you'll learn how to create engaging and interactive app interfaces.