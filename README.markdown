# Swift Style Guide.

This style guide is a work in progress aimed at readability, simplicity, and conciseness. Based on the Official raywenderlich.com Swift Style Guide.

## Table of Contents

* [Spacing](#spacing)
* [Comments](#comments)
* [Numbers](#numbers)
* [Conditionals](#conditionals)
* [Naming](#naming)
  * [Class Prefixes](#class-prefixes)
* [Semicolons](#semicolons)
* [Classes and Structures](#classes-and-structures)
* [Use of Self](#use-of-self)
* [Function Declarations](#function-declarations)
* [Closures](#closures)
* [Types](#types)
  * [Constants](#constants)
  * [Optionals](#optionals)
  * [Type Inference](#type-inference)
  * [Syntactic Sugar](#syntactic-sugar)
* [Control Flow](#control-flow)
* [Credits](#credits)


## Spacing

* Indent using 4 spaces rather than tabs. 4 spaces helps keep indentation levels very clear. Be sure to set this preference in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.

**Preferred:**
```swift
if user.isHappy {
    //Do something
} else {
    //Do something else
}
```

**Not Preferred:**
```swift
if user.isHappy
{
  //Do something
}
else {
  //Do something else
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.

Any comma separated list should have a space after each comma.

```swift
enum Planet {
    case Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}
```
```swift
func describePoint(x: Int, y: Int) -> String {
    return "I am a circle at (\(x), \(y)) with an area of \(x * y)"
}
```
```swift
let array = ["one", "two", "three"]
```

Binary and Ternary operators should include spaces between the operator(s) and targets while unary operators should not:

```swift
let four = (1 + 1) * 2
```
```swift
let mood = isSunnyOut ? "happy" : "foul"
```
```swift
let i = 0
let plusOne = ++i
```

## Comments

When they are needed, use comments to explain **why** a particular piece of code does something. Comments must be kept up-to-date or deleted.

Avoid block comments inline with code, as the code should be as self-documenting as possible. *Exception: This does not apply to those comments used to generate documentation.*

## Numbers

Very large numbers should use an underscore after every third power for visual clarity:

```swift
let fileSize = 100_000_000
```

## Conditionals

Conditionals should not use parenthesis unless needed to group statements:

**Preferred:**
```swift
let name = "world"
if name == "world" {
    println("hello, world")
}
```

**Not Preferred:**
```swift
let name = "world"
if (name == "world") {
    println("hello, world")
}
```

## Naming

Use descriptive names with camel case for classes, methods, variables, etc. Class names and constants in module scope should be capitalized, while method names and variables should start with a lower case letter.

**Preferred:**

```swift
let MaximumWidgetCount = 100

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

For enumerated types, as per Apple's recommendation, the type name should be in the singular.

**Preferred:**

```swift
enum Planet {
    case Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}
```

**Not Preferred:**

```swift
enum Planets {
    case Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}
```

For functions and init methods, prefer named parameters for all arguments unless the context is very clear. Include external parameter names if it makes function calls more readable.

```swift
func dateFromString(dateString: NSString) -> NSDate
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

### Class Prefixes

Swift types are all automatically namespaced by the module that contains them. As a result, prefixes are not required in order to minimize naming collisions. If two names from different modules collide you can disambiguate by prefixing the type name with the module name:

```swift
import MyModule

var myClass = MyModule.MyClass()
```

You **should not** add prefixes to your Swift types.

If you need to expose a Swift type for use within Objective-C you can provide a suitable prefix (following our [Objective-C style guide](https://github.com/raywenderlich/objective-c-style-guide)) as follows:

```swift
@objc (RWTChicken) class Chicken {
    ...
}
```

## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

The only exception to this rule is the `for-conditional-increment` construct, which requires semicolons. However, alternative `for-in` constructs should be used where possible.

**Preferred:**
```swift
var swift = "not a scripting language"
```

**Not Preferred:**
```swift
var swift = "not a scripting language";
```

**NOTE**: Swift is very different to JavaScript, where omitting semicolons is [generally considered unsafe](http://stackoverflow.com/questions/444080/do-you-recommend-using-semicolons-after-every-statement-in-javascript)

## Classes and Structures

Here's an example of a well-styled class definition:

```swift
class Circle: Shape {
    var x: Int, y: Int
    var radius: Double
    var diameter: Double {
    get {
        return radius * 2
    }
    set {
        radius = newValue / 2
    }
    }

    init(x: Int, y: Int, radius: Double) {
        self.x = x
        self.y = y
        self.radius = radius
    }

    convenience init(x: Int, y: Int, diameter: Double) {
        self.init(x: x, y: y, radius: diameter / 2)
    }

    func describe() -> String {
        return "I am a circle at (\(self.x), \(self.y)) with an area of \(self.computeArea())"
    }

    func computeArea() -> Double {
        return M_PI * radius * radius
    }  
}
```

The example above demonstrates the following style guidelines:

 + Specify types for properties, variables, constants, argument declarations and other statements with a space after the colon but not before, e.g. `x: Int`, and `Circle: Shape`.
 + Indent getter and setter definitions and property observers.
 + Define multiple variables and structures on a single line if they share a common purpose / context.

## Use of Self

You should always use `self` to access an object's properties and invoke its methods. This will help maintain clarity about what you are referring to at the cost of a small amount of verbosity.

```swift
class BoardLocation {
    let row: Int, column: Int

    init(row: Int, column: Int) {
        self.row = row
        self.column = column
    }

    func describe() -> String {
        return "Row \(self.row), column \(self.column)"
    }
}
```

## Function Declarations

Keep short function declarations on one line including the opening brace:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

For functions with long signatures, add line breaks at appropriate points and add an extra indent on subsequent lines:

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
    translateConstant: Int, comment: String) -> Bool {
    // reticulate code goes here
}
```

Don't specify the output type as Void. Simply leave it out.

**Preferred:**
```swift
func reticulateSplines(spline: [Double]) {
    // reticulate code goes here
}
```

**Not Preferred:**
```swift
func reticulateSplines(spline: [Double]) -> Void {
    // reticulate code goes here
}
```

```swift
func reticulateSplines(spline: [Double]) -> () {
    // reticulate code goes here
}
```

## Closures

Use trailing closure syntax wherever possible. In all cases, give the closure parameters descriptive names:

```swift
return SKAction.customActionWithDuration(effect.duration) { node, elapsedTime in 
    // more code goes here
}
```

For single-expression closures where the context is clear, use implicit returns:

```swift
attendeeList.sort { a, b in
    a > b
}
```

For very simple closures, you should put them on a single line without naming the parameters. You should include a space after the opening curly brace and before the closing brace:

```swift
attendeeList.sort { $0 > $1 }
```

## Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Preferred:**
```swift
let width = 120.0                                           //Double
let widthString = width.bridgeToObjectiveC().stringValue    //String
```

**Not Preferred:**
```swift
let width: NSNumber = 120.0                                 //NSNumber
let widthString: NSString = width.stringValue               //NSString
```

In Sprite Kit code, use `CGFloat` if it makes the code more succinct by avoiding too many conversions.

### Constants

Constants are defined using the `let` keyword, and variables with the `var` keyword. Any value which **is** a constant **must** be defined appropriately, using the `let` keyword. As a result, you will likely find yourself using `let` far more than `var`.

**Tip:** One technique that might help meet this standard is to define everything as a constant and only change it to a variable when the compiler complains!

### Optionals

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Avoid using `!` whereever possible. Instead try to use `if let` to create a non-optional. If you're using `!` it should be in a conditional that ensures the variable is non-nil.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

```swift
myOptional?.anotherOne?.optionalView?.setNeedsDisplay()
```

Use optional binding when it's more convenient to unwrap once and perform multiple operations or if you need to do any assignment:

```swift
if let view = self.optionalView {
    // do many things with view
}
```

### Type Inference

The Swift compiler is able to infer the type of variables and constants. You can provide an explicit type via a type alias (which is indicated by the type after the colon), but in the majority of cases this is not necessary.

Prefer compact code and let the compiler infer the type for a constant or variable.

**Preferred:**
```swift
let message = "Click the button"
var currentBounds = computeViewBounds()
```

**Not Preferred:**
```swift
let message: String = "Click the button"
var currentBounds: CGRect = computeViewBounds()
```

**NOTE**: Following this guideline means picking descriptive names is even more important than before.

### Syntactic Sugar

Prefer the shortcut versions of type declarations over the full generics syntax.

**Preferred:**
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Not Preferred:**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## Control Flow

Prefer the `for-in` style of `for` loop over the `for-condition-increment` style.

**Preferred:**
```swift
for _ in 0..<3 {
    println("Hello three times")
}

for person in attendeeList {
    // do something
}
```

**Not Preferred:**
```swift
for var i = 0; i < 3; i++ {
    println("Hello three times")
}

for var i = 0; i < attendeeList.count; i++ {
    let person = attendeeList[i]
    // do something
}
```

## Credits

This style guide has been assembled by:

* [David Mauro](http://dmauro.com)

And is derived from a collaborative effort from the most stylish raywenderlich.com team members: 

* [Soheil Moayedi Azarpour](https://github.com/moayes)
* [Scott Berrevoets](https://github.com/Scott90)
* [Eric Cerney](https://github.com/ecerney)
* [Sam Davies](https://github.com/sammyd)
* [Evan Dekhayser](https://github.com/edekhayser)
* [Jean-Pierre Distler](https://github.com/pdistler)
* [Colin Eberhardt](https://github.com/ColinEberhardt)
* [Greg Heo](https://github.com/gregheo)
* [Matthijs Hollemans](https://github.com/hollance)
* [Erik Kerber](https://github.com/eskerber)
* [Christopher LaPollo](https://github.com/elephantronic)
* [Andy Pereira](https://github.com/macandyp)
* [Ryan Nystrom](https://github.com/rnystrom)
* [Cesare Rocchi](https://github.com/funkyboy)
* [Ellen Shapiro](https://github.com/designatednerd)
* [Marin Todorov](https://github.com/icanzilb)
* [Chris Wagner](https://github.com/cwagdev)
* [Ray Wenderlich](https://github.com/rwenderlich)
* [Jack Wu](https://github.com/jackwu95)

Hat tip to [Nicholas Waynik](https://github.com/ndubbs) and the [Objective-C Style Guide](https://github.com/raywenderlich/objective-c-style-guide) team!

We also drew inspiration from Apple’s reference material on Swift:

* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html)
* [Swift Standard Library Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html)
