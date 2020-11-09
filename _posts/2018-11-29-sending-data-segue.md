---
layout: post
title: Sending Data via Segue
author: Vikash Tank
---

When creating transitions between different view controllers in your Swift app storyboard, you may want to send data across each segue. This post outlines how you would typically do this in a Swift app, and provides an extension that improves on this further.

In the ‘source’ view controller, a ‘prepare’ method must be created that defines the data sent to the ‘destination’ view controller. This can be seen in the code below.

```swift
import UIKit

class ViewController: UIViewController {

    @IBAction func onClick(_ sender: Any) {
        // Create a segue in the story board with the identifier below
        self.performSegue(withIdentifier: "segueWithData", sender: self)
    }

    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        switch segue.destination {
        case let viewController as DestinationViewController:
            // Send a string to the destination view controller
            viewController.text = "set by source controller"
        default:
            break
        }
    }

}
```

This method accepts the ‘destination’ view controller and allows you give the destination view controller the required data. A switch statement can be used when working with the many different segues originating from the ‘source’ view controller.

When working with multiple segues, the prepare method can become large and clunky. A better option, which is more readable, is to pass the data into the segue so that you don't have to handle all the data in the prepare method, for all the different segues. The code below extends on the UIViewController and does exactly this.


```swift
import UIKit

typealias PrepareHandler = (UIViewController) -> ()

class SegueManager {

    var viewController: UIViewController

    var handler: PrepareHandler?

    init(_ viewController: UIViewController) {
        self.viewController = viewController
    }

    func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if let handler = self.handler {
            handler(segue.destination)
            self.handler = nil
        }
    }

    func performSegue(withIdentifier segue: String, sender: Any?, prepareHandler: @escaping PrepareHandler) {
        self.handler = prepareHandler
        self.viewController.performSegue(withIdentifier: segue, sender: sender)
        self.handler = nil
    }

}
```

The above code is used in a view controller in the following way:

```swift
import UIKit

class DestinationViewController: UIViewController {

    @IBOutlet weak var textLabel: UILabel!

    lazy var segueManager = SegueManager(self)

    var text: String = ""

    override func viewDidLoad() {
        super.viewDidLoad()
        // when the view has loaded set the text to the
        // text received from the source view controller
        self.textLabel.text = self.text

    }

    @IBAction func onClick(_ sender: Any) {
        self.segueManager.performSegue(withIdentifier: "secondDestination", sender: self) { nextController in
            let controller = nextController as! SecondDestinationViewController
            controller.text = "Wahey the new method works"
        }
    }

    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        self.segueManager.prepare(for: segue, sender: sender)
    }

}
```

This is a solution that has worked for me and the [full code is available here](https://github.com/orycion/ios-segue-example/tree/master/SeguesWithData). I would love to hear your thoughts on this.
