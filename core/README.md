# Core Java

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

## Overriding versus Overloading
Method overriding occurs when a subclass provides its own implementation of a method inherited from a superclass. The method must have the same name and parameter list, and its return type must be the same or covariant. Overriding requires inheritance and enables runtime polymorphism, meaning that the method implementation is determined by the actual object type rather than the reference type. The @Override annotation can be used to indicate that a method overrides an inherited method.

Method overloading occurs when multiple methods have the same name but different parameter lists. The methods can differ in the number, types, or order of their parameters. Overloading does not require inheritance and enables compile-time polymorphism, meaning that the compiler determines which method to call based on the arguments provided. Changing only the return type or parameter names is insufficient to overload a method.

## Encapsulation
Encapsulation is an object-oriented programming principle that combines data (fields) and methods into a single class while restricting direct access to the internal state of an object. This is typically achieved by declaring fields as private and providing public getter and setter methods to access or modify them. This protects data from unauthorized or invalid modifications and allows the class to control how its internal state is accessed.

Encapsulation improves data hiding, maintainability, and modularity by separating a class's internal implementation from its public interface. It allows implementation details to change without affecting other classes that use the object. For example, a Person class can declare its age field as private and provide a setter that ensures the age cannot be negative.
