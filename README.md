# The Swift Style Guide

This guide is based on the following sources:

- [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language)
- [Github Swift style guide](https://github.com/github/swift-style-guide)
- [Ray Wenderlich Swift style guide](https://github.com/raywenderlich/swift-style-guide)
- [SlideShare Swift style guide](https://github.com/SlideShareInc/swift-style-guide)
- [Netguru Swift style guide](https://github.com/netguru/swift-style-guide)

### Purpose of the style guide

This guide is intended to outline the set of shared practices Fueled will apply to current and future Swift Projects in order to implement a uniform Swift code style, readability, consistency and simplicity.

## Table of contents

* [Xcode Preferences](#xcode-preferences)
* [Spacing](#spacing)
* [Naming](#naming)
  * [Prose](#prose)
  * [Class Prefixes](#class-prefixes)
* [Comments](#comments)
* [Code organization](#code-organization)
* [Value types over reference types](#value-types-over-reference-types)
* [Forbidden](#forbidden)


## Xcode Preferences
- Use spaces for tabs and 2 spaces per tab (Change the default in Xcode->Preferences->Text Editing->Indentation)



## Spacing

Indent code with tabs (above modification will automatically implement spaces). End files with an empty line.

Vertical spaces should be used in long methods to separate its name from implementation. You may also want to use vertical spaces to separate logic within a function. Shorter methods (one or two lines) don't need such spacing.

  ![Xcode indent settings](screens/indentation.png)

* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* Tip: You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu). Some of the Xcode template code will have 4-space tabs hard coded, so this is a good way to fix that.

**Preferred:**
```swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
```

**Not Preferred:**
```swift
if user.isHappy
{
    // Do something
}
else {
    // Do something else
}
```

## Naming

Use descriptive names with camel case for classes, methods, variables, etc. Class names should be capitalized, while method names and variables should start with a lower case letter.

**Preferred:**

```swift
private let maximumWidgetCount = 100

class WidgetContainer {
  var widgetButton: UIButton
  let widgetHeightPercentage = 0.85
}
```

**Not Preferred:**

```swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
  var wBut: UIButton
  let wHeightPct = 0.85
}
```

For functions and init methods, prefer named parameters for all arguments unless the context is very clear. Include external parameter names if it makes function calls more readable.

```swift
func dateFromString(dateString: String) -> NSDate
func convertPointAt(#column: Int, #row: Int) -> CGPoint
func timedAction(#delay: NSTimeInterval, perform action: SKAction) -> SKAction!

// would be called like this:
dateFromString("2014-03-14")
convertPointAt(column: 42, row: 13)
timedAction(delay: 1.0, perform: someOtherAction)
```

For methods, follow the standard Apple convention of referring to the first parameter in the method name:

```swift
class Guideline {
  func combineWithString(incoming: String, options: Dictionary?) { ... }
  func upvoteBy(amount: Int) { ... }
}
```

### Enumerations

Use UpperCamelCase for enumeration values:

```swift
enum Shape {
  case Rectangle
  case Square
  case Triangle
  case Circle
}
```

### Prose

When referring to functions in prose (tutorials, books, comments) include the required parameter names from the caller's perspective or `_` for unnamed parameters.

> Call `convertPointAt(column:row:)` from your own `init` implementation.
>
> If you call `dateFromString(_:)` make sure that you provide a string with the format "yyyy-MM-dd".
>
> If you call `timedAction(delay:perform:)` from `viewDidLoad()` remember to provide an adjusted delay value and an action to perform.
>
> You shouldn't call the data source method `tableView(_:cellForRowAtIndexPath:)` directly.

When in doubt, look at how Xcode lists the method in the jump bar – our style here matches that.

![Methods in Xcode jump bar](screens/xcode-jump-bar.png)

### Class Prefixes

Swift types are automatically namespaced by the module that contains them and you should not add a class prefix. If two names from different modules collide you can disambiguate by prefixing the type name with the module name.

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```




## Comments

Use comments to describe why something is written as it is, or working like it does. Remember that code should be self-documenting, so use comments only if necessary.

If you decide to add comments, keep them up-to-date. Unmaintained comments should be removed.

## Code organization

Source files should have the following organization.

```swift
// 1. imports

import MoneyKit

// 2. classes, structs, enums

class Wallet {

    // 2.1. public, internal properties

    let cards: [Card]
    private(set) var cash: Cash
    
    // 2.2. private properties
    
    unowned private let owner: Person
    
    // 2.3. initializers
    
    init(cash: Cash, cards: [Card], owner: Person)
    
    // 2.4. public
    
    func affordsTransaction(transaction: Transaction) -> Bool
    
    // 2.5 internal functions

    func calculateTransactionWithCard(card: Card) -> Transaction
    
    // 2.6. private functions
    
    private func cardWithSuffiecientCash(cash: Cash) -> Card?
    
}

// 3. extensions, protocol implementations

extension Wallet: Printable {

    var description: String {
        return "\(owner.name) has \(cash) cash and \(cards.count) cards"
    }

}
```

Such organization helps others to reach important content earlier. It also saves time, confusion and improves readability.

## Value types over reference types

Value types, such as structs, enums and tuples are usually simpler than reference types (classes) and they're always passed by copying. This means they're independent thread-safe instances, which makes code simpler and safer. In addition, `let` and `var` work as expected.

Value types are great for representing **data** in the app.

```swift
struct Country {
    let name: String
    let capital: City
}
```

On the other hand, reference types, such as classes, are passed by referencing the same mutable instance in memory. There are several cases when classes should be preferred. The first case emerges when subclassing current Objective-C classes. The second one is when you need to use reference types with mutable state, or if you need to perform some actions on deinitialization.

Reference types are great to represent **behavior** in the app.

```swift
class FileStream {
    let file: File
    var currentPosition: StreamPosition
}
```

Keep in mind that inheritance is not a sufficient argument to use classes. You should try to compose your types using protocols.

```swift
// preferred

protocol Polygon {
    var numberOfSides: Int { get }
}

struct Triangle: Polygon {
    let numberOfSides = 3
}
```

```swift
// not preferred

class Polygon {
    let numberOfSides: Int
    init(numberOfSides: Int)
}

class Triangle: Polygon {
    init() {
        super.init(numberOfSides: 3)
    }
}
```

## Forbidden

Types should never have prefixes because their names are already implicitly mangled and prefixed by their module name.

Semicolons are obfuscative and should never be used. Statements can be distributed in different lines.

Rewriting standard library functionalities should never take place. Your code will most probably be less optimized and more confusing to other developers.
