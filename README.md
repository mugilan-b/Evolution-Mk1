# Evolution-Mk1 <br>
Basic evolution simulation using Python. This simulates a predator-prey relationship in an finite sized environment, with reproduction and automatic energy regaining ability for the Prey.

![Sample video](/001.avi) 

If the above sample video does not load, check it out at https://youtu.be/JgUkicFaiG0

# What does this do?

There are several modules involved. Check the index on the top left.

# The playground / world: <br>
The playground is by default a square, with boundaries that 'curve' on to each other. That is, an object travelling right will emerge from the left when hitting the boundary, preserving speed.

# A 'Being' Class: <br>
This is the parent class for all beings - Both types, predator **and** prey. It contains all functions that are exactly same in both.

# A 'Prey' Class: <br>
This is the prey class. Preys have a Neural Network that decides what they do. The input to their Neural Network is determined by the **raycast** function. Rays of a finite length, in pre-determined angles (relative to the direction it looks) are casted and checked for collision with **all** Predators. Each ray will output 0 if no predators are hit, or 1 if a predator is hit. -1 if the bodies of Prey and Predator intersect. If there are **N** rays spread over the Field of view of the prey, this will be all the input to the neural network (N input nodes). There are 2 hidden layers of configurable size that then map to an output layer with 2 nodes - one determines the linear speed (along the direction of look) and the other determines the angular speed (which affects direction of look). 

A prey will reproduce according to the repr_chance function. It is designed in such a way that the higher energy the prey possesses leads to a much higher reproduction chance. Energy is regained when the Prey moves slower than a threshold speed. Energy is lost continually when the Prey moves faster than said threshold. It is also lost as a chunk when it reproduces. They die when they are either eaten by a Predator or when their energy reaches 0. When a Prey reproduces, it makes a copy of its own at the same location, except the fact that the Neural Network is slightly mutated according to the mutate() function - a random number within a given range is added to each weight in the neural network, thereby slightly changing its decision making process. No other factor changes. A prey at the start of the simulation will have the same "physical characteristics" as one at the end of a simulation.

**It is important to note that the stats indicate natural deaths of Preys (Death by no energy), not total deaths**

# A 'Predator' Class: <br>
This is the predator class. A lot of the functions are similar to Prey. The differences are: <br>
- Energy is gained through 'eating' (technically colliding) with any Preys in vicinity.
- The physical characteristics are different. A different FoV is kept by default (smaller, but longer range vision).
- They die only when their energy reaches 0.

# The stats display and output video: <br>
OpenCV2 is used to render a video and save to the disk. The make_window() function is a basic GUI renderer - it writes elements based on **relative** screen position, therefore can be tweaked accordingly. <br>
