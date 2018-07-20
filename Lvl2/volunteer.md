Don't Get Volunteered!
======================



As a henchman on Commander Lambda's space station, you're expected to be resourceful, smart, and a quick thinker. It's not easy building a doomsday device and capturing bunnies at the same time, after all! In order to make sure that everyone working for her is sufficiently quick-witted, Commander Lambda has installed new flooring outside the henchman dormitories. It looks like a chessboard, and every morning and evening you have to solve a new movement puzzle in order to cross the floor. That would be fine if you got to be the rook or the queen, but instead, you have to be the knight. Worse, if you take too much time solving the puzzle, you get "volunteered" as a test subject for the LAMBCHOP doomsday device!



To help yourself get to and from your bunk every day, write a function called answer(src, dest) which takes in two parameters: the source square, on which you start, and the destination square, which is where you need to land to solve the puzzle.  The function should return an integer representing the smallest number of moves it will take for you to travel from the source square to the destination square using a chess knight's moves (that is, two squares in any direction immediately followed by one square perpendicular to that direction, or vice versa, in an "L" shape).  Both the source and destination squares will be an integer between 0 and 63, inclusive, and are numbered like the example chessboard below:


```
-------------------------
| 0| 1| 2| 3| 4| 5| 6| 7|
-------------------------
| 8| 9|10|11|12|13|14|15|
-------------------------
|16|17|18|19|20|21|22|23|
-------------------------
|24|25|26|27|28|29|30|31|
-------------------------
|32|33|34|35|36|37|38|39|
-------------------------
|40|41|42|43|44|45|46|47|
-------------------------
|48|49|50|51|52|53|54|55|
-------------------------
|56|57|58|59|60|61|62|63|
-------------------------
```


Languages
=========



To provide a Python solution, edit solution.py

To provide a Java solution, edit solution.java



Test cases
==========



Inputs:

    (int) src = 19

    (int) dest = 36

Output:

    (int) 1



Inputs:

    (int) src = 0

    (int) dest = 1

Output:

    (int) 3



Use verify [file] to test your solution and see how it does. When you are finished editing your code, use submit [file] to submit your answer. If your solution passes the test cases, it will be removed from your home folder.

Solution
========
We start on the chess board at the location and we explore ( recusivly ) our surroundings.
If it's an unexplored section or we are on a shorter path we move there, otherwise we return and explore another path.
We do this until we either run out of paths to explore or we reach our destination on our shortes path and we return.

```python

def valid_move(src, new_x, new_y) :
    # get coordinates on the board
    x = src/8 + new_x ; y = src%8 + new_y
    # figure out if we fall out
    if x<0 or x>7 : return -1
    if y<0 or y>7 : return -1
    return x*8+y

def back_track(src, dest, visited) :
    if src == dest : return 0
    min_v = 99999999
    for move in [ (+1,+2),(+2,+1),
                  (-1,-2),(-2,-1),
                  (+2,-1),(-2,+1),
                  (+1,-2),(-1,+2),
                  ] :
        v = valid_move(src,move[0],move[1])
        if v >-1 and (visited[v] == 0 or visited[v] > visited[src]) :
            visited[v] = visited[src]+1
            min_v = min(min_v,1+back_track(v,dest,visited))
    return min_v

def answer(src, dest):
    visited = [ 0 for i in range(0,64) ]
    visited[src] = 1
    if src == dest : return 0
    return back_track(src,dest,visited)
    # your code here

print answer(19,36)
print answer(0,1)
```
