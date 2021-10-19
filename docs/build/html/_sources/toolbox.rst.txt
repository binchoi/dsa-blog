************************
Python Toolkit
************************

General Purpose Built-in Containers
=========================================
Dictionary
------------
[From Python Documentation:] Dictionaries are sometimes found in other languages as “associative memories” or “associative arrays”. Unlike sequences, 
which are indexed by a range of numbers, dictionaries are indexed by keys, which can be any *immutable type*; strings and 
numbers can always be keys. Tuples can be used as keys if they contain only strings, numbers, or tuples; if a tuple 
contains any mutable object either directly or indirectly, it cannot be used as a key. You can’t use lists as keys, 
since lists can be modified in place using index assignments, slice assignments, or methods like ``append()`` and ``extend()``.

It is best to think of a dictionary as a *set of key: value pairs*, with the requirement that the keys are unique 
(within one dictionary). A pair of braces creates an empty dictionary: ``{}``. Placing a comma-separated list of key:value 
pairs within the braces adds initial key:value pairs to the dictionary; this is also the way dictionaries are written on 
output.

The main operations on a dictionary are storing a value with some key and extracting the value given the key. It is also 
possible to delete a key:value pair with ``del``. If you store using a key that is already in use, the old value associated 
with that key is forgotten. It is an error to extract a value using a non-existent key.

Performing ``list(d)`` on a dictionary returns a list of all the keys used in the dictionary, in insertion order (if you 
want it sorted, just use ``sorted(d)`` instead). To check whether a single key is in the dictionary, use the ``in`` keyword.


``d.keys()``
    Returns iterator of the dictionary keys (different from list of keys given by ``list(d)``) 

``d.values()``
    Returns iterator of the dictionary values (to get list of values -> ``list(d.values())``) 

``d.get(...)``

List
-----------------------



Specialized Built-in Containers
=========================================
Collections
-------------
Fill in soon `link <https://docs.python.org/3/library/collections.html#module-collections>_`

************
test
************
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
