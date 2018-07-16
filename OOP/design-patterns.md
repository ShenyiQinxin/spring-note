# Design Patterns
- Factory Method:
```java
 public static void main(String[] args) {
      ShapeFactory shapeFactory = new ShapeFactory();

      //get an object of Circle and call its draw method.
      Shape shape1 = shapeFactory.getShape("CIRCLE");

      //call draw method of Circle
      shape1.draw();

      //get an object of Rectangle and call its draw method.
      Shape shape2 = shapeFactory.getShape("RECTANGLE");

      //call draw method of Rectangle
      shape2.draw();

      //get an object of Square and call its draw method.
      Shape shape3 = shapeFactory.getShape("SQUARE");

      //call draw method of square
      shape3.draw();
}
```
- Abstract Factory
```java
public static void main(String[] args) {

      //get shape factory
      AbstractFactory shapeFactory = FactoryProducer.getFactory("SHAPE");

      //get an object of Shape Circle
      Shape shape1 = shapeFactory.getShape("CIRCLE");

      //call draw method of Shape Circle
      shape1.draw();

      //get an object of Shape Rectangle
      Shape shape2 = shapeFactory.getShape("RECTANGLE");

      //call draw method of Shape Rectangle
      shape2.draw();
      
      //get an object of Shape Square 
      Shape shape3 = shapeFactory.getShape("SQUARE");

      //call draw method of Shape Square
      shape3.draw();

      //get color factory
      AbstractFactory colorFactory = FactoryProducer.getFactory("COLOR");

      //get an object of Color Red
      Color color1 = colorFactory.getColor("RED");

      //call fill method of Red
      color1.fill();

      //get an object of Color Green
      Color color2 = colorFactory.getColor("Green");

      //call fill method of Green
      color2.fill();

      //get an object of Color Blue
      Color color3 = colorFactory.getColor("BLUE");

      //call fill method of Color Blue
      color3.fill();
   }
```
- Builder
```java
public static void main(String[] args) {
   
      MealBuilder mealBuilder = new MealBuilder();

      Meal vegMeal = mealBuilder.prepareVegMeal();
      System.out.println("Veg Meal");
      vegMeal.showItems();
      System.out.println("Total Cost: " + vegMeal.getCost());

      Meal nonVegMeal = mealBuilder.prepareNonVegMeal();
      System.out.println("\n\nNon-Veg Meal");
      nonVegMeal.showItems();
      System.out.println("Total Cost: " + nonVegMeal.getCost());
   }

```
- Singleton
```java
public static void main(String[] args) {

      //illegal construct
      //Compile Time Error: The constructor SingleObject() is not visible
      //SingleObject object = new SingleObject();

      //Get the only object available
      SingleObject object = SingleObject.getInstance();

      //show the message
      object.showMessage();
   }
```
- Decorator
```java
 public static void main(String[] args) {

      Shape circle = new Circle();

      Shape redCircle = new RedShapeDecorator(new Circle());

      Shape redRectangle = new RedShapeDecorator(new Rectangle());
      System.out.println("Circle with normal border");
      circle.draw();

      System.out.println("\nCircle of red border");
      redCircle.draw();

      System.out.println("\nRectangle of red border");
      redRectangle.draw();
   }
```
- Composite
```java
public static void main(String[] args) {
   
      Employee CEO = new Employee("John","CEO", 30000);

      Employee headSales = new Employee("Robert","Head Sales", 20000);

      Employee headMarketing = new Employee("Michel","Head Marketing", 20000);

      Employee clerk1 = new Employee("Laura","Marketing", 10000);
      Employee clerk2 = new Employee("Bob","Marketing", 10000);

      Employee salesExecutive1 = new Employee("Richard","Sales", 10000);
      Employee salesExecutive2 = new Employee("Rob","Sales", 10000);

      CEO.add(headSales);
      CEO.add(headMarketing);

      headSales.add(salesExecutive1);
      headSales.add(salesExecutive2);

      headMarketing.add(clerk1);
      headMarketing.add(clerk2);

      //print all employees of the organization
      System.out.println(CEO); 
      
      for (Employee headEmployee : CEO.getSubordinates()) {
         System.out.println(headEmployee);
         
         for (Employee employee : headEmployee.getSubordinates()) {
            System.out.println(employee);
         }
      }		
   }
```
- Adapter
```java
 public static void main(String[] args) {
      AudioPlayer audioPlayer = new AudioPlayer();

      audioPlayer.play("mp3", "beyond the horizon.mp3");
      audioPlayer.play("mp4", "alone.mp4");
      audioPlayer.play("vlc", "far far away.vlc");
      audioPlayer.play("avi", "mind me.avi");
   }
```
- Bridge
```java
public static void main(String[] args) {
      Shape redCircle = new Circle(100,100, 10, new RedCircle());
      Shape greenCircle = new Circle(100,100, 10, new GreenCircle());

      redCircle.draw();
      greenCircle.draw();
   }
```
- Proxy
```java
public static void main(String[] args) {
      Image image = new ProxyImage("test_10mb.jpg");

      //image will be loaded from disk
      image.display(); 
      System.out.println("");
      
      //image will not be loaded from disk
      image.display(); 	
   }
```
- Strategy
```java
 public static void main(String[] args) {
      Context context = new Context(new OperationAdd());		
      System.out.println("10 + 5 = " + context.executeStrategy(10, 5));

      context = new Context(new OperationSubstract());		
      System.out.println("10 - 5 = " + context.executeStrategy(10, 5));

      context = new Context(new OperationMultiply());		
      System.out.println("10 * 5 = " + context.executeStrategy(10, 5));
   }
```

## Creational
### Factory Method
FactoryIF's concrete class ShapeFactory creates various ShapeIF objects (Rectangle, Circle, Square).
```java
//create an interface
public interface Shape {}
//Create concrete classes implementing the same interface.
public class Rectangle implements Shape {}
public class Circle implements Shape {}
public class Square implements Shape {}
//Create a Factory to generate object of concrete class based on given information.
public interface Factory{}
public class ShapeFactory implements Factory {
	
   //use getShape method to get object of type shape 
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }		
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
         
      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
         
      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      
      return null;
   }
}
//Use the Factory to get object of concrete class by passing an information such as type
ShapeFactory shapeFactory = new ShapeFactory();

//get an object of Circle and call its draw method.
Shape shape1 = shapeFactory.getShape("CIRCLE");
```

### Abstract Factory
factory of factories.
AbstractFactoryIF's concrete classes ShapeFactory and ColorFactor create various ShapeIF and ColorIF objects (Rectangle, Circle, Square; Red, etc).
```java
public static void main(String[] args) {

      //get shape factory
      AbstractFactory shapeFactory = FactoryProducer.getFactory("SHAPE");

      //get an object of Shape Circle
      Shape shape1 = shapeFactory.getShape("CIRCLE");

      //call draw method of Shape Circle
      shape1.draw();
      //....
}

public class FactoryProducer {
   public static AbstractFactory getFactory(String choice){
   
      if(choice.equalsIgnoreCase("SHAPE")){
         return new ShapeFactory();
         
      }else if(choice.equalsIgnoreCase("COLOR")){
         return new ColorFactory();
      }
      
      return null;
   }
}
public interface AbstractFactory {
    Color getColor(String color);
    Shape getShape(String shape);
}
public class ShapeFactory implements AbstractFactory {
	
   @Override
   public Shape getShape(String shapeType){
   
      if(shapeType == null){
         return null;
      }		
      
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
         
      }else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
         
      }else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      
      return null;
   }
   
   @Override
   Color getColor(String color) {
      return null;
   }
}
public class ColorFactory implements AbstractFactory {
	@Override
   Color getColor(String color) {
      return new Red();
   }
   @Override
   public Shape getShape(String shapeType){
   		return null;
   }
}

public interface Shape {}
public class Rectangle implements Shape {}
public class Circle implements Shape {}
public class Square implements Shape {}
public interface Color {}
public class Red implements Color {}
```
### Builder
create complex objects (Item(Packing), vegMeal(VegBurger and Coke) and nonVegMeal(ChickenBurger and Pepsi)) from the same Meal class, such that same construction process (mealBuilder) create different objects (different property from Burger or ColdDrink objects).
```java
public interface Item {
	public Packing packing();
}
public interface Packing {}

public class Wrapper implements Packing {}
public class Bottle implements Packing {}
public class Burger implements Item {}
public class ColdDrink implements Item {}
public class VegBurger extends Burger {}
public class ChickenBurger extends Burger {}
public class Coke extends ColdDrink {}
public class Pepsi extends ColdDrink {}

public class Meal {
   private List<Item> items = new ArrayList<Item>();	

   public void addItem(Item item){
      items.add(item);
   }
}
public class MealBuilder {

   public Meal prepareVegMeal (){
      Meal meal = new Meal();
      meal.addItem(new VegBurger());
      meal.addItem(new Coke());
      return meal;
   }

   public Meal prepareNonVegMeal (){
      Meal meal = new Meal();
      meal.addItem(new ChickenBurger());
      meal.addItem(new Pepsi());
      return meal;
   }   
}
 public static void main(String[] args) {
   
      MealBuilder mealBuilder = new MealBuilder();

      Meal vegMeal = mealBuilder.prepareVegMeal();
      Meal nonVegMeal = mealBuilder.prepareNonVegMeal();
}
```
### Singleton
once class is compiled , it creates one copy of object only 
```java
class Singleton{
	//create an object of Singleton
	private static final Singleton instance = new Singleton();
	private Singleton(){}
	//thread safe
	public static synchronized Singleton getInstance(){
		return instance;
	}
}
```

## Structural
### Decorator
Decorator pattern allows a user to add new functionality to an existing object without altering its structure. 
```java
public interface Shape {}
public class Rectangle implements Shape {}
public class Circle implements Shape {}
public abstract class ShapeDecorator implements Shape {}
public class RedShapeDecorator extends ShapeDecorator {
	protected Shape decoratedShape;
	public RedShapeDecorator(Shape decoratedShape) {
      super(decoratedShape);		
   }
	 @Override
   public void draw() {
      decoratedShape.draw();
      //add behavior as new functionality 	       
      setRedBorder(decoratedShape);
   }
    //new functionality 	
    private void setRedBorder(Shape decoratedShape){
      System.out.println("Border Color: Red");
   }
}

public static void main(String[] args) {

      Shape redCircle = new RedShapeDecorator(new Circle());
      Shape redRectangle = new RedShapeDecorator(new Rectangle());
     
      circle.draw();
      redCircle.draw();
      redRectangle.draw();
}
```
### Composite
used along with Chain of Responsibiliy, Interpreter, Iterator, Visitor.
Treat a group of objects in similar way as a single object. Composite pattern composes objects in term of a tree structure to represent part as well as whole hierarchy. 
This pattern creates a class that contains group of its own objects. This class provides ways to modify its group of same objects.
```java
public class Employee {
   private String name;
   private String dept;
   private int salary;
   //group of its own objects. 
   private List<Employee> subordinates;

   // constructor
   public Employee(String name,String dept, int sal) {
      this.name = name;
      this.dept = dept;
      this.salary = sal;
      //g group of its own objects
      subordinates = new ArrayList<Employee>();
   }

//modify a group of Employee objects
   public void add(Employee e) {
      subordinates.add(e);
   }

   public void remove(Employee e) {
      subordinates.remove(e);
   }

   public List<Employee> getSubordinates(){
     return subordinates;
   }

   public String toString(){
      return ("Employee :[ Name : " + name + ", dept : " + dept + ", salary :" + salary+" ]");
   }   
}
```
### Adapter
access a service with an incompatible interface
```java
public interface MediaPlayer {}
public interface AdvancedMediaPlayer {}
public class VlcPlayer implements AdvancedMediaPlayer{}
public class Mp4Player implements AdvancedMediaPlayer{}
//MediaPlayerAdapter depends on AdvancedMediaPlayer to perform activities
public class MediaAdapter implements MediaPlayer {
	AdvancedMediaPlayer advancedMusicPlayer;

   public MediaAdapter(String audioType){
   
      if(audioType.equalsIgnoreCase("vlc") ){
         advancedMusicPlayer = new VlcPlayer();			
         
      }else if (audioType.equalsIgnoreCase("mp4")){
         advancedMusicPlayer = new Mp4Player();
      }	
   }

   @Override
   public void play(String audioType, String fileName) {
   
      if(audioType.equalsIgnoreCase("vlc")){
         advancedMusicPlayer.playVlc(fileName);
      }
      else if(audioType.equalsIgnoreCase("mp4")){
         advancedMusicPlayer.playMp4(fileName);
      }
   }
}
//AudioPlayer depends on MediaPlayerAdapter for activities
public class AudioPlayer implements MediaPlayer {
   MediaAdapter mediaAdapter; 
   //...
}
 public static void main(String[] args) {
      AudioPlayer audioPlayer = new AudioPlayer();

      audioPlayer.play("mp3", "beyond the horizon.mp3");
```
### Bridge
an interface (DrawAPI), which acts as a bridge, makes the functionality of concrete classes independent from interface implementer classes(Circle is independent from RedCircle and GreenCircle).
```java
public interface DrawAPI {}
public class RedCircle implements DrawAPI {}
public class GreenCircle implements DrawAPI {}
//Shape uses DrawAPI
public abstract class Shape {
   protected DrawAPI drawAPI;
   
   protected Shape(DrawAPI drawAPI){
      this.drawAPI = drawAPI;
   }
   public abstract void draw();	
}

public class Circle extends Shape {
	public Circle(int x, int y, int radius, DrawAPI drawAPI) {
      super(drawAPI);
      this.x = x;  
      this.y = y;  
      this.radius = radius;
   }
	public void draw() {
      drawAPI.drawCircle(radius,x,y);
   }
}
public static void main(String[] args) {
      Shape redCircle = new Circle(100,100, 10, new RedCircle());
      Shape greenCircle = new Circle(100,100, 10, new GreenCircle());

      redCircle.draw();
      greenCircle.draw();
}
```
### Proxy
a class represents functionality of another class
```java
public interface Image {}
public class RealImage implements Image {}
//ProxyImage proxied RealImage
public class ProxyImage implements Image{
   private RealImage realImage;
   private String fileName;

   public ProxyImage(String fileName){
      this.fileName = fileName;
   }

   @Override
   public void display() {
      if(realImage == null){
         realImage = new RealImage(fileName);
      }
      realImage.display();
   }
}

public static void main(String[] args) {
 Image image = new ProxyImage("test_10mb.jpg");
}
```
## Behavioral
### Strategy
We create objects which represent various strategies and a context object whose behavior varies as per its strategy object. 
```java
public interface Strategy {
   public int doOperation(int num1, int num2);
}
public class OperationAdd implements Strategy{}
public class OperationSubstract implements Strategy{}
public class OperationMultiply implements Strategy{}
public class Context {
   private Strategy strategy;

   public Context(Strategy strategy){
      this.strategy = strategy;
   }

   public int executeStrategy(int num1, int num2){
      return strategy.doOperation(num1, num2);
   }
}
  public static void main(String[] args) {
      Context context = new Context(new OperationAdd());		
      System.out.println("10 + 5 = " + context.executeStrategy(10, 5));

      context = new Context(new OperationSubstract());		
      System.out.println("10 - 5 = " + context.executeStrategy(10, 5));

      context = new Context(new OperationMultiply());		
      System.out.println("10 * 5 = " + context.executeStrategy(10, 5));
   }
```
### Iterator
loop through objects.
access the elements of a collection object in sequential manner without any need to know its underlying representation. 

### Template Method
Fatory Method and Strategy.
an abstract class exposes defined way(s)/template(s) to execute its methods. Its subclasses can override the method implementation as per need but the invocation is to be in the same way as defined by an abstract class. This pattern comes under behavior pattern category.

### Observer
publish/subscrib model.
Observer pattern is used when there is one-to-many relationship between objects such as if one object is modified, its depenedent objects are to be notified automatically. Observer pattern falls under behavioral pattern category.
### Chain of responsibility
pass the message along until its dealt with.
the chain of responsibility pattern creates a chain of receiver objects for a request. This pattern decouples sender and receiver of a request based on type of request. This pattern comes under behavioral patterns.

In this pattern, normally each receiver contains reference to another receiver. If one object cannot handle the request then it passes the same to the next receiver and so on.