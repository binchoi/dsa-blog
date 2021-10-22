********************
Useful Algorithms
********************

Backtracking
=============
Backtracking is a general algorithm for finding all (or some) solutions to some computational 
problems (notably Constraint satisfaction problems or CSPs), which incrementally build candidates 
to the solution and abandons a candidate ('backtracks') as soon as it determines that the candidate 
cannot lead a valid solution. 

Conceptual Background: 
 * Procedure is similar to **tree traversal**: Start from root node and search for solutions at the leaf 
   nodes (each intermediate node ~ *partial* candidate solution).
 * If determined that a certain node cannot lead to a valid solution, we **backtrack** to parent node 
   to explore alternative possibilities.
 * The *backtracking*-step provides increased efficiency over a brute-force search algorithm.

Examples: 
 * **Searching Word Tree:** Given a set of words represented in the form of a tree (each node is a letter),  
   find out if a given word is present in the tree ... **Backtracking**: when we read a node 
   that seems to not lead to our desired word (e.g. searching: ``K O R E A``, found ``K O T _ _``, we *back-track*! 
 * **N-Queen Puzzle:** Place ``N`` queens on a ``[N*N]`` chessboard such that no two queens can attack each 
   other... What is the number of unique solutions? ... **Backtracking**: consider the template solution below 
   (helper functions are not defined yet!)

.. code-block:: python

    def backtrack_nqueen(row = 0, count = 0):
        for col in range(n):
            # iterate through columns at the curent row.
            if is_not_under_attack(row, col):
                # explore this partial candidate solution, and mark the attacking zone
                place_queen(row, col)
                if row + 1 == n:
                    # we reach the bottom, i.e. we find a solution!
                    count += 1
                else:
                    # we move on to the next row
                    count = backtrack(row + 1, count)
                # backtrack, i.e. remove the queen and remove the attacking zone.
                remove_queen(row, col)
        return count
