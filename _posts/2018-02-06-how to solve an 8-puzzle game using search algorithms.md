# how to solve an 8-puzzle game using search algorithms

### I have created a search agent that solved the puzzle using python 

For this project, I created a full working puzzle game solver because it was the first assignment in the edx artificial intelligence course I'm taking.

We needed to apply search algorithms we learned in the course to solve the 3 x 3 puzzle game and to use python standard library with no additional framework or library to finish the assignment.
This project is important because I need to pass it to earn the final certificate, it's was very challenging for me as it was my first real project on artificial intelligence, I shouldn't be along the group of people who give in a MOOC when they find a challenging project.

Here is how this article will be structured :

- Problem Statement 
- Algorithm review 


Before starting and diving into the code let me introduce the problem as it is in the course :

An instance of the N-puzzle game consists of a board holding n^2-1 distinct movable tiles, plus an empty space. The tiles are numbers from the set 1,..,n^2-1. For any such board, the empty space may be legally swapped with any tile horizontally or vertically adjacent to it. In this assignment, the blank space is going to be represented by the number 0. Given an initial state of the board, the combinatorial search problem is to find a sequence of moves that transitions this state to the goal state; that is, the configuration with all tiles arranged in ascending order  0,1,… , n^2−1. The search space is the set of all possible states reachable from the initial state. The blank space may be swapped with a component in one of the four directions {‘Up’, ‘Down’, ‘Left’, ‘Right’}, one move at a time. The cost of moving from one configuration of the board to another is the same and equal to one. Thus, the total cost of the path is equal to the number of moves made from the initial state to the goal state.


This problem is used to illustrate how a typical intelligent agent more precisely and a Goal-based agent works.

A goal-based is an agent that works to identify action or series of actions that lead to a goal.
We have an initial state(The initial puzzle ) and we want to make a series of actions (move up, down, left and right ) in order to solve the puzzle. (Get to final state )

By making those series of actions we are searching through a graph for all possibles states until we find a final solution that solves our puzzle.



II. Algorithm review.

The searches begin by visiting the root node of the search tree, given by the initial state. Among other book-keeping details, three major things happen in a sequence in order to visit a node:

- First, we remove a node from the frontier set and we add it to the explored set
- Second, check if it's the goal if the condition is satisfied we built the path to that node and returned the path.
- we check the state against the goal state to determine if a solution has been found.
- Finally, if the result of the check is negative, we then expand the node. To expand a given node, we generate successor nodes adjacent to the current node and add them to the frontier set. Note that if these successor nodes are already in the frontier, or have already been visited, then they should not be added to the frontier again.
The order in which children are added to the frontier set depend on the algorithm we are using.

This describes the life-cycle of a visit and is the basic order of operations for search agents in this assignment—(1) remove, (2) check, and (3) expand. In this assignment, we will implement algorithms as described here.
Here is a pseudo code of different algorithm for Breadth First Search and Depth First search implementation.

![Algo Pseudo code](https://thepracticaldev.s3.amazonaws.com/i/v92v36d1wjg0ci436klw.png) 

My task was to translate the 2 pseudo-code into python code and solve the problem.
But how??



**III. Technical Challenges:**

**III. 1 How to Start**

The First technical challenge I faced was how to start the problem, how to translate it into a python solvable problem.


Here is how the program should work :
Suppose the program is executed for breadth-first search as follows:

$ python driver.py bfs 1,2,5,3,4,0,6,7,8

Which should lead to the following solution to the input board:

![ouput](https://thepracticaldev.s3.amazonaws.com/i/uzwm5tr1zibclez9u8k0.png)


The output file should contain the following statistics:

path_to_goal: the sequence of moves taken to reach the goal

cost_of_path: the number of moves taken to reach the goal

nodes_expanded: the number of nodes that have been expanded

search_depth: the depth within the search tree when the goal node is found

max_search_depth:  the maximum depth of the search tree in the lifetime of the algorithm

running_time: the total running time of the search instance, reported in seconds

max_ram_usage: the maximum RAM usage in the lifetime of the process as measured by the ru_maxrss attribute in the resource module, reported in megabytes
 

**__The problem was how to start?__** how to represent the initial state or initial board?
Which Data structure should I use for the initial board? 

I first thought about a 2D list or a 1D list but I discovered that a list cannot be a set member because it's mutable, I choose a set or a tuple for the first item, but still difficult and complicated to use.
**__Solution__** :
After Struggling 2 days to find the appropriate data structure I googled and found that some persons with the same problem use an object-oriented approach.
So a state could be an object with attributes and method, this was the ideal approach for this project.

A node is represented by a class and has the following attributes :

- state: which is a list that saves the current state.
- parent: the parent node that generates this node
- movement: the movement we made to generate this node
- level: the level where this node can be found.
- cost: used for a star algorithm.

The node has a hash method because it will be used as a set element and as a dictionary keys.


```python
class Node:
    """
    in the OOPS approach this will save our node, 
    each node has the following attributes:
    state : this state
    parent  : the parent that generate this node
    movement: the movement we made to generate the node
    level: the level where the node can be found in the graph
    cost : will be used in astar, always 0 for BFS and DFS
    """
    def __init__( self, state, parent, movement, level, cost ):
        self.state = state
        self.parent = parent
        self.movement = movement
        self.level = level
        self.cost = cost
    
    def __repr__(self):
        return str(self.state)

    def __eq__(self, other):
        return self.state == other.state

    def __hash__(self):
        return hash(str(self.state))

    def __ne__(self, other):
        return not self.__eq__(other)
```

Here is how we create a node :

```python
def create_node( state, parent, movement, level, cost ):
    """
    used to create a node
    """
    return Node( state, parent, movement, level, cost)
```

This was the first milestone I made in this project.
**__What I have learned__** : 
- You can't add a list to a set because lists are mutable, meaning that you can change the contents of the list after adding it to the set.
- Set elements, as well as dictionary keys, have to be hashable (ie define a __hash__ function)
 You can find more on [this StackOverflow answer](https://stackoverflow.com/a/1306653/4683950) 



** III. 2. Building The neighbors**

**__The Problem__**:
how to generate neighbors (Or children) for a given state : 
**__The Solution__**:

Given an initial state, we will list all possible move we can make from that state and result of each move will be yield to the result.
here is the action we will do:
    - locate the position of zero from the initial state
    - from that position perform all possible move that can be made, and return the resulting state.

Notice the following remark for the puzzle :
    - if the empty space is at position [2, 5, 8]: we cannot move right
    - if the empty space is at [0, 3, 6 ] we can't move left
    - if the empty space is at [0, 1, 2 ] not move up
    - if empty space at [6, 7, 8 ] we cannot  move down
also, note that each move is performed by doing a  swapping :
    - move up we swap the empty element with the element at [index_empty-3]
    - move down swap the element with the element at [index_empty + 3]
    - move left move down swap the element with the element at [index_empty - 1]
    - move right move down swap the element with the element at [index_empty + 1]

The following code implements the 4 moves and builds neighbor using the moves.

```python
def move_right (state):
    """
    this function check if empty space is not at [2, 5, 8 ]
    and  move right by swapping the elment [index_empty + 1]
    """
    empty_position = state.index(0)
    goal = list(range(len(state)))
    if empty_position not in range(2, len(state), 3) :#and (state[empty_position + 1:] != goal[empty_position + 1:]) :
        new_state = state[:]
        new_state [empty_position] = new_state [empty_position  + 1]
        new_state [empty_position + 1] = 0
        return new_state
    else :
        raise ValueError('cannot move  right')

def move_left (state):
    """
    this function check if empty space is not at [0, 3, 6 ]
    and  move left by swapping the elment [index_empty - 1]
    """
    empty_position = state.index(0)
    if empty_position not in range(0, len(state), 3):
        new_state = state[:]
        new_state[empty_position] = new_state[empty_position -1]
        new_state[empty_position -1] = 0
        return new_state
    else :
        raise ValueError('Cannot move left')


def move_up (state):
    """
    this function check if empty space is not at [0, 1, 2 ]
    and  move up by swapping the elment [index_empty - 3]
    """

    empty_position = state.index(0)
    if empty_position not in range(3):
        new_state = state[:]
        new_state[empty_position] = new_state[empty_position  - 3]
        new_state[empty_position - 3] = 0
        return  new_state
    else :
        raise ValueError('cannot move  up')

def move_down (state):
    """
    this function check if empty space is not at [6, 7, 8 ]
    and  move down by swapping the elment [index_empty + 3]
    """
    empty_position = state.index(0)
    goal = list(range(len(state)))
    if empty_position not in range(6,  len(state)) :
        new_state = state[:]
        new_state[empty_position] = new_state[empty_position  + 3]
        new_state[empty_position +  3] = 0
        return new_state
    else :
        raise ValueError('cannot move  down')

def build_neighbour(initial_state, mode=None):
    """
    this function will generate all possible sate (children, neighbour ) that occurs when we made 
    a move , up, down and left,
    mode = 1 will be used for DFS
    mode = -1 will be used for BFS
    note for depth first search we will be puting element in RLDU and pull them in UDLR
    for BFS we will put them in UDLR and pull and dequeu them in the same order
    """
    for direction, action in zip(['Right', 'Left', 'Down', 'Up'][::mode] , [move_right, move_left, move_down, move_up][::mode]):
        try:
            new_state = create_node(state=action(initial_state.state), 
                                    parent=initial_state, 
                                    movement=direction, 
                                    level=initial_state.level+1, 
                                    cost=0)
            #need to check if not in frontier
            yield new_state
        except ValueError:
            pass

```

**__What I have Learned__**:
- As you can see in the moves functions, 
I'm creating a new_state list with this :
```new_state = state[:]```
previously I was doing this :
```new_state = state```
but this doesn't work, and I spent hours trying to figure out why it was not working and I found [this answer](https://stackoverflow.com/a/2612815/4683950) on SO
With new_list = my_list, you don't actually have two lists. The assignment just copies the reference to the list, not the actual list, so both new_list and my_list refer to the same list after the assignment.

- Also instead of creating a function that returns a list of elements, I have learned how to create a generator function, with the yield keyword. the build_neighbor is a  generator you can loop over it as you do with python lists (it behaves like an iterator). read about it [here](https://wiki.python.org/moin/Generators) 


As we know how to represent a board, an initial state, and we have learned how to generate children the third challenge was how to implement the 3 search algorithms(Depth First Search, Breadth First Search, and A start search ) we learned in python in order to solve this problem.

** III.3 Breath First Search Implementation:**


- For this algorithm, the inputs are the initial_sate node, the goal node list
- The output is the path to the goal if the goal is found (We didn't take care of unsolvable puzzles )
We need also to have a data structure for the explored nodes, and frontiers :
For explored we have to use a python set.
For Frontier, As shown in the pseudo code we need a queue which is known to be FIFO (First in First Out), items are added in UDLR order and removed in the same order.
In python, there is no queue data-structure but from [python doc](https://docs.python.org/3/tutorial/datastructures.html#using-lists-as-queues) we learned that: __It is possible to use a list as a queue, where the first element added is the first element retrieved (“first-in, first-out”); however, lists are not effective for this purpose. While appends and pops from the end of a list are fast, doing inserts or pops from the beginning of a list is slow (because all of the other elements have to be shifted by one).__
To implement a queue, use collections.deque which was designed to have fast appends and pops from both ends.
we add an element using append and remove it using the popleft method.

- For checking if an item is in frontier U explored we check in the explored set and in the frontier queue but the algorithm was extremely slow.
Then I learned this important tip [here](https://stackoverflow.com/a/7717046/4683950)  :
__Membership testing in a set is vastly faster, especially for large sets. That is because the set uses a hash function to map to a bucket. Since Python implementations automatically resize that hash table, the speed can be constant (O(1)) no matter the size of the set (assuming the hash function is sufficiently good).
In contrast, to evaluate whether an object is a member of a list, Python has to compare every single member for equality, i.e. the test is O(n).
__
So I decided to not  check in the frontier queue directly but we have created an alternative frontier set were we kept states of the explored node and check membership testing in it  this helps us to gain a considerable amount  of time and increase performance for our algorithm)
Thanks [to this comment](https://courses.edx.org/courses/course-v1:ColumbiaX+CSMM.101x+1T2018/discussion/forum/8f54cd8c0caae0e077c55dfa875f51546157e091/threads/5a9258d024451a09ac0017e0) in the edx  forum 

``` python 
def breadth_first_search(initial_state, goal):
    """
    We will implement breadth first search as approach for solving
    problem
    """
    level = 0
    frontier = deque([initial_state])
    explored = set()
    frontier_set = {initial_state} #very important
    max_search_depth = 0
    nodes_expanded = 0
    while frontier: #repeat  until the frontier will be empty
        node = frontier.popleft() #first remove the node from the frontier set
        explored.add(node) #add it to explored list
        #if the state is found build teh path to that state
        if node.state == goal:
            moves = build_path(node)
            return moves, node.level, max_search_depth, nodes_expanded
        nodes_expanded +=1
        for neighbour in build_neighbour(node, mode=-1): #expand the node by generating all children
            if neighbour not in explored and neighbour not in frontier_set: # check if it's already explored, important
                frontier.append(neighbour) #add it to the frontier
                frontier_set.add(neighbour) #very important
            if neighbour.level > max_search_depth:
                max_search_depth += 1

```
**__what I have learned__**:

The most important thing I learned here is the fact that membership testing in a set is faster than membership testings in lists.


III. 4  Depth First Search :

As said the main difference between DFS and BFS approach and is the data-structure we used and how items are added and removed.
depth-first search uses a stack as the frontier, a stack is LIFO (Last In Last Out ) and this helps us to visit the graph deeper expanding node by node.
note in python doesn't have a special data structure for a stack:
Here is how python suggest us to implement a stack from [pyhton doc](https://docs.python.org/3/tutorial/datastructures.html#using-lists-as-stacks)
The list methods make it very easy to use a list as a stack, where the last element added is the first element retrieved (“last-in, first-out”). To add an item to the top of the stack, use append(). To retrieve an item from the top of the stack, use pop() without an explicit index.

Neighbors are added in RLDU order (Right,  Left,  Down, Up ) and retrieved in the reversed order: Up, Down, Left, Right order.
we keep the same ideas for the frontier and frontier set:

Here is the implementation of DFS for this problem:

```python
def depth_first_search(initial_state, goal):
    """
    here is an implmentation of depth first search
    """
    level = 0
    frontier = [initial_state]
    explored = set()
    frontier_set = {initial_state}
    max_search_depth = 0
    nodes_expanded = 0
    while frontier : #repeat  until the frontier will be empty
        node = frontier.pop() #first remove the node from the frontier set
        explored.add(node) #add it to explored list
        #print('out :', node.state, node.movement, node.level, node.state == goal)
        if node.state == goal:
            moves = build_path(node)
            return moves, node.level, max_search_depth, nodes_expanded
        nodes_expanded +=1
        for neighbour in build_neighbour(node, mode=1): #expand the node by generating all children
            if neighbour not in explored and neighbour not in frontier_set: # check if it's already explored
                #print('In ', neighbour, 'and the frontier is :', frontier)
                frontier.append(neighbour) #add it to the frontier
                frontier_set.add(neighbour)
                if neighbour.level > max_search_depth:
                    max_search_depth += 1

```

**__What I learned __**:
We Don't need to think twice to create a stack in python, just use a list: 
simpler.

**III. 6 A Star implementation:

For A star algorithm we use a priority queue, In python, a priority queue is implemented with a list and operation for priority queue are designed with the help of heapq module, find more [here](https://docs.python.org/3/library/heapq.html#priority-queue-implementation-notes).
implementations details of priority queue code are at the below link.

To calculate the priority  :
For the choice of heuristic, used the Manhattan priority function; that is, the sum of the distances of the tiles from their goal positions. **(Add an explanation here )**

```python
def a_start(initial_state, goal):
    """
    here is an implmentation of depth first search
    """
    initial_priority = 0
    frontier = []
    entry_finder = {} #mapping a task to the entry this dictionnary will act as a frontier set
    REMOVED = 'remove' #placeholder for removed task
    counter = count() #unique sequence count
    explored = set()
    nodes_expanded = 0
    max_search_depth = 0
    def add_task(task, priority=0):
        """Add a new task or update the priority of an existing task
        this method should update priority if a new task exist.
        """
        if task in entry_finder:
            old_task = entry_finder[task]
            #print('old_task priority', old_task[0], 'new priority', priority, priority < old_task[0] )
            if priority < old_task[0]:
                #print('let change')
                remove_task(task)
                count = next(counter)
                entry = [priority, count, task]
                entry_finder[task] = entry
                heap_queue.heappush(frontier, entry)
            else:
                #print('dont change')
                pass #don't add it
        else:
            count = next(counter)
            entry = [priority, count, task]
            entry_finder[task] = entry
            heap_queue.heappush(frontier, entry)
    add_task(initial_state) #initialize initial state
    def remove_task(task):
        'Mark an existing task as REMOVED.  Raise KeyError if not found.'
        entry = entry_finder.pop(task)
        entry[-1] = REMOVED #the last task is saved as removed

    def pop_task():
        'Remove and return the lowest priority task. Raise KeyError if empty.'
        while  frontier:
            priority, count, task = heap_queue.heappop(frontier)
            if task is not REMOVED:
                del entry_finder[task]
                return task
        raise KeyError('pop from an empty priority queue')

    while frontier: #repeat  until the frontier will be empty
        node = pop_task() #first remove the node from the frontier set
        explored.add(node) #add it to explored list
        #print('out :', node.state, node.movement, node.level, node.state == goal)
        if node.state == goal:
            moves = build_path(node)
            return moves, node.level, max_search_depth, nodes_expanded

        nodes_expanded +=1
        for neighbour in build_neighbour(node, mode=1): #expand the node by generating all children
            if neighbor not in explored: # check if it's already explored
                add_task(neighbour, priority=initial_priority+1)
                if neighbour.level > max_search_depth:
                    max_search_depth += 1

```

Note that in the algorithm we have implemented 3 different methods :
one to add an item to the queue with the priority if an item is already added and his priority is superior to this we updated the priority with the lowest one.
- another to remove an item from the queue.
- and another one to pop the item with the lowest priority in the queue.

