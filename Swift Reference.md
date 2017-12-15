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

[Arrays](#arrays)</br>
[Casting](#casting)</br>
[Classes](#classes)</br>
[Constants](#constants)</br>
[Control Flow](#control-flow)</br>
[Dictionaries](#dictionaries)<br/>
[Enum](#enum)<br/>
[For-In](#for-in)</br>
[Functions](#functions)</br>
[If](#if)</br>
[If-Let](#if-let)</br>
[Insert Values Into Strings](#insert-values-into-strings)</br>
[Optionals](#optionals)</br>
[Protocols](#protocols)</br>
[Structures](#structures)</br>
[Tuples](#tuples)</br>
[Variables](#variables)</br>


---

## ARRAYS

 * **EMPTY**
 
    Data type **_cannot_** be inferred - requires () parens
  
```swift
    var arrayName[dataType]()
    
    var arrayName[String]();
```

 * **PREFILLED**
 
   Data types **_can_** be inferred.
   
```swift
    var arrayName[“value”, “value”, “value”]
```


## CASTING

  Values are **NEVER IMPLICITLY CONVERTED** between types
  
  Use either of the two typical forms:

```swift
    (targetType)value
    
    (Int)numberOfFoos


    targetType(value)
    
    Double(numberOfBars)
```


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
* REPEAT-WHILE
* SWITCH
* WHILE


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

  A method of testing optional values. You may also simply use   
  ```swift
      if (value != nil) { . . . }
  ```
  If the optional value is `nil`, it won’t be assigned to the new variable, causing the if condition to result to false, skipping the body:

```swift
    if let name = someOptionalVariable { . . . }
```

  This is the same as
```swift
    if (someOptionalVariable != nil) { . . . }
```


## INSERT VALUES INTO STRINGS

```swift
  \(varName)
  \(constName)
  \(some + kind + of + expression)

  print("This is a string with \(varName)'s value in it.")
  print("This string has a constant's value: \(constName).")
  print("This string adds \(some + kind + of + expression).");
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
  > that all implement the same protocol. They’ll have a runtime type of the implemented protocol, but the compiler will
  > treat each one as an object of its declared type, thus preventing you from accessing properties/methods they don’t contain.

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



## TUPLES

  Allow you to return multiple values from a function.
  
  An empty tuple `()` can also be used to indicate `Void`
  
  Refer to the values stored in a tuple by name OR index.
  
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


## VARIABLES

  Use the keyword: **var**

* **EXPLICIT** 

   Data type required only when value is not provided
```swift
    var varName: dataType = value
```

* **INFERRED** 

   No data type required when value provided
```swift
    var varName = 7
```
