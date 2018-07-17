# Array and String

## Merge arrays

> given a list of meeting time ranges `new Meeting(startTime, endTime)`, 
> Q: Merge them sequentially
Solution: 
1. sort the list of meetings by starTime
```java
Collections.sort(sortedMeetings, new Comparator<Meeting>{
  public int compare(Meeting m1, Meeting m2){
    return m1.startTime-m2.startTime;
  }
});
```

2. 
- walk through the list of objects from left to right.
- for each current meeting, compare it with lastMergedMeeting.
- If there is an overlap, then set the endTime of the current mergedMeeting
as the larger one. 
- If there is no overlap between the currentMeeting and the 
lastMergedMeeting, add the currentMeeting into mergedMeeting ArrayList.
```java
if(currentMeeting.startTime<=lastMergedMeeting.endTime){
  lastMergedMeeting.setEndTime(Math.max(lastMergedMeeting.endTime,
                    currentMeeting.endTime));
} else {
   mergedMeetings.add(currentMeeting);
}
```

3. ArrayList mergedMeetings

4. Time: O(nlgn)----- sorting algorithm

Space: O(n) ------ arraylist
```java
```

# Hash table

## Find two movies equals the exact flight length

> given an array integers of movieLengths and a flightLength,
> Find two integers in the array where the sum = flightLength

Solution:
1. create a `new HashSet<Integer>()` for movieLengths

2. For each movieLength, calculate the exact right movieLength for fitting in
the flightLength. If the 2nd movieLength is in the HashSet, then both movies are 
the two, the sum of which is the flightLength. If the 2nd movieLength is not in 
HashSet, add it in the HashSet for late matches.
```java
for(int first: movieLengths){
  int matching2nd = flightLength-first;
  if(movieLengthSeen.contains(matching2nd)){
    return true;
  }
}
return false;
```
3. If the for loop end, then there is not a pair fit in flightLength.

4. Time: O(n)----- for loop

Space: O(n) ------ HashSet

# Greedy

## To get the maxProfit from buying and selling stock prices

> given an array `int[] stockPrices` of integers and `getMaxProfit(stockPrices)`

Solution:
1. length should be not lesser than 2
```java
if(stockPrices.length<2){
  throw new IllegalArgumentException();
}
```
2. maintain 2 variables, minPrice and maxProfit, and keeping updating them.
They are initialized as 
```java
int minPrice = stockPrices[0];
int maxProfit = stockPrices[1]- stockPrices[0];
```
3. Start from index 2, for each stockPrices[i], 
calculate the maxProfit and minPrice
```java
maxProfit=Math.max(maxProfit, currentPrice-minPrice);
minPrice=Math.min(minPrice, currentPrice);
```
4. Time: O(n)----- for loop

Space: O(n) ------ array

# Sorting, Searching

## Find rotation point

> given an array of String, the elements are arranged alphabetically
> but need to be rotated in order to start from 'a' 
> initially 'a' is somewhere in the middle of the array

Solution:
1. maintain floorIndex and ceilingIndex, calculate the midIndex
```java
int floorIndex = 0;
int ceilingIndex = words.length-1;
int midIndex = floorIndex+(ceilingIndex-floorIndex)/2;
```
2. when `words[midIndex].compareTo(firstWord)>=0`, rotate to right.
or else rotate to left. if rotate to right, then floorIndex become the last 
midIndex; or else ceilingIndex becomes the last midIndex.
```java
if(words[guessIndex].compareTo(firstWord)>=0){
  floorIndex = midIndex;
} else {
  ceilingIndex = midIndex;
}
if(floorIndex+1==ceilingIndex){ break;
}
//..
return ceilingIndex;
```
# Tree
# Dynamic Programming and recursion
# Queue and stack
# Linked list
# general programming
# bit manipulation
# math
