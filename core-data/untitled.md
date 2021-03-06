# Core Data Helpers

## Getting Super Powers

Becoming a super hero is a fairly straight forward process:

{% code title="CoreDataStack.swift" %}
```swift
import CoreData
import UIKit

class CoreDataStack {

    static let shared = CoreDataStack(model: "Model")

    var model: String

    init(model: String) {
        self.model = model
    }

    lazy var context: NSManagedObjectContext = {
        self.persistentContainer.viewContext
    }()

    private lazy var persistentContainer: NSPersistentContainer = {
        /*
         The persistent container for the application. This implementation
         creates and returns a container, having loaded the store for the
         application to it. This property is optional since there are legitimate
         error conditions that could cause the creation of the store to fail.
        */
        let container = NSPersistentContainer(name: self.model)
        container.loadPersistentStores(completionHandler: { (storeDescription, error) in
            if let error = error as NSError? {
                // Replace this implementation with code to handle the error appropriately.
                // fatalError() causes the application to generate a crash log and terminate. You should not use this function in a shipping application, although it may be useful during development.

                /*
                 Typical reasons for an error here include:
                 * The parent directory does not exist, cannot be created, or disallows writing.
                 * The persistent store is not accessible, due to permissions or data protection when the device is locked.
                 * The device is out of space.
                 * The store could not be migrated to the current model version.
                 Check the error message to determine what the actual problem was.
                 */
                fatalError("Unresolved error \(error), \(error.userInfo)")
            }
        })
        return container
    }()
}
```
{% endcode %}

{% code title="CoreDataStack+CRUD.swift" %}
```swift
extension CoreDataStack {

    func fetch<T: NSFetchRequestResult>(_ request: NSFetchRequest<T>) -> [T]  {
        do {
            let items = try context.fetch(request)
            return items
        }
        catch let error as NSError {
            print("Could not fetch \(error), \(error.userInfo)")
            return [T]()
        }
    }

    func save() {
        if context.hasChanges {
            do {
                try context.save()
            } catch let error as NSError {
                print("Unresolved error \(error), \(error.userInfo)")
            }
        }
    }

    func delete(_ item: NSManagedObject) {
        context.delete(item)
    }
}
```
{% endcode %}

{% code title="UIViewController+CoreDataStack.swift" %}
```swift
extension UIViewController {

    var context: NSManagedObjectContext {
        CoreDataStack.shared.context
    }

    // MARK: - CRUD

    func fetch<T: NSFetchRequestResult>(_ request: NSFetchRequest<T>) -> [T]  {
        CoreDataStack.shared.fetch(request)
    }

    func save() {
        CoreDataStack.shared.save()
    }

    func delete(_ item: NSManagedObject) {
        CoreDataStack.shared.delete(item)
    }
}
```
{% endcode %}

{% hint style="info" %}
Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Once you're strong enough, save the world:

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}

