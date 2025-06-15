# **Core Expertise**:
- **4D Language Development**

## **Variables**
- Variables start with `$`.
- Must be declared using `var $varname: Type`.

### **Types**
- Available types: `Integer, Real, Boolean, Text, Object, Collection, Date, Time, Picture, Pointer`.
- Use `Variant` if the type is unknown.
- If assigning directly, type declaration could be omitted.

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

### other loops

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

## **Methods**
- Method code is stored in `.4dm` files inside `Project/Sources/Methods`.
- Use `#DECLARE($arg1: Type1; $arg2: Type2, etc...)` to define method parameters.
- The first line beginnning with '//%attributes' shall never be touched.

## **Classes**
- Class code is stored in `.4dm` files inside `Project/Sources/Classes`.
- Each class must have its own file.
- When using a class, check its functions, properties, and computed properties.

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
- There's no `End function` statement, a function ends with the following function declaration or with the end of the class file for the last function.

### **Class properties**
- Placed above the class constructor and defined as:
  ```4d
 property propertyName : Type'
  ```

### **Inheritance (Extends)**
- If extending another class, declare at the beginning of the file:
  ```4d
  Class extends ParentClass
  ```
  - prefer composition over inheritance,
  ```4d
  Class constructor ($arg1: Type1; $arg2: Type2, etc...)
        This.class:=cs.MyOtherClass.new($arg1; $arg2, etc...)
  ```

### **Class Computed Properties**
- Similar to functions but use `function get` or `function set`.
- If only `get` is defined, the property is read-only.
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
## **Unit Tests**
- Test methods are stored in `.4dm` files inside `Project/Sources/Methods`.
- Must be prefixed with `ut_`.
- Use assertions to validate tests:
  ```4d
  ASSERT(booleanValue; "message")
  ```
## **Ternary operator**
- Define a ternary operator as:
  ```4d
  $result:=(boolean test) ? value if true : value if false
  ```
## File Structure & Grouping
- The `folders.json` file in `Project/Sources/` defines method and class groups.
- When adding a unit test, ensure it's placed inside `"UnitTests"` if the group exists.
- For other files, find the relevant folder.
