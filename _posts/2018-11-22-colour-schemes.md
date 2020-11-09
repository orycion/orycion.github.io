---
layout: post
title: Colour schemes in your app
author: Thomas Leese
---

When you come to change the design of your app and you want to update all the colours everywhere, you realise that it’s very easy to forget which colours you’ve used and where. Updating all the colours correctly will end up being very time consuming and no doubt a few odd shades will be left out.

Instead of setting custom colours in your storyboard and XIBs using their red, green and blue values, you can use a feature of UIKit called Named Colours. You define all the different colours your app uses as assets in one place and then you can reference them by name in your storyboards, XIBs and in your code.

![Creating a named colour](/assets/posts/2018-11-22-colour-schemes/create-named-colour.gif)

This allows you to quickly and easily change the colours of your app in one place. To use the colours in the Interface Builder, you can go to any colour option of a view and select it from the list.

![Using the colours in the Interface Builder](/assets/posts/2018-11-22-colour-schemes/using-interface-builder.gif)

To use these colours in your code, you can write an extension of the UIColor class.

```swift
extension UIColor {
    static let accent = UIColor(
        named: "Accent",
        in: Bundle(for: AppDelegate.self),
        compatibleWith: nil
    )
}
```

It’s important to include the bundle reference to make sure that the colours can load properly from the Interface Builder, for example in your custom view classes. Using this new colour in your code is then very simple.

```swift
self.view.backgroundColor = .accent
```

I’m sure you can see how this makes the colours in your project much more maintainable and easy to manage. You can find all the code and [an example project on GitHub](https://github.com/orycion/ios-named-colours-example).
