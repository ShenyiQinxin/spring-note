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
## Sorting
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
##  Sort A Text File
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
## Find Longest Substring Without Repeating Characters 
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
##
```java
```
##
```java
```
##
```java
```
