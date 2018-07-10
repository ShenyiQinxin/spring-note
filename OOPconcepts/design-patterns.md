# Design Patterns
## Creational
### Factory Method
factory for creating one interface of object
```java
//create an interface
public interface Shape {
   void draw();
}
//Create concrete classes implementing the same interface.
public class Rectangle implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Rectangle::draw() method.");
   }
}
//...
//Create a Factory to generate object of concrete class based on given information.
public class ShapeFactory {
	
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
hapeFactory shapeFactory = new ShapeFactory();

//get an object of Circle and call its draw method.
Shape shape1 = shapeFactory.getShape("CIRCLE");
```

### Abstract Factory
An interface for creating multi interfaces of objects.
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
public abstract class AbstractFactory {
   abstract Color getColor(String color);
   abstract Shape getShape(String shape) ;
}
public class ShapeFactory extends AbstractFactory {
	
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
//ColorFactory.java

public interface Shape {
   void draw();
}
public class Rectangle implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Rectangle::draw() method.");
   }
}
//...
public interface Color {
   void fill();
}
public class Red implements Color {

   @Override
   public void fill() {
      System.out.println("Inside Red::fill() method.");
   }
}
```
### Builder
create complex objects such that same construction process create different objects.
```java
public interface Item {
	public Packing packing();
}
public interface Packing {
   public String pack();
}
public class Wrapper implements Packing {}
//...
public abstract class Burger implements Item {}
//...
public class VegBurger extends Burger {}
//...
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
}
 public static void main(String[] args) {
   
      MealBuilder mealBuilder = new MealBuilder();

      Meal vegMeal = mealBuilder.prepareVegMeal();
      vegMeal.showItems();
      vegMeal.getCost();
}
```
### Singleton
once class is compiled , there is one copy of object only 
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
	 @Override
   public void draw() {
      decoratedShape.draw();
      //add behavior on top 	       
      setRedBorder(decoratedShape);
   }
    private void setRedBorder(Shape decoratedShape){
      System.out.println("Border Color: Red");
   }
}

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
### Composite
used along with Chain of Responsibiliy, Interpreter, Iterator, Visitor
we need to treat a group of objects in similar way as a single object. Composite pattern composes objects in term of a tree structure to represent part as well as whole hierarchy. This type of design pattern comes under structural pattern as this pattern creates a tree structure of group of objects.

This pattern creates a class that contains group of its own objects. This class provides ways to modify its group of same objects.
### Adapter
access a service with an incompatible interface
```java
public interface MediaPlayer {}
public interface AdvancedMediaPlayer {}
public class VlcPlayer implements AdvancedMediaPlayer{}
public class Mp4Player implements AdvancedMediaPlayer{}
//inside of MediaPlayerAdapter, it depends on AdvancedMediaPlayer to perform activities
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
decouple the interface/abstraction with implementation, such that the two are independent with each other
```java
public interface DrawAPI {}
public class RedCircle implements DrawAPI {}
public class GreenCircle implements DrawAPI {}
public abstract class Shape {
   protected DrawAPI drawAPI;
   
   protected Shape(DrawAPI drawAPI){
      this.drawAPI = drawAPI;
   }
   public abstract void draw();	
}
public class Circle extends Shape {
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
we create objects which represent various strategies and a context object whose behavior varies as per its strategy object. 
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