A simple example of how somebody can use semaphore in Swift


let semaphore = DispatchSemaphore(value: 2)

func accessSharedResource(taskId: Int) {
    print("Task \(taskId) waiting to access resource.")
    
    // Wait to acquire the semaphore. If the semaphore count is 0, this will block until it becomes available.
    semaphore.wait()
    
    // Simulate work
    print("Task \(taskId) is now accessing the resource.")
    Thread.sleep(forTimeInterval: 2)  // Simulating work by sleeping for 2 seconds
    
    // Release the semaphore, allowing another task to access the resource.
    semaphore.signal()
    
    print("Task \(taskId) has finished and released the resource.")
}

// Create multiple tasks that will try to access the shared resource
DispatchQueue.global(qos: .background).async {
    accessSharedResource(taskId: 1)
}

DispatchQueue.global(qos: .background).async {
    accessSharedResource(taskId: 2)
}

DispatchQueue.global(qos: .background).async {
    accessSharedResource(taskId: 3)
}

DispatchQueue.global(qos: .background).async {
    accessSharedResource(taskId: 4)
}

// Keep the main thread alive so async tasks can complete
RunLoop.main.run()
