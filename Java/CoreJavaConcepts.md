# Collection
### important interfaces in the Collection Hierarchy
`interface Collection<E> extends Iterable<E>{}`
- **no dup** elements `if obj1.equals(obj2) then `only one of them can be in the Set; **no order** of elements
	- `interface Set<E> extends Collection<E>{}`
- focusing on operations on certain **positions** of elements, if no position specified, maintains the insert order, add/remove at the end
	- `interface List<E> extends Collection<E>{}`
- elements order is **FIFO**
	- `interface Queue<E> extends Collection<E>{}`
- `{("key1", value1),("key2", value2),...}`  **K-V pairs** and **unique keys**
	- `interface Map<K,V>{}`
### methods in Collection
```java
boolean add(E); boolean addAll(Collection< extends E>);

boolean remove(E); boolean removeAll(Collection< extends E>);

boolean contains(Object); boolean containsAll(Collection<>);

boolean retainAll(Collection< extends E>);

int size(); boolean isEmpty(); void clear();

Iterator<E> iterator();
```
### methods in `List<E>`
```java
E get(int); E set(int, E);

void add(int, E); boolean addAll(int, Collection<? extends E>);

E remove(int, E); 

int indexOf(Object); int lastIndexOf(Object);

List<E> subList(int, int);

ListIterator<E> listIterator(); ListIterator<E> listIterator(int);
``` 
### ArrayList
`class ArrayList<E> implements List<E>{}`
>iterate around an ArrayList
```java
Iterator<E> arrayListIterator = arraylist.iterator();
while(arrayListIterator.hasNext()){
	E str = arrayListIterator.next();
}
```
>sort an ArrayList **alphabetically**
```java
**Collections.sort**(arrayList<E>);
```
>sort an ArrayList in a defined order -- **`Comparable<E>`**
```java
class Artist implements Comparable<Artist> {
	String name;
	int score;
//override toString()
	public String toString(){
		return name +" : "+score;
	}
//override compareTo(E);
	public int compareTo(Artist her){
		if(this.score > her.score){
			return 1;
		} 
		if(this.score < her.score){
			return -1;
		}
		return 0;
	}
}
Collections.sort(new ArrayList<Artist>);
```
> sort an ArrayList in a defined order -- **`Comparator<E>`**
```java
class DescendingSorter implements Comparator<Artist>{
//override compare(E, E)
	public int compare(Artist a1, Artist a2){
		if(a1.score > a2.score){
			return -1;
		}
		if(a1.score < a2.score){
			return 1;
		}
		return 0;
	}
}
Collections.sort(new ArrayList<Artist>, new DescendingSorter());
```
### class Vector vs. ArrayList
1. The methods are the same except Vector methods are all **synchronized**, aka Thread Safe.
2. `class Vector implements List<E>, RandomAccess{}` RandomAccess is a marker interface which supports **fast access**(constant time).
### `LinkedList<E>`
`class LinkedList<E> implements List<E>, Queue<E>{}`
1. **doubly** linked list - forwards and backwords
2. choice to implement **stack** and **queue**
3. Queue methods : **`peek(), poll()`** getting the elements at head  of the queue.  poll() removes the head from the queue.

|| ArrayList | LinkedList |
|--|--|--|
| iteration/**search** | quick |slow|
|**insertion n deletion**|slow|quick|
|**implementation**|Array|linked list|


### interfaces in `Set<E>` hierachy
>`SortedSet<E>` elements are in **sorted** order
```java
interface SortedSet<E> extends Set<E>{
	SortedSet<E> subSet(E, E);
	SortedSet<E> headSet(E);
	SortedSet<E> tailSet(E);
	E first();
	E last();
}
```
>`NavigableSet<E>` reporting the closest matches for given search targets
```java
interface NavigableSet<E> extends SortedSet<E>{
	// <E          >E
	E lower(E); E higher(E);
	//<=E          >=E
	E floor(E); E ceiling(E);
	E pollFirst(); E pollLast();
}
```
### classes implement `Set<E>` interface
> `HashSet<E>` elements are **not in order of insertion**, **unsorted**
```java
class HashSet implements Set{
	hashCode();
}
```
>`LinkedHashSet` elements are in **order of insertion**, **unsorted**
```java
class LinkedHashSet implements Set{
	hashCode();
}
```
>`TreeSet<E>` elements are **sortedin natural order**, i.e. A,C,B->A,B,C
```java
class TreeSet implements NavigableSet{
}
```
### Queue interface
```java
interface Queue<E> extends Collection<E> {
	boolean add(E);//exception
	boolean offer(E);//return false
	E remove();//exception
	E poll();//return null
	E peek();
```
#### Deque n BlockingQueue interfaces
>Deque : operating **both ends**
```java
interface DeQue<E> extends Queue<E> {
	boolean addFirst(E);//exception
	boolean addLast(E);//exception
	boolean offerFirst(E);//return false
	boolean offerLast(E);//return false
	E removeFirst();//exception
	E removeLast();//exception
	E pollFirst();//return null
	E pollLast();//return null
	E peekFist();
	E peekLast();
	E getFirst();
	E getLast();
	E removeFirstOccurrence();//exception
	E removeLastOccurrence();//exception
```
> BlockingQueue : makes sure only when the queue is non-empty to retrieve and when the space is available when storing. it meets the scenario where consumer needs to wait for an element of producer being available before get that element.
```java
interface BlockingQueue<E> extends Queue<E>{
	//waiting up to the specified wait time
	boolean offer(E e, long timeout, TimeUnit unit) throws InterruptedException;
	//waits for specified time and returns null if time expires
	E poll(long timeout, TimeUnit unit) throws InterruptedException;
	//Inserts when there is space available.
	void put(E e) throws InterruptedException;
	//waits until element becomes available
	E take() throws InterruptedException;
	int remainingCapacity();
	public boolean contains(Object o);
	int drainTo(Collection<? super E> c);
	int drainTo(Collection<? super E> c, int maxElements);
}
```
#### PriorityQueue implements Queue
>elements are **ordered naturally**
>i.e. smaller number has highter priority
#### ArrayBlockingQueue implements BlockingQueue
> uses Array
#### LinkedBlockingQueue implements BlockingQueue
>uses linked list
>higer throughput but lesser predictable performance in concurrent app
# Map interface
```java
interface Map<K, V> {
	int size(); boolean isEmpty();
	
	boolean containsKey(K); boolean containsValue(V);
	
	V get(K); V put(K, V); remove(K);
	void putAll(Map<? extends K, ? extends V>);

	Set<K> keySet();
	Collection<V> values();
	Set<Entry<K, V> entrySet();

	public static interface Map.Entry<K,V>{
		K getKey(); 
		V getValue(); 
		V setValue(V);
	}
```

### SortedMap extends Map
keys are sorted
```java
public interface SortedMap<K, V> extends Map<K, V> { 
	Comparator<? super K> comparator();
	SortedMap<K, V> subMap(K fromKey, K toKey); 
	SortedMap<K, V> headMap(K toKey); 	
	SortedMap<K, V> tailMap(K fromKey);
	K firstKey();
	K lastKey(); }
```
### HashMap
```java
//found: Ponting 11500
get("ponting");
//not found: null
get("lara");
//"ponting" is already there, so the value 11800 is updated into the map
put("ponting", new Cricketer("Ponting",
11800)
```
### `TreeMap<K,V> implements NavigableMap<K,V>`
keys are in sorted order
```java
put("dravid", new Artist("Dravid", 12000);
```
### `NavigableMap<K,V> extends SortedMap<K,V>`
>`Entry<K,V>` or `K`
```java
Map.Entry<K, V> lowerEntry(K key);//<
K lowerKey(K key);
Map.Entry<K, V> floorEntry(K key);//<=
K floorKey(K key);
Map.Entry<K, V> ceilingEntry(K key);
K ceilingKey(K key);
Map.Entry<K, V> higherEntry(K key);
K higherKey(K key);
Map.Entry<K, V> firstEntry();
Map.Entry<K, V> lastEntry();
Map.Entry<K, V> pollFirstEntry();
Map.Entry<K, V> pollLastEntry();
NavigableMap<K, V> descendingMap();
NavigableSet<K> navigableKeySet();
NavigableSet<K> descendingKeySet(); }
```
### static methods in `Collection<E>`
```java
	static int binarySearch(List, Key); //on sorted list
	static int binarySearch(List, Key, Comparator);
	static void reverse(List);  
	static Comparator reverseOrder(); //
	static void sort(List);
	static void sort(List, Comparator);
```
# Advanced Collection
### Synchronized n Concurrent
- Synchronized  
	- **synchronized** methods/blocks
	- where **one thread** can be executing at a point of time
- Concurrent 
	-  **copy on write** (**array** read >>write)
	`CopyOnWriteArrayList & CopyOnWriteArraySet`
		- any **modification** of the collection _creates a new array_
		- **read** is not synchronized 
		- only **write** is synchronized
	- **compare and swap**
	`ConcurrentLinkedQueue`
		- before modification , the value of a variable is **cached**
		- after modification and the **value is changed** by a thread, then repeforms **operation** and the value is updated.
	- **locks**
		- lock and unlock methods divides 10 methods into blocks
### Capacity of a Java Collection
- initial capacity : number of buckets in the HashMap/HashSet
- load factor: 0.75 
- when the number of entries  exceeds *icapacity*lf* , the hash table is rehashed and the hash table has approximately twice the number of buckets.- 
### UnsupportedOperationException
when an implementation class of Collection interface does not support a certain method from Collection and this method is called, then it throws the 
`UnsupportedOperationException`
```java
List<String> list =Arrays.asList(new String[]{"str1", "str2"});
list.remove();
//throws UnsupportedOperationException
```
### fail-safe vs. fail-fast iterators
- fail-fast iterator : it throws a CocurrentModificationException when there is a modification while iterting the collection.
- fail-safe iterator: it copies the collection and iterates over the copied collection , so even the collection is modified, iterator wont throw an exception.
```java
//fast-safe
ConcurrentHashMap<String, String> map = new ConcurrentHashMap<String, String>();
map.put("key1", "value1");
map.put("key2", "value2");
map.put("key3", "value3");
while (iterator.hasNext()) { 
	System.out.println(map.get(iterator.next())); 
	//while iterating, the map is being put a new element
	map.put("key4", "value4");
}
```
### atomic operations
an operation is all-or-null.
- an example of **i++** is not atomic, cus there are 3 steps, any multi-thread could get to any step, so it is not atomic. 
- **AtomicInteger** is an thread-safe, its operation **incrementAndGet()** makes sure the operation is atomic.
# Generics
>generic classes/methods can work with different types of elements.
### why do Generics make programs more flexible?
```java
class MyListGeneric<T> {
	private List<T> list;
	void add(T e){..}
	void remove(T e){...}
	T get(int index){...}
}

MyListGeneric<String> stringList = new MyListGeneric<String>();
stringList.add("str1");
MyListGeneric<Integer> integerList = new MyListGeneric<Integer>();
integerList.add(1);
```
### Generics restriction
>class MyListGeneric only can be with **any class extends Number** i.e. Float, Integer, Double etc.
```java
class MyListGeneric<T extends Number> {
```
>class MyListGeneric only can be with **any class that is a super class of Number** class
```java
class MyListGeneric<T super Number> {
```
> - using ? , it makes List of Shape works with different types of subclasses of Shape and Shape itself. i.e. Square, Circle, Triangle, Rectangle etc.
> - But with upper bounded list or just wildcards ?, we are not allowed to add any object to the list except null.
> - java compiler allows to add lower bound object types to the list.
```java
List<? extends Shape> //no adding except null
List<?> //not adding except null
new List<? super Number>().add(1);
```
### Generics in Method
`<X extends Number>` must be **before return type** `X`
>X could be parameter type, return type, variable type
```java
static <X extends Number> X doSomething(X number){ 
	X result = number;
	//do something with result
	return result;
}
```
# OOP
### toString()
it is used for us to override it, so it prints the content of the object when the object is called and printed out. By default it returns the hashcode of the object
```java
Animal animal = new Animal("Tommy","Dog");
System.out.println(**animal**);
//Before overriden: com.rithus.Animal@f7e6a96
public String toString() {
	return "Animal [name=" + name + ", type=" + type
	+ "]";
}
//After overriden: Animal [name=Tommy, type=Dog]
```
### == & equals()
By default, `ref1.equals(ref2)` is the same as `ref1==ref2`. They check if the two references are pointing to the same object.
```java
//client1 and client2 are pointing to different client objects. 
System.out.println(client1 == client2);//false
Client client3 = client1;
System.out.println(client1 == client3);//true
```
but after overriding the `equals()`, it checks 2 references are pointing 2 objects which have the same content as per `equals()` implemented.
```java
public boolean equals(Object obj) { 
Client other = (Client) obj;
	if (id != other.id)
	return false; 
return true;
}

System.out.println(client1 == client2);//true
```
>when implementing `equals` method, we must consider: 
```java
x.equals(x) == true; and mutiple invocations return the same result
x.equals(y); y.equals(x);
x.equals(y); y.equals(z); => x.equals(z)
x.equals(null) == false;
```
### hashcode() implementation
>- it decides which bucket of object should be placed into, evenly distributes objects
```java
if obj1.equals(obj2) == true, then obj1.hashCode() == obj2.hashCode()
if obj is not changed, obj.hashCode() should be the same value in muti-invocation
if obj1.equals(obj2) == false, then obj1.hashCode() ==/!= obj2.hashCode()
```
### inheritance 
is-a relationship
### Method Overloading
>in a class or its subclasses, same name, differnt params , for others not specific. i.e. constructors
### Method overriding
>sub class and super class have methods with the same signature but usually different implementation
>i.e. `HashMap and AbstractMap size() method`

### Super class can refer the object of any of its subclass
```java
Actor actor1 = new Comedian();
Actor actor2 = new Hero();
```
### multi inheritance
- classes no; interfaces yes; a class can implements mutliple interfaces
- they can have inheritance chain

### interface
> interfaces are **public abstract**
> variables of interfaces are **public static final**
> methods of interfaces are **public abstract**
```java
default void method5() { System.out.println("Method5");
}
```
### abstract class
- provides common implemented funtionality
- subclasses implement the abstract methods
- could be fully implemented but mainly partially implemented
### Abstract class vs. Interface
|  |Abstract class |Interface|
|--|--|--|
| modifier |any visibility  |public abstract methods/public static final variables
|abstract method|could have non-abstract methods|basically abstract except default method
|subclass method visibility|implements methods with same or wider visibility|all public
|subclass|single inheritance|muti-inheritance for class-interface/interface-interface
### constructor
|default | non-default|
|--|--|
|public ClassName(){ super();}|public ClassName(param){ super();}|
|provided if not have one|if there is a non-default,nor the default, then it is the one who instantiates the instance |
>constructor cannot be called from a normal method

### Polymorphism
- Same code (from super interface and super class) but (subclass) implements differnt behaviors
- Implemented by **methods overridin**g among sub and super classes, so that superclass/interface could refer subclass instances
```java
Human artist = new Artist();
artist.perform();//painting -- the behavior of the artist
artist.score();//compile error. cannot be invoked, cus Human does not have this behavior
```

### instanceof
>it checks if an object is of a perticular type in the inheritance hierachy
```java
subClass instanceof SubClass);//true 
subClass instanceof SuperClass);//true 
subClassObj instanceof SuperClass);//true
subClass2  instanceof SuperClassImplementingInteface);//true
subClass2 instanceof Interface
```
### Cohesion n Coupling
- low coupling : classes should be independent from each other. the changes in one class should not affect the code of other classes.
- high cohesion: each class has single responsibility

### Encapsulation
variable properties are private, use public method to access it, and behavior logic is implemented in methods. Outside just call methods for class functionality and properties. Therefore, when implementation is changed, other code would not be broken.
### Inner Class
can be static, inside of  a class, or inside of a method.

# MISCELLANEOUS
## platform
### Java is platform independent
**.java** 
-source code is compiled-> (by **javac**)
**.class(JAR files)** 
-bytecode is converted to executable instructions (by **JVM** for each OS)
executable instruction -running on -> different OS 

### JDK vs. JRE vs. JVM
**JDK** : javac (compiler), jar, debugging tools (debuggers), javap
contains
**JRE** : java (run bytecode), javaw, libs, rt.jar
contains
**JVM** : Just In Time (JIT) Compiler (run bytecode)

### classloader --JVM uses classloader to find 
system class loader -- classes from CLASSPATH (jar /war /ear)
->
extension class loader -- classes from extension dir (jre /ext /lib)
->
bootsrap class loader -- java core files (pre-packaged with Java)
->
ClassNotFoundException

## wrapper classes
>null, Collection, methods, String -> Integer/Float/Boolean
### ways of creating Wrapper Class Instances
```java
Integer number = new Integer(55);//Integer("55");
Boolean b4 = new Boolean("SomeString"); // b4 is false
//static method valueOf(arg);
Integer seven = Integer.valueOf("111", 2); //binery 111 to Integer -> 7
Integer hundred = Integer.valueOf("100"); //Integer -> 100
```
## String
### String vs. StringBuffer vs. StringBuilder
| String | StringBuffer |StringBuilder
|--|--|--
| tread safe | tread safe |not thread safe
|cannot be modified|modifiable|modifiable
|worst|not as good as StringBuilder|performs better
`new StringBuffer("str1").append("str2");`

### String methods
```java
str.charAt(2);// the 2 index, starting from 0
str.substring(3); //sub string, starting from index 3
str.substring(3,7);//sub string, starting from index 3 end at 6
str.length();
str.toString();
str.equalsIgnoreCase("Abc");
"ac;bd;ef;d".split(";");
```
## Modifiers
|default|public |private|protected
|--|--|--|--
|package|out-of-parckage|class|package & super is available to sub

### final
- class cannot be extended
- methods cannot be overriden
- variable values cannot be changed 
- method argument cannot be modified

### volatile
contance variables values are written/read from main memory and not cached in Java Thread cache.

### conditions & loops
if... else...
switch(var) {case var_value: ...; default: ...;}
for( int i: num){...}

### try with resources
- try block ends and resources are released 
- no finally block
- try block includes instance of a class which implements AutoCloseable interface

### Arrays
- comparing 2 arrays
```java
Arrays.equals(numbers1, numbers2);
```
### Enum
specifies a list of values for a type
```java
enum Season {
WINTER, SPRING, SUMMER, FALL
};
```
### Variable Arguments
- Variable Arguments allow calling a method with different number of parameters
```java
public int sum(int... numbers) { ...
```
### Assertions
> validate an assumpution in assert(), if fails return false, not used on public methods
> public method uses IllegalArgumentException
```java
private int computerSimpleInterest(int principal,float interest,int years){ assert(principal>0);
return 100;
}
```
### GC
memory management manages **heap** for a program, JVM remove objects on the heap when there are not referencs to the objects .
- when cpu is free, or memory on heap is low
- GC is requested when calling `System.gc()`

### Initialization blocks
static blocks : when classes are loaded
`static {...}`
instance initilization: whenever a new object is created,  it runs
`class Ex{ 
	int i{ ...}
}`
### Serialization
- serialization : convert object state to some internal object representation
- de-serialization : convert internal object representation to object 
```java
ObjectOutputStream.writeObject() // serialize and write to file
ObjectOutputStream.readObject() // read from file and deserialize

//write object of Ex into file Rectangle.ser
class Ex implements Serializable {}
FileOutputStream fileStream = new FileOutputStream("Rectangle.ser"); ObjectOutputStream objectStream = new ObjectOutputStream(fileStream); objectStream.writeObject(new Rectangle(5, 6));
objectStream.close();

//deserialize and read object from file Rectangle.ser
FileInputStream fileInputStream = new FileInputStream("Rectangle.ser"); ObjectInputStream objectInputStream = new ObjectInputStream(
fileInputStream);
Rectangle rectangle = (Rectangle) objectInputStream.readObject(); objectInputStream.close(); System.out.println(rectangle.length);// 5 
System.out.println(rectangle.breadth);// 6 
System.out.println(rectangle.area);// 30
```
- transient properties are not serialized `transient int area;`
-  classes as a type of properties inside of a serialized class 
```java
class House implements Serializable {
	transient Wall wall;
	int number; 
}
class Wall implements Serializable { 
	int length;
}
```
- static variable are not part of the object, hence not serialized.

# Thread
## ways to create a thread
```java
class MyThread1 extends Thread{
	public void run() {}
}
new MyThread1().start();

class MyThread2 implements Runnable{
	public void run() {}
}
new Thread(new MyThread2()).start();
```
## ExecutorService
similar to a thread pool.
```java
ExecutorService executorService = Executors.newSingleThreadExecutor();
executorService.execute(new Runnable() { public void run() {
	System.out.println("From ExecutorService");
} });
System.out.println("End of Main"); executorService.shutdown();
//return
Future futureFromCallable = executorService1.submit(new Callable() { 	   		public String call() throws Exception {
	return "RESULT";
} });
System.out.println("futureFromCallable.get() = " + futureFromCallable.get());

```
