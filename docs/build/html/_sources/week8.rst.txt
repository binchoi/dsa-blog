************************
Week 8 [26-31 Mar 2022]
************************

Day 37 [26 Mar]
================

Question 62: Validate Binary Search Tree
------------------------------------------
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:
 * The left subtree of a node contains only nodes with keys less than the node's key.
 * The right subtree of a node contains only nodes with keys greater than the node's key.
 * Both the left and right subtrees must also be binary search trees.

My Solution: 

.. code-block:: Java
    :linenos:

    public boolean isValidBSTHelper(TreeNode root, Long lo, Long hi) {
        if (root==null) {
            return true;
        } else {
            if (root.val>=hi || root.val<=lo) {
                return false;
            }
            
            boolean res = true;
            if (root.left!=null) {
                if (root.left.val>=root.val) {
                    return false;
                } else {
                    res = res && this.isValidBSTHelper(root.left, lo, new Long(root.val));
                }
            }
            if (root.right!=null) {
                if (root.right.val<=root.val) {
                    return false;
                } else {
                    res = res && this.isValidBSTHelper(root.right, new Long(root.val), hi);
                }
            }
            return res;
        }
    }

    public boolean isValidBST(TreeNode root) {
        return this.isValidBSTHelper(root, new Long(Integer.MIN_VALUE)-1, new Long(Integer.MAX_VALUE)+1);
    }


Remarks and Complexity Analysis: 
 * I wish to reduce the number of times I press ``Submit`` before getting the question correct. 
 * I had to revise some essential characteristics of BST's (i.e. no duplicate node)
 * Struggled with the edge case involving the ``Integer.MAX_VALUE`` and ``Integer.MIN_VALUE`` -- used ``Long`` (using null may have been easier)
 * **Time Complexity**: ``O(n)`` as all nodes of trees are traversed
 * **Space Complexity**: ``O(1)`` 

Recursion is cool and easy to understand. However, it can be more costly due to the overhead of calling methods. When  dealing with 
large inputs, it may cause stack overflow in which the recursive method calls overload the call stack. Let's explore ways to approach and 
implement in-order Tree traversals in an iterative fashion. 

My Iterative Solution: 

.. code-block:: Java
    :linenos:

    public boolean isValidBST(TreeNode root) {
        Stack<TreeNode> stk = new Stack<>();
        TreeNode prev = null;
        while (root!=null || !stk.isEmpty()) {
            while (root!=null) {
                stk.push(root);
                root = root.left;
            }
            root = stk.pop();
            if (prev!=null && root.val<=prev.val) {
                return false;
            }
            prev = root;
            root = root.right;
        }
        return true;
    }

Recursive Solution:

.. code-block:: Java
    :linenos:

    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    public boolean isValidBST(TreeNode root, long minVal, long maxVal) {
        if (root == null) return true;
        if (root.val >= maxVal || root.val <= minVal) return false;
        return isValidBST(root.left, minVal, root.val) && isValidBST(root.right, root.val, maxVal);
    }

Question 63: Binary Tree Inorder Traversal
--------------------------------------------
Given the root of a binary tree, return the inorder traversal of its nodes' values.

My solution:

.. code-block:: Java
    :linenos:

    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stk = new Stack<>();
        while (root!=null || !stk.isEmpty()) {
            while (root!=null) {
                stk.push(root);
                root = root.left;
            }
            root = stk.pop();
            res.add(root.val);
            root = root.right;
        }
        return res;
    }

Question 64: Binary Tree Level Order Traversal
-------------------------------------------------------
Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

My Solution: 

.. code-block:: Java
    :linenos:

    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root==null) {
            return res;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while (!q.isEmpty()) {
            List<Integer> sub = new ArrayList<>();
            int n = q.size();
            for (int i=0; i<n; i++) {
                root = q.poll();
                sub.add(root.val);
                if (root.left!=null) {
                    q.add(root.left);
                }
                if (root.right!=null) {
                    q.add(root.right);
                }
            }
            res.add(sub);
        }
        return res;
    }

Remarks and Complexity Analysis: 
 * I am happy with the process I took to solve this question. Inspired by the use of Stacks and Queues when dealing with 
   tree traversals, I tried implementing a queue-based method and it worked well.
 * **Time Complexity**: ``O(n)`` as all nodes of trees are traversed
 * **Space Complexity**: ``O(n)`` as at the ``h``th level, there are ``2^h`` elements. Given that the height of the tree is ``log n``, 
   ``2^(log n)=n``

Interesting solutions

.. code-block:: Java
    :linenos:

    // DFS and Pre-order traversal
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        levelHelper(res, root, 0);
        return res;
    }
    
    public void levelHelper(List<List<Integer>> res, TreeNode root, int height) {
        if (root == null) return;
        if (height >= res.size()) {
            res.add(new LinkedList<Integer>());
        }
        res.get(height).add(root.val);
        levelHelper(res, root.left, height+1);
        levelHelper(res, root.right, height+1);
    }


Day 38 [28 Mar]
================

Question 65: Insert Interval
------------------------------------------
You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

My Solution: 

.. code-block:: Java
    :linenos:

    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> resList = new ArrayList<>();
        int[] mergeInt = null;
        boolean done = false;
        for (int[] i : intervals) {
            if (done) {
                resList.add(i);
            } else if (mergeInt==null) {
                if (i[1]>=newInterval[0]) {
                    if (newInterval[1]<i[0]) {
                        resList.add(newInterval);
                        resList.add(i);
                        done = true;
                    } else {
                        mergeInt = new int[] {Math.min(newInterval[0], i[0]), Math.max(newInterval[1], i[1])};
                    }
                } else {
                    resList.add(i);
                }
            } else { // not done and mergeInt is not null
                if (mergeInt[1]>=i[0]) {
                    mergeInt[1] = Math.max(mergeInt[1], i[1]);
                } else {
                    resList.add(mergeInt);
                    resList.add(i);
                    done = true;
                }
            }
        }
        if (!done) {
            if (mergeInt==null) {
                resList.add(newInterval);
            } else {
                resList.add(mergeInt);
            }
        }
        return resList.toArray(new int[resList.size()][]);
    }

Remarks and Complexity Analysis: 
 * I took an extensive amount of time brainstorming and writing up the pseudocode this time. I solved it in one submission attempt which 
   I believe is promising. I just need to speed up the process as a whole as it took me a little too long. 
 * **Time Complexity**: ``O(n)`` where ``n=intervals.length``. Each existing interval is traversed once.
 * **Space Complexity**: ``O(n)`` where ``n=intervals.length`` just for sake of using ArrayList. Apart from that ``O(1)``.

.. note:: 

    Converting from Java Array to ArrayList is something that should be second nature! 

    Array to ArrayList: List<T> list = Arrays.asList(arr);

    ArrayList to Array: list.toArray(new T[list.size()]);

Alternative Solution: 

.. code-block:: Java
    :linenos:

    public int[][] insert(int[][] intervals, int[] newInterval) {
		List<int[]> result = new LinkedList<>();
	    int i = 0;
	    // add all the intervals ending before newInterval starts
	    while (i < intervals.length && intervals[i][1] < newInterval[0]){
	        result.add(intervals[i]);
	        i++;
	    }
	    
	    // merge all overlapping intervals to one considering newInterval
	    while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
	    	// we could mutate newInterval here also
	        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
	        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
	        i++;
	    }
	    
	    // add the union of intervals we got
	    result.add(newInterval); 
	    
	    // add all the rest
	    while (i < intervals.length){
	    	result.add(intervals[i]); 
	    	i++;
	    }
	    
	    return result.toArray(new int[result.size()][]);
    }

I enjoy how well organized this is. It also handles all cases (including when the new interval should be appended at the front of the list) in a 
strategic manner. 

Question 66: Construct Binary Tree from Preorder and Inorder Traversal
-----------------------------------------------------------------------
Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.

LeetCode's Solution: 

.. code-block:: Java
    :linenos:

    int preorderIndex;
    Map<Integer, Integer> inorderIndexMap;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        preorderIndex = 0;
        // build a hashmap to store value -> its index relations
        inorderIndexMap = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            inorderIndexMap.put(inorder[i], i);
        }

        return arrayToTree(preorder, 0, preorder.length - 1);
    }

    private TreeNode arrayToTree(int[] preorder, int left, int right) {
        // if there are no elements to construct the tree
        if (left > right) return null;

        // select the preorder_index element as the root and increment it
        int rootValue = preorder[preorderIndex++];
        TreeNode root = new TreeNode(rootValue);

        // build left and right subtree
        // excluding inorderIndexMap[rootValue] element because it's the root
        root.left = arrayToTree(preorder, left, inorderIndexMap.get(rootValue) - 1);
        root.right = arrayToTree(preorder, inorderIndexMap.get(rootValue) + 1, right);
        return root;
    }

Remarks and Complexity Analysis: 
 * I noticed most of the patterns that are key to the above implementation, but my approach was too near-sighted and 'brute-force'. 
 * Next time, I will observe and notice patterns then take a step back and consider various CS techniques and tools that could help solve the question (e.g. Divide and Conquer).
 * **Time Complexity**: ``O(n)`` where ``n=preorder.length=inorder.length``.
 * **Space Complexity**: ``O(n)`` where ``n=intervals.length`` just for sake of using ArrayList. Apart from that ``O(1)``.
