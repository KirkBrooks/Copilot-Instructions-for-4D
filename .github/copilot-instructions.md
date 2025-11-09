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

## Structure

The structure of a 4D database is defined in the `.4DCatalog` file. This is an XML file. The dtd "http://www.4d.com/dtd/2007/base.dtd" does not exist and is not the valid dtd.

- follow the pattern of the existing formatting pattern of the catalog

## **Collection Operations**

### **Filtering**
```4d
// Filter collection with formula
$filtered:=$collection.filter(Formula($1.value.status="active"))

// Filter with named parameters for clarity
$filtered:=$collection.filter(Formula($1.value.width>This.minWidth))
```

### **Mapping & Transformation**
```4d
// Extract property values
$ids:=$collection.extract("id")

// Transform with formula
$volumes:=$collection.map(Formula($1.value.width*$1.value.height*$1.value.depth))
```

### **Sorting**
```4d
// Single property
$sorted:=$collection.orderBy("volume desc")

// Multiple properties
$sorted:=$collection.orderBy("priority desc; name asc")
```

### **Reducing**
```4d
// Sum values
$total:=$collection.reduce(Formula($1.accumulator+$1.value.price); 0)

// Build object
$result:=$collection.reduce(Formula($1.accumulator[$1.value.id]:=$1.value); New object)
```

### **Query**
```4d
// Query entity selection or collection
$results:=ds.Table.query("field = :1"; $value)
$results:=$collection.query("status = :1 AND price > :2"; "active"; 100)
```

---

## **Object Operations**

### **Property Assignment**
```4d
// Direct assignment
$obj:=New object("name"; "value"; "count"; 5)

// Dynamic property names
$obj[$dynamicKey]:=$value

// Nested objects
$obj.address:=New object("street"; "123 Main"; "city"; "Boston")
```

### **Object Copying**
```4d
// Shallow copy
$copy:=OB Copy($original)

// Deep copy (for nested structures)
$deepCopy:=OB Copy($original; ck shared; $groups)  // Use ck shared carefully
```

### **Property Existence**
```4d
If (OB Is defined($obj; "propertyName"))
  // Property exists
End if

// For nested properties
If (OB Is defined($obj; "address.city"))
  // Nested property exists
End if
```

---

## **Class Design Patterns**

### **Data Classes (Entity-like)**
```4d
// For classes representing data structures
Class constructor
  This.id:=Generate UUID
  This.created:=Current date
  This.data:=New object

Function toObject() -> $obj : Object
  // Convert to plain object for storage/transmission
  $obj:=New object
  $obj.id:=This.id
  $obj.created:=This.created
  $obj.data:=OB Copy(This.data)

Function fromObject($obj : Object) -> $instance : cs.ClassName
  // Static-like function to create from object
  $instance:=cs.ClassName.new()
  $instance.id:=$obj.id
  $instance.created:=$obj.created
  $instance.data:=$obj.data
```

### **Strategy Pattern**
```4d
// Base strategy class
Class constructor
  // Base setup

Function execute($context : Object)
  ASSERT(False; "Override execute() in subclass")

// Concrete strategy
Class extends BaseStrategy

Function execute($context : Object)
  // Implementation specific logic
  $context.result:=This._process($context.data)

Function _process($data : Variant) -> $result : Variant
  // Private helper (leading underscore convention)
  $result:=$data
```

### **Builder Pattern**
```4d
Class constructor
  This._config:=New object

Function setProperty($value : Variant) -> $builder : cs.Builder
  This._config.property:=$value
  $builder:=This  // Return self for chaining

Function build() -> $instance : cs.TargetClass
  // Validate and create
  ASSERT(This._isValid(); "Invalid configuration")
  $instance:=cs.TargetClass.new(This._config)

Function _isValid() -> $valid : Boolean
  $valid:=(This._config.property#Null)
```

---

## **Error Handling**

### **Assertions**
```4d
// Use assertions for development/testing
ASSERT($value>0; "Value must be positive")
ASSERT($collection.length>0; "Collection cannot be empty")

// Use for type checking
ASSERT(Value type($param)=Is object; "Expected object parameter")
```

### **Try/Catch (Component Calls)**
```4d
// When calling methods that might fail
var $error : Object
$error:=Try(ComponentMethod($param))

If ($error.success)
  // Process result
Else
  // Handle error
  LOG EVENT(Into system standard outputs; $error.errorMessage)
End if
```

### **Method Return Objects**
```4d
// Return success/error status from methods
Function process($data : Variant) -> $result : Object
  $result:=New object("success"; False)

  If ($data=Null)
    $result.error:="Data cannot be null"
    return $result
  End if

  // Process...
  $result.success:=True
  $result.data:=$processedData
```

---

## **Performance Considerations**

### **Collection vs. Array**
- Prefer Collections over Arrays for most use cases
- Collections are more flexible and have rich built-in methods
- Arrays may be faster for very large datasets with simple operations

### **Object Property Access**
```4d
// Faster - direct property access when key is known
$value:=$obj.propertyName

// Slower - dynamic property access
$value:=$obj[$variableKey]
```

### **Formula Scope**
```4d
// Formulas can access This and local context
$filtered:=$collection.filter(Formula($1.value.width>This.minWidth))

// Be aware of scope and avoid unintended closures
var $threshold : Real
$threshold:=100
$filtered:=$collection.filter(Formula($1.value.price>$threshold))  // Captures $threshold
```

---

## **UUID Generation**
```4d
// Generate unique identifiers
$uuid:=Generate UUID

// Common pattern for entity IDs
Class constructor
  This.id:=Generate UUID
  This.created:=Timestamp
```

---

## **Logging & Debugging**

### **Console Output**
```4d
// Log to system console
LOG EVENT(Into system standard outputs; "Message: "+$value)

// Log objects/collections (auto-formatted)
LOG EVENT(Into system standard outputs; JSON Stringify($obj; *))
```

### **Trace**
```4d
// Enter debugger
TRACE

// Conditional debugging
If ($debugMode)
  TRACE
End if
```

---

## **Common Patterns in This Project**

### **Volume Calculation**
```4d
// For box/item dimensions
Function get volume() -> $vol : Real
  $vol:=This.width*This.height*This.depth
```

### **Collection Queries**
```4d
// Find items that fit criteria
$fits:=$items.query("width <= :1 AND height <= :2 AND depth <= :3"; \
  $box.width; $box.height; $box.depth)
```

### **Fluent Interfaces**
```4d
// Method chaining for configuration
$shipment:=cs.Shipment.new($items; $strategy; $boxes).pack()
```

---

## **Testing Patterns**

### **Unit Test Structure**
```4d
// filepath: Project/Sources/Methods/ut_ClassName_functionName.4dm
//%attributes = {"invisible":true}

var $instance : cs.ClassName
var $result : Variant

// Arrange
$instance:=cs.ClassName.new($params)

// Act
$result:=$instance.functionName($args)

// Assert
ASSERT($result.success; "Function should succeed")
ASSERT($result.value=42; "Expected value 42")
```

### **Test Data Setup**
```4d
// Create test fixtures
Function _createTestBox() -> $box : cs.zBox
  $box:=cs.zBox.new()
  $box.width:=10
  $box.height:=10
  $box.depth:=10
  $box.name:="Test Box"
```

---

## **Naming Conventions Used in This Project**

- **Classes**: PascalCase (e.g., `zBox`, `PackingStrategy`, `ShipmentOptimizer`)
- **Functions/Methods**: camelCase (e.g., `pack`, `getSummary`, `findOptimalShipment`)
- **Properties**: camelCase (e.g., `packedBoxes`, `itemsToPack`, `strategyName`)
- **Private members**: Leading underscore (e.g., `_config`, `_process`, `_isValid`)
- **Unit tests**: Prefix `ut_` (e.g., `ut_zBox_volume`)
- **Constants**: ALL_CAPS (when needed, though 4D doesn't have true constants)

---
