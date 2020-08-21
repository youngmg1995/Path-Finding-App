[<img width=140 heigth=140 align="left" src="/public/android-chrome-192x192.png">](https://youngmg1995.github.io/Path-Finding-App/)

# Path-Finder
React App for Visualizing Path-Finding Algorithms

<br></br>
## Overview
Path-Finder is a single page web-app bootstrapped using [Create React App](https://create-react-app.dev/) that serves as tool for visualizing various path-finding and maze building algorithms. I decided to build this project while learning Javascript for the first time in order to put the new language to use and as a nice way of reviewing some of old algorithms. The app can be reached using the following link via desktop or mobile: [Path-Finder](https://youngmg1995.github.io/Path-Finding-App/), which can also be saved to your home screen for easy access later. If you like and enjoy the game, please star/follow the page and consider contributing to the project.

## Path-Finding Algorithms
Below are short descriptions of the path-finding algorithms used in the app. The algorithms are broken up into two categories: 

  - __Unweighted:__ algorithms which ignore the cost of moving from one node to another (ignores weights on board)
  - __Weighted:__ algorithms which take into account the cost of moving through an open node vs a weighted node (1:10 cost respectably)

All algorithms, besides Random Walk,  utilize a priority queue to order the paths and to allow for backtracking as well as an extended list to prevent paths from revisiting previously extended nodes. Also, in our implementation for algorithms that utilize an admissable heuristic we leverage the Manhattan Distance for hexagonal grids to get a perfect lower bound on the distance between the current node and the target node.

### Unweighted
  - __*Random Walk:*__ The simplest possible algorithm, just randomly wander from one node to the next, not even checking if it has already visited, hoping that eventually we stumble upon the target.
  - __*Depth-First Search:*__ Along with breadth first search, this is one of the basic search algorithms. This one keeps extending current path to first available node until target is found. Guarantees a path will be found, if possible, but does not gaurantee the shortest path.
  - __*Breadth-First Search:*__ This algorithm is the complement to depth-first search as one of the two basic search algorithms. It works in the exact opposite manner as depth-first search, spreading out like a wave and always extending the shortest path so far to all available nodes. Guarantees the shortest path will be found, if possible.
  - __*Hill Climbing:*__ An altered version of depth-first search that utilizes an admissable heuristic. Like depth-first search, this algorithm always extends the current path; however, instead of choosing the first available node at random, we choose the node that we estimate to be closer to the target using the heuristic. Guarantees a path will be found, if possible, but does not gaurantee the shortest path.
  - __*Beam Search (w=2):*__ An altered version of breadth-first search that utilizes tree-trimming and an admissable heuristic. Like breadth-first search, this algorithm always extends the shortest path so far; however, instead of extending the path to all available nodes, we only extend the path to the first *w* nodes that are closest to the target according to our heuristic. In our case, we will use *w=2*. Does not guarantee a path will be found or that the path found will be the shortest possible.
  - __*Best-First Search:*__ A greedy algorithm that utilizes an admissible heuristic. Like breadth-first search, this algorithm will extend the current path to all available nodes; however, unlike any of the other algorithms, we do not necessarily continue along one of these extensions. Instead, the algorithm always chooses to extend the path that is currently closest to the target according to the prescribed heuristic. Guarantees a path will be found, if possible, but does not gaurantee the shortest path.

### Weighted
  - __*Branch & Bound:*__ Weighted equivalent of breadth-first search, always extending the least costly path so far to all available nodes. Guarantees the least costly path will be found, if possible.
    - *Note* - This algorithm is equivalent to Dijkstra's. I prefer to call it a branch & bound algorithm that utilizes an extended list because that is how I was taught it and this makes my understanding of it more general.
  - __*A\*:*__ An altered version of branch & bound or Dijkstra's that utilizes an admissable heuristic. Like branch & bound the algorithm always extends the best path so far to all available nodes; however, this algorithm considers the best path to be the one with the smallest combined path cost and estimated cost from the current node to the target using the heuristic. Guarantees the least costly path will be found, if possible.

## Mazes-Building Algorithms
Below are short descriptions of the various algorithms used to build the mazes for the app. I have broken them up into the following four categories:

  - __Random:__ according to a given probability, randomly fill each node on the board with a wall or weight
  - __Perfect Mazes:__ these are true maze building algorithms which generate a single unique path between the start and target nodes
  - __DLA Fractals:__ these are patterns generated by simulating DLA ([Diffusion Limited Aggregation](http://paulbourke.net/fractals/dla/)) processes. Essentially, particles undergoing random walks are allowed to stick together to form Brownian trees, fractals that resemble lightning, tree roots, and certain mineral deposits.
  - __Caves:__ these produce procedurally generated caves, or open spaces, within an otherwise walled-off background. Similar techniques are used in countless video games to generate completely unique maps and dungeons on the fly.

All the algorithms build a maze of open hexes over top of a board filled with walled off nodes, except for the DLA Fractals which build on top of a board filled with weighted nodes. To ensure the algorithms do not randomly wall off the start or target nodes, we leverage flood-fill and an A* path-finding algorithm to ensure a path between the two exists for the Random and Cave algorithms. This is not an issue for the perfect mazes which ensure a perfect path exists.

### Random
  - __*Random Walls:*__ Clears then loops over the entire board giving each node a 50% chance of becoming a wall.
  - __*Random Weights:*__ Clears then loops over the entire board giving each node a 50% chance of becoming a weight.

### Perfect Mazes
With the exception of Kruskal's algorithm, each of these maze building algorithms are similar in practice to the path-finding algorithms. Specifically, after starting with a random node, we iteratively generate a path that serves as the open spaces within the maze, storing the available extensions of the path in a priority queue that allows for backtracking, and also utilizing several lists to keep track of what nodes are still available and which can no longer be incorporated into a path (each open node is only allowed 2 open neighbors). Starting from this basis, each of the algorithms are created by changing how we add new extensions to the priority queue.
  - __*Depth-First Maze:*__ Like depth-first search, this algorithm generates our maze path by continuously extending to a random neighboring node. If the path reaches a point with no available neighbors, we then backtrack along the path to the first node that does and repeat the process until no neighbors are available. This results in very long, "rivery" maze paths that are difficult to solve by hand.
  - __*Breadth-First Maze:*__ Like breadth-first search, this algorithm generates our maze in a wave like fashion, always extending the path to one of the neighboring nodes nearest to our starting point at random and repeated this process until no more neighbors are available. This results in a maze which appears to radiate in a set pattern from a single point and is much easier to solve by hand since all paths lead to this bottleneck.
  - __*Hunt & Kill:*__ This algorithm is very similar to the depth-first maze algorithm, but with a small change to the backtracking procedure. Instead of backtracking to the first node with available neighbors, we instead choose a random node along the path. This results in very long, "rivery" maze paths like the depth-first maze, but with longer side paths along the way, making it the hardest maze to solve by hand.
  - __*Prim's Maze:*__ This algorithm is very similar to the breadth-first maze algorithm, but with a small change to the procedure for drawing the next neighbor that extends the path. Instead of picking from the path neighbors closest to the start, we instead pick randomly amongst all the available neighboring nodes. Like the breadth-first mazes this results in a maze that radiates from a point which all paths converge to; however, the paths will be much more random and difficult to solve by hand.
  - __*Kruskal's Maze:*__ Unlike the above algorithms, which all iteratively extend a single connected path, Kruskal's algorithm generates, extends, and combines multiple paths simultaneously. As appose to randomly picking neighbors along the single path in the other algorithms, Kruskal's algorithm iteratively picks a random node from any where on the board. If this node creates an entirely new disjoint path or connects two disjoint paths, then make it empty, else make it a wall. This results in very random and very balanced mazes that are not too "rivery" and have plenty of side paths.
    - *Note:* The above implementation of Kruskal's algorithm does not actually create perfect mazes, since the resulting mazes may have small loops. I attribute this to the nature of my hex-grid and the fact that the nodes themselves serve as the walls rather than the edges of each node.

### DLA Fractals
  Each of these algorithms have the same general structure. After setting the entire board to weights, we start with a random node to be the first empty cell. Then virtual brownian particles are generated that randomly drift around the board. When one of these particles contacts a neighbor of th empty cell, it too becomes empty. The particle is then destroyed and recreated in a new random location to repeat the process until the path of empty cells reaches a certain size (1/6th the area of the board). The only difference between the two algorithms is how the brownian particles are generated.
  - __*Random DLA Fractal:*__ This algorithm follows the prescription given above with the brownian particles randomly being generated in any of the nodes on the board that are not already part of the path. This results in a brownian tree structure that appears to radiate evenly from a single point on the board and sometimes become thicker towards its center.
  - __*Wall DLA Fractal:*__ Like the Random DLA Fractal, this algorithm follows the prescription given above, however instead of generating the brownian particles anywhere on the board, we only generate them along the edges of the board. This results in a much purer brownian tree with an even thickness that appears to quickly grow towards one wall and slowly towards the others.

### Caves
  - __*Cellular Caves:*__ This algorithm works similar to [Conway's Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) treating each node as a cellular automata whose state is determined by its neighbors. First we start by randomly filling the board with some walls. Then we iteratively determine the next state of each node cell by looking at its neighbors, repeating this annealing process several times. With the proper chose of starting walls and rules for the cells, this miraculously results in nice cave and dungeon style rooms.
    - *Note:* For my implementation of this algorithm, I used a starting percentage of 42% walls, repeated the annealing process 5 times, and found the following rule to results in acceptable caves: If the current cell is a wall and it has at least 2 wall neighbors or if the current cell is empty and has at least 4 wall neighbors then make the current cell a wall, else make the current cell empty.
  - __*Simplex Caves:*__ This algorithm works by sampling from the [Simplex Noise function](https://en.wikipedia.org/wiki/Simplex_noise). Using the center position of each node, we generate a noise value between 0 and 1 for each node. Then using a cutoff value of .5, we let all nodes above this threshold be walls and those below it be open spaces. Additionally, we set this cutoff to decrease to 0 near the board edges to promote a walls boundary.
    - *Note:* This is the only algorithm in the app that I did not entirely write myself. I used the javascript library [noisejs](https://github.com/josephg/noisejs) to generate the simplex function from which we sample. This can be found in the file "perlin2d.js" which is entirely from said library.
  
## Resources
  - __*React:*__ As mentioned previously, this project was bootstrapped using [Create React App](https://create-react-app.dev/). I would highly recommend this tool and the linked resources to anyone else considering building a single page app, since it makes building, developing, and deploying the app much simpler.
  - __*Javascript:*__ With this being my first project in Javascript, I needed a good Javascript reference to turn to when I ran into roadblocks. I found the book [Eloquent Javascript](https://eloquentjavascript.net/) to be the best resource for educating myself on the ins and outs of the language and a good reference for examples on typical Javascript scenarios. In particular the book has several chapters and a project focusing on working with canvas elements that proved to be incredibly applicable to this project. I would strongly recommend that anyone looking to learn Javascript start with this book since it is available online for free and has numerous exercises to work through.
  - __*Hex-Grids:*__ There are a lot of path-finding games out on the web, so in an attempt to be unique I decided to implement mine on a hexagonal grid as apposed to a square grid. This turned out to be much more difficult than I anticipated, involving a lot more complexity and math behind the implementations of the tools in the app. I wish I had found it earlier, but the following website is a very good resource for anyone else considering using hex-grids in their projects: [Hexagonal Grids](https://www.redblobgames.com/grids/hexagons/). It covers various ways to set-up the grids, the benefits of certian axes over others, and how to implement common interactions with the grid.
  - __*Path-Finding Algorithms:*__ Though I tried to describe each algorithm concisely, these descriptions do not explain how to implement each algorithm in detail and likely will go over the heads of anyone who has never worked with pathfinding. For those who find my explanations lacking or just want to learn more, I recommend watching the following two videos provided by MIT: [Unweighted Algorithms](https://www.youtube.com/watch?v=j1H3jAAGlEA), [Weighted Algorithms](https://www.youtube.com/watch?v=gGQ-vAmdAOI&t=1394s). In general, MIT's OpenCourseWare is a fantastic website for taking fully immersive computer science courses online and for free.
  - __*Maze-Building Algorithms:*__
