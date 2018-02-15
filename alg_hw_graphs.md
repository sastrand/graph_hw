## Graph Homework
### Due at 5pm, Friday February 24, 2018

Directed, acyclic graphs are used in several ways to model how large software projects are developed and deployed.

One such application of a DAG is a dependency graph. In a large project code may be broken up into distinct services that each run on their own CPU. Some can run indenpendently and many will require some combination of others to be already running. This complexity is well modeled in a DAG with each service represented by a node and the graph as a whole representing the product. 

A topological sort of this DAG represents any valid start-up order of the product.

1. List five different valid topological sorts of the graph.

2. Based on the following pseudocode found [here](https://en.wikipedia.org/wiki/Topological_sorting), implement Khan's algorithm to find a topological sort for any given graph. Include your code with your submission

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

3. Test you algorithm on the Cascadia Web Services graph, include your test case and result with your submission.

Above is a map of Amtrak's current rail lines in the United States. I represented this graph as a weighted edge list with all the cities removed that are neither the terminal of a line nor a junction between at least three cities (except Portland--we're still there). I also removed the "Auto Train" from Lorton, VA to Sanford, FL.

The weights in the edge list are the miles between the cities. The colors on the map represent frequency of service and can be ignored here. We'll assume the graph is undirected.

4. Based on the following pseudocode from Skiena §6.3.1, implement Dijkstra's algorithm to find the shortest path between Emeryville and Raleigh.

       ShortestPath-Dijkstra(G, s, t) 
           known = {s}
           for i = 1 to n, dist[i] = infinity
           for each edge (s, v), dist[v] = w(s, v) 
           last = s
           while (last != t)
               select vnext, the unknown vertex minimizing dist[v]
               for each edge (vnext, x), dist[x] = min[dist[x], dist[vnext] + w(vnext, x)] 
               last = vnext
               known = union(known, {vnext})


5. In the event that Amtrak needs to discontinue parts of the system, we may hope they do so without completely removing service to any given city. For instance they could remove the route from Tampa to Orlando while still serving both cities through Miami. Based on the following pseudocode found [here](https://www.geeksforgeeks.org/greedy-algorithms-set-2-kruskals-minimum-spanning-tree-mst/) implement Prim's algorithm to find the minimum spanning tree of the given Amtrak system, that is, the lines that use the fewest miles of track to connect all the cities currently served.

       1. Sort all the edges in non-decreasing order of their weight.
       2. Pick the smallest edge. Check if it forms a cycle with the spanning tree formed so far. If cycle is not formed, include this edge. Else, discard it.
       3. Repeat step#2 until there are (V-1) edges in the spanning tree.

### Submission Options
* Submit your assignment as a single pdf to `sastrand@pdx.edu` before the due date above.
* Create a private github repository and add github user `sastrand` as a collaborator. Email me with your repo link anytime before the due date. I will pull a final copy when I grade. Please make one `.txt` or `.md` file in the repo that will serve as your submission. For each question that includes code, include a link in your submission document to the file in the repo containing the relevant code.
