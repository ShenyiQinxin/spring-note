## OOP design concepts
- methods defined in a class represent the **behavior** that instances of the class will have during **runtime**. this behavior is invoked by sending **messages**(calling methods) to that object.
- **associations**
	- **Dependency** association ( `Driver --->Vehicle`)
		- i.e. `class Driver{Vehicle}`
		- **driver** depends on **vehicle**
	- **Composition** association (`Vehicle<solid>---->Engine`)
		- i.e. `class Vehicle{Engine myEngine;}` 
		- **Vehicle** owns an **Engine**
	- **Aggregation** association (`SpanishCouse <empty>---> Student`)
		- i.e. class SpanishCourse{Student}
		- **SpanishCourse** has many **Student**, but it does not own any Student
## Software Design best practice
- keey diagrams simple
- consider the behavior not the physical relationships
i.e. `Customer --stopNewspaperSubscription()--> NewspaperCompany`
rather than `NewspaperCompany--->Customer` has-A relationship
- class names should be nouns based on their intent
## principles
- single responsibility
- open closed
	- open for extension & closed for modification
- liskov substitution
	- subclasses must be substitutable with their baseclasses
- interface segregation
	- use separated interfaces to decouple concerns , indead of using a "fat" class as a dependency. 
	- prevent clients depends on methods that they do not use(all in one class).
- dependency inversion
	- depend on abstract classes/interfaces that dont change as often as concrete classes. 
## OO design patterns
### dependency injection
### observer pattern
>a subject notifies a list of oberservers of its state changes, by calling obervers' methods. Mainly used to implement distributed event handling systems. it is a key part of MVC pattern.
>in Spring, any bean (observer) who implements ApplicationListener interface will receive ApplicationEvent when it is published by event publisher(subject)
### builder pattern
>for creating object instances for classes have large number of constructor arguments
```java
public class User {
 private String userName; // Required
    private String emailAddress; // Required
    private User(Builder builder){
        this.userName = builder.userName;
        this.emailAddress = builder.emailAddress;
    }
    public static class Builder{
         
        private String userName; // Required
        private String emailAddress; // Required
        public Builder(String userName, String email){
            this.userName = userName;
            this.emailAddress = email;
     }
    public Builder firstName(String value){
            this.firstName = value;
            return this;
    }
     public User build(){
            return new User(this);
     }
}
 User websiteUser = new User.Builder("bobMax", "bobMax@gmail.com").firstName("bob").lastName("max").build();
```
### Factory design pattern
```java
public interface Vehicle {
	public void startEngine();
}
//Car, ElectricCar, Truck 
public class Car implements Vehicle {
    @Override
    public void startEngine() {
        System.out.println("started simple engine of car...");
    }
 
}
//
public class VehicleFactory {  
    public Vehicle getVehicle( VehicleType vehicleType){
        //if vehicleType.TRUCK, return newTruck();
        return vehicleType.getVehicle();
    }
}
public enum VehicleType   {         
    TRUCK{
        public Vehicle getVehicle(){
            return new Truck();
        }
    },
    CAR{
        public Vehicle getVehicle(){
            return new Car();
        }
    },
    ELECTRIC{
        public Vehicle getVehicle(){
            return new ElectricCar();
        }
    };
 
    abstract Vehicle getVehicle();
}
public class App {
    public static void main(String args[]){
        VehicleFactory vehicleFactory = new VehicleFactory();
         
        Vehicle vehicle = vehicleFactory.getVehicle(VehicleType.CAR);
        vehicle.startEngine();
    }
}
```
### singleton design pattern

>private constructor
```java
class PerformanceStage{
	private static PerformanceStage INSTANCE = null;
	private PerformanceStage(){}
	public synchronized static PerformanceStage getInstance(){
		if(INSTANCE == null){
			INSTANCE = new PerformanceStage();
		}
		return INSTANCE;
	}
}

class Application{
	public static void main(String[] args){
		PerformanceStage stage = PerformanceStage.getInstance();
	}
}

```



