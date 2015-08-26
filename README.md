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
  * [Functions and Arguments](#functions-and-arguments)
  * [Enumerations](#enumerations)
  * [Prose](#prose)
  * [Class Prefixes](#class-prefixes)
* [Comments](#comments)
* [Code organization](#code-organization)
  * [File](#file-code-organization)
  * [Project](#project-code-organization)
* [Value types over reference types](#value-types-over-reference-types)
* [Forbidden](#forbidden)


## Xcode Preferences
- Use spaces for tabs and 2 spaces per tab (Change the default in Xcode->Preferences->Text Editing->Indentation)



## Spacing

Indent code with tabs (above modification will automatically implement spaces). End files with an empty line.

Vertical spaces should be used in long methods to separate its name from implementation. You may also want to use vertical spaces to separate logic within a function. Shorter methods (one or two lines) don't need such spacing. 
*Make liberal use of vertical whitespace to divide code into logical chunks.
*Don’t leave trailing whitespace.
*Not even leading indentation on blank lines.

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


### Functions and Arguments

For functions and init methods, prefer named parameters for all arguments unless the context is very clear. Include external parameter names if it makes function calls more readable.

Function names should be as descriptive and meaningful as possible. Try to express its intent in the name, by keeping it compact at the same time.
Arguments should also be descriptive. Remember that you can use argument labels, which may be more meaningful to a user.

**Preferred**

```swift
func convert(#point: Point, toView view: View) -> Point
```
**Not Preferred**

```swift
func convertPoint(point: Point, fromView view: View) -> Point
```

Use default values for arguments where a function expects any value or some specific value most of the time. If a particular argument is not required for a function, it's good to make it optional and `nil` by default.

Follow the standard Apple convention of referring to the first parameter in the method name in cases where there is just a single argument or when adding parameters to methods only increases verbosity


```swift
class Guideline {
  func combineWithString(incoming: String, options: Dictionary?) { ... }
  func upvoteBy(amount: Int) { ... }
}
```

Keep short function declarations on one line including the opening brace:

```swift
var isSomeStuff:Bool = true

func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
    return isSomeStuff
}
```

For functions with long signatures, add line breaks at appropriate points 
and add an extra indent on subsequent lines:

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
    translateConstant: Int, comment: String) -> Bool {
        // reticulate code goes here
           return isSomeStuff
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

It is strongly misadvised to name suffix your types with words like Manager, Helper or Utility because they're meaningless and their role can be easily misinterpreted.
On the other hand developer discretion is best.


## Comments

Use comments to describe why something is written as it is, or working like it does. Remember that code should be self-documenting, so use comments only if necessary.

If you decide to add comments, keep them up-to-date. Unmaintained comments should be removed.

## Code organization

### File Code Organization
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

### Project Code Organization

*   The filesystem directories should be kept in sync with the Xcode file groups.

*   Files within groups may be kept alphabetized (case-insensitively, with groups before files).

*   An Xcode project repository should follow this structure:
    *   base folder (contains Gemfile, Podfile, lock files, .rvmrc, other non-Xcode configuration files as necessary)
        *   `Pods/` (if using CocoaPods)
        *   `ProjectName/`
        *   `ProjectNameTests/`
        *   `ProjectName.xcodeproj/`
        *   `ProjectName.xcodeworkspace/` (if using CocoaPods)

*   There should be no files directly within an Xcode ProjectName directory. The subfolders (and corresponding groups) should follow this structure:
    *   `Models/`
        *   `Editable/` (Core-data Entity Categories)
        *   `Generated/` (Core-Data Generated Entity files, which should not be edited)
        *   `ProjectName.xcdatamodeld`
    *   `Views/` (contains `.xib`s, and UI subclasses within a folder structure that mirrors the app navigation)
    *   `Storyboards/` (contains `.storyboards`s)
    *   `Controllers/` (contains view controllers within a folder structure that mirrors the app navigation)
    *   `Base.lproj/` (if using localized strings)
    *   `Shared/`
        *   `Views/` (contains `.xib`s and UI subclasses used throughout the app)
        *   `Controllers/` (contains view controllers used or subclassed throughout the app)
        *   `Utilities/` (contains utility classes and singletons)
    *   `Resources/`
        *   `Fonts/`
        *   `Images/` (contains some sort of internal folder structure and uses sane naming conventions and contains Images.xcassets)
        *   `Strings/` (contains plists for localized strings)
    *   `Supporting Files/` (AppDelegate, InfoPlist, ProjectName-Info.plist, ProjectName-Prefix.pch, bridging-headers)


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
