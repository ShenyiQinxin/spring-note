## 1 Reverse a string
- StringBuffer
```java
	 StringBuffer sb = new StringBuffer("MyJava");
	 return sb.reverse();
```

- iterative 	**str.toCharArray();**	
```java
	char[] charArray = str.toCharArray();
	for(int i=charArray.length-1; i>=0; i--){
		print(strArray[i]);
	} 
``` 
- recursive
```java
String recursiveMethod(String str){
	if((str==null) || (str.length()<=1)){
		return str;
	} 
	return recursiveMethod(str.subString(1))+str.charAt(0);
}
```
## 2reverse an array without an additional array
```java
for(int i=0; i<arr.length/2; i++){
//swap 2 ends elements
	temp = arr[i];
	arr[i] = arr[arr.length-1-i];
	arr[arr.length-1-i]=temp;
}
Arrays.toString(arr);
```
## 3reverse each word of a string
>Java Concept Of The Day
avaJ tpecnoC fO ehT yaD
```java
String[] words = str.split(" ");
String reversedStr = "";
for(int i=0; i<words.length; i++){
	String word = words[i];
	String reversedWord = "";
	for(int j=word.length()-1; j>=0; j--){
		reversedWord = reversedWord + word.charAt(j);
	}
	reversedStr = reverseStr+ reversedWord +" ";
}
```
## 4 remove white spaces
 - replaceAll(originStr, fillerStr);
 ```java
	 return str.replaceAll("\\s","");
```
- iterative
```java
Char[] charArray = str.toCharArray();
StringBuffer sb = new StringBuffer();
for(int i=0; i<charArray.length; i++){
	if((charArray[i] != ' ') && (charArray[i] != '\t')){
		sb.append(charArray'[i];
	}
	return sb;
}
```
## 5 Reverse The String With Preserving The Position Of Spaces
>I Am Not String —> g ni rtS toNmAI
```java
char[] arr = str.toCharArray();
char[] newArr = new char[length];
int length = arr.length;

for(int i=0; i<length; i++){
	if(arr[i] == ' '){
		newArr[i]= ' ';
	}
}
int j= length-1;
for(int i=0; i<length; i++){
	if(arr[i] != ' '){
		if(newArr[j] == ' '){
			j--;
		}
		newArr[j] = arr[i];
		j--;
	}
}
return String.valueOf(newArray);
```
## 6 dup in ArrayList
>Elements are shuffled after duplicate elements are removed. They are not in the insertion order.
```java
ArrayList<String> dupAL;
HashSet<String> set = new HashSet<String>(dupAL);
ArrayList<String> noDupAL = new ArrayList<String>(set);
print(noDupAL);
```
>remove duplicate elements from ArrayList and also maintain the insertion order of elements
```java
ArrayList<String> dupAL;
LinkedHashSet<String> set = new LinkedHashSet<String>(dupAL);
ArrayList<String> noDupAL= new ArrayList<String>(set);
println(noDupAL);
```


## 7 occurrences of each char/element in array
 ```java
	HashMap<Character, Integer> charCountMap = new HashMap<>();
	char[] strArr = str.toCharArray();
	for(char c: strArr){
		if(charCountMap.containsKey(c){
			charCountMap.put(c, charCountMap.get(c)+1);
		} else {
			charCountMap.put(c, 1);
		}
	}
	print(charCountMap());
	 
```
## 8 dup elements in a String / char array
 ```java
	 HashMap<Character, Integer> countMap = new HashMap<>();
	 char[] charArr = str.toCharArray();
	 for(char c: charArr){
		 if(countMap.containsKey(c){
			 countMap.put(c, countMap.get(c)+1);
		 } else {
			 countMap.put(c,1);
		 }
	 }
	 Set<Character> charSet = countMap.keySet();
	 for(Character c: charSet){
		 if(countMap.get(c)>1){
			 print(c+":"+countMap.get(c));
		 }
	 }
```
##  9 Find First Repeated And Non-Repeated Character In A String
```java
char[] charArr = str.toCharArray();
for(char c: charArr){
		 if(countMap.containsKey(c){
			 countMap.put(c, countMap.get(c)+1);
		 } else {
			 countMap.put(c,1);
		 }
}
//checking for first non-repeated character
for(char c: charArr){
	if(charCountMap.get(c) == 1){
		print(c);
		break;
	}
}

//checking for first repeated character
for(char c: charArr){
	if(charCountMap.get(c) > 1){
		print(c);
		break;
	}
}
```
## 10 most repeated word in text file
```java

BufferedReader reader = new BufferedReader(new FileReader("C:\\sample.txt"));
String currentLine = reader.readLine();
HashMap<String, Integer> wordMap= new HashMap<>();

while (currentLine != null){
	for(String w: words){
		if(wordMap.containsKey(w)){
			wordMap.put(w, wordMap.get(w)+1;
		} else {
			wordMap.put(word,1);
		}
	}
	currentLine = read.readLine();
}
//find the most repeated word in wordMap
int count=0;
String target = null;
Set<Entry<String,Integer>> entrySet = wordMap.entrySet();
for(Entry<String, Integer> entry: entrySet){
	if(entry.getValue()>count){
		target = entry.getKey();
		count = entry.getValue();
	}
}
print("The most repeated word is "+target +": "+count)
```
## 11 equality of 2 arrays
- iterative
```java
	boolean equals= true;
	if(arr1.length != arr2.length){
		 equals = false;
	} else {
		for(int i=0; i<arr1.length; i++){
			if(arr1[i] != arr2[i]){
				equals = false;
			}
		}
	}
	
	return equals;
```
- **`Arrays.equals()`**
```java
// exactly equals
	return Arrays.equals(s1, s2); 
// same set of characters but different order
	Arrays.sort(s1);
	Arrays.sort(s2);
	Arrays.equals(s1, s2);
```
- **`Arrays.deepEquals()`**
>multi dimentional arrays for equality deep comparison
```java
 String[][] s1={{"java", "swings", "j2ee"},{"struts", "jsp", "hibernate"}};
 String[][] s2={{"java", "swings", "j2ee"},{"struts", "jsp", "hibernate"}};
 Arrays.deepEquals(s1, s2);
```
## 12 Anagram
 > In a string, the same set of characters but in different order and upper/lower cases
 > “Mother In Law” and “Hitler Woman” 
- **`sort(), equals()`** 
```java
	String s1 = s1.replaceAll("\\s", "").toLowerCase();
	String s2 = s2.replaceAll("\\s", "").toLowerCase();
	boolean status = true;	 
	if(s1.length() != s2.length()){
		status = false;
		return status;
	} else {
		char[] s1Arr = s1.toCharArray();
		char[] s2Arr = s2.toCharArray();
		Arrays.sort(s1Arr);
		Arrays.sort(s2Arr);
		return Arrays.equals(s1Arr, s2Arr);
	}
```
- iterative
```java
	String s1 = s1.replaceAll("\\s", "").toLowerCase();
	String s2 = s2.replaceAll("\\s", "").toLowerCase();
	boolean status = true;
	if(s1.length() != s2.length()){
		status = false;
	} else {
		char[] s1Arr = s1.toCharArray();
		for(char c: s1Arr){
			int index = s2.indexOf(c);
			//int index=new StringBuilder(s2).indexOf(""+c);
			if(index != -1){
				//if the char is in s2, then take the char out
				s2 = s2.subString(0,index)
				+s2.subString(index+1,s2.length());
			//new StringBuilder(s2).deleteCharAt(index);
			} else {
				//once found a char is not in s2
				status = false;
				return false;
			}
		}
		return true;
	}
```
-StringBuilder
```java
String s1 = s1.replaceAll("\\s","").toLowerCase();
String s2 = s2.replaceAll("\\s","").toLowerCase();	 

if(s1.length() != s2.length()){
	return false;
} else {
	char[] s1Arr = s1.toCharArray();
	StringBuilder sb2 = new StringBuilder(s2);
	for(char c: s1Arr){
		int index = sb2.indexOf(""+c);
		if(index != -1){
			sb2 = sb2.deleteCharAt(index);
		} else {
			status = false;
			break;
		}
	}
}
if(status){
    System.out.println(s1+" and "+s2+" are anagrams");
}else{
    System.out.println(s1+" and "+s2+" are not anagrams");
}
```
- HashMap
```java
String s1 = s1.replaceAll("\\s","").toLowerCase();
String s2 = s2.replaceAll("\\s","").toLowerCase();	 

if(s1.length() != s2.length()){
	return false;
} else {
	HashMap<Character, Integer> map = new HashMap<>();
	for(int i=0; i<s1.length(); i++){
		char c = s1.charAt(i);
		int count = 0;
		if(map.containsKey(c)){
			count = map.get(c);
		}
		map.put(c, ++count);
		//
		c = s2.charAt(i);
		count = 0;
		if(map.containsKey(c)){
			count = map.get(c);
		}
		map.put(c, --count);
	}//end iteratively count++ / --
	for(int value: map.values()){
		if(value !=0){
			return false;
		}
		return true;
	}//end checking each value
}//end else
```
 
## 13 armstrong number
 >153 = 1^3 + 53 + 33 = 1 + 125 + 27 = 153

>371 = 33 + 73 + 13 = 27 + 343 + 1 = 371

>407 = 43 + 03 + 73 = 64 + 0 + 343 = 407

>9474 = 9^4 +  44 + 74 + 44 = 6561 + 256 + 2401 + 256 = 9474

>54748 = 5^5 + 45 + 75 + 45 + 85 = 3125 + 1024 + 16807 + 1024 + 32768 = 54748
```java
String numStr = Integer.toString(numInt);
int noDigit = numStr.length();
int sum = 0;
while(num != 0){
	int last = num %10;
	int lastToThePower = 1;
	for(int i=0; i< noDigit; i++){
			lastToThePower*=last;
	}
	sum = sum + lastToThePower;
	num = num/10;
}
if( sum == num){
	return true;
}	 
	return false;
```
 
## 14 missing number
> if n=8, array is {1,4,5,3,7,8,6}, then find the missing number 6
> Missing_Number = (Sum of 1 to ‘n’ numbers) – (Sum of elements of the array)
```java
int sumOfNnumbers(int n){
	int sum = (n*(n+1))/2;
	return sum;
}
int sumOfElements(int[] array){
	int sum=0;
	for(int i=0; i<array.length; i++){
		sum +=array[i];
	}
	return sum;
}
int missingNumber = sumOfNumbers(n)-sumOfElements(n);
```

## 15 sum of all digits
 - iterative
```java
int sum = 0;
while(num != 0){
	 //get the last digit
	int digit = num%10;
	sum += digit;
	//remove the last digit
	num = num /10;
}
	//0 or other numbers
	return sum;
```
- recursive
```java
 int sumDigits(num){
	 int sum = 0;
	 if(num == 0){
		 retun sum;
	 } else {
		int digit  = num %10;
		sum += digit;
		num /= 10;
		return sumDigits(num);	 
	}
}
```
## 16 second largest number
```java
	 int largest, secondLargest;
	 if(input[0] > input[1]){
		 largest = input[0];
		 secondLargest = input[1];
	 } else {
		 largest = input[1];
		 secondLargest = input[0];
	 }
	 for(int i=2; i<input.length; i++){
		 if(input[i] > largest){
			 secondLargest = largest;
			 largest = input[i];
		 } else (input[i]<largest && input[i]>secondLargest){
			 secondLargest = input[i];
		 }
	 }
	 return secondLargest;
```
 

## 17 largest number < a number without a given digit
```java
 //convert the digit to a char
	char c = Integer.toString(digit).charAt(0);
	for(int i=number; i>0; --i){
	//decrease the number to check each one containing character c or not
		if(Integer.toString(i).indexOf(c) == -1){
			return i;
		}
	}
	return -1;
```
## 18  all pairs of elements whose sum must be equal to a given number
 >find all pairs of elements in this array such that whose sum must be equal to a given number.
```java
	 Array.sort(arr);
	 int i=0;
	 int j= arr.length-1;
	 while(i<j){
		 if(arr[i]+arr[j]==num){
			print(num);
			i++;
			j--;
		} else if(arr[i]+arr[j]<num){
			i++;
		} else if(arr[i]+arr[j]>num){
			j--;
		}
	}
	 
```
## 19 Sub array whose sum is equal to a given number
```java
//arr is the original array
	int sum = arr[0];
	int start = 0;
	for(int =1; i< arr.length; i++){
		sum += arr[i];
		while(sum > num && start <=i-1){
		//remove starting elements from the sum
			sum -= arr[start];
			start ++;
		}
		if(sum == num){
			print("The original array: "+Arrays.toString(arr));
			for(int j=start; j<=i; j++){
				print(arr[j]+" ");
			}
		}
	}
``` 
## 20 check whether one string is a rotation of another
 >“StrutsHibernateJavaJ2ee”, “J2eeStrutsHibernateJava”, “HibernateJavaJ2eeStruts”.
 >“JavaJ2eeStrutsHibernateJavaJ2eeStrutsHibernate”
 ```java
	if(s1.length() != s2.length()){
		return false;
	} 
	String s3 = s1+s1;
	if(s3.contains(s2)){
		return true;
	} 
	return false;
```
## 21 check whether given number is binary or not?
```java
	
	 while(num != 0){
		 int last = num %10;
		 if(last >1){	 
			 return false;
		 } else {
			 num /= 10;
		 }
	 }
	 return true;
```
## 22 find common elements of two arrays
- retainAll()
```java
Integer[] arr1;
Integer[] arr2;
HashSet<Integer> set1 = new HashSet<>(Arrays.asList(arr1));
HashSet<Integer> set2 = new HashSet<>(Arrays.asList(arr2));
set1.retainAll(set2);
```

## 23 check whether user input is number or not 
```java
class Utility{
	static boolean numberOrNot(String input){
		try{
			Integer.parseInt(input);
		}catch(NumberFormatException ex){
			return false;
		}
		return true;
	}
}
....
if(Utility.numberOrNot(input) && (input.length()==10){
	return true;
} else {
	return false;
}
```
## 24 Move Zeros To End Of An Array
>{12, 0, 7, 0, 8, 0, 3}
>[12, 7, 8, 3, 0, 0, 0]
```java
int counter = 0;
for(int i=0; i<arr.length; i++){
	if(arr[i] !=0 ){
	//moves to the 1st position
		arr[counter]=arr[i];
	}
	//then the 2nd position
	counter++; //the final value is the right one next to non-zero element 89230-->4
}
//remaining elements are set to 0
while(counter < arr.length){
	arr[counter]=0;
	counter++;
}
```
## 25 Move Zeros To Front Of An Array
```java
int counter = arr.length-1;
for(int i=arr.length-1; i>=0; i--){
	if(arr[i] !=0){
		arr[counter] = arr[i];	
	}
	counter--;
}
while (counter>=0){
	arr[counter]=0;
	counter--;
}
```
## 26 Decimal To Binary, Decimal To Octal And Decimal To HexaDecimal 
- decimal to binary
```java
int remainder=0;
String binary="";
while(num>0){
	remainder = num%2;
	binary = remainder+binary;
	num = num/2;
}
```
- Decimal To Octal
```java
int remainder=0;
String octal="";
while(num>0){
	remainder = num%8;
	octal = remainder + octal;
	num = num/8;
}
```
- Decimal To HexaDecimal 
>
```java
char hexaDecimals={'0','1',..,'9','A','B','C','D','E','F'}
int remainder = 0;
String hexa = "";
while(num>0){
	remainder = num%16;
	hexa = hexaDecimals[remainder]+hexa;
	num = num/16;
}
```
## 27 Find All The Leaders In An Integer Array
>{14, 9, 11, 7, 8, 5, 3} is the given array then {14, 11, 8, 5, 3} are the leaders in this array.
```java
int length = arr.length;
int max = arr[length-1];
for(int i=length-2; i>=0; i--){
	if(arr[i]>max){
		max = arr[i]
		print(arr[i]);
	}
}
```
## 28 Reverse And Add A Number Until You Get A Palindrome
>7325
>7325 (Input Number) + 5237 (Reverse Of Input Number) = 12562
>12562 + 26521 = 39083
>...
>144353 + 353441 = 497794 (Palindrome)
```java
//reverse a number
int reverseNumber(int num){
	int reverse = 0;
	int remainder = 0;
	while(num !=0){
		remainder = num % 10;
		reverse = reverse*10 + remainder;
		num = num/10;
	}
	return reverse;
}
//check palindrome
boolean checkPalindrome(int num){
	int reverse = reverseNumber(num);
	if(reverse == num){
		return true;
	} else {
		return false;
	}
}
//reverse and add
boolean reverseAndAdd(int num){
	boolean isPalindrome = checkPalindrome(num);
	if(isPalindrome){
		return true;
	} else {
		while(!isPalindrome){
			int reverse = reverseNumber(num);
			int num= num + reverse;
		}
	}
	return false;
}
```


## 29 HashSet examples
> unique elements
> no order
> constant time for insertion, removal, retrieval
> ordered HashSet : LinkedHashSet, TreeSet
```java
class Student{

    String name;
	int rollNo;
	String department;
 
    public Student(String name, int rollNo, String department){
        this.name = name;
        this.rollNo = rollNo;
        this.department = department;
    }
 
    @Override
    public int hashCode(){
        return rollNo;
    }
 
    @Override
    public boolean equals(Object obj){
        Student student = (Student) obj;
        return (rollNo == student.rollNo);
    }
 
    @Override
    public String toString(){
        return rollNo+", "+name+", "+department;
    }
}
set.add(new Student("Avinash", 121, "ECE"));
et.add(new Student("Amit", 301, "IT"));//duplicate element
Iterator<Student> it = set.iterator();
while (it.hasNext()){
    Student student = (Student) it.next();
	System.out.println(student);
}
//output:
//121, Avinash, ECE
// duplicate elements are not added to HashSet.
```
> hashCode() and equals() methods are overrided 
> Students objects will be compared solely based on rollNo, same rollNo will be considered as duplicates irrespective of other fields.
## 30 LinkedHashSet
### implementation mechanism
>- all methods are inherited from **HashSet**
>- elements are stored as the keys in a **LinkedHashMap** to maintain an insertion order. values are the same constant i.e "PRESENT". 
>- key value pairs are in **Entry**<K,V> extends HashMap.Entry<K,V>
>- In LinkedHashMap, the same set of Entry objects (rather references to Entry objects) are arranged in two different manner. One is the **HashMap** and another one is **Doubly linked list**. 
```java
//LinkedHashMap.Entry objects are arranged by HashMap and DoublyLinkedList
private static class Entry<K,V> extends HashMap.Entry<K,V>{
 // These fields comprise the doubly linked list used for iteration.
        Entry<K,V> before, after;
		Entry(int hash, K key, V value, HashMap.Entry<K,V> next) {
            super(hash, key, value, next);
        }
}
private transient Entry<K,V> header;//Stores the head of the doubly linked list

HashSet(int initialCapacity, float loadFactor, boolean dummy){
        map = new LinkedHashMap<>(initialCapacity, loadFactor);
}
public LinkedHashSet(int initialCapacity, float loadFactor){
      super(initialCapacity, loadFactor, true);              
}
//other constructors
public LinkedHashSet(int initialCapacity)
public LinkedHashSet()
public LinkedHashSet(Collection<? extends E> c)
```
### LinkedHashSet Examples
>no dup, insertion order
> override equals() and hashCode(), so comparison is depended on id only
```java
LinkedHashSet<Customer> set = new LinkedHashSet<Customer>();
set.add(new Customer("Julian", 814));
set.add(new Customer("Avinash", 105));      //Duplicate Element
set.add(new Customer("Sapna", 879));
Iterator<Customer> it = set.iterator();
while (it.hasNext()){
    Customer customer = (Customer) it.next();
    System.out.println(customer);
}
//output:
814 : Julian
879 : Sapna
//duplicate elements are not added to LinkedHashSet.
```
##  31 TreeSet Example
> no dup, ordered by a comparator
> no comparator, natural order
> if compare() is based on id, then the same id elements are seen as dups, the later added dup is removed
> - HashSet, LinkedHashSet use hashCode(), equals() to compare 2 elements. TreeSet uses compare() or compareTo().
```java
TreeSet<Integer> set = new TreeSet<Integer>();
set.add(23);
//...
class Student{}
class MyComparator implements Comparator<Student>
{
    @Override
    public int compare(Student s1, Student s2)
    {
        if(s1.id == s2.id)
        {
            return 0;
        }
        else
        {
            return s2.perc_Of_Marks_Obtained - s1.perc_Of_Marks_Obtained;
        }
    }
}
MyComparator comparator = new MyComparator();
 
//Creating TreeSet with 'MyComparator' as Comparator.
 
TreeSet<Student> set = new TreeSet<Student>(comparator);
set.add(new Student(121, "Santosh", 85));
 
set.add(new Student(231, "Cherry", 71));
 
set.add(new Student(417, "David", 82));
 
set.add(new Student(562, "Praveen", 91));
 
set.add(new Student(231, "Raj", 61));//231 dup removed
Iterator<Student> it = set.iterator();
 
while (it.hasNext())
{
    System.out.println(it.next());
}
//output ranked by perc_Of_Marks_Obtained
562 : Praveen : 91
121 : Santosh : 85
417 : David : 82
231 : Cherry : 71 
```
## 32 PriorityQueue Example
>order elements on a priority basis i.e. employees salaries in ascending order or customers are ordered on their id
>- with default comparator - natural ascending order
```java
PriorityQueue<Integer> pQueue = new PriorityQueue<Integer>();
pQueue.offer(21);
pQueue.offer(17);
pQueue.offer(37);
//remove head
pQueue.poll();
//...
//output
17
21
37
```
- with implemented Comparator
```java
class MyComparator implements Comparator<Employee>{
    @Override
    public int compare(Employee e1, Employee e2){
        return e1.salary - e2.salary;
    }
}
PriorityQueue<Employee> pQueue = new PriorityQueue<Employee>(7, comparator);
pQueue.offer(new Employee("BBB", 12000));
pQueue.offer(new Employee("CCC", 7500));
pQueue.offer(new Employee("GGG", 14300));
pQueue.poll();
//...
//output
CCC : 750
BBB : 12000
GGG : 14300
```
## 33 LinkedList 
>same with ArrayList 
```java
list.contains(111.11);//check a given element is in the arraylist or not
//get the postion of a given element
//[JAVA, J2EE, JSP, JAVA, SERVLETS, JAVA, STRUTS]
list.indexOf("Java");// 0
//append any Collection type at the end of LinkedList
linkedList.addAll(arrayList);
list.add("ONE");
list.add(2, "ONE");
list1.addAll(2, list2);
```
>different from ArrayList
```java
//iterates elements in reverse direction
Iterator<String> it = list.descendingIterator();
//printing the elements of list in reverse order
while (it.hasNext()){
            System.out.println(it.next());
}
//linkedList works as a Queue
linkedList.offer(20);//add to the end
linkedList.offer(30);//head [20,30] end
linkedList.poll();//remove from the head -->20
linkedList.poll();//30
//insert at the tail
linkedList.add(22);addLast(22);offer(22);offerLast(22);
//insert at the head
linkedList.addFirst(22);addFirst(22);
```
## 34 ArrayList
>ArrayList() —> It creates an empty ArrayList with initial capacity of 10.
>ArrayList(int initialCapacity) —> It creates an empty ArrayList with supplied initial capacity.
>ArrayList(Collection c) —> It creates an ArrayList containing the elements of the supplied collection.
```java
ArrayList<Integer> list3 = new ArrayList<Integer>(list1);
ArrayList<String> list2 = new ArrayList<String>(20);
ArrayList<Integer> list1 = new ArrayList<Integer>();
list.ensureCapacity(20); //to increase current capacity, when add elements, the capacity automatically increased
list.trimToSize();//minimize the capacity to current size
list.size();//number of elements in ArrayList
list.isEmpty(); //or size(); when return 0
list.contains(111.11);//check a given element is in the arraylist or not
//get the postion of a given element
//[JAVA, J2EE, JSP, JAVA, SERVLETS, JAVA, STRUTS]
list.indexOf("Java");// 0
list.lastIndexOf("Java")//5
//convert an ArrayList to an Array
list.toArray();
//retrieve an element from a position [111, 222, 333, 444]
list.get(3);//444
//replace the element at index 1 with 000
list.set(1, 000);
//append an element at the end of ArrayList
list.add("ONE");
list.add("TWO");
list.add("THREE");
list.add("FOUR");//[ONE, TWO, THREE, FOUR]
//insert to a position
list.add(1, "AAA");//[ONE, AAA, TWO, THREE, FOUR]
//remove an element from a position
list.remove(2);//[ONE, AAA, THREE, FOUR]
list.remove("AAA");//[ONE, THREE, FOUR]
list.remove("NA");//[ONE, THREE, FOUR]
list.clear();//remove all elements
list.subList(1,4);//retrieve a portion of the ArrayList (index 1,2,3) [1,2,3]
//change of subList and ArrayList will be reflected to each other
list.set(2, 000);//replace index 2 to 000, subList becomes [1,0,3]
//append one arraylist to the end of the other arraylist
list1.addAll(list2);
//add one arraylist to a position of the other arraylist
list1.addAll(2, list2);//[0,1,2,3][4,5,6,7]->[0,1,4,5,6,7,2,3]
```
## 35 Array and ArrayList conversion
- Array to ArrayList
```java
String[] array = new String[]{"ANDROID", "JSP", "JAVA", "STRUTS"};
//1
new ArrayList<String>(Arrays.asList(array));
//2
ArrayList<String> list = new ArrayList<String>();
Collections.addAll(list, array);
//3
list.addAll(Arrays.asList(array));
```
- ArrayList to Array
```java
ArrayList<String> list = new ArrayList<String>();
String[] array = new String[list.size()];
list.toArray(array);//now the array has all the elements from the list.
for(String str: array){
	print(str);
}
```
## 36 Arrays.deepToString()
```java
//Printing dimensional array contents
print(Arrays.deepToString(oneDArray));
print(Arrays.deepToString(twoDArray));
print(Arrays.deepToString(threeDArray));
```
## 37 Launch External Applications Through Java Program
```java
//every java application has only one RunTime instance in which application is running.
Runtime runtime = Runtime.getRuntime();
try{
//executes the specified system command in a separate process
	runtime.exec("notepad.exe");
} catch (IOException e){
	e.printStackTrace();
}
```
## 38 Find The Percentage Of Uppercase Letters, Lowercase Letters, Digits And Other Special Characters In A String 
>Character.isUpperCase(ch) –> This method checks whether ‘ch’ is in uppercase or not.
>Character.isLowerCase(ch) –> This method checks whether ‘ch’ is in lowercase or not.
>Character.isDigit(ch) –> This method checks whether ‘ch’ is a digit or not.
```java
for(int i=0; i<str.length(); i++){
	char c = str.charAt(i);
	if(Character.isUpperCase(c)){
		upperCaseLetters++;
	} else if(Character.isLowerCase(c)){
		lowerCaseLetters++;
	} else if(Character.idDigit(c)){
		digits++;
	} else{
		others++;
	}
}
double upperCaseLetterPercentage = (upperCaseLetters * 100.0) / totalChars ;
double lowerCaseLetterPercentage = (lowerCaseLetters * 100.0) / totalChars;
double digitsPercentage = (digits * 100.0) / totalChars;
double otherCharPercentage = (others * 100.0) / totalChars;
DecimalFormat formatter = new DecimalFormat("##.##");
formatter.format(upperCaseLetterPercentage);//19.90
```
## 39 Sorting
- selection sort
>  Select the lowest (or highest) element
>  Swap the lowest with the current 
>  the lowest, the 2nd lowest, the 3rd lowest and so on.
```java
for(int i=0; i<arr.length-1; i++){
	pos = i;//i indexes the current 
	for(int j=i+1; j<arr.length; j++){
		if(arr[j]<arr[pos]{
			pos =j;//pos indexes the min 
		}
	}
	//swap current and min
	temp = arr[i];
	arr[i] = arr[pos];
	arr[pos] = temp;
}
```
- sort an ArrayList
>Collections.sort(List<T> list)   natural order
>sort(List<T> list, Comparator<? super T> c) order defined in Comparator
```java
//number string types
ArrayList<String> list = new ArrayList<String>();
Collections.sort(list);
Collections.sort(list, String.CASE_INSENSITIVE_ORDER);
Collections.sort(list, Collections.reverseOrder());
//complex types
class Student implements Comparable<Student>{
	@Override
    public int compareTo(Student s){return this.id - s.id; }
ArrayList<Student> listOfStudents = new ArrayList<Student>();
Collections.sort(listOfStudents);
//
class OrderByPercentage implements Comparator<Student>{
    @Override
    public int compare(Student s1, Student s2){
	    return s1.percentage - s2.percentage;
	}
}
Collections.sort(listOfStudents, new OrderByPercentage());
```
## 40 Append Text To An Existing File
```java
FileWriter fileWriter = null;
BufferedWriter bufferedWriter = null;
PrintWriter printWriter = null;
try{
	fileWriter = new FileWriter("C:\\sample.txt", true);
	bufferedWriter = new BufferedWriter(fileWriter);
	printWriter = PrintWriter(bufferedWriter);
	printWriter.println();
	printWriter.println("Venkatesh : 789546");
	printWriter.println("Venkatesh : 789546");
	printWriter.println("Venkatesh : 789546");
}
catch(IOException e){
	e.printStackTrace();
}
finally{
	try
            {
                printWriter.close();
                bufferedWriter.close();
                fileWriter.close();
            }
            catch (IOException e)
            {
                e.printStackTrace();
            }
}
```
##  41 Sort A Text File
>one line of Strings
>
|  input| output |
|--|--|
| 56 Suresh Mahesh Abhi 81 Vikas Bhavani Nalini 62 |56 62 81 Abhi Bhavani Mahesh Nalini Suresh Vikas
```java
BufferedReader reader = null; 
BufferedWriter writer = null;
ArrayList<String> lines = new ArrayList<String>();
try{
	reader = new BufferedReader(new FileReader("c:\\input.txt"));
	String current = reader.readLine();
	while(current != null){
		lines.add(current);
		current = reader.readLine();
	}
	Collections.sort(lines);
	writer.new BufferedWriter(new FileWriter("c:\\output.txt"));
	for(String line: lines){
		writer.write(line);
		writer.newLine();
	}
}
catch(IOException e){ e.printStackTrace();}
finally {
	try{
	    if (reader != null){
                    reader.close();
        }
        if(writer != null){
                    writer.close();
        }
     }catch (IOException e) {
                e.printStackTrace();
     }
}
```
>two lines of strings
> Vikas 92
Mahesh 89
Shloka 84
Shyam 81
Abhi 71
Bhavani 68
Nalini 62
Suresh 56
```java
/Student Class
 
class Student{
    String name;
	int marks;
    public Student(String name, int marks) {
        this.name = name; 
        this.marks = marks;
    }
}
 
//nameCompare Class to compare the names
class nameCompare implements Comparator<Student>{
    @Override
    public int compare(Student s1, Student s2){
        return s1.name.compareTo(s2.name);
    }
}
 
//marksCompare Class to compare the marks
 class marksCompare implements Comparator<Student>{
    @Override
    public int compare(Student s1, Student s2){
        return s2.marks - s1.marks;
    }
}
//
BufferedReader reader = new BufferedReader(new FileReader("C:\\input.txt")); 
BufferedWriter writer = = new BufferedWriter(new FileWriter("C:\\output.txt"));
ArrayList<Student> lines = new ArrayList<>();
String current = reader.readLine();
while(current != null){
	String[] student = current.split(" ");
	String name = student[0];
	int mark = Integer.valueOf(student[1]);
	lines.add(new Student(name, mark));
	current = reader.readLine();
}
Collections.sort(lines, new marksCompare());
for (Student student : lines){
            writer.write(student.name);     
            writer.write(" "+student.marks);
            writer.newLine();
}
reader.close();
writer.close();
```
## 42 Find Longest Substring Without Repeating Characters 
```java
char[] arr = str.toCharArray();
String longest = null;
int longestLength = 0;
LinkedHashMap<Character,Integer> map = new LinkedHashMap<>();
for(int i=0; i<arr.length; i++){
	char ch = arr[i];
	if(!map.containsKey(ch)){
		map.put(ch, i);
	} else {
	//reposioning the cursor i to the position of ch
	//the new substring start from i+1
		i=map.get(ch);
		//clearing the charPosMap
		map.clear();
	}
	if(map.size()>longestLength){
		longestLength = map.size();
		longest = map.keySet().toString();
	}
}
```
## 43 Swap Two String Variables Without Using Third Variable
```java
s1 = s1+s2;
s2 = s1.substring(0, s1.length()-s2.length());
s1=s1.substring(s2.length());
```
## 44 Stop A Thread
```java
//volatile will make thread to read its value from the main memory, thus making sure that thread always gets its updated value.
class MyThread extends Thread{
	private volatile boolean flag = true;
	public void stopRunning(){
        flag = false;
    }
    @Override
    public void run()
    {
        //Keep the task in while loop
        //This will make thread continue to run until flag becomes false
         
        while (flag){
            System.out.println("I am running....");
        }
        System.out.println("Stopped Running....");
    }
}
public static void main(String[] args){ 
	MyThread thread = new MyThread();
        thread.start();
         
        try{
            Thread.sleep(100);
        } 
        catch (InterruptedException e){
            e.printStackTrace();
        }
         
        //call stopRunning() method whenever you want to stop a thread
		thread.stopRunning();
}
```
## 45 Synchronize ArrayList, HashSet And HashMap 
```java
 ArrayList<String> list = new ArrayList<String>();
 HashSet<String> set = new HashSet<String>();
 HashMap<String, Integer> map = new HashMap<String, Integer>();
 
 List<String> synchronizedList = Collections.synchronizedList(list);
  Set<String> synchronizedSet = Collections.synchronizedSet(set);
  Map<String, Integer> synchronizedMap = Collections.synchronizedMap(map);
//
 synchronized (synchronizedList) {
 Iterator<String> it = synchronizedList.iterator();
     while (it.hasNext()) {
                System.out.println(it.next());
            }
 }
 //
 synchronized (synchronizedSet) 
        {
            Iterator<String> it = synchronizedSet.iterator();
             
            while (it.hasNext()) 
            {
                System.out.println(it.next());
            }
        }
//
 Set<String> keySet = synchronizedMap.keySet();
 synchronized (synchronizedMap) 
        {
            Iterator<String> it = keySet.iterator();
             
            while (it.hasNext()) 
            {
                System.out.println(it.next());
            }
        }
//
Collection<Integer> values = synchronizedMap.values();
synchronized (synchronizedMap) 
        {
            Iterator<Integer> it = values.iterator();
             
            while (it.hasNext()) 
            {
                System.out.println(it.next());
            }
        }
```
## 46 Number Of Characters, Words And Lines In File
```java
BufferedReader reader = null;
 int charCount = 0;
 int wordCount = 0;
 int lineCount = 0;
 try{
	reader = new BufferedReader(new FileReader("C:\\sample.txt"));
	String currentLine = reader.readLine();
	while(currentLine!=null){
		lineCount++;
		String[] words = currentLine.split(" ");
		wordCount = wordCount + words.length;
		for(String w: words){
			charCount = charCount+word.length();
		}
		//Reading next line into currentLine
		currentLine = reader.readLine();
	}

}
 catch(IOException e){e.printStackTrace();}
 finally{
try
            {
                reader.close();           //Closing the reader
            }
            catch (IOException e) 
            {
                e.printStackTrace();
            }
}
```
## 47 Convert HashMap To ArrayList
```java
HashMap<String, String> map = new HashMap<String, String>();
Set<String> keySet = map.keySet();
Collection<String> values = map.values();
Set<Entry<String, String>> entrySet = map.entrySet();
ArrayList<String> listOfKeys = new ArrayList<String>(keySet);
ArrayList<String> listOfValues = new ArrayList<String>(values);
ArrayList<Entry<String, String>> listOfEntry = new ArrayList<Entry<String,String>>(entrySet);

```
## 48 All Permutations Of String
```java
void StringPermutation(String prefix, String input){
	int n=input.length();
	if(n==0){
		print(prefix);
	} 
	for(int i=0; i<n; i++){
		StringPermutation(prefix+input.charAt(i),
		input.substring(0,i)+input.substring(i+1, n));
	}
}
void StringPermutation(String input){
	StringPermutation("", input);
}
```
## 49 Check If Number Belongs To Fibonacci Series Or Not
>fibonacci Series : 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89……..
```java
int firstTerm = 0;
int secondTerm = 1;
int thirdTerm = 0;
while (thirdTerm < inputNumber){
  thirdTerm = firstTerm + secondTerm;
  firstTerm = secondTerm;
  secondTerm = thirdTerm;
}
 if(thirdTerm == inputNumber)
        {
            System.out.println("Number belongs to Fibonacci series");
        }
        else
        {
            System.out.println("Number doesn't belongs to Fibonacci series");
        }
```
## 50 String To Integer And Integer To String Conversion
- String To Integer 
```java
Integer.parseInt(str);
Integer.valueOf(str);
```
- Integer To String
```java
Integer.toString(num);
String.valueOf(num);
```

## 51 HashMap
> creating HashMap 
```java
 HashMap<String, Integer> map1 = new HashMap<String, Integer>();
//2. Creating HashMap with 30 as initial capacity 
HashMap<String, Integer> map2 = new HashMap<String, Integer>(30);
//3. Creating HashMap with 30 as initial capacity and 0.5 as load factor
HashMap<String, Integer> map3 = new HashMap<String, Integer>(30, 0.5f);
//4. Creating HashMap by copying another HashMap
HashMap<String, Integer> map4 = new HashMap<String, Integer>(map1);
```
- about key-value pairs
```java
map.put("ONE", 1);
HashMap<String, Integer> anotherMap = new HashMap<String, Integer>();
anotherMap.putAll(map);
//when the pair is absent, add the pair
map.putIfAbsent("ONE", 111);
//retrieve a value associated with a given key 
int value = map.get("TWO");
//check whether a particular key/value exist
map.containsKey(3);//true
map.containsValue(3.3);//true
//k-v pair count
map.size();
//clear the map
 map.clear();
//retrieve key set
Set<Integer> keySet = map.keySet();
//retrieve values
map.values();
//retrieve all k-v pairs
Set<Entry<String, String>> keyValueSet = map.entrySet();
//remove a key-value pair
map.put("ONE", "AAA");
map.remove("ONE");
//remove sucessfully
map.remove("ONE","AAA");
//remove failed
map.remove("ONE","CCC");
//replace a value given a key
map.replace("THREE", "333");
//replace a value only when the paired key 
map.put("FOUR", "DDD");    
map.replace("FOUR", "DDD", "444");//success becomes FOUR: 444
// synchronized HashMap
 Map<String, Integer> syncMap = Collections.synchronizedMap(map);

```
##
```java
```
##
```java
```
##
```java
```
##
```java
```
