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
[Characters in Strings](#characters-in-strings)</br>
[Classes](#classes)</br>
[Collection Types](#collection-types)</br>
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
[Operators](#operators)</br>
[Optionals](#optionals)</br>
[Preconditions](#preconditions)</br>
[Protocols](#protocols)</br>
[Repeat-While](#repeat-while)</br>
[Strings](#strings)</br>
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

  Technically written as `Array<Type>`, but the shorthand `[Type]` is preferred.

  Arrays can be added together to create bigger arrays.

  Access elements with `[x]` bracket notation, where `x` is a valid `index`.

 * **EMPTY - EXPLICIT**
 
    Data type **_cannot_** be inferred - requires () parens
  
    ```swift
      var arrayName = [dataType]()
    
      var arrayName = [String]();
    ```

 * **EMPTY - IMPLICIT**

    Data type **_can_** be inferred.

    ```swift
      var arrayName = []
    ```

 * **PREFILLED & INFERRED** (array literal)
 
   Data types **_can_** be inferred.
   
    ```swift
      var arrayName = [“value”, “value”, “value”]
    ```

 * **PREFILLED & EXPLICIT** (array literal)

    ```swift
      let arrayName: [dataType] = [values]

      let helloArray: [Character] = [“H”, “e”, “l”, “l”, “o”, “!”]
    ```

 * **PREFILLED WITH REPEATING VALUE**

   You need to use **initializer syntax** and the `Array` keyword:

   ```swift
       var prefilledArray = Array(repeating: 7.2, count: 3)
   ```

   The above code creates an array called `prefilledArray` with `3` elements, each containing the value `7.2`.

 * **COMMON PROPERTIES AND METHODS**

  ```swift
    .count       // number of elements            .isEmpty        // shortcut to see if .count == 0
    .append()    // same as: arr += value(s)      [x]             // a single subscript, x
    [a...b]      // range of subscripts           .insert(_:at:)  // insert a value at a specified index
    .remove(at:) // remove item at index          .removeLast()   // shorthand to skip checking an array’s count
  ```
    > **NOTE**: the `.insert(_:at:)` method adjusts the contents of the array, it does not replace a value
    > **NOTE**: the `.remove(at:)` method removes the item at the specified `index` *and returns it*, but if you’re not planning on using it, you can ignore this fact

 * **ITERATION**

  If you only need the values, use a simple `for-in` loop.

  If you need both the value *and* its index, use a `for-in` loop with the `.enumerated()` method 
  which returns a tuple with named properties in the form of `(index, value)`.

  ```swift
    for (index, value) in myArray.enumerated() {
      print(“Item #\(index + 1) is \(value).”)
    }
  ```

## ASSERTIONS

  Use the library function `assert(_:_:file:line:)` used as: `assert(condition, “message”)`</br>
  Use the library function `assertionFailure(_:file:line:)` used as: `assertionFailure(“message”)`</br>
  See also: **[Preconditions](#preconditions)**

  Use assertions for `debug` builds. Assertions are not evaluated in `production` builds, so they have no effect on
  production builds.

  Pass the assert() function a condition and a message to display if the condition fails.

```swift
    let visitors = -10
    assert(visitors >= 0, “A physical quantity cannot be negative.”)
```

  You can use `assertionFailure(:_file:line:)` and drop the `condition` if the condition is already being checked in code:
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


## CHARACTERS IN STRINGS

  Dataype is: `Character`

* **ITERATION**

  * Iterate using a `for-in` loop over a `String` literal or a `String` constant/variable

  ```swift
      for char in “Hello!” {
        // do stuff
      }
  ```

  * Iterate using an array of `Character`

  ```swift
      let someCharArray: [Character] = [“a”, “r”, “r”, “a”, “y”]
  ```

* **UNICODE SCALARS**

  Produce any unicode scalar using the format `\u{x}` where `x` is any valid unicode code point, **_not including_**
  the *surrogate pair* code points.

  `U+0000` through `U+D7FF` **valid range**</br>
  `U+E000` through `U+10FFFF` **valid range**</br>
  `U+D800` through `U+DFFF` **not valid**: surrogate pair code points

* **EXTENDED GRAPHEME CLUSTERS**

  A convenient way to represent complex characters such as Korean Hangul characters or accented Spanish.

  Essentially, any unicode character that is a combination, such as `é` which is `e` and `´’ can be represented
  as a single character or as its component unicode parts.

  For example:
  ```swift
      let eComposed: Character = “\u{E9}”         // é
      let eDecomped: Character = “\u{65}\u{301}”  // e followed by ´

      let koreanComposed: Character = “\u{D55C}”                   // 한
      let koreanDecomped: Character = “\u{1112}\u{1161}\u{11AB}”   // ᄒ, ᅡ, ᆫ
  ```

  In the above example, `eComposed` and `eDecomped` both actually contain the same character.</br>
  Likewise, `koreanComposed` and `koreanDecomped` both contain the same character, though one version is the
  unicode “*precomposed*” version, and the other is the “*decomposed*” version.
 

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


## COLLECTION TYPES

  There are 3 main types provided by Swift:

  `Array`: Ordered collections of values</br>
  `Set`: Unordered collections of unique values</br>
  `Dictionary`: Unordered collections of key-value associations

  Assigned with `let`, a collection’s contents and size **_cannot_** be altered.



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
    for fruit in fruitsArray {
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

* **BY NAME**:
```swift
    func greet(name: String) -> String { . . . }
    greet(name: “Billy Joe”);
```

* **BY A CUSTOM LABEL**:
```swift
    func greet(person name: String) -> String { . . . }
    greet(person: “Billy Bob”);
```

* **WITH NO LABEL** (most convenient):
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
    0b    // binary
    0o    // octal
    0x    // hex
```

  For example, all of the following statements represent the decimal number 17:
```swift
    17
    0b10001
    0o21
    0x11
```


## OPERATORS

  Support for all of the operators you’re used to.

  By default, they detect and prevent overflow. You must opt in to allow overflow by using the
  [Overflow Operators](#overflow-operators).

* **RANGE OPERATORS**

  ```swift
      a..<b     // half open [a, b)

      a...b     // closed [a, b]

      [a...]    // one-sided [a, N] where N is the number of elements in an array
      [...b]    // one-sided [0, b]

      for name in names[2...] {
          // do something for elements 2 through the end of the array
      }

      for name in names[...4] {
          // do something for elements 0 through 4
      }
  ```

  > **NOTE:** You can use range operators in contexts other than just ranges of subscripts. For instance, you can
  > actually assign a range operator to a variable, `let range = ...5`.</br>
  > You **_cannot_** iterate over a one-sided range with no initial value, as it’s not clear where it begins.</br>
  > You can check if a range contains a value by using the `.contains()` method:
  > `range.contains(7)`  would return `false` in the range mentioned earlier in this note.


* **NIL-COALESCING OPERATOR**

  `??`
  Shorthand for `a != nil ? a! : b`

  (basically, if `a` is not `nil`, unwrap `a` and use it, else use `b`.

  ```swift
      let numberToUse = someOptionalValue ?? someDefaultValue
  ```

  In the above example, if `someOptionalValue` is `nil`, `someDefaultValue` is used instead, but if it’s **_not_**
  `nil`, then `someOptionalValue` is unwrapped and `someDefaultValue` is never evaluated.
  

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


## PRECONDITIONS

  Use the library function: **precondition(_:_:file:line:)**</br>
  Use the library function: **precondtionFailure(_:file:line:)**</br>
  See also: **[Assertions](#assertions)**

  Use a `precondition` whenever a condition **_may_** be false, but absolutely **_must be true_** for your code to continue,
  such as checking for an `index out of bounds`.

```swift
    precondition(index > 0, “Index cannot be negative.”)
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


## STRINGS

  Swift `String` is a value type - they are copied when passed to functions.

  Concatenation works as usual, but appending a single string requires the `.appen(:Character)` method.</br>
  Get a count of the characters in a string using the `.count` **property**.

  Create them as `var`, `let`, or `”string literals”` or by constructing from an array of `[Character]`</br>
  ```swift
      var stringVar = “some text!”
      let stringCon = “a string constant!”
      print(“just a literal”)
    
      let charArray: [Character] = [“h”, “i”]
      let makeString = String(charArray)
  ```

* **EMPTY STRINGS**
  
  Both of the following are equivalent when creating an empty string:
  ```swift
      var emptyString = “”

      var emptyString = String()
  ```

  Check if a string is empty using the `.isEmpty` property. Remember, it’s a **_property_**, not a method.

* **MULTI-LINE STRING LITERALS**

  Use 3 double quotes `”””` to enclose a multi-line string.

  The indentation of the *closing* quotations will determine whether or not leading whitespace on a given line
  is ignored. For example, in the following code, the third line would appear indented 2 spaces because that
  line is indented 2 spaces further than the closing quotation marks.

  ```swift
      let multiLineString = “””
      This string can span multiple
        lines and contain “quotes” without
      escaping if you’d like.
      “””
  ```

* **MUTABILITY**

  Assign a string to a `var` to indicate mutability or to a constant (`let`) to indicate immutability.

  Attempting to concatenate a string declared with `let` will result in a compiler error.



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
