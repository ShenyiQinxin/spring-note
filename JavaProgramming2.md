## string - char[]
```java
//string -> arry
char[] charArr = str.toCharArray();
//array -> string
Arrays.toString(arr);
```
## Array - ArrayList
```java
//array -> arraylist
new ArrayList<String>(Array.asList(strArray));
Collections.addAll(arrayList, strArray);
arrayList.addAll(Arrays.asList(strArray));
//ArrayList -> Array
//suppose there is a ArrayList with elements, and empty string array
arrayList.toArray(stringArray);
```
## HashMap To ArrayList
```java
//key arraylist
new ArrayList<String>(hashmap.keySet());
//value arraylist
new ArrayList<String>(hasmap.values());
//entry arraylist
new ArrayList<Entry<String,String>>(hashmap.entrySet());
```
## Number- String
```java
//integer -> string
Integer.toString(number);
String.valueOf(number)
//string -> integer 
new Integer("55");
Integer.valueOf("55");
Integer.parseInt("55");//if input is not a int, then NumberFormatException is thrown
```
## StringBuffer/StringBuilder
```java
StringBuffer sb = new StringBuffer(str);
//reverse a string
sb.reverse();
sb.append();
sb.deletCharAt(index);
```
## String 
```java
str.charAt(index);
str.split(" ");
str.replaceAll("\\s","");
str.toLowerCase();
str.indexOf(character);
sb.indexOf(string);
str.substring(0,index);
```
## Character
```java
Character.isUpperCase(char);
Character.isLowerCase(char);
Character.isDigit(char);
```
## Array
```java
arr[i] & arr[arr.length-1-i]
Arrays.sort(str);
Arrays.equals(string, string);
Arrays.deepEquals();
Arrays.deepToString();
```
## HashMap
```java
HashMap<Character, Integer> map= new HashMap<>();
map.containsKey(c);
map.get(c);
map.put(c, v);
map.keySet();
map.entrySet();
Entry<K,V> entry;
entry.getKey();
entry.getValue();
```
## file 
```java
BufferedReader reader = new BufferedReader(new FileReader("c:\\sample.txt"));
reader.readLine();
BufferedWriter writer = new BufferedWriter(new FileWriter("c:\\sample.txt", true));
```
## Sorting
```java
Collections.sort(arrayList)
Collections.sort(arrayList, String.CASE_INSENSITIVE_ORDER);
Collections.sort(list, Collections.reverseOrder());
Collections.sort(listOfStudents, new OrderByPercentage());
```
## Synchronize collections
```java
Collections.synchronizedList(list);
Collections.synchronizedSet(set);
Collections.synchronizedMap(map);
```
##
```java
```
