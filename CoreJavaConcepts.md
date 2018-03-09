# Collection
### important interfaces in the Collection Hierarchy
`interface Collection<E> extends Iterable<E>{}`
>- **no dup** elements `if obj1.equals(obj2) then `only one of them can be in the Set; **no order** of elements
`interface Set<E> extends Collection<E>{}`
>- focusing on operations on certain **positions** of elements, if no position specified, maintains the insert order, add/remove at the end
`interface List<E> extends Collection<E>{}`
>- elements order is **FIFO**
`interface Queue<E> extends Collection<E>{}`
>- `{("key1", value1),("key2", value2),...}`  **K-V pairs** and **unique keys**
`interface Map<K,V>{}`
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
3. Queue methods : **`peek(), poll()`**

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
	E lower(E); E higher(E);
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
# Advanced Collection
# Generics
# OOP

```java
```

> Written with [StackEdit](https://stackedit.io/).
