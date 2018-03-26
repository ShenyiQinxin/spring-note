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
## Append Text To An Existing File
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
##  Swap Two String Variables Without Using Third Variable
```java
s1 = s1+s2;
s2 = s1.substring(0, s1.length()-s2.length());
s1=s1.substring(s2.length());
```
##  Stop A Thread
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
## Synchronize ArrayList, HashSet And HashMap 
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
## Number Of Characters, Words And Lines In File
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
## Convert HashMap To ArrayList
```java
HashMap<String, String> map = new HashMap<String, String>();
Set<String> keySet = map.keySet();
Collection<String> values = map.values();
Set<Entry<String, String>> entrySet = map.entrySet();
ArrayList<String> listOfKeys = new ArrayList<String>(keySet);
ArrayList<String> listOfValues = new ArrayList<String>(values);
ArrayList<Entry<String, String>> listOfEntry = new ArrayList<Entry<String,String>>(entrySet);

```
## All Permutations Of String
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
## Check If Number Belongs To Fibonacci Series Or Not
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
## String To Integer And Integer To String Conversion
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

## HashMap
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
