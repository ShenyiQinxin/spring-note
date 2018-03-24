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
 > In a string, the same set of characters but in different order and upper/lower cases
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
	if(s1.length() != s2.length(){
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
 ### 8. armstrong number
```java
	Integer num = Integer.valueOf(numInt);
	String numStr = num.toString();
	int noDigit = numStr.length();
	int sum = 0;
	while(num != 0){
		int last = num %10;
		int lastToThePower = 1;
		for(int i=0; i< noDigit; i--){
			lastToThePower=lastToThePower*last;
		}
		sum = sum + lastToThePower;
		num = num/10;
	}
	if( sum == num){
		return true;
	}	 
	return false;
```
 ### 9 dup elements
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
 ### 10. sum of all digits
 - iterative
 ```java
	 int sum = 0;
	 while(num != 0){
	 //get the last digit
		 int digit = num%10;
		 sum = sum + digit;
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
		sumDigits(num);	 
	}
}
```
### 11 second largest number
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
 ### 12. matrix operation
 ```java
	 
```
 ### 13. occurences of each char
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
