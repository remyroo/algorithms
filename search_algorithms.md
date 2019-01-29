Unlike linear data structures which have only one logical way to traverse them, trees or graphs can be traversed in various ways. 

Example of a tree node:
```ruby
  node = {
    data: 1,
    left: reference_to_left_node,
    right: reference_to_right_node
  }
```

1. Depth-first search (DFS)
    - Goes all the way *down* one subtree to its leaf before moving back up to look at the next branch.
    - At each node, you can only do these three actions: read the data in the node (D), check to see if there is a left node (L), check to see if there is a right node (R). These three actions are repeated at each node over and over, so in code, DFS can be written as a recursive function - calling the function repeatedly until you meet the base case which is the end of the subtree and there are no more children.
    - Big(O):
      - Because you check each individual node only once before moving on, the time it takes to run DFS on a tree is proportional to the number of nodes (n) in the tree. It's time complexity is linear time `O(n)`. Best case scenario would be `O(log n)`.
      - Each recursive function call from the first node to the node you're searching for (or the final leaf node if you don't find it) is stored in memory in the call stack. So the space complexity of DFS is proportional to the height "(h)" of the tree/distance from root to leaf aka `O(h)`.
    - Uses: 
      - DFS is often used in game simulations or to solve puzzles with only one solution.

    a. Preorder:
      - Returns data in the order it appears in the tree, therefore can be used to create a copy of the tree
      - Instructions:
        1. Base case: check for presence of node
        2. Visit the root (D)
        3. Visit the left subtree (L)
        4. Visit the right subtree (R)
      - Code:
        ```ruby
          def preorder_search(node)
            return if node == null

            puts node.data
            preorder_search(node.left)
            preorder_search(node.right)
          end
        ```

    b. Inorder:
      - Returns data in a sorted order, therefore can be used on binary search trees because of the way they are ordered (i.e. left_child.value < parent_node.value && right_child.value > parent_node.value). 
      - Instructions:
        1. Base case: check for presence of node
        2. Visit the left subtree (L)
        3. Visit the root (D)
        4. Visit the right subtree (R)
      - Code:
        ```ruby
          def inorder_search(node)
            return if node == null

            inorder_search(node.left)
            puts node.data
            inorder_search(node.right)
          end
        ```

    c. Postorder:
      - Maps each child node to its parent, therefore can be used to delete a tree without leaving out any orphan child nodes.
      - Instructions:
        1. Base case: check for presence of node
        2. Visit the left subtree (L)
        3. Visit the right subtree (R)
        4. Visit the root (D)
      - Code:
        ```ruby
          def postorder_search(node)
            return if node == null

            postorder_search(node.left)
            postorder_search(node.right)
            puts node.data
          end
        ```


  2. Breadth-first search (BFS)
      - Goes across each level from left to right, visiting each node at that level, before going down.
      - Each node contains the references to its left and right child. To enable breadth traversal, you have to ask each parent node to give you the references (discovered nodes) before you start moving to its left child, or else you won't know where to find the right child.
      - Big(O):
        - Time complexity is linear time `O(n)` since we only visit each node in the queue once, so the total amount of time is proprotional to the number of nodes in the queue.
        - Space complexity is proportional to the max width of the tree `O(w)`
      - Uses:
        - Finding the shortest path between two nodes, GPS navigation, peer2peer networks, social networks etc. that all require you to find neighbouring nodes.
        - Also useful if you have a lot of data but are not exactly sure where the node you're looking for is located. You could begin with BFS to explore and once you find the right "neighbourhood", use DFS to retrieve the node.
      - Instructions:
        1. Base case: check for presence of root node
        2. Visit root node and put it in a queue (D)
        3. Get any child references it may have and enqueue them after the root node
        4. Dequeue the root node. Repeat with the first node in the queue until the queue is empty/there are no more child nodes to enqueue.
       - Code:
      ```ruby
        def level_order_search(root)
          return if root == null

          queue = []
          queue.push(root)

          while queue.length > 0
            current_node = queue[0]

            if current_node.left
              queue.push(current_node.left)
            end

            if current_node.right
              queue.push(current_node.right)
            end

            queue.shift()
          end
        end
      ```
    

3. Dijkstra's Algorithm
    - It finds the shortest distance between two nodes in a weighted graph.
    - BFS can be viewed as a special case of Dijkstra's algorithm but on an unweighted graph since BFS uses a FIFO queue instead of a Priority Queue. BFS fans out uniformly in its search whereas Dijkstra's prioritizes which paths to explore based on their weights/costs (e.g. distance, road vs. forest etc.).
    - Big(O):
      - Time complexity is `O(E VlogV)` where `E` = total no. of edges and `V` = total no. of vertices/nodes. It's very fast.
    - Uses:
      - Used in routing protocols, road networks etc. 
    - Instructions:
      1. Create a set of unvisited nodes
      2. Assign to every node a tentative distance value which will initially just be 0 for the current/start node and infinity for all the rest
      3. For the current node, look at all its neighbours and calculate their tentative distances from the current. Whichever neighbhour node has the smallest distance value, it becomes the current node and the old current node gets marked as "visited" and removed from the unvisited set.
      4. Repeat until the destination node has been marked visited. 


4. A-star Algorithm
    - It finds the shortest distance between two nodes in a weighted graph. 
    - It is an extension of Dijkstra's algorithm, but with the addition of heuristics. A heuristic is a function that provides a best guess/estimate solution to a problem.
    - The A-star algorithm makes educated guesses about which path to pursue, cutting down on the total graph area to be searched, thereby providing better performance. 
    - Big(O):
      - Time complexity `O(E) = O(b^d)` where `E` is total no of edges which is determined by `b` the branching factor/total number of successive branches per state.
    - Uses:
      - It is popularly used in pathfinding and graph traversal.
    - Instructions:
      1. At each node, it has to decide which of its paths to travel/extend. It does this by minimizing `f(n) = g(n) + h(n)` where `n` is the next node on the path, `g(n)` is the cost of the path from the start node to `n` and `h(n)` is a heuristic function that estimates the cost of the cheapest path from `n` to the goal destination.
      2. It uses a Priority Queue to store the nodes with the lowest `f(x)` cost, and once its calculated the new `f(x)` values for all the neighbouring nodes, it dequeues the current node and moves on to the next neighbour in the queue with the lowest `f(x)` value until goal node has been reached or the queue is empty.
