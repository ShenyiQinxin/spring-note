# OOAD
> object oriented analysis and design models a **system** as **a group of interacting objects**. 
> each object represents an entity of interest in the system being modeled.
> each object is characterized by its
>  1. class
>  2. state
>  3. behavior
> models can be created to illustrate the static structure, dynamic behavior and run-time deployment of these collaborating objects.
## terms
### objects = state + behavior, objects are runtime feature of an OO system.
- object has identity, so variables refer to objects such that objects could be passed by their references.
- an object is an instance of its class.
- an object has its set of attributes values. the change of attribute values of one object does not affect other objects in memory.
### classes
- DNA to its objects
- provides metadata for attributes : variable type and initial values
- provides signature and implementation for methods.
## design principles:
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
```
Viechle v = car;
Viechle v = v;
Viechle v = bike;
```
**In Java**
>a sublasses inherites its superclass and overrides methods, at runtime, the type of object decides whether subclass method implementations or superclass method implementations will be executed.
```
v.run();
car.run();
bike.run();
```
>a variable of type T can be assigned objects of T's subtypes at runtime. 
```
Viecle v = new Viecle();
Viecle car = new Car();
Viecle bike = new Bike();
```
### abstract classes
an abstract class is just like a typical class (attributes, methods, constructors (for subclasses to use)), but contains >=0 abstract methods, hence never could be instantiated.
### interfaces
An interface is a contract of its implementation classes.
1. attributes must public static final
2. all abstract methods for implementation purpose. (except default and static methods from Java 8)
3. no constructor.
4. subinterface could inherit uperinterfaces (multiple inheritance).
