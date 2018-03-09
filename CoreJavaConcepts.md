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
boolean add(E); boolean addAll(Collection<? extends E>);

boolean remove(E); boolean removeAll(Collection<? extends E>);

boolean contains(Object); boolean containsAll(Collection<?>);

boolean retainAll(Collection<? extends E>);

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
> BlockingQueue : makes sure only when the queue is non-empty to retrieve and when the space is available when storing
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
# Generics
# OOP

```java
```

> Written with [StackEdit](https://stackedit.io/).
