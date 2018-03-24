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
		return strArray[i];
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
## 2 pyramid of numbers
## 3 remove white spaces
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
### 4 remove dup elements from arraylist
 ```java
	 
```
### 5 find dup chars
 ```java
	HashMap<Character, Integer> countMap = new HashMap<>();
	char[] charArray = str.toCharArray();
	for(char c: charArray){
		if(countMap.containsKey(c)){
			countMap.put(c, countMap.get(c)+1);
		} else {
			countMap.put(c, 1);
		}
	}
	Set<Charcter> chars = countMap.keySet();
	for(Character ch: chars){
		if(countMap.get(ch)>1){
			println(countMap.get(ch));
		}
	} 
```
### 6 equality of 2 arrays
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
 ### 7. Anagram
 > same set of characters but different order in a string
- **`sort(), equals()`** 
```java
	String s1 = s1.replaceAll("\\s", "");
	String s2 = s2.replaceAll("\\s", "");
	boolean status = true;	 
	if(s1.length() != s2.length()){
		status = false;
	} else {
		char[] s1Arr = s1.toLowerCase().toCharArray();
		char[] s2Arr = s2.toLowerCase().toCharArray();
		Arrays.sort(s1Arr);
		Arrays.sort(s2Arr);
		return Arrays.equals(s1Arr, s2Arr);
	}
```
- iterative
```java
	String s1 = s1.replaceAll("\\s", "");
	String s2 = s2.replaceAll("\\s", "");
	boolean status = true;
	if(s1.length() != s2.length()
```
-
```java
	 
```
-
```java
	 
```
 10. armstrong number
```java
	 
```
 11. dup elements
 ```java
	 
```
 12. sum of all digits
 ```java
	 
```
 13. 2nd largest number
 ```java
	 
```
 14. matrix operation
 ```java
	 
```
 15. occurences of each char
 ```java
	 
```
 16. largest number < a number without a given digit
 ```java
	 
```
 17.  all pairs of elements
 ```java
	 
```
 18. sub array whose sum is equal to a given number
 ```java
	 
```
 19. check whether a number is binary
 ```java
	 
```
 20. 
```java
	 
```
