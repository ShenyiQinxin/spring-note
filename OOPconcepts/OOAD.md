
# OOAD
> object oriented analysis and design models a **system** as **a group of interacting objects**. 
> each object represents an entity of interest in the system being modeled.
> each object is characterized by its
>  1. class
>  2. state
>  3. behavior
> models can be created to illustrate the static structure, dynamic behavior and run-time deployment of these collaborating objects.


## objects 
> state + behavior, objects are runtime feature of an OO system.
- object has identity, so variables refer to objects such that objects could be passed by their references.
- an object is an instance of its class.
- an object has its set of attributes values. the change of attribute values of one object does not affect other objects in memory.

## class
- DNA to its objects
- provides metadata for attributes : variable type and initial values
- provides signature and implementation for methods.

## abstract classes
an abstract class is just like a typical class (attributes, methods, constructors (for subclasses to use)), but contains >=0 abstract methods, hence never could be instantiated.

##interfaces
An interface is a contract of its implementation classes.
1. attributes must public static final
2. all abstract methods for implementation purpose. (except default and static methods from Java 8)
3. no constructor.
4. subinterface could inherit uperinterfaces (multiple inheritance).

# OO features:
### abstraction
- according to system requirements, an object is created in a simplified view and ignore irrelevant details. for instance, to drive a car, we only care the data on dashboard and how to use the auto parts for driving. we do not need to know how a car works under the hood.

### encapsulation
an object is featured as a capsule that hold its internal state within its boundaries. to create a proper capsulation, all attributes must be private and getters and setters are provided as **abstract interface** for data access. to use an encapsulated object, we only need to know the purpose and signature of public methods (abstract interface).
- good for code refactoring and decoupling.
### inheritance
- subclass inherites the attributes and methods from superclass
- subclass methods can override superclass methods
- Java supports single inheritance (one subclass inherites one single super class)
- inheritance should conform to Liskov substitution principle. If we substitute a subclass(Audi) for a superclass(Car), all the methods expected to have in superclass are still accessible including the overriden methods.

### polymorphism
- compile time
>ex: for two methods with different method arguments, the compiler decides which method is to be called based on parameters types/numbers.
- run time
concept: A variable denotes objects of subclasses or superclass.
```java
Vehicle v = car;
Vehicle v = v;
Vehicle v = bike;
```
**In Java**
>a sublasses inherites its superclass and overrides methods, at runtime, the type of object decides whether subclass method implementations or superclass method implementations will be executed.
```java
v.run();
car.run();
bike.run();
```
>a variable of type T can be assigned objects of T's subtypes at runtime. 
```java
Vehicle v = new Vehicle();
Vehicle car = new Car();
Vehicle bike = new Bike();
```



# UML unified modeling language
- structure diagram - class diagram - structure of classes and relations
- behavior diagram - sequence diagram -interactions 

### generalization
```java
Vehicle is a generalization of Bike and Car
```
### specialization
```java
Car is a specialization of Vehicle
```
### Realization 
```java
Car class is a realization of Lockable interface
```
## Associations
one class uses the service from another class

### Dependency
```java
Engine <---- Car
Car is dependent on Engine
Car{
	Engine engine;
}
```
### Aggregation
```java
Department has-a Teacher, 
but Teacher could exists without Department
Department <empty>____>Teacher
```
### Composition
```java
House is-composed-of-a Room, 
so Room could not survive without House
House <solid>_____>Room
```
# Design Principles SOLID
## single responsibility
a class/module has only one responsibility
```java
class Car{name, model, year}
but wont includ CarDAO CRUD operations 
```
## open/close principle
open for extension & closed for modification
once developed and tested a module, we should not modify it anymore, instead we could extend it in terms of changing.
changes in one module might affect other functionalities so that it may bring additional work/risks.

## Liskov substitution principle
```java
Car must be substitutable for Vehicle, meaning Car can be used as a Vehicle
this substitution must be from behavior point of view, called strong behavioral substyping
```
## interface segregation principle
client should not depend on interfaces that they do not use,
use separated interfaces to decouple concerns, instead of using a fat class as a dependency
```java
Mechanic --->IRepairable
Dealer --->ISellable
Car --implement--|>IRepairable, ISellable
not
Mechanic --->ICar
```
## Dependency Inversion Principle
low-level module depends on high-level module, 
both depends on abstraction. Details depend on abstraction.
it means depend on abstract classes/interfaces that dont change as often as concrete classes.