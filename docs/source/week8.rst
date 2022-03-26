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