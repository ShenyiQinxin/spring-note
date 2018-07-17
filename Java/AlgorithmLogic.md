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
2. walk through the list of objects from left to right.
for each current meeting, compare it with lastMergedMeeting.
If there is an overlap, then set the endTime of the current mergedMeeting
as the larger one. If there is no overlap between the currentMeeting and the 
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
# Greedy
# Sorting, Searching
# Tree
# Dynamic Programming and recursion
# Queue and stack
# Linked list
# general programming
# bit manipulation
# math
