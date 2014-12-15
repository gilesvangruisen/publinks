# Publinks!

Publinks is a simple and safe implementation of the publish-subscribe pattern in Swift.

# Example

Publinks can be used anywhere for broadcasting a value or optional value to a set of subscribers. You can find some example `Publinks-Example.playground`. Please note that you must open the playground from within the Xcode project and build both the module and `Publinks-Example` targets for the playground.

`ViewModel` holds the publink to which `MyTableViewCell` will subscribe.`ViewModel` holds a reference to the `Model`. Once this model has been set, we want to format data and publish the `ViewModel` to publink subscribers.
```
class ViewModel {
  var publink = Publink()

  var model: Model {
    didSet {
      reload() // Properly formats and sets data properties based on new model
      publink.publish(self) // Sends the newly formatted ViewModel to any subscribers
    }
  }
}
```

`MyTableViewCell` holds a reference to the `ViewModel`. As soon as this `ViewModel` is set, we will subscribe to its publink, setting the values of out text label in the subscription block.
```
class MyTableViewCell: UITableViewCell {
  var viewModel: ViewModel! {
    willSet {

      // Subscribe in property observer, newValue is thew new noteViewModel
      newValue.publink.subscribe { (object: AnyObject?) -> () in

        if let newNoteViewModel = object as? NoteViewModel {
          // Set text label
          self.textLabel.text = newNoteViewModel.text
        }

      } // subscribe
    } // willSet
  }
}
