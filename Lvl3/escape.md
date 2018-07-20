Prepare the Bunnies' Escape
===========================



You're awfully close to destroying the LAMBCHOP doomsday device and freeing Commander Lambda's bunny prisoners, but once they're free of the prison blocks, the bunnies are going to need to escape Lambda's space station via the escape pods as quickly as possible. Unfortunately, the halls of the space station are a maze of corridors and dead ends that will be a deathtrap for the escaping bunnies. Fortunately, Commander Lambda has put you in charge of a remodeling project that will give you the opportunity to make things a little easier for the bunnies. Unfortunately (again), you can't just remove all obstacles between the bunnies and the escape pods - at most you can remove one wall per escape pod path, both to maintain structural integrity of the station and to avoid arousing Commander Lambda's suspicions. 



You have maps of parts of the space station, each starting at a prison exit and ending at the door to an escape pod. The map is represented as a matrix of 0s and 1s, where 0s are passable space and 1s are impassable walls. The door out of the prison is at the top left (0,0) and the door into an escape pod is at the bottom right (w-1,h-1). 



Write a function answer(map) that generates the length of the shortest path from the prison door to the escape pod, where you are allowed to remove one wall as part of your remodeling plans. The path length is the total number of nodes you pass through, counting both the entrance and exit nodes. The starting and ending positions are always passable (0). The map will always be solvable, though you may or may not need to remove a wall. The height and width of the map can be from 2 to 20. Moves can only be made in cardinal directions; no diagonal moves are allowed.



Languages
=========



To provide a Python solution, edit solution.py

To provide a Java solution, edit solution.java



Test cases
==========



Inputs:

    (int) maze = [[0, 1, 1, 0], [0, 0, 0, 1], [1, 1, 0, 0], [1, 1, 1, 0]]

Output:

    (int) 7



Inputs:

    (int) maze = [[0, 0, 0, 0, 0, 0], [1, 1, 1, 1, 1, 0], [0, 0, 0, 0, 0, 0], [0, 1, 1, 1, 1, 1], [0, 1, 1, 1, 1, 1], [0, 0, 0, 0, 0, 0]]

Output:

    (int) 11



Use verify [file] to test your solution and see how it does. When you are finished editing your code, use submit [file] to submit your answer. If your solution passes the test cases, it will be removed from your home folder.

Solution
========
Classic pathfinding algo. From the current position, visit the surroundings and fill them with the shortest path there.
Once you hit your destination you print the total path.<br/>
The twist lies in the walls, therefore we have to go through the whole single walls and knock them out one by one and retrace our path for this given variation.

```python
class Point(object):
    def __init__(self,x=0,y=0):
        self.x = x ; self.y = y
# check if this wall is worth knocking down
def candidate_wall(pos,maze):
    #edge case handled by try clauses
    try : 
        if 1 not in (maze[pos.x][pos.y+1], maze[pos.x][pos.y-1]) : return True
    except : pass
    try : 
        if 1 not in (maze[pos.x-1][pos.y], maze[pos.x+1][pos.y]) : return True
    except : pass
    return False

# yeld the next valid move from this move
def valid_moves(pos, maze):
    for move in [(1,0),(-1,0),(0,1),(0,-1)] :
          if pos.x+move[0] <0 or pos.x+move[0] > len(maze)-1 :continue
          if pos.y+move[1] <0 or pos.y+move[1] > len(maze[0])-1 :continue
          if maze[pos.x+move[0]][pos.y+move[1]] == 1 : continue
          point = Point(pos.x+move[0],pos.y+move[1])
          yield point

# for all the valid moves, floodfill the surface
def flood_fill(pos,fill,maze) :
        for move in valid_moves(pos,maze) :
            if fill[pos.x][pos.y]+1 < fill[move.x][move.y] :
               fill[move.x][move.y] = fill[pos.x][pos.y]+1   
               flood_fill(move,fill,maze)

def answer(maze):
    pos = Point(0,0)
    # max val is 20x20, go higher just in case 9000
    fill = [[ 9000 for x in range(0,len(maze[0]))] for x in range(0,len(maze))]
    fill[0][0] = 1
    flood_fill(pos,fill,maze)
    min_walk = fill[len(maze)-1][len(maze[0])-1]
    #naive way of figuring out what wall to knock down
    for x in range(0,len(maze)) :
        for y in range(0,len(maze[0])) :
           if maze[x][y] == 1 :
               knock_pos = Point(x,y)
               if candidate_wall(knock_pos,maze) :
                   maze[x][y] = 0
                   # max val is 20x20, go higher just in case 9000
                   fill = [[ 9000 for x in range(0,len(maze[0]))] for x in range(0,len(maze))]
                   fill[0][0] = 1
                   flood_fill(pos,fill,maze)
                   min_walk = min(fill[len(maze)-1][len(maze[0])-1],min_walk)
                   maze[x][y] = 1
    return min_walk

maze =[
 [0, 1, 1, 0], 
 [0, 0, 0, 1], 
 [1, 1, 0, 0], 
 [1, 1, 1, 0]]

print answer(maze)

maze =[
[0, 0, 0, 0, 0, 0], 
[1, 1, 1, 1, 1, 0], 
[0, 0, 0, 0, 0, 0], 
[0, 1, 1, 1, 1, 1], 
[0, 1, 1, 1, 1, 1], 
[0, 0, 0, 0, 0, 0]
]

print answer(maze)

```
