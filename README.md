# Swift Reference
This is meant to be a quick look-up reference document for Apple's **Swift** programming language as I set out to begin gaining experience with iOS development. This document is not meant to be exhaustive and will not dive too deeply into concepts common to other languages such as C/C++, C#/Java, as these are the languages I work with on an almost daily basis. The examples and content are current as of Swift 4.
All of the sample content is based on the **[Swift Tour](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/GuidedTour.html#//apple_ref/doc/uid/TP40014097-CH2)**, the **[Swift Language Guide](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID309)**, and the **[Swift Language Reference](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AboutTheLanguageReference.html#//apple_ref/doc/uid/TP40014097-CH29-ID345)** located at [https://developer.apple.com/](https://developer.apple.com/).

As noted above, I've only just begun to work with the Swift language, so feel free to contribute if you:
* find any errors
* feel like there are important details missing from any topics
* have an example you'd like to share
* want to point out any common pitfalls for people in the beginning stages of learning iOS development
* want to suggest additional reference material (books, documentation, tutorials, videos, etc.)
* or for any other reason

This document will be updated regularly as I continue to work through different references including, but not limited to, the sources listed above.

---

# Contents

[Aliases](#aliases)</br>
[Arrays](#arrays)</br>
[Assertions](#assertions)</br>
[Casting](#casting)</br>
[Classes](#classes)</br>
[Constants](#constants)</br>
[Control Flow](#control-flow)</br>
[Dictionaries](#dictionaries)</br>
[Do-Catch](#do-catch)</br>
[Enum](#enum)</br>
[Error Handling](#error-handling)</br>
[For-In](#for-in)</br>
[Functions](#functions)</br>
[If](#if)</br>
[If-Let](#if-let)</br>
[Insert Values Into Strings](#insert-values-into-strings)</br>
[Numeric Literals / Radices](#numeric-literals-in-different-radices)</br>
[Optionals](#optionals)</br>
[Protocols](#protocols)</br>
[Repeat-While](#repeat-while)</br>
[Structures](#structures)</br>
[Switch](#switch)</br>
[Try-Catch (Do-Try-Catch)](#error-handling)</br>
[Tuples](#tuples)</br>
[Variables](#variables)</br>
[While](#while)</br>


---

## ALIASES

  Use the keyword: **typealias**

  Basically just a `typedef`

```swift
    typealias NewTypeName = actualTypeName

    typealias OneByte = Int8
```


## ARRAYS

 * **EMPTY**
 
    Data type **_cannot_** be inferred - requires () parens
  
```swift
    var arrayName = [dataType]()
    
    var arrayName = [String]();
```

 * **PREFILLED**
 
   Data types **_can_** be inferred.
   
```swift
    var arrayName = [“value”, “value”, “value”]
```

## ASSERTIONS

  Use the library function **assert(_:_:file:line:)**</br>
  Use the library function **assertionFailure(_:file:line:)**

  Use assertions for `debug` builds. Assertions are not evaluated in `production` builds, so they have no effect on
  production builds.

  Pass the assert() function a condition and a message to display if the condition fails.

```swift
    let visitors = -10
    assert(visitors >= 0, “A physical quantity cannot be negative.”)
```

  You can use the `assertionFailure(:_file:line:)` drop the `message` if the condition is already being checked in code:
```swift
    if age >= 16 {
        print(“You may operate a motor vehicle.”)
    } else if age > 0 {
        print(“You may operate a foot powered vehicle.”)
    } else {
        assertionFailure(“A person cannot have a negative age.”)
    }
```


## CASTING

  Values are **NEVER IMPLICITLY CONVERTED** between types</br>
  As a result, if you’re adding an `Int8` and an `Int16`, you would need to cast the `Int8` up to `Int16` explicitly
  for any maths operations.
  
  Use either of the two typical forms:

```swift
    (targetType)value
    
    (Int)numberOfFoos


    targetType(value)
    
    Double(numberOfBars)
```

  > **NOTE ON CONVERSIONS**</br>
  > When casting/converting, say, a `String` to an `Int`, you’re actually calling an initializer that returns type `Int?`
  > because it’s possible that the initializer might fail - for instance, if you tried to convert `”123”` to an `Int`,
  > the resulting `Int?` would have a value of `123`, but if you tried to convert `”hello”`, the resulting `Int?` would
  > have a value of `nil` because the initializer would have failed.


## CLASSES

  Use the keyword: **class**</br>
  Use the keyword: **self**, not `this`

```swift
    class ClassName {
      // instance variables
      // instance methods

      init(args: dataTypes) {
 	 self.varName = argName;
	 . . .
      }
    }

    var someObject = ClassName(anyRequiredArguments);
```

  The following are examples of a class and a subclass. The first example includes classes with **simple properites** only, while the second example has a class with a **complex property**.


 * **SIMPLE PROPERTIES**

```swift
  class Shape {
    var name: String;
    var numberOfSides: Int = 0;

    func description() -> String { return “A shape named \(name).”; }

    init(name: String) { self.name = name; }
  }

  class Square : Shape {
    var sideLength: Double;

    init(name: String, sideLength: Double) {
      self.sideLength = sideLength;  // we can assign this value here
      super.init(name: name);	  // now we call the super constructor
      numberOfSides = 4;		  // now that we’ve called the super constructor, we can access ‘numberOfSides’
    }

    override func description() -> String { return “A square named \(name) with side length \(sideLength).”; }
  }

  var mySquare = Square(name: “My Square”, sideLength: 9.2);  // creates a square
```

* **COMPLEX PROPERTIES**

  Complex properties use getters and setters.
  
  In this example, when you **_GET_** the `perimeter` value, it is based off of the `sideLength` value.
  
  When you **_SET_** the `perimeter` value, it has a side effect of updating the `sideLength` value.
  
```swift
    class EquilateralTriangle {
        // any desired simple properties
  	// any desired instance methods
	// init()
	var perimeter: Double {
	  get { return 3.0 * sideLength; }
	  set { sideLength = newValue / 3.0; }
	}
    }
```


## CONSTANTS

  Use the keyword: **let**

* **EXPLICIT** 

  Data type required only when value is **_not_** provided

```swift
    let constName: dataType = value
    
    let name: String = "A name that won't change"
```

* **INFERRED**

  No data type required when value is provided or the type can be inferred

```swift
    let constName = “Some String”
```


## CONTROL FLOW

See the following:
* [FOR-IN](#for-in)
* [IF](#if)
* [REPEAT-WHILE](#repeat-while)
* [SWITCH](#switch)
* [WHILE](#while)


## DICTIONARIES

* **EMPTY**

  Data types **_cannot_** be inferred - requires () parens

```swift
    var dictionaryName = [ keyDataType : valueDataType ]()
    
    var dictionaryName = [ String : Int ]()
```

* **PREFILLED**

  Data types **_can_** be inferred
 
```swift
    var dictionaryName = [
	  “keyName” : “value”,
	  “keyName” : “value”
    ]

    var fruitTotals = [
	“banana” : 7,
	“apple” : 3,
	“tomato” : 12
    ]
```

* **ADD OR EDIT A KEY / VALUE PAIR**
 
```swift
    dictName[“keyName”] = value
```

> When you print a value, such as a dictionary of `[String : Int]` the value is IMPLICITLY COERCED TO 
> `‘Any’ (Int?)`. You have 3 options to resolve this:
>
> 1.) Force unwrap with an exclamation !
```swift
    print(fruitTotals[“apple”]!)            // output: 3
```
> 2.) Provide a default value with ?? so when a key is `nil` the default value will be used instead
```swift
    print(fruitTotals[“apple”] ?? 27)       // output: 3 (because apple's value isn't nil, but had it
                                            //         been nil, it would print 27
```
> 3.) Explicitly cast ‘as Any'
```swift
    print(dictName[“key”] as Any)	    // output: Optional(3)
```



## DO-CATCH

  Use keyword: **do** (instead of `try`)</br>
  Use keyword: **try** (as a marker)</br>
  Use keyword: **catch**

  Basically a `try-catch` block, but with a few slight differences.

  Within the `do` block, you’ll mark the code that may throw an error with `try` or `try?`.

  Within the `catch` block, the error being caught is given the name `error` by default, unless you provide a different one.

* **DO-CATCH WITH TRY**

  See [ERROR HANDLING](#error-handling) for the definition of the function in this example.

```swift
    do {
      let phoneResponse = try makePhoneCall(recipient: targetNumber)
      print(phoneResponse)
    } catch PhoneError.busySignal {				// catch a specific error
        print(“The line is busy. Must be a telemarketer.”)
    } catch let phoneError as PhoneError {			// catch other phone errors, and rename from ‘error’ to ‘phoneError’
        print(“Phone error: \(phoneError)”)
    } catch {								// catch any other kind of error
        print(error)
    }
```

* **DO-CATCH WITH TRY?**

  If an error is thrown, the error is discarded, and the result is `nil`.

  If no error is thrown, the result is an optional containing the value returned by the function.

```swift
    let phoneResponse = try? makePhoneCall(recipient: targetNumber)
```

  


## ENUM

  Different from an `enum` that you’re used to.

  Use the keyword: **enum**</br>
  Use the keyword: **case** (for enum value names, but **_do not_** follow it with a `:`)
  
  To access an instance’s raw value, you’ll use the `.rawValue` property of that instance to obtain it.
  > **NOTE:** The `.rawValue` property does not exist on enum types with no explicitly defined `rawValue` type

  Enums can have `rawValue` types of `Int`, `Float`, and `String`, but if there isn’t a *meaningful* `rawValue` type,
  you don’t even need to provide one.

  Refer to *this instance* of the enum by the `self` keyword within the enum’s definition.

```swift
    enum EnumName: rawValueDataType {
      case: enumValueName = value            // if you don’t want to start at the default of 0
      case: valueName, valueName, . . .
      // as many cases as you need
      // you can also include functions
    }
```

  As mentioned above, you can have an enum with a *meaningful* `rawValue` type, or without.

  The following two examples show the difference between the two.

* **WITH A MEANINGFUL RAW VALUE TYPE**

   These are enum types in which the `rawValue` might actually mean something. In the following example, it is indicative
   of the card’s face value.

```swift
    enum Rank: Int {
      case ace = 1
      case two, three, four, five, six, seven, eight, nine, ten
      case jack, queen, king

      func description() -> String {
        switch self {			// notice how we just switch on ‘self’
          case .ace:				// and how the enum values are referred to as .ace, etc.
            return “ace”			// within the enum’s definition
          case .jack:
            return “jack”
        }
      }
    }

    let ace = Rank.ace			// but outside the enum definition, we need the fully qualified name
    let aceRawValue = ace.rawValue

    print(ace)				// output: ace
    print(aceRawValue)			// output: 1
```

* **WITHOUT A MEANINGFUL RAW VALUE TYPE**

   These are enums in which the `rawValue` wouldn’t necessarily mean anything, and so we leave it out completely.
   The example we’ll use is suits of cards in a deck - although, depending on which game you’re playing it is possible to
   actually rank them in an order. But for the sake of an easy example, let’s just follow the card suit example!
```swift
   enum Suit {				// notice how we left off the rawValue data type indicator, and provide only a name
     case spades, hearts, diamonds, clubs

     func description() -> String {
       switch self {				// we still switch on ‘self’
         case .spades:			// and refer to enum values by .valueName
           return “spades”
         case .hearts:
           return “hearts”
       }
     }
   }

   let hearts = Suit.hearts			// okay, assigns the enum value of Suit.hearts to the hearts variable
   let heartsDesc = hearts.description();	// also okay, assigns the String value “hearts” to the heartsDesc variable

   let heartsRaw = hearts.rawValue		// produces a compiler error
```

   The last line above (`let heartsRaw = hearts.rawValue`) would produce a compiler error.</br>
   This is due to the fact that you defined your enum type **_without_** a `rawValue` data type specifier.</br>
   As a result, the enum type `Suit` does **_not_** even contain a property for `rawValue`.

* **CREATE AN INSTANCE FROM A RAW VALUE**
   You can use the initializer function provided to generate an instance of a particular enum given some raw value.</br>
   The syntax of the initializer is: `init?(rawValue:)` where you’d replace `init?` with the name of the enum, and provide a
   raw value as an argument. Keep in mind that if the raw value you provide is out of range of the actual enum, `nil` will
   be returned, so it’s a good idea to combine this with an [IF-LET](#if-let) if you’re concerned about `nil` values.
```swift
    let myInstance = Rank(rawValue: 3)
```


## ERROR HANDLING

   You can represent an error by any type that “adopts” (implements) the `Error` protocol.

```swift
    enum PhoneError : Error {
      case noDialTone
      case busySignal
      case numberOutOfService
    }
```

  To throw an error, you simply use `throw` followed by the error you wish to throw.

```swift
    throw PhoneError.busySignal
```

  To mark a function as one that can throw an error, use the `throws` keyword. You don’t need to indicate which type
  of error it throws, just that it *can* throw.

```swift
    func makePhoneCall(recipient: Int) throws -> String {
      if lineIsBusy(recipient) {
        throw PhoneError.busySignal
      }
    }

    return “Call is connected.”
```

  To work with code that can throw errors, you’ll use a [DO-CATCH](#do-catch) block.



## FOR-IN
  
  Not quite what you’re used to. Works on a **collection** or a specified **range**.
  
  Do **_not_** use parens to set off the conditions of the loop.

* **On collections**
```swift
    for item in collection { . . . }

    var fruitsArray = [“banana”, “apple”, “tomato”, “peach”];
    for fruit in fruits {
      print(fruit);
    }
    
    // output: banana
    //	       apple
    //         tomato
    //         peach
```

* **On a range** (**_not_** inclusive)
```swift
    for i in 0..<10 { . . . }   // 0 to 9
```

* **On a range** (inclusive)
```swift
    for i in 0...10 { . . . }   // 0 to 10
```


## FUNCTIONS

  Use the keyword: **func**

```swift
  func functionName(paramNameOne: dataType, paramNameTwo: dataType) -> returnType { . . . }
```

  Within the function body, you’ll refer to the params by name. 
  
  When calling the function, you have 3 options for how to specify the arguments, depending on how the function was declared.

* BY NAME:
```swift
    func greet(name: String) -> String { . . . }
    greet(name: “Billy Joe”);
```

* BY A CUSTOM LABEL:
```swift
    func greet(person name: String) -> String { . . . }
    greet(person: “Billy Bob”);
```

* WITH NO LABEL (most convenient):
```swift
    func greet(_ name: String) -> String { . . . }
    greet(“Billy Bob Joe”);
```

> PASSING AN ARRAY TO A FUNCTION? 
> After the label, put the array’s data type in [brackets]
```swift
	func doStuff(values: [Int]) -> Int { . . . }
```


## IF

  The usual, but the condition **MUST BE** a boolean, and parens are optional but { braces } are **_NOT_**.
```swift
  if condition { . . . }

  if (condition) { . . . }
```


## IF-LET

  A method of testing optional values.</br>
  If the optional value is `nil`, it won’t be assigned to the new variable, causing the `if` condition to result to false, skipping the body:

```swift
    if let name = someOptionalVariable { . . . }
```

  This is technically the same as
```swift
    if (someOptionalVariable != nil) { . . . }
```
  The key difference to note between the two above examples is that the first example you can simply refer to `name` within
  the body of the `if let`, but in the second example you’d need to force-unwrap `someOptionalVariable` within the `if` body.
  For variable naming simplicity, you can use the following:

```swift
    if let someOptional = someOptional { . . . }
```
   That way, within the scope of the `if let` body, you can just refer to `someOptional`.

  > **NOTE:** Using the `if-let` allows you to refer to the newly created variable in the `if-let`’s body 
  > (such as `name` above).</br>
  > *However*, if you prefer the simple `!= nil` check, once inside the `if` statement’s body, you would **force**
  > **unwrap** the `someOptionalVariable` by appending a `!` to the end of it. When printing `someOptionalVariable`,
  > failure to force unwrap it in the `if`’s body will cause it to output as `Optional(x)` where `x` is its value.


## INSERT VALUES INTO STRINGS

```swift
  \(varName)
  \(constName)
  \(some + kind + of + expression)

  print("This is a string with \(varName)'s value in it.")
  print("This string has a constant's value: \(constName).")
  print("This string adds \(some + kind + of + expression).");
```



## NUMERIC LITERALS IN DIFFERENT RADICES

  To express a numeric literal in binary, octal, or hex, use the following prefixes respectively:

```swift
    0b    binary
    0o    octal
    0x    hex
```

  For example, all of the following statements represent the decimal number 17:
```swift
    17
    0b10001
    0o21
    0x11
```

## OPTIONALS

  Explicit data type is required
  
  Place a `?` after the data type
  
  You’ll later want to check for `nil` values when working with these variables.
  
  See [IF-LET](#if-let)

```swift
  var varName: dataType? = value      // (or, = nil)
  var optionalInteger: Int? = 8
  var optionalString: String? = nil
```



## PROTOCOLS

  Use the keyword: **protocol**

  Think of a protocol as an interface.

  They define a set of functionality / properties that can later be implemented by `class`, `enum`, and `struct`.

  Implement a `protocol` by using `:`

  > **NOTE:** You can use protocols just like interfaces, where you can have a collection of objects of different types
  > that all implement the same protocol. They’ll have a runtime type of the actual class, but the compiler will
  > treat each one as an object of the implemented type, thus preventing you from accessing properties/methods they don’t contain.

```swift
    protocol SampleProtocol {
      var description: String { get }	// define a required property

      mutating func adjust()		// define a required function
    }
```
  > **NOTE:** Notice the keyword `mutating` in the function declaration. For `struct` types, you’ll also need to include
  > this keyword in the definition of the structure’s implementation of this function because it marks the function as
  > one that modifies the `struct`.
  > A `class` does **_not_** require use of the `mutating` keyword, as member functions can always modify instances of that class.




## REPEAT-WHILE

  Basically a `do-while` loop that you’re used to.</br>
  Again, the condition **must be a boolean** and the `( )` parens are not needed.

```swift
    repeat {
        // do some stuff
    } while condition
```



## STRUCTURES

  Use the keyword: **struct**

  Act the same way as you’re used to, and can include variables and functions.

  > **NOTE:** An important thing to remember is that a `struct` is copied as it is passed around in your code,
  > while an `object` is passed by reference.

  ```swift
      struct StructName {
        // variables
        // functions
      }
  ```

  ```swift
      struct Card {
        var rank: Rank		// assumes you’ve made the enum: Rank
        var suit: Suit		// assumes you’ve made the enum: Suit

        func description() -> String {
          return “The \(rank.description) of \(suit.description).”
        }
      }

      let threeOfClubs = Card(rank: .three, suit: .clubs);
  ```

  > **NOTE:** Notice how the `struct`, Card, doesn’t define an `init()` function.
  > Also notice that when instantiating a new variable of type `Card`, we were able to use the `.value` of the
  > enumerations and were not required to use the fully qualified names.


## SWITCH

  A typical `switch` that you’re used to, but with some additional functionality.</br>
  **_NOT_** limited to integers and tests for equality.</br>
  Support all types of data and a variety of comparison operators.
  > **NOTE:** `switch` does **_not_** support fall-through like other languages, so a `break` statement is not required
  > after each `case` definition. Instead, to handle fall-through, the case is a comma-separated list of values.

```swift
    switch comparitor {
        case value_0:
          // do something
        case value_1:
          // do something else
        case value_2, value_3:
          // fall-through style
        default:
          // no cases matched
    }
```
  

## TUPLES

  Allow you to store multiple values (of varying types) in one variable.

  Allow you to return multiple values from a function.
  
  An empty tuple `()` can also be used to indicate `Void`
  
  Refer to the values stored in a tuple by name OR index.

  >**NOTE:** Tuples are preferred for simple temporary-scoped related values.
  > If your data structure is complex or will persist, consider a `class` or `struct`
  
  ### DEFINING A TUPLE

  Suppose you want to store an HTTP status code. You might use a tuple:
```swift
   let httpCode = (404, “Not Found”)
```

  ### RETURNING TUPLES FROM FUNCTIONS

  Suppose a function that returns 3 `Int` values: min, max, sum:
 ```swift
    func returnTuple(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
      // . . .
      // logic for assigning/computing min, max, and sum goes here
      // . . .
      return (min, max, sum)
    }

    var result = returnTuple(scores: myScoresArray);

    print(result.max);  // prints the max value off the tuple
    print(result[1]);   // prints the max value off the tuple
```

  ### DECONSTRUCTING TUPLES

  When your tuple does **_not_** contain named properties, and you don’t want to access by index:
```swift
    let http404NotFound = (404, “Not Found”)       // define a tuple without named properties

    // DECONSTRUCT THE TUPLE INTO 2 CONSTANTS
    let (statusCode, description) = http404NotFound

    // IGNORE PARTS OF THE TUPLE YOU DON’T NEED USING _ UNDERSCORE
    let (statusCode, _) = http404NotFound
```


## VARIABLES

  Use the keyword: **var**

* **EXPLICIT** 

   Data type required only when value is not provided or when creating an optional
```swift
    // SIMPLE VARIABLE:
    var varName: dataType = value

    // OPTIONAL:
    var varName: dataType? = value	// may contain a value or nil - requires unwrapping using the ! operator

    print(varName!)                  // access an optional using ! operator to prevent printing as: “Optional(value)”

    // IMPLICITLY UNWRAPPED OPTIONAL
    var varName: dataType! = value   // may contain a value or nil, but after definition is assumed to contain a
                                     // value and does not require unwrapping
    print(varName)
```

* **INFERRED** 

   No data type required when value provided
```swift
    var varName = 7
```


## WHILE

  Just what you’re used to, minus the `( )` parens around the condition.</br>
  Keep in mind that as with other control-flow statements, the condition **must be a boolean**.

```swift
    while condition {
      // do stuff
    }
```
