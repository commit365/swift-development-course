# Lesson 2: Swift Programming Basics

## Introduction to Swift Syntax and Basic Programming Concepts

Swift is a powerful and intuitive programming language developed by Apple for iOS, macOS, watchOS, and tvOS development. It is designed to be easy to read and write, making it an excellent choice for both beginners and experienced developers. In this lesson, we will cover the basic syntax of Swift and introduce fundamental programming concepts.

### Swift Syntax

1. **Comments**:
   - Comments are used to explain code and make it more readable. In Swift, you can create single-line comments using `//` and multi-line comments using `/* ... */`.
   ```swift
   // This is a single-line comment
   /* This is a 
      multi-line comment */
   ```

2. **Printing to the Console**:
   - You can use the `print()` function to output text to the console.
   ```swift
   print("Hello, World!")
   ```

3. **Variables and Constants**:
   - In Swift, you can declare variables using the `var` keyword and constants using the `let` keyword. Variables can be changed after they are initialized, while constants cannot.
   ```swift
   var name = "Alice" // Variable
   let age = 30      // Constant
   ```

4. **Basic Data Types**:
   - Swift has several built-in data types, including:
     - `Int`: Integer numbers (e.g., 1, 2, 3)
     - `Double`: Floating-point numbers (e.g., 3.14, 2.0)
     - `String`: Text (e.g., "Hello")
     - `Bool`: Boolean values (`true` or `false`)

## Data Types, Variables, and Control Flow in Swift

### Data Types

Understanding data types is crucial for effective programming. Here are some common data types in Swift:

1. **Integers**:
   - Used to represent whole numbers.
   ```swift
   var score: Int = 100
   ```

2. **Floating-Point Numbers**:
   - Used for decimal numbers.
   ```swift
   var pi: Double = 3.14159
   ```

3. **Strings**:
   - Used to represent text. Strings can be created using double quotes.
   ```swift
   var greeting: String = "Hello, World!"
   ```

4. **Booleans**:
   - Used to represent true or false values.
   ```swift
   var isGameOver: Bool = false
   ```

### Variables

Variables are used to store data that can change. Here’s how to declare and use variables in Swift:

- **Declaring Variables**:
  ```swift
  var playerName: String = "Bob"
  var playerScore: Int = 0
  ```

- **Changing Variable Values**:
  ```swift
  playerScore = 10 // Update the score
  ```

### Control Flow

Control flow statements allow you to dictate the flow of your program based on certain conditions. The most common control flow statements in Swift are `if`, `else`, and `switch`.

1. **If Statements**:
   - Used to execute code based on a condition.
   ```swift
   if playerScore >= 10 {
       print("You have enough points!")
   } else {
       print("Keep playing to earn more points.")
   }
   ```

2. **Switch Statements**:
   - Used to execute different code based on the value of a variable.
   ```swift
   let level = 2
   switch level {
   case 1:
       print("Beginner Level")
   case 2:
       print("Intermediate Level")
   case 3:
       print("Advanced Level")
   default:
       print("Unknown Level")
   }
   ```

3. **For Loops**:
   - Used to repeat a block of code a specific number of times.
   ```swift
   for i in 1...5 {
       print("Iteration \(i)")
   }
   ```

4. **While Loops**:
   - Used to repeat a block of code while a condition is true.
   ```swift
   var count = 0
   while count < 5 {
       print("Count is \(count)")
       count += 1
   }
   ```

## Conclusion

In this lesson, we introduced the basics of Swift syntax and fundamental programming concepts, including variables, data types, and control flow. Understanding these concepts is essential as you continue to build iOS applications using Swift. In the next lesson, we will explore object-oriented programming in Swift, where you'll learn about classes, objects, and inheritance.