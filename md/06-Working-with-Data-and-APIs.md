# Lesson 6: Working with Data and APIs

## Fetching Data from Web APIs Using URLSession

In modern app development, fetching data from web APIs is a common task. `URLSession` is a powerful class in Swift that allows you to perform network requests and retrieve data from the internet. In this lesson, we will learn how to use `URLSession` to fetch data from a web API.

### Setting Up a Network Request

1. **Creating a URL**:
   - First, you need to create a `URL` object that points to the web API endpoint you want to access.
   ```swift
   guard let url = URL(string: "https://api.example.com/data") else {
       print("Invalid URL")
       return
   }
   ```

2. **Creating a URLSession**:
   - You can create a `URLSession` instance, which is responsible for managing network requests.
   ```swift
   let session = URLSession.shared
   ```

3. **Creating a Data Task**:
   - A data task is created to fetch the data from the specified URL. You can use a closure to handle the response.
   ```swift
   let dataTask = session.dataTask(with: url) { data, response, error in
       // Handle the response here
   }
   ```

4. **Starting the Data Task**:
   - Finally, you need to start the data task.
   ```swift
   dataTask.resume()
   ```

### Example: Fetching Data from an API

Here’s a complete example of fetching data from a web API:

```swift
import UIKit

class DataViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        fetchData()
    }

    func fetchData() {
        guard let url = URL(string: "https://api.example.com/data") else {
            print("Invalid URL")
            return
        }

        let session = URLSession.shared
        let dataTask = session.dataTask(with: url) { data, response, error in
            // Check for errors
            if let error = error {
                print("Error fetching data: \(error.localizedDescription)")
                return
            }

            // Check for valid data
            guard let data = data else {
                print("No data received")
                return
            }

            // Process the data
            self.parseJSON(data)
        }

        dataTask.resume()
    }

    func parseJSON(_ data: Data) {
        // This function will be implemented in the next section
    }
}
```

## Parsing JSON Data and Handling Asynchronous Tasks

Once you have fetched data from a web API, the next step is to parse the JSON data into a format that your app can use. Swift provides powerful tools for working with JSON, including the `Codable` protocol.

### Understanding JSON and Codable

1. **JSON Structure**:
   - JSON (JavaScript Object Notation) is a lightweight data interchange format that is easy to read and write. It consists of key-value pairs and can represent arrays and nested objects.
   - Example JSON:
   ```json
   {
       "id": 1,
       "name": "John Doe",
       "email": "john.doe@example.com"
   }
   ```

2. **Defining a Codable Struct**:
   - You can define a struct that conforms to the `Codable` protocol to represent the data structure of the JSON.
   ```swift
   struct User: Codable {
       let id: Int
       let name: String
       let email: String
   }
   ```

### Parsing JSON Data

Now, let’s implement the `parseJSON` function to decode the JSON data into Swift objects:

```swift
func parseJSON(_ data: Data) {
    let decoder = JSONDecoder()

    do {
        let users = try decoder.decode([User].self, from: data)
        for user in users {
            print("User ID: \(user.id), Name: \(user.name), Email: \(user.email)")
        }
    } catch {
        print("Error decoding JSON: \(error.localizedDescription)")
    }
}
```

### Handling Asynchronous Tasks

Network requests are asynchronous, meaning they run in the background and do not block the main thread. This is important for maintaining a responsive user interface. 

1. **Updating the UI on the Main Thread**:
   - If you need to update the UI after fetching data, make sure to do it on the main thread. You can use `DispatchQueue.main.async` for this purpose.
   ```swift
   DispatchQueue.main.async {
       // Update the UI here
   }
   ```

### Complete Example

Here’s the complete example of fetching and parsing JSON data:

```swift
import UIKit

struct User: Codable {
    let id: Int
    let name: String
    let email: String
}

class DataViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        fetchData()
    }

    func fetchData() {
        guard let url = URL(string: "https://api.example.com/users") else {
            print("Invalid URL")
            return
        }

        let session = URLSession.shared
        let dataTask = session.dataTask(with: url) { data, response, error in
            if let error = error {
                print("Error fetching data: \(error.localizedDescription)")
                return
            }

            guard let data = data else {
                print("No data received")
                return
            }

            self.parseJSON(data)
        }

        dataTask.resume()
    }

    func parseJSON(_ data: Data) {
        let decoder = JSONDecoder()

        do {
            let users = try decoder.decode([User].self, from: data)
            DispatchQueue.main.async {
                // Update the UI with users data
                for user in users {
                    print("User ID: \(user.id), Name: \(user.name), Email: \(user.email)")
                }
            }
        } catch {
            print("Error decoding JSON: \(error.localizedDescription)")
        }
    }
}
```

## Conclusion

In this lesson, we learned how to fetch data from web APIs using `URLSession` and parse JSON data using the `Codable` protocol. We also discussed the importance of handling asynchronous tasks to maintain a responsive user interface. In the next lesson, we will explore persistence and Core Data, where you will learn how to store and manage data locally in your iOS applications.