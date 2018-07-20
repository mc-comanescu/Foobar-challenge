Find the Access Codes
=====================

In order to destroy Commander Lambda's LAMBCHOP doomsday device, you'll need access to it. But the only door leading to the LAMBCHOP chamber is secured with a unique lock system whose number of passcodes changes daily. Commander Lambda gets a report every day that includes the locks' access codes, but only she knows how to figure out which of several lists contains the access codes. You need to find a way to determine which list contains the access codes once you're ready to go in. 

Fortunately, now that you're Commander Lambda's personal assistant, she's confided to you that she made all the access codes "lucky triples" in order to help her better find them in the lists. A "lucky triple" is a tuple (x, y, z) where x divides y and y divides z, such as (1, 2, 4). With that information, you can figure out which list contains the number of access codes that matches the number of locks on the door when you're ready to go in (for example, if there's 5 passcodes, you'd need to find a list with 5 "lucky triple" access codes).

Write a function answer(l) that takes a list of positive integers l and counts the number of "lucky triples" of (li, lj, lk) where the list indices meet the requirement i < j < k.  The length of l is between 2 and 2000 inclusive.  The elements of l are between 1 and 999999 inclusive.  The answer fits within a signed 32-bit integer. Some of the lists are purposely generated without any access codes to throw off spies, so if no triples are found, return 0. 

For example, [1, 2, 3, 4, 5, 6] has the triples: [1, 2, 4], [1, 2, 6], [1, 3, 6], making the answer 3 total.

Languages
=========

To provide a Python solution, edit solution.py
To provide a Java solution, edit solution.java

Test cases
==========

Inputs:
    (int list) l = [1, 1, 1]
Output:
    (int) 1

Inputs:
    (int list) l = [1, 2, 3, 4, 5, 6]
Output:
    (int) 3

Use verify [file] to test your solution and see how it does. When you are finished editing your code, use submit [file] to submit your answer. If your solution passes the test cases, it will be removed from your home folder.

Solution
========
This one was tricky because google wanted all repetitions of access codes, not unique access codes. This fact wasn't made clear in the description, therefore I wasted quite a bit of time trying out different ways to solve the puzzle.
The idea is to generate the sequences of x,y,z that respect the requirements and count them.<br/>
A faster way is to see for a given number in the sequence, how many other numbers it divides.
If a given i number divides a j number, then it divides all other numbers that j divides as well.
Therefore we found an x\<y\<z and we can count all those divisions in the grand total.
```python
#slow answer.. this timeouts a lot
def slow_answer(l):
    v = [ (l[x],l[y],l[z]) 
               for x in range(0,len(l)) 
               for y in range(x+1,len(l)) 
               for z in range(y+1,len(l))
               if x < y < z and l[z] % l[y] == l[y] % l[x] == 0 
               ] 
    return len(v)

def answer(l):
    pass_count = 0
    i = len(l) - 1
    div_count = len(l) * [0]
    while i >= 0 :
        for j in range(i+1,len(l)) :
            if l[j] % l[i] == 0 : 
                div_count[i] += 1
                pass_count += div_count[j]
        i -= 1
    return pass_count

tests = [
[1,1,1,2],
[1, 2, 3, 4, 5, 6],
[1,1,2,3,4,5,6],
[1,1,2,3,4,5,6,6],
[1,1,2,3,4,4,5,6],
[2,3,7],
[1,1,2,3,4,4,5,6,8],
[1,1,2,4,3,4,4,4,5,6,8,8],
[1,2,3,4,5,6,6],
[1,1,2,3,4,5,6,7],
        ]
for test in tests:
  print 'answer : ',answer(test) , ' vs ', slow_answer(test)

```
