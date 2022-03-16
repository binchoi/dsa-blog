************************
Week 7 [16-23 Mar 2022]
************************

Day 30 [16 Mar]
================

Question 49: Missing Number
-------------------------------------
Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

My solution:

.. code-block:: Java
    :linenos: 

    public int missingNumber(int[] nums) {
        int n = nums.length;
        int res = n * (n + 1) / 2;
        for (int num:nums) {
            res-=num;
        }
        return res;
    }

Remarks and Complexity Analysis: 
 * It took me more time than it should have to solve this question in ``O(n)`` (~10 min). I had a confusion where 
   I wondered if there is a data structure that permits ``O(1)`` deletion -- there isn't.  
 * **Time Complexity**: ``O(n)`` where ``n=nums.length``. Only one traversal across the ``nums`` array is required 
 * **Space Complexity**: ``O(1)``.

Interesting aproach using **XOR** :: 

    public int missingNumber(int[] nums) {
        int xor = 0;
        for (int i = 0; i < nums.length; i++) {
            xor = xor ^ i ^ nums[i];
        }
        return xor ^ nums.length;
    }

Bit manipulation isn't my forte, but here goes my attempt at an explanation - xor is short for exclusive or (i.e. like regular or but 1 and 1 => 0). 
Hence, when we take ``int a`` and take the xor of itself - ``a^a`` -- the result will be ``0``. As xor of 0 and any integer ``z`` is ``z``, we can say 
that 0 behaves like an additive identity in our scenario. Hence, if we xor all integers from ``0`` to ``n`` and all integer elements of the ``nums`` array, 
we should be left with the one number that hasn't been 'cancelled out' which is our result! 

Remarks and Complexity Analysis: 
 * Clever solution!
 * **Time Complexity**: ``O(n)`` where ``n=nums.length``. Only one traversal across the ``nums`` array is required 
 * **Space Complexity**: ``O(1)``.


Question 51: Invert Binary Tree
-------------------------------------
Given the root of a binary tree, invert the tree, and return its root.

My recursive solution:

.. code-block:: Java
    :linenos: 

    public TreeNode invertTree(TreeNode root) {
        if (root!=null) { 
            this.invertTree(root.left);
            this.invertTree(root.right);
            
            TreeNode tempLeft = root.left;
            root.left = root.right;
            root.right = tempLeft;
        }
        return root;
    }

Wondering if I should have implimented a helper function that returns void to make the code more deliberate/articulate.

My revised solution:

.. code-block:: Java
    :linenos: 

    public TreeNode invertTree(TreeNode root) {
        if (root!=null) {
            TreeNode tempLeft = root.left;
            root.left = this.invertTree(root.right);;
            root.right = this.invertTree(tempLeft);
        }
        return root;
    }


Remarks and Complexity Analysis: 
 * Many recursive calls can cause overflow. Using a stack and while-loop can prevent this. Example provided below.
 * **Time Complexity**: ``O(n)`` where ``n=number_of_nodes``. Every node is traversed once.
 * **Space Complexity**: ``O(1)`` - ignoring the implicit recursive calls.

Robust solution:

.. code-block:: Java
    :linenos: 

    public TreeNode invertTree(TreeNode root) {
        
        if (root == null) {
            return null;
        }

        final Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        
        while(!stack.isEmpty()) {
            final TreeNode node = stack.pop();
            final TreeNode left = node.left;
            node.left = node.right;
            node.right = left;
            
            if(node.left != null) {
                stack.push(node.left);
            }
            if(node.right != null) {
                stack.push(node.right);
            }
        }
        return root;
    }

Question 52: Reverse Bits
-------------------------------------
Reverse bits of a given 32 bits unsigned integer.

My failed solution:

.. code-block:: Java
    :linenos: 

    public int reverseBits(int n) {
        StringBuilder sb = new StringBuilder(String.format("%32s", Integer.toBinaryString(n)).replace(' ', '0'));
        sb.reverse();
        return Integer.parseInt(sb.toString(),2);
    }

.. note:: 

    Fails because it tries to parse a number that would require 33 bits to store as a signed integer. It was foolish to 
    approach this from a String manipulation approach as it requires lots of conversions. 

Solution:

.. code-block:: Java
    :linenos: 

    public int reverseBits(int n) {
        if (n == 0) return 0;
        
        int result = 0;
        for (int i = 0; i < 32; i++) {
            result <<= 1;
            if ((n & 1) == 1) result++;
            n >>= 1;
        }
        return result;
    }

.. note:: 

    ``result <<= 1;`` shifts the bit-representation of ``result`` to the left by one unit. Then, if the last digit of 
    the ``n`` is a ``1`` we append it to our result by adding 1. Then, we shift the ``n``'s binary representation to the right by 
    one unit to consider the next digit. 

Remarks and Complexity Analysis: 
 * **Time Complexity**: ``O(1)`` -- loop will repeat a fixed number of times (i.e. 32)
 * **Space Complexity**: ``O(1)``