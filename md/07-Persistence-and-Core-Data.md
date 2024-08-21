# Lesson 7: Persistence and Core Data

## Introduction to Data Persistence in iOS Using UserDefaults and Core Data

Data persistence is an essential aspect of app development, allowing applications to save and retrieve data even after they are closed. In iOS, there are several ways to persist data, with `UserDefaults` and `Core Data` being two of the most commonly used methods.

### UserDefaults

`UserDefaults` is a simple key-value storage system that is ideal for saving small amounts of data, such as user preferences or settings. It is easy to use and provides a straightforward way to store basic data types.

1. **Storing Data**:
   ```swift
   let defaults = UserDefaults.standard
   defaults.set("John Doe", forKey: "username")
   defaults.set(25, forKey: "age")
   ```

2. **Retrieving Data**:
   ```swift
   let username = defaults.string(forKey: "username") ?? "Guest"
   let age = defaults.integer(forKey: "age")
   ```

3. **Removing Data**:
   ```swift
   defaults.removeObject(forKey: "username")
   ```

### Core Data

`Core Data` is a powerful framework that provides an object graph and persistence framework for managing data in iOS applications. It is suitable for complex data models and allows you to perform advanced queries and data relationships.

1. **When to Use Core Data**:
   - Use Core Data when you need to manage large amounts of structured data, support complex data models, or require advanced querying capabilities.

## Setting Up a Core Data Stack

To use Core Data in your application, you need to set up a Core Data stack, which consists of the following components:

1. **Managed Object Model**: Defines the data model.
2. **Persistent Store Coordinator**: Manages the persistent stores.
3. **Managed Object Context**: Represents a single object space for managing data.

### Step-by-Step Setup

1. **Add Core Data to Your Project**:
   - When creating a new project in Xcode, check the "Use Core Data" option. This will automatically set up the Core Data stack for you.

2. **Accessing the Core Data Stack**:
   - In your `AppDelegate`, you will find the Core Data stack setup. Here’s an example of how it looks:
   ```swift
   lazy var persistentContainer: NSPersistentContainer = {
       let container = NSPersistentContainer(name: "YourModelName")
       container.loadPersistentStores(completionHandler: { (storeDescription, error) in
           if let error = error as NSError? {
               fatalError("Unresolved error \(error), \(error.userInfo)")
           }
       })
       return container
   }()
   ```

3. **Creating a Managed Object Context**:
   - You can access the managed object context from the persistent container:
   ```swift
   let context = persistentContainer.viewContext
   ```

## Basic CRUD Operations

CRUD stands for Create, Read, Update, and Delete, which are the four basic operations for managing data. Here’s how to perform these operations using Core Data.

### 1. Creating Data

To create a new object, you first need to create an instance of the entity and set its properties.

```swift
func createUser(name: String, age: Int) {
    let context = (UIApplication.shared.delegate as! AppDelegate).persistentContainer.viewContext
    let user = User(context: context) // Assuming "User" is your entity name
    user.name = name
    user.age = Int16(age)

    do {
        try context.save() // Save the context
        print("User saved successfully!")
    } catch {
        print("Failed to save user: \(error.localizedDescription)")
    }
}
```

### 2. Reading Data

To fetch data from Core Data, you typically use a `NSFetchRequest`.

```swift
func fetchUsers() -> [User] {
    let context = (UIApplication.shared.delegate as! AppDelegate).persistentContainer.viewContext
    let fetchRequest: NSFetchRequest<User> = User.fetchRequest() // Assuming "User" is your entity name

    do {
        let users = try context.fetch(fetchRequest)
        return users
    } catch {
        print("Failed to fetch users: \(error.localizedDescription)")
        return []
    }
}
```

### 3. Updating Data

To update an existing object, you first fetch it, modify its properties, and then save the context.

```swift
func updateUser(user: User, newName: String, newAge: Int) {
    let context = (UIApplication.shared.delegate as! AppDelegate).persistentContainer.viewContext
    user.name = newName
    user.age = Int16(newAge)

    do {
        try context.save() // Save the updated context
        print("User updated successfully!")
    } catch {
        print("Failed to update user: \(error.localizedDescription)")
    }
}
```

### 4. Deleting Data

To delete an object, you fetch it, remove it from the context, and then save the context.

```swift
func deleteUser(user: User) {
    let context = (UIApplication.shared.delegate as! AppDelegate).persistentContainer.viewContext
    context.delete(user)

    do {
        try context.save() // Save the context after deletion
        print("User deleted successfully!")
    } catch {
        print("Failed to delete user: \(error.localizedDescription)")
    }
}
```

## Conclusion

In this lesson, we explored data persistence in iOS using `UserDefaults` for simple key-value storage and `Core Data` for managing complex data models. We learned how to set up a Core Data stack and perform basic CRUD operations to create, read, update, and delete data. Understanding these concepts is crucial for building robust iOS applications that can store and manage data effectively. In the next lesson, we will delve into networking and asynchronous programming, where you will learn how to handle network requests and manage asynchronous tasks in your applications.