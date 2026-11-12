# Core Java

## Method hiding
Method hiding in Java occurs when a subclass declares a static method with the same signature as a static method in its superclass.

Unlike method overriding, method hiding uses compile-time reference types to determine which method is called, rather than the actual object type.

## Field declarations

| Modifier | Same class | Same package | Subclass (different package) | Other packages |
|---|---|---|---|---|
| `private` | Yes | No | No | No |
| *(no modifier)* package-private | Yes | Yes | No | No |
| `protected` | Yes | Yes | Yes* | No |
| `public` | Yes | Yes | Yes | Yes |

| Modifier | Meaning |
|---|---|
| `static` | Belongs to the class rather than an instance |
| `final` | Can only be assigned once |
| `transient` | Excluded from default serialization |
| `volatile` | Guarantees visibility and ordering of reads/writes across threads |

Transient fields in Java are instance variables declared with the transient keyword. They are excluded from the default Java serialization process.

When an object is serialized, transient fields are not saved to the serialized representation. When the object is deserialized, they receive their default values.

Volatile fields in Java are instance variables declared with the volatile keyword. They ensure that reads and writes to a variable are visible across threads.

When one thread modifies a volatile field, other threads will observe the updated value rather than relying on a potentially stale cached value.

## Overriding versus Overloading
Method overriding occurs when a subclass provides its own implementation of a method inherited from a superclass. The method must have the same name and parameter list, and its return type must be the same or covariant. Overriding requires inheritance and enables runtime polymorphism, meaning that the method implementation is determined by the actual object type rather than the reference type. The @Override annotation can be used to indicate that a method overrides an inherited method.

Method overloading occurs when multiple methods have the same name but different parameter lists. The methods can differ in the number, types, or order of their parameters. Overloading does not require inheritance and enables compile-time polymorphism, meaning that the compiler determines which method to call based on the arguments provided. Changing only the return type or parameter names is insufficient to overload a method.

Polymorphism can be achieved through method overriding (runtime polymorphism) and method overloading (compile-time polymorphism). Java does not support operator overloading.

## Encapsulation
Encapsulation is an object-oriented programming principle that combines data (fields) and methods into a single class while restricting direct access to the internal state of an object. This is typically achieved by declaring fields as private and providing public getter and setter methods to access or modify them. This protects data from unauthorized or invalid modifications and allows the class to control how its internal state is accessed.

Encapsulation improves data hiding, maintainability, and modularity by separating a class's internal implementation from its public interface. It allows implementation details to change without affecting other classes that use the object. For example, a Person class can declare its age field as private and provide a setter that ensures the age cannot be negative.

Java supports several mechanisms for encapsulation and information hiding, which control access to an object's internal state and implementation details:
1. Data hiding: Declare fields as private to prevent direct access from other classes.
2. Access modifiers: Use private, package-private, protected, and public to control visibility of fields, methods, and constructors.
3. Getters and setters: Provide controlled access to private fields, allowing validation and restricting modifications.
4. Implementation hiding: Expose only the public interface of a class while hiding internal implementation details.
5. Abstraction: Use interfaces and abstract classes to define functionality without exposing implementation details.
6. Immutability: Use final fields and avoid setters to prevent an object's state from changing after construction.
7. Constructor validation: Validate input during object creation to ensure that objects start in a valid state.
8. Package encapsulation: Use packages and Java modules to restrict access to implementation classes and expose only intended APIs.

## Immutable objects
Additional mechanisms include immutable objects, where fields cannot be changed after construction (often using final fields and no setters), and validation within setter methods to prevent invalid values. Java also supports encapsulation through packages and modules, which control access between classes and define which packages are exposed to other modules.

## Operators
| Operator | Name | Meaning |
|---|---|---|
| `&` | Bitwise AND | Bit is `1` if both bits are `1` |
| `^` | Bitwise XOR | Bit is `1` if the bits are different |
| `\|` | Bitwise OR | Bit is `1` if at least one bit is `1` |

## Interface, Abstract Class and Concrete Class

```java
// Interface
interface Animal {

    // Abstract methods
    void makeSound();
    void eat();

    // Default method
    default void sleep() {
        System.out.println("Animal is sleeping");
    }
}

// Abstract class implementing the interface
abstract class Mammal implements Animal {

    protected String name;

    public Mammal(String name) {
        this.name = name;
    }

    // Implementing an interface method
    @Override
    public void eat() {
        System.out.println(name + " is eating");
    }

    // Concrete method
    public void breathe() {
        System.out.println(name + " is breathing");
    }

    // Abstract method
    public abstract void walk();
}

// Concrete class extending the abstract class
class Dog extends Mammal {

    public Dog(String name) {
        super(name);
    }

    // Implementing interface abstract method
    @Override
    public void makeSound() {
        System.out.println(name + " says: Woof!");
    }

    // Implementing abstract class method
    @Override
    public void walk() {
        System.out.println(name + " is walking");
    }

    // Overriding interface default method
    @Override
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
}

// Main class
public class Main {

    public static void main(String[] args) {

        Dog dog = new Dog("Buddy");

        dog.makeSound();
        dog.eat();
        dog.sleep();
        dog.breathe();
        dog.walk();
    }
}
```

### Output

```text
Buddy says: Woof!
Buddy is eating
Buddy is sleeping
Buddy is breathing
Buddy is walking
```

### Key concepts

| Concept | Example | Description |
|---|---|---|
| Interface | `Animal` | Defines a contract |
| Abstract method | `void makeSound()` | Must be implemented by a concrete subclass |
| Default method | `default void sleep()` | Provides a default implementation |
| Abstract class | `Mammal` | Provides shared state and functionality |
| Constructor | `Mammal(String name)` | Initializes the object |
| Inheritance | `extends Mammal` | Inherits fields and methods |
| Interface implementation | `implements Animal` | Implements an interface |
| Method overriding | `@Override` | Provides a new implementation of an inherited method |
| Concrete class | `Dog` | Implements the required methods and can be instantiated |
