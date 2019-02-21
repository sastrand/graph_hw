## Graph Homework

### Part I

Directed, acyclic graphs are used in several ways to model how large software projects are developed and deployed.

One such application of a DAG is a dependency graph. In a large project code may be broken up into distinct services. Some can run independently, but many will require some combination of others to be already running. These relationships are modeled well in a DAG with each service in the product represented by a node and a directed edge from A to B indicating that B depdends on A.

A topological sort of this DAG represents any valid start-up order of the product.

![CWS_CAP](https://github.com/sastrand/graph_hw/blob/master/etc/dag.png)

1. Using the numbers that represent each service, list five different topological sorts of the graph of the Cascadia Web Services' Customer Analysis Product.

2. Based on the following pseudocode (sourced from [here](https://en.wikipedia.org/wiki/Topological_sorting)), implement Khan's algorithm to find a topological sort for any given graph. Include your code with your submission

       L ← Empty list that will contain the sorted elements
       S ← Set of all nodes with no incoming edge
       while S is non-empty do
           remove a node n from S
           add n to tail of L
           for each node m with an edge e from n to m do
               remove edge e from the graph
               if m has no other incoming edges then
                   insert m into S
       if any edges are left in the graph then
           return error (graph has at least one cycle)
       else 
           return L (a topologically sorted order)

3. Test your algorithm on the Cascadia Web Services graph, include your test case and result with your submission.

### Part II

![Amtrak](https://github.com/sastrand/graph_hw/blob/master/etc/Amtrakfreqmapcolor.png)

Above is a map of Amtrak's current rail lines in the United States. This graph is represented as a weighted edge list [here](https://github.com/sastrand/graph_hw/blob/master/amtrak.txt) with all the cities removed that are neither the terminal of a line nor a junction between at least three cities (except Portland--we're still there). I also removed the "Auto Train" from Lorton, VA to Sanford, FL because I was suspicious that it was not an actual train.

The weights in the edge list are the miles between cities. The colors on the map represent frequency of service and can be ignored here. We'll assume the graph is undirected.

4. Based on the following pseudocode (sourced from [here](https://en.wikipedia.org/wiki/A*_search_algorithm)), implement the A\* algorithm to find the shortest path between Emeryville and Raleigh. Include your code and test output with your submission.

For use by a heuristic function, the file `amtrac_euc.txt` contains the Euclidean distances between each city in the dataset. The original `amtrac_euc.txt` file and the script that generated it are available [here](https://github.com/jeff-lund/AmtrakHeuristics). 

        function A*(start, goal)
            // The set of nodes already evaluated
            closedSet := {}

            // The set of currently discovered nodes that are not evaluated yet.
            // Initially, only the start node is known.
            openSet := {start}

            // For each node, which node it can most efficiently be reached from.
            // If a node can be reached from many nodes, cameFrom will eventually contain the
            // most efficient previous step.
            cameFrom := an empty map

            // For each node, the cost of getting from the start node to that node.
            gScore := map with default value of Infinity

            // The cost of going from start to start is zero.
            gScore[start] := 0

            // For each node, the total cost of getting from the start node to the goal
            // by passing by that node. That value is partly known, partly heuristic.
            fScore := map with default value of Infinity

            // For the first node, that value is completely heuristic.
            fScore[start] := heuristic_cost_estimate(start, goal)

            while openSet is not empty
                current := the node in openSet having the lowest fScore[] value
                if current = goal
                    return reconstruct_path(cameFrom, current)

                openSet.Remove(current)
                closedSet.Add(current)

                for each neighbor of current
                    if neighbor in closedSet
                        continue    // Ignore the neighbor which is already evaluated.

                    if neighbor not in openSet  // Discover a new node
                        openSet.Add(neighbor)
                    
                    // The distance from start to a neighbor
                    //the "dist_between" function may vary as per the solution requirements.
                    tentative_gScore := gScore[current] + dist_between(current, neighbor)
                    if tentative_gScore >= gScore[neighbor]
                        continue    // This is not a better path.

                    // This path is the best until now. Record it!
                    cameFrom[neighbor] := current
                    gScore[neighbor] := tentative_gScore
                    fScore[neighbor] := gScore[neighbor] + heuristic_cost_estimate(neighbor, goal) 

            return failure

        function reconstruct_path(cameFrom, current)
            total_path := [current]
            while current in cameFrom.Keys:
                current := cameFrom[current]
                total_path.append(current)
            return total_path

In the event that Amtrak needs to discontinue parts of the system, we may hope they do so without completely removing service to any given city. For instance, they could remove the route from Tampa to Orlando while still serving both cities through Miami. 

5. Based on the following pseudocode (sourced from [here](https://www.geeksforgeeks.org/greedy-algorithms-set-2-kruskals-minimum-spanning-tree-mst/)) implement Kruskal's algorithm to find the minimum spanning tree of the given Amtrak system, that is, the lines that use the fewest miles of track to connect all the cities currently served (in the `amtrak.txt` file). Include your code and test output with your submission.

       1. Sort all the edges in non-decreasing order of their weight.
       2. Pick the smallest edge. Check if it forms a cycle with the spanning tree formed so far. 
          If cycle is not formed, include this edge. Else, discard it.
       3. Repeat step #2 until there are (V-1) edges in the spanning tree.

6. What percentage of their network could Amtrak discontinue if they implemented this new minimal route? 

### Submission Options
* Zip your program files and example test output up into one zipped directory and submit it to D2L.
* Create a private github repository and add github user `sastrand` as a collaborator. Email me with your repo link anytime before the due date. Please make one `.txt` or `.md` file with your example output.

> Amtrak map image credit: https://upload.wikimedia.org/wikipedia/commons/9/9f/Amtrakfreqmapcolor.png
> `amtrak_euc.txt` source: https://github.com/jeff-lund/AmtrakHeuristics
