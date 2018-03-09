# Collection
### important interfaces in the Collection Hierarchy
`interface Collection<E> extends Iterable<E>{}`
>no dup elements
`interface Set<E> extends Collection<E>{}`
>position of elements, if no position specified, maintains the insert order, add/remove at the end
`interface List<E> extends Collection<E>{}`
>FIFO
`interface Queue<E> extends Collection<E>{}`
>`{("key1", value1),("key2", value2),...}` keys must be unique
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
### methods in List<E>
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
# Advanced Collection
# Generics
# OOP



> Written with [StackEdit](https://stackedit.io/).
