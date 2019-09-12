# Result

## How to fire success or failure

```swift
completion(Swift.Result.success(()))
completion(Swift.Result.failure(error))
```

## How they work

An enum representing success or failure in an operation. If success return the success. If failure return the failure.

```swift
/// A value that represents either a success or a failure, including an
/// associated value in each case.
public enum Result<Success, Failure> where Failure : Error {

    /// A success, storing a `Success` value.
    case success(Success)

    /// A failure, storing a `Failure` value.
    case failure(Failure)
```

Example

```swift
func refreshOrders(completion: @escaping ((Result<[OrderItem], Error>) -> Void)) {

   // Success
   self.selectedAccount?.orderItems = NSSet(array: collection)
   completion(Result.success(collection))             

   // Error
   completion(Result.failure(ActivationError.coreData))
}
```

And then when you call you pull the payload or error from the result like this by defining lets

```swift
fetchUnreadCount1(from: "https://www.hackingwithswift.com") { result in
    switch result {
    case .success(let count):
        print("\(count) unread messages.")
    case .failure(let error):
        print(error.localizedDescription)
    }
}
```

Alternatively

```swift
session.refreshOrders(completion: { (result) in
    subject = try? result.get().first
})

do {
    let orderItems = try? result.get()
} catch error {
    print("Error fetching value: \(error)")
}
```

### How to return a Void Success Result

```swift
    public func activate(completion: @escaping (Result<Void, Error>) -> Void) {
        completion(Swift.Result.success(()))
    }
```

"Void" doesn't mean "returns nothing." It's a type in Swift, a type that has exactly one value: the empty tuple (). That also happens to be the type:

```swift
public typealias Void = ()
```

As a matter of convention, we use Void to mean the type, and () to mean the value. The one thing that's a bit strange about using Void this way in a Result is the syntax. You wind up with something like:

```swift
return .success(())
```

### How to inline an error

```swift
guard error == nil else {
    enum ForcedActivationError: Error { case failure }
    completion(Swift.Result.failure(ForcedActivationError.failure))
    return
}
```

### Links that help

- [Void Success Result](https://stackoverflow.com/questions/44067192/how-to-handle-void-success-case-with-result-lib-success-failure)