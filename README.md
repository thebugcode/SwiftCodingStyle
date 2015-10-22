# SwiftCodingStyle

###Constants and Variables 

Swift constants and variables associate a name with a value of a particular type. The value of a //constant// cannot be changed once it is set, whereas a //variable// can be set to a different value in the future.
```swift
var x = 1.2
let isBlue = true
```
### Make use of constants over variables 

Use ''let foo = …'' over ''var foo = …'' wherever possible (and when in doubt). Only use ''var'' if you absolutely have to (i.e. you know that the value might change, e.g. when using the ''weak'' storage modifier).

//Rationale:// The intent and meaning of both keywords is clear, but //let-by-default// results in safer and clearer code.

A ''let''-binding guarantees and //clearly signals to the programmer// that its value is supposed to and will never change. Subsequent code can thus make stronger assumptions about its usage.
It becomes easier to reason about code. Had you used ''var'' while still making the assumption that the value never changed, you would have to manually check that.
Accordingly, whenever you see a ''var'' identifier being used, assume that it will change and ask yourself why.

### Avoid using force-unwrapping of optionals

If you have an identifier ''foo'' of type ''FooType?'', don't force-unwrap it to get to the underlying value (''foo!'') if possible.

Instead, prefer this:

```swift
if let foo = foo {
    // Use unwrapped `foo` value in here
} else {
    // If appropriate, handle the case where the optional is nil
}
```
Alternatively, you might want to use Swift's Optional Chaining in some of these cases, such as:

```swift
// Call the function if `foo` is not nil. If `foo` is nil, ignore we ever tried to make the call
foo?.callSomethingIfFooIsNotNil()
```
//Rationale:// Explicit ''if let''-binding of optionals results in safer code. Force unwrapping is more prone to lead to runtime crashes.

###Avoid using force casting

Prefer using conditional form of type-casting ''as?'' instead of force ''as!''.

**Good:**
```swift
let a = 12 as? Int64
// a being of Int64? type
```

**Bad:**
```swift
let a = 12 as! Int64
// a being of Int64! type
```

//Rationale:// Casting can fail, and handling the variable as a conditional can avoid the program from crashing.


###Use Swift native types =====

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Good:**
```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**Bad:**
```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```


###Start with action

For methods that represent an action an object takes, start the name with the action.

```swift
<code c>
func convertVariablesToFunctions() { ... }
```

```swift
<code c>
func variablesToFunctions() { ... }
```


### Keep methods small

It is usually a good ideea to split large functions into smaller ones.  Prefer small concise functions over smaller ones.
Whenever you have a large function it can usually be split up into two more atomic functions. 


### Number of parameters
Avoid creating methods with too many parameters. The ARM v6/v7/64 architectures allow for 3,5 and 7 parameters to be passed to a function via registers(fast storage), and following parameters are sent through RAM(slow storage). So remember, if you go beyond 3 parameters, you are not only making your method slower by using slower memory, you're also making it harder to read. 
[[https://www.mikeash.com/pyblog/friday-qa-2014-07-04-secrets-of-swifts-speed.html|Further Documentation]]

**Good:**
```swift
func outputDetailsForFile(file: File) { ... }
// File being an object that contains all those params
```

**Bad:**
```swift
func outputDetailsForPath(path: String, fileSize: Int, extension: String,
    encoding enc: NSStringEncoding) { ... }
```


### Whitespaces around binary and unary operators.
Rationale: Operators are usually special symbols(+-/)  and when they dont have enough whitespaces they make code much harder to read. The white spaces rule is very universal and simple to remember:
#####Binary operators: One space before and one space after
Examples:
**Good:**
```swift
func <|< <A>(lhs: A, rhs: A) -> A
```
**Bad:**
```swift
func <|<<A>(lhs: A, rhs: A) -> A
```
The same holds for arithmetic operators
**Good:**
```swift
let size = viewWidth - 2 * FIRST_CONSTANT - secondConstant - totalConstant
```
**Bad:**
```swift
let size=viewWidth-2*FIRST_CONSTANT-secondConstant-totalConstant
```

The same rule for the return type of functions:

**Good:**
```swift
func sizeOfObject() -> Float {
}
```
**Bad:**
```swift
func sizeOfObject()->Float{
}
```

The same rule holds for the Swift ternary operator (from Apple's Swift guide)
**Good:**
```swift
let rowHeight = hasHeader ? 50 : 20
```
**Bad:**
```swift
let rowHeight=hasHeader?50:20
```

```
Also, it stands for if else statements:
**Good:**
```swift
if (condition) {
    performTask()
} else {
    sleep()
}
```
**Bad:**
```swift
if(condition){
    performTask()
}else{
    sleep()
}
```

Prefer the One true bracing style. It is also used by Apple. : http://en.wikipedia.org/wiki/Indent_style#Variant:_1TBS

```swift
if condition {
    // do stuff
} else {
    // do other stuff
}
```


####Return early

If you have to meet certain criteria to continue execution, return early.

**Good:**
```swift
guard condition else { return error }
```

**Bad:**
```swift
if (condition) {
    // condition satisfied code
} else {
    if (anothertest) {
        foo()
    } else {
        return error
    }
}
```

When returning early prefer ''guard'' over ''if''.

####Comments

Preffer writing self explicit code which doesn't require comments. Comments suffer from rot and become obsolete soon after being written. Write comments only when you code fails to express itself.

IMPORTANT: If your code may create confusion for other developers, leaving a comment is mandatory.

Exception: comments used to generate documentation
