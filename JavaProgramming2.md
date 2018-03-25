## Find The Percentage Of Uppercase Letters, Lowercase Letters, Digits And Other Special Characters In A String 
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
##
```java
```
##
```java
```
##
```java
```
