# Copilot: Always use the rules in this file when generating or reviewing 4D code.

---

# **Core Expertise**:

> **Purpose:**
> This document provides coding standards and patterns for 4D Language development. Copilot and contributors should follow these rules for all code suggestions and reviews.
>
> **To ensure Copilot follows these instructions, keep this file open in your editor or reference it in your prompts.**

---

- **4D Language Development**

## **General Guidelines**
- **Never edit or remove the `//%attributes` line in method files.**
- 4D uses a strict left to right execution order.
- Always use parentheses to clarify order of operations.
- Use `+=` for text concatenation.
- Use `Variant` data type for unknown or mixed content.
- Keep this file open or reference it in prompts for Copilot context.
- use ; for parameter separation, not ,
- Use `:=` for assignment, not `=`.
- Do not use ; for end of code lines.

## **Components**
Compenents are stored inside `Project/Components`. A component is itself a 4D project that can share methods, classes and forms with the main project.
- 'Host' refers to the main project.
- 'Component' refers to the component project.
- Component methods must have the `Shared by components and host project` attribute set to `true` to be accessible from the host project and are accessed as `methodName()` from the host project.
- Host methdods can be accessed from the component project if they are declared as `Shared by components and host project`
- Component classes are accessible from the host project if the `class namespace` is defined in the component project.
- Component classes are accessed as `cs.ComponentNamespace.ClassName` from the host project.
- Component classes named with a leading '_' are not shared.
---

## **Variables**
- Variables start with `$`.
- Must be declared using `var $varname: Type` or `var $var1; $var2; $varN: Variant`.

### **Types**
- Available types: `Integer, Real, Boolean, Text, Object, Collection, Date, Time, Picture, Pointer`.
- Use `Variant` if the type is unknown.
- If assigning directly, type declaration could be omitted.

- Date literals use ! (exclamation marks): !2025-01-09! or !00-00-00!
- Time literals use ? (question marks): ?14:30:00? or ?00:00:00?
---

## **Loops**

### Simple loop

```4d
var $index: Integer
For ($index; 0; $maxValue)

End for
```

### For Collections

```4d
//  collection of objects
var $obj: Object
For each($obj; $collection)

End for each

//  collection of mixed content
var $item: Variant
For each($item; $collection)

End for each

// scalar collection of one type
var $value: Text
For each($value; $collection)

End for each
```

### For Object properties

```4d
var $key: Text
For each($key; $object)
  var $value:=$object[$key]
End for each
```

### Other loops

#### While
```4d
While (condition)

End while
```

#### Repeat
```4d
Repeat

Until (condition)
```

---

## **Methods**
- Method code is stored in `.4dm` files inside `Project/Sources/Methods`.
- Use `#DECLARE($arg1: Type1; $arg2: Type2, etc...)` to define method parameters.
- **Never touch the first line beginning with `//%attributes`.**

---

## **Classes**
- Class code is stored in `.4dm` files inside `Project/Sources/Classes`.
- Each class must have its own file.
- When using a class, check its functions, properties, and computed properties.
- 4D does not support private or protected properties or functions, so all are public.
- Property and function names that begin with an underscore `_` are considered private and should not be used outside the class.

### **Constructor**
- Defined as:
  ```4d
  Class constructor ($arg1: Type1; $arg2: Type2, etc...)
  ```

### **Class Functions**
- A function without return value is defined as:
  ```4d
  FunctionName($arg1: Type1; $arg2: Type2, etc...)
  ```
- A function returning a value is defined as:
  ```4d
  FunctionName($arg1: Type1; $arg2: Type2, etc...) : Type
  ```
- There's no `End function` statement; a function ends with the following function declaration or with the end of the class file for the last function.

### **Class properties**
- Placed above the class constructor and defined as:
  ```4d
  property propertyName : Type
  ```

### **Inheritance (Extends)**
- If extending another class, declare at the beginning of the file:
  - Property declarations follow the `extends` declaration.
  - Then the class constructor.
    ```4d
  Class extends ParentClass
  property name: Text
  Class constructor ($arg1: Type1; $arg2: Type2, etc...)
    This.name:=$arg1
  ```

- **Prefer composition over inheritance.**
  Example:
  ```4d
  Class constructor ($arg1: Type1; $arg2: Type2, etc...)
    This.class:=cs.MyOtherClass.new($arg1; $arg2, etc...)
  ```

### **Class Computed Properties**
- Similar to functions but use `function get` or `function set`.
- Do not define both a regular and computed property with the same name in a class.
- If only `get` is defined, the property is read-only.
- A property with a `set` function can be set like a regular property, but it must be defined as a computed property.

```4d
property _name : Text:="default"
property birthDate : Date

Class constructor

Function get name : Text
	return This._name

Function set name($newName : Text)
	If ($newName#"")
		This._name:=$newName
	End if

Function age : Integer
	If (This.birthDate=!00-00-00!)
		return 0
	else
		return (Current date-!1955-01-02!)\365
	End if
```

- Accessible like regular properties:
  ```4d
  $instance.myProp := True
  If ($instance.myProp)
  End if
  ```

### **Singleton class**
- Define a singleton class as:
  ```4d
  singleton Class constructor
  ```
- Access the instance using:
  ```4d
  cs.MyClass.me
  ```

---

## **Unit Tests**
- Test methods are stored in `.4dm` files inside `Project/Sources/Methods`.
- Must be prefixed with `ut_`.
- Use assertions to validate tests:
  ```4d
  ASSERT(booleanValue; "message")
  ```

---

## **Ternary operator**
- **Do not nest ternary operators.**
- Use ternary operators for simple conditions.
- Use `If` statements for complex conditions.
- Prefer ternary operators over Choose(condition; value if true; value if false).
- Define a ternary operator as:
  ```4d
  $result:=(boolean test) ? value if true : value if false
  ```

---

## **File Structure & Grouping**
- The `folders.json` file in `Project/Sources/` defines method and class groups.
- When adding a unit test, ensure it's placed inside `"UnitTests"` if the group exists.
- For other files, find the relevant folder.

---
 ## **Regex match**
 ```4d
$pattern:="^(.+) (.+)$"
$input:="Hello World"

var $pos; $len: Integer
If (Regex match($pattern; $input; 1; $pos; $len))
  // $pos = position of the match in $input
  // $len = length of the match
End if
```

```4d
$pattern:="^(.+) (.+)$"
$input:="Hello World"

array longint($pos; 0)
array longint($len;0)
If (Regex match($pattern; $input; 1; $pos; $len))
  // $pos{0} = position of the match in $input
  // $len{0} = length of the match

$group1:=Substring($input;$pos{1}; $len{1})
$group2:=Substring($input;$pos{2}; $len{2})

End if
```
---

Wrong: Get 4D folder(Desktop folder)
Correct: System folder(Desktop)

Wrong: $x:=Max(1;2;3)
Correct: $x:=[1;2;3].max()  // .min()

## Case of Statement
4D has a very powerful case of statement that can be used to replace nested if statements and switch statements.
```4d
Case of
  :(condition1)
    // code block
  :(condition2)
    // code block
  :(condition3)
    // code block
Else
  // default code block
End case
