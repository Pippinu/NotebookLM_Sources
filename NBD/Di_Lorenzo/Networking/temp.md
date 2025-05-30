
### Dijkstra’s Algorithm Example: Step-by-Step

The figure illustrates the application of Dijkstra's algorithm to find the shortest path from node A to all other nodes, particularly D. Each node is labeled with `(distance, predecessor)`. A black filled circle indicates a node whose shortest path from A has been found and is now permanent.

* **Figure (a): Initial Network Graph**
    This shows the network topology with routers (A, B, C, D, E, F, G, H) and the costs (distances) associated with the links between them. 
    * For example, the link A-B has a cost of 2 and E-F has a cost of 2.

* **Figure (b): Step 1 - Node A made permanent**
    * Node A (the source) is made permanent, with a distance of 0 from itself (***(0,-)***).
    * Its direct neighbors are updated with their **tentative distances from A**:
        * Node B is labeled (2,A), meaning the distance to B via A is 2.
        * Node G is labeled (6,A), meaning the distance to G via A is 6.

* **Figure (c): Step 2 - Node B made permanent**
    * From the tentative nodes in (b) (B, G), Node B (2,A) has the **smallest** distance and is made **permanent**.
    * The algorithm now examines paths from B to its neighbors:
        * To Node E: The path A-B-E has a cost of $2+2=4$. E's label is updated to (4,B).
        * To Node C: The path A-B-C has a cost of $2+7=9$. C's label is updated to (9,B).

* **Figure (d): Step 3 - Node E made permanent**
    * Comparing the current tentative distances (E(4,B), G(6,A), C(9,B), etc.), Node E (4,B) is the **smallest**. E is made **permanent**.
    * Paths from E (whose own shortest path from A is 4, via B) to its neighbors are considered:
        * To Node F: The path A-B-E-F has a cost of $4+2=6$. F's label is updated to (6,E).
        * To Node G: The path A-B-E-G has a cost of $4+1=5$. G's label is updated from (6,A) to (5,E)

* **Figure (e): Step 4 - Node G made permanent**
    * Comparing the current tentative distances (G(5,E), C(9,B)), Node G (5,E) is the **smallest**. G is made **permanent**.
    * From G(5,E):
        * To node H: The path A-B-E-G-H has cost of $5+4=9$. H's label is updated to (9, G)

* **Figure (f): Step 5 - Node F made permanent**
    * From figure (e), F(6,E) has the smallest tentative distance. F is made permanent.
    * Paths from F (shortest path from A is 6, via E) to its neighbors:
        * To C: Current label (9,B). Path A-...-E-F-C cost is $6+3=9$. C's label remains (9,B) or could be updated to (9,F) if **path is equivalent**.
        * To H: Current label (9,G). Path A-B-E-F-H cost is $6+2=8$. H's label is updated to (8,F).

* **No figure: Step 6 - Node D made permanent**
    * Paths from D neighbors are C(9,B) and H(8,F), latter is the shortest one so D is labed as (10, H).

This process continues until node D becomes permanent, at which point its label will represent the shortest distance from A to D and the predecessor on that path.