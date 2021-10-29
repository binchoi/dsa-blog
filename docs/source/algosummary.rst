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


Useful Reference (Backtracking)
----------------------------------
A general approach to backtracking questions 

Cred: `issac <https://leetcode.com/problems/combination-sum/discuss/16502/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partitioning)>`_

`Subsets <https://leetcode.com/problems/subsets/>`_

.. code-block:: Java

    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> list = new ArrayList<>();
        Arrays.sort(nums);
        backtrack(list, new ArrayList<>(), nums, 0);
        return list;
    }

    private void backtrack(List<List<Integer>> list , List<Integer> tempList, int [] nums, int start){
        list.add(new ArrayList<>(tempList));
        for(int i = start; i < nums.length; i++){
            tempList.add(nums[i]);
            backtrack(list, tempList, nums, i + 1);
            tempList.remove(tempList.size() - 1);
        }
    }
    Subsets II (contains duplicates) : https://leetcode.com/problems/subsets-ii/

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> list = new ArrayList<>();
        Arrays.sort(nums);
        backtrack(list, new ArrayList<>(), nums, 0);
        return list;
    }

    private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, int start){
        list.add(new ArrayList<>(tempList));
        for(int i = start; i < nums.length; i++){
            if(i > start && nums[i] == nums[i-1]) continue; // skip duplicates
            tempList.add(nums[i]);
            backtrack(list, tempList, nums, i + 1);
            tempList.remove(tempList.size() - 1);
        }
    } 

`Permutations <https://leetcode.com/problems/permutations/>`_

.. code-block:: Java

    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> list = new ArrayList<>();
        // Arrays.sort(nums); // not necessary
        backtrack(list, new ArrayList<>(), nums);
        return list;
    }

    private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums){
        if(tempList.size() == nums.length){
            list.add(new ArrayList<>(tempList));
        } else{
            for(int i = 0; i < nums.length; i++){ 
                if(tempList.contains(nums[i])) continue; // element already exists, skip
                tempList.add(nums[i]);
                backtrack(list, tempList, nums);
                tempList.remove(tempList.size() - 1);
            }
        }
    } 
    Permutations II (contains duplicates) : https://leetcode.com/problems/permutations-ii/

    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> list = new ArrayList<>();
        Arrays.sort(nums);
        backtrack(list, new ArrayList<>(), nums, new boolean[nums.length]);
        return list;
    }

    private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, boolean [] used){
        if(tempList.size() == nums.length){
            list.add(new ArrayList<>(tempList));
        } else{
            for(int i = 0; i < nums.length; i++){
                if(used[i] || i > 0 && nums[i] == nums[i-1] && !used[i - 1]) continue;
                used[i] = true; 
                tempList.add(nums[i]);
                backtrack(list, tempList, nums, used);
                used[i] = false; 
                tempList.remove(tempList.size() - 1);
            }
        }
    }

`Combination Sum <https://leetcode.com/problems/combination-sum/>`_

.. code-block:: Java

    public List<List<Integer>> combinationSum(int[] nums, int target) {
        List<List<Integer>> list = new ArrayList<>();
        Arrays.sort(nums);
        backtrack(list, new ArrayList<>(), nums, target, 0);
        return list;
    }

    private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, int remain, int start){
        if(remain < 0) return;
        else if(remain == 0) list.add(new ArrayList<>(tempList));
        else{ 
            for(int i = start; i < nums.length; i++){
                tempList.add(nums[i]);
                backtrack(list, tempList, nums, remain - nums[i], i); // not i + 1 because we can reuse same elements
                tempList.remove(tempList.size() - 1);
            }
        }
    }

`Combination Sum II (can't reuse same element) <https://leetcode.com/problems/combination-sum-ii/>`_

.. code-block:: Java

    public List<List<Integer>> combinationSum2(int[] nums, int target) {
        List<List<Integer>> list = new ArrayList<>();
        Arrays.sort(nums);
        backtrack(list, new ArrayList<>(), nums, target, 0);
        return list;
        
    }

    private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, int remain, int start){
        if(remain < 0) return;
        else if(remain == 0) list.add(new ArrayList<>(tempList));
        else{
            for(int i = start; i < nums.length; i++){
                if(i > start && nums[i] == nums[i-1]) continue; // skip duplicates
                tempList.add(nums[i]);
                backtrack(list, tempList, nums, remain - nums[i], i + 1);
                tempList.remove(tempList.size() - 1); 
            }
        }
    } 

`Palindrome Partitioning <https://leetcode.com/problems/palindrome-partitioning/>`_

.. code-block:: Java
        
    public List<List<String>> partition(String s) {
        List<List<String>> list = new ArrayList<>();
        backtrack(list, new ArrayList<>(), s, 0);
        return list;
    }

    public void backtrack(List<List<String>> list, List<String> tempList, String s, int start){
        if(start == s.length())
            list.add(new ArrayList<>(tempList));
        else{
            for(int i = start; i < s.length(); i++){
                if(isPalindrome(s, start, i)){
                    tempList.add(s.substring(start, i + 1));
                    backtrack(list, tempList, s, i + 1);
                    tempList.remove(tempList.size() - 1);
                }
            }
        }
    }

    public boolean isPalindrome(String s, int low, int high){
        while(low < high)
            if(s.charAt(low++) != s.charAt(high--)) return false;
        return true;
    } 

Tree Traversal Algorithms
===========================