# Lesson 17: Notifications and Background Tasks

## Understanding Local and Push Notifications in iOS

Notifications are an essential feature in iOS that allow apps to communicate important information to users, even when the app is not actively in use. There are two primary types of notifications: **local notifications** and **push notifications**.

### 1. Local Notifications

Local notifications are scheduled by the app and delivered to the user on the same device. They are useful for reminding users about tasks or events.

#### Setting Up Local Notifications

1. **Requesting Permission**:
   - Before sending notifications, you must request permission from the user.
   ```swift
   import UserNotifications

   UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) { granted, error in
       if granted {
           print("Permission granted")
       } else if let error = error {
           print("Error requesting permission: \(error.localizedDescription)")
       }
   }
   ```

2. **Creating a Notification**:
   - Create a notification content object and configure its properties.
   ```swift
   let content = UNMutableNotificationContent()
   content.title = "Reminder"
   content.body = "Don't forget to check your tasks!"
   content.sound = UNNotificationSound.default
   ```

3. **Scheduling the Notification**:
   - Create a trigger for the notification and schedule it.
   ```swift
   let trigger = UNTimeIntervalNotificationTrigger(timeInterval: 60, repeats: false) // Trigger after 60 seconds
   let request = UNNotificationRequest(identifier: "reminder", content: content, trigger: trigger)

   UNUserNotificationCenter.current().add(request) { error in
       if let error = error {
           print("Error scheduling notification: \(error.localizedDescription)")
       }
   }
   ```

### 2. Push Notifications

Push notifications are sent from a remote server to the user's device. They are typically used to inform users of new content, updates, or events when the app is not running.

#### Setting Up Push Notifications

1. **Registering for Push Notifications**:
   - In your app delegate, register for remote notifications.
   ```swift
   import UIKit

   @UIApplicationMain
   class AppDelegate: UIResponder, UIApplicationDelegate {
       func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
           UNUserNotificationCenter.current().delegate = self
           application.registerForRemoteNotifications()
           return true
       }
   }
   ```

2. **Handling Device Token**:
   - Implement the method to receive the device token for push notifications.
   ```swift
   func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
       let tokenString = deviceToken.map { String(format: "%02.2hhx", $0) }.joined()
       print("Device Token: \(tokenString)")
   }
   ```

3. **Receiving Push Notifications**:
   - Implement the delegate method to handle incoming push notifications.
   ```swift
   func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
       let userInfo = response.notification.request.content.userInfo
       print("Received Push Notification: \(userInfo)")
       completionHandler()
   }
   ```

## Implementing Background Tasks and Handling Background Fetch

Background tasks allow your app to perform specific tasks while it is not in the foreground. This is useful for updating content, downloading data, or performing maintenance tasks without interrupting the user experience.

### 1. Background Fetch

Background fetch allows your app to periodically fetch data in the background. This is useful for updating content before the user opens the app.

#### Enabling Background Fetch

1. **Enable Background Modes**:
   - In your Xcode project, go to your target’s settings, select the "Capabilities" tab, and enable "Background Modes." Check the "Background fetch" option.

2. **Implementing Background Fetch**:
   - In your app delegate, implement the method to handle background fetch.
   ```swift
   func application(_ application: UIApplication, performFetchWithCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
       // Fetch data from your server or perform tasks
       fetchData { newData in
           if newData {
               completionHandler(.newData)
           } else {
               completionHandler(.noData)
           }
       }
   }

   func fetchData(completion: @escaping (Bool) -> Void) {
       // Simulate a network fetch
       DispatchQueue.global().async {
           // Perform your data fetching logic here
           // Call completion(true) if new data is fetched, otherwise completion(false)
           completion(true) // Example
       }
   }
   ```

### 2. Using Background Tasks with `BGTaskScheduler`

Starting from iOS 13, you can use the `BGTaskScheduler` to schedule background tasks more efficiently.

#### Setting Up Background Tasks

1. **Registering Background Tasks**:
   - In your app delegate, register your background task identifier.
   ```swift
   import BackgroundTasks

   func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
       BGTaskScheduler.shared.register(forTaskWithIdentifier: "com.example.app.refresh", using: nil) { task in
           self.handleAppRefresh(task: task as! BGAppRefreshTask)
       }
       return true
   }
   ```

2. **Scheduling a Background Task**:
   - Schedule a background task when appropriate, such as after a successful fetch.
   ```swift
   func scheduleAppRefresh() {
       let request = BGAppRefreshTaskRequest(identifier: "com.example.app.refresh")
       request.earliestBeginDate = Date(timeIntervalSinceNow: 15 * 60) // 15 minutes from now

       do {
           try BGTaskScheduler.shared.submit(request)
       } catch {
           print("Could not schedule app refresh: \(error)")
       }
   }
   ```

3. **Handling Background Tasks**:
   - Implement the method to handle the background task.
   ```swift
   func handleAppRefresh(task: BGAppRefreshTask) {
       scheduleAppRefresh() // Schedule the next refresh

       let queue = OperationQueue()
       queue.maxConcurrentOperationCount = 1

       let operation = BlockOperation {
           // Perform your background task here
           // Ensure to call task.setTaskCompleted(success:) when done
           task.setTaskCompleted(success: true)
       }

       task.expirationHandler = {
           queue.cancelAllOperations() // Cancel if the task expires
       }

       queue.addOperation(operation)
   }
   ```

## Conclusion

In this lesson, we explored how to implement local and push notifications in iOS to keep users informed and engaged. We also covered background tasks, including background fetch and using the `BGTaskScheduler` to perform tasks while the app is not in the foreground. Understanding these concepts will help you create more responsive and user-friendly applications that effectively utilize system resources. In the next lesson, we will explore advanced Swift features, where you will learn about protocols, generics, and error handling in Swift.