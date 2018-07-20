Bomb, Baby!
===========

You're so close to destroying the LAMBCHOP doomsday device you can taste it! But in order to do so, you need to deploy special self-replicating bombs designed for you by the brightest scientists on Bunny Planet. There are two types: Mach bombs (M) and Facula bombs (F). The bombs, once released into the LAMBCHOP's inner workings, will automatically deploy to all the strategic points you've identified and destroy them at the same time. 

But there's a few catches. First, the bombs self-replicate via one of two distinct processes: 
Every Mach bomb retrieves a sync unit from a Facula bomb; for every Mach bomb, a Facula bomb is created;
Every Facula bomb spontaneously creates a Mach bomb.

For example, if you had 3 Mach bombs and 2 Facula bombs, they could either produce 3 Mach bombs and 5 Facula bombs, or 5 Mach bombs and 2 Facula bombs. The replication process can be changed each cycle. 

Second, you need to ensure that you have exactly the right number of Mach and Facula bombs to destroy the LAMBCHOP device. Too few, and the device might survive. Too many, and you might overload the mass capacitors and create a singularity at the heart of the space station - not good! 

And finally, you were only able to smuggle one of each type of bomb - one Mach, one Facula - aboard the ship when you arrived, so that's all you have to start with. (Thus it may be impossible to deploy the bombs to destroy the LAMBCHOP, but that's not going to stop you from trying!) 

You need to know how many replication cycles (generations) it will take to generate the correct amount of bombs to destroy the LAMBCHOP. Write a function answer(M, F) where M and F are the number of Mach and Facula bombs needed. Return the fewest number of generations (as a string) that need to pass before you'll have the exact number of bombs necessary to destroy the LAMBCHOP, or the string "impossible" if this can't be done! M and F will be string representations of positive integers no larger than 10^50. For example, if M = "2" and F = "1", one generation would need to pass, so the answer would be "1". However, if M = "2" and F = "4", it would not be possible.

Languages
=========

To provide a Python solution, edit solution.py
To provide a Java solution, edit solution.java

Test cases
==========

Inputs:
    (string) M = "2"
    (string) F = "1"
Output:
    (string) "1"

Inputs:
    (string) M = "4"
    (string) F = "7"
Output:
    (string) "4"

Use verify [file] to test your solution and see how it does. When you are finished editing your code, use submit [file] to submit your answer. If your solution passes the test cases, it will be removed from your home folder.

Solution
========
Well if you look well enough at the combinations and you try to retrace from the inputs to (1,1), you will notice that in fact you're using the substraction method to find the lowest common divider of 2 numbers.</br>
Google wants to see if said numbers are primes among them, with a twist, ofc. You deal with huge numbers (as strings) and you have to figure out that you either substract A from B or you either flip A and B if A < B, all while your "numbers" are still strings.

```python
# do M-F , save result in M to save memory and time
def cmmdc(M,F) :
    offset = len(M)-len(F)
    i = len(F)-1
    while i >= 0 :
        sub = M[i+offset]-F[i]
        if sub < 0 : 
            if i-1+offset < 0 :return "impossible"
            M[i-1+offset] -= 1
            sub += 10
        M[i+offset] = sub
        i -= 1
    #substract with borrow, until we run out of number in M.
    while offset >= 0 :
        if M[offset] < 0 :
            M[offset] = 10 - M[offset]
            try : M[offset-1] -=1
            except : return "impossible"
        offset -=1
    i = 0 
    # trimm leading zeros ( 00013445)
    while i<len(M) and M[i] == 0 : i += 1
    return M[i:]


#function that increments on a big number
def increment(counter) :

    add_one = 1
    i = len(counter) - 1
    while add_one > 0 and i>=0 :
        counter[i] += 1
        add_one = counter[i] / 10
        counter[i] %= 10
        i -= 1
    if add_one : counter.insert(1,0)
    return counter

def answer(M, F):
    # convert stringy numbers into in digits to save time and brains
    M = [ ord(d)-48 for d in M ]
    F = [ ord(d)-48 for d in F ]
    res = []
    counter  = [0]
    while F != M and M != [1] :
        if len(M) > len(F) or (len(M) == len(F) and M > F) :
           M = cmmdc(M,F)
        else :
           M,F = F,M
        increment(counter)
        if  type(F) == str or type(M) == str : 
            return "impossible"

    return "".join(chr(48+d) for d in counter)
    # your code here

print answer("12334367845546756765423453620457","3467576545647575467678664124")
print answer("6877","6790")
print answer("4","7")
print answer("2","4")
print answer("2","1")
```
