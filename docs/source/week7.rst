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


Day 31 [17 Mar]
================

Question 53: Number of 1 Bits
-------------------------------------
Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

.. note::

    Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
    
    In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 3, the input represents the signed integer. -3.

My solution:

.. code-block:: Java
    :linenos:

    public int hammingWeight(int n) {
        int res = 0;
        while (n!=0) {
            res+=(n&1);
            n>>>=1;
        }
        return res;
    }


Remarks and Complexity Analysis: 
 * Quite similar to logic used in :ref:`Question 52: Reverse Bits`.
 * Wasted time struggling because I didn't know the difference between ``>>>`` (Unsigned Right Shift Operator) and 
   ``>>`` (Signed Right Shift Operator). We should be using the unsigned shift operator as specified in the question.
 * **Time Complexity**: ``O(1)`` there is a defined upper limit for the number of iterations as Java's int is stored as 32-bit (i.e. 32 iterations).
 * **Space Complexity**: ``O(1)``

Question 54: Subtree of Another Tree
-------------------------------------------
Given the roots of two binary trees root and subRoot, return true if there is a subtree of root with the same structure and node values of subRoot and false otherwise.

A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.


My faulty solution: 

.. code-block:: Java
    :linenos:

    private TreeNode findSubTreeRoot(TreeNode root, int strVal) {
        if (root==null) {
            return null;
        } else if (root.val==strVal) {
            return root;
        } else {
            TreeNode left = this.findSubTreeRoot(root.left, strVal);
            TreeNode right = this.findSubTreeRoot(root.right, strVal);
            if (left==null)  {
                if (right==null) {
                    return null;
                } else {
                    return right;
                }
            } else {
                return left;
            }
        }
    }
    
    private boolean isSameTree(TreeNode root1, TreeNode root2) {
        if (root1==null | root2==null) {
            return (root1==null & root2==null);
        } else if (root1.val!=root2.val) {
            return false;
        } else {
            return this.isSameTree(root1.left, root2.left) & this.isSameTree(root1.right, root2.right);
        }
    }
    
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        TreeNode subRootFound = this.findSubTreeRoot(root,subRoot.val);
        if (subRootFound==null) {
            System.out.println("Not FOUND");
            return false;
        } else {
            return this.isSameTree(subRootFound, subRoot);
        }        
    }

161 / 182 test cases passed => didn't handle the scenario of duplicate node values. 

Second Attempt:

.. code-block:: Java
    :linenos:

    private TreeNode[] findSubTreeRoot(TreeNode root, int strVal) {
        if (root==null) {
            return new TreeNode[0];
        } else {
            TreeNode[] left = this.findSubTreeRoot(root.left, strVal);
            TreeNode[] right = this.findSubTreeRoot(root.right, strVal);
            if (root.val==strVal) {
                return ArrayUtils.add(ArrayUtils.addAll(left,right),root);
            } else {
                return ArrayUtils.addAll(left,right);
            }
        }
    }
    
    private boolean isSameTree(TreeNode root1, TreeNode root2) {
        if (root1==null | root2==null) {
            return (root1==null & root2==null);
        } else if (root1.val!=root2.val) {
            return false;
        } else {
            return this.isSameTree(root1.left, root2.left) & this.isSameTree(root1.right, root2.right);
        }
    }
    
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        TreeNode[] subRootFound = this.findSubTreeRoot(root,subRoot.val);
        if (subRootFound.length==0) {
            return false;
        } else {
            for (TreeNode candNode: subRootFound) {
                if (this.isSameTree(candNode, subRoot)) {
                    return true;
                }
            }
            return false;
        }        
    }

LeetCode doesn't recognize ArrayUtils... why?

Solution 

.. code-block:: Java
    :linenos:

    public boolean isSubtree(TreeNode s, TreeNode t) {
        if (s == null) return false;
        if (isSame(s, t)) return true;
        return isSubtree(s.left, t) || isSubtree(s.right, t);
    }
    
    private boolean isSame(TreeNode s, TreeNode t) {
        if (s == null && t == null) return true;
        if (s == null || t == null) return false;
        
        if (s.val != t.val) return false;
        
        return isSame(s.left, t.left) && isSame(s.right, t.right);
    }

    // Another solution

    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if (isEqualTree(root, subRoot)) return true;
        if (root == null) return false;
        return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
    }

    private boolean isEqualTree(TreeNode root1, TreeNode root2) {
        if (root1 == null || root2 == null) return root2 == root1;
        return root1.val == root2.val && isEqualTree(root1.left, root2.left) && isEqualTree(root1.right, root2.right);
    }

Remarks and Complexity Analysis: 
 * It was not a difficult question but I failed. And I believe the cause of the failure is the fact that I did not 
   have a clear design in my head about the implementation before I went to coding it. Moreover, I did not consider 
   the potential edge cases and various scenarios leading to a futile attempt to refactor my code after the ship has sailed. 
 * **Time Complexity**: ``O(n*m)`` where ``n=num_of_nodes_of_main_tree`` and ``m=num_of_nodes_of_sub_tree`` as each node is traversed once.
 * **Space Complexity**: ``O(1)``

.. note:: 

    You can never plan enough!

Day 32 [18 Mar]
================

Question 55: 3Sum
-------------------------------------
Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

My solution after misunderstanding the question: 

.. code-block:: Java
    :linenos:

    public void newTwoSum(Set<Integer> nums, int target, List<List<Integer>> res) {
        // List<List<Integer>> res = new ArrayList<>();
        Set<Integer> memo = new HashSet<>();
        for (int n:nums) {
            if (memo.contains(n)) {
                res.add(new ArrayList<Integer>(Arrays.asList(-target,n,target-n)));
            } else {
                memo.add(target-n);
            }
        }
    }
    
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        
        Set<Integer> negNums = new HashSet<>();
        Set<Integer> posNums = new HashSet<>();
        boolean zeroExists = false;
        
        for (int n:nums) {
            if (n==0) {
                zeroExists = true;
            } else if (n>0) {
                posNums.add(n);
            } else {
                negNums.add(n);
            }
        }
        
        // zero case
        if (zeroExists) {
            for (int posN: posNums) {
                if (negNums.contains(-posN)) {
                    // List<Integer> tmp = new ArrayList<>(Arrays.asList(0,posN,-posN));
                    res.add(new ArrayList<Integer>(Arrays.asList(0,posN,-posN)));
                }
            }
        }
        // 1 pos, 2 neg case
        for (int posN:posNums) {
            this.newTwoSum(negNums,-posN,res);
        }
        // 1 neg, 2 pos case
        for (int negN:negNums) {
            this.newTwoSum(posNums,-negN,res);
        }
        return res;
    }

I did not read the test cases carefully and mistakenly thought that it was a requirement that the triplets are 
consisted of unique integers. 

.. note:: 

    Read the test cases and instructions carefully!

My solution:

.. code-block:: Java
    :linenos:

    public void newTwoSum(List<Integer> nums, int target, List<List<Integer>> res) {
        Set<Integer> memo = new HashSet<>();
        Set<Integer> done = new HashSet<>();
        for (int n:nums) {
            if (done.contains(n)) {
                continue;
            } else if (memo.contains(n)) {
                res.add(new ArrayList<Integer>(Arrays.asList(-target,n,target-n)));
                done.add(n);
            } else {
                memo.add(target-n);
            }
        }
    }
    
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        
        List<Integer> negNums = new ArrayList<>();
        List<Integer> posNums = new ArrayList<>();
        boolean zeroExists = false;
        
        for (int n:nums) {
            if (n>=0) {
                posNums.add(n);
            } else {
                negNums.add(n);
            }
        }
        
        Set<Integer> done = new HashSet<>();
        // 1 pos, 2 neg case
        for (int posN:posNums) {
            if (!done.contains(posN) && posN!=0) {
                this.newTwoSum(negNums,-posN,res);
                done.add(posN);
            }
        }
        done.clear();
        // 1 neg, 2 pos case
        for (int negN:negNums) {
            if (!done.contains(negN)) {
                this.newTwoSum(posNums,-negN,res);
                done.add(negN);
            }
        }
        
        if (Collections.frequency(posNums, Integer.valueOf(0))>=3) {
            res.add(new ArrayList<Integer>(Arrays.asList(0,0,0)));
        }
        
        return res;
    }

Remarks and Complexity Analysis: 
 * The solution is suboptimal as it handles edge cases and some scenarios separately rather than seamlessly under one logical framework.
 * When I decided to implement the ``O(n^2)`` solution (or anything more than ``O(nlogn)``), I should have immediately thought about sorting and making the algorithm more efficient after sorting.
 * **Time Complexity**: ``O(n^2)`` where ``n=nums.length``
 * **Space Complexity**: ``O(n)``

Cleaner solution: 

.. code-block:: Java
    :linenos:

    public List<List<Integer>> threeSum(int[] num) {
        Arrays.sort(num);
        List<List<Integer>> res = new LinkedList<>(); 
        for (int i = 0; i < num.length-2; i++) {
            if (i == 0 || (i > 0 && num[i] != num[i-1])) {
                int lo = i+1, hi = num.length-1, sum = 0 - num[i];
                while (lo < hi) {
                    if (num[lo] + num[hi] == sum) {
                        res.add(Arrays.asList(num[i], num[lo], num[hi]));
                        while (lo < hi && num[lo] == num[lo+1]) lo++;
                        while (lo < hi && num[hi] == num[hi-1]) hi--;
                        lo++; hi--;
                    } else if (num[lo] + num[hi] < sum) lo++;
                    else hi--;
            }
            }
        }
        return res;
    }

Day 33 [19 Mar]
================
Question 56: Remove Nth Node From End of List
----------------------------------------------
Given the head of a linked list, remove the nth node from the end of the list and return its head.

My Two-traversal solution: 

.. code-block:: Java
    :linenos:

    public ListNode removeNthFromEnd(ListNode head, int n) {
        // determine length of list
        int length = 1;
        ListNode curr = head;
        while (curr.next!=null) {
            curr = curr.next;
            length++;
        }
        if (length==1) {
            return null;
        } else if (length==n) {
            return head.next;
        } else {
            curr = head;
            for (int i=0;i<length-n-1;i++) {
                curr=curr.next;
            }
            curr.next = curr.next.next;
            return head;
        }
    }

My One Traversal solution:

.. code-block:: Java
    :linenos:

    public ListNode removeNthFromEnd(ListNode head, int n) {
        int length = 1;
        ListNode curr = head;
        Map<Integer,ListNode> idxToNode = new HashMap<>();
        while (curr.next!=null) {
            idxToNode.put(length-1,curr);
            curr = curr.next;
            length++;
        }
        if (length==1) {
            return null;
        } else if (length==n) {
            return head.next;
        } else {
            ListNode nodeBefore = idxToNode.get(length-n-1);
            nodeBefore.next = nodeBefore.next.next;
            return head;
        }
    }

Remarks and Complexity Analysis: 
 * The question was quite simple compared to the difficulty indicated
 * Thinking about the time complexity of the solution I am considering gave me more confidence that I am doing the right thing (i.e. most efficient possible)
 * **Time Complexity**: both. ``O(n)`` where ``n=list.size``
 * **Space Complexity**: two-traversal. ``O(1)`` one-traversal. ``O(n)``

Really creative solution that reduces space complexity to ``O(1)``: 

.. code-block:: Java
    :linenos:

    public ListNode removeNthFromEnd(ListNode head, int n) {
    
        ListNode start = new ListNode(0);
        ListNode slow = start, fast = start;
        start.next = head;
        
        //Move fast in front so that the gap between slow and fast becomes n
        for(int i=1; i<=n+1; i++)   {
            fast = fast.next;
        }
        //Move fast to the end, maintaining the gap
        while(fast != null) {
            slow = slow.next;
            fast = fast.next;
        }
        //Skip the desired node
        slow.next = slow.next.next;
        return start.next;
    }

The utility of creating a dummy head is that it can return an empty list without having to handle that case separately! It also helps 
manage indices :-)

.. note:: 

    It seems that I am not utilizing even 20% of the full power that two-pointers support.

Day 34 [21 Mar]
================
Question 57: Search in Rotated Sorted Array
----------------------------------------------
There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

My solution:

.. code-block:: Java
    :linenos:

    public int search(int[] nums, int target) {
        int nLen = nums.length;
        int lo = 0;
        int hi = nLen-1;
        
        // locate pivot/minimum - O(log n)
        while (lo<hi) {
            int mid = (hi+lo)/2;
            if (nums[mid]<nums[hi]) {
                hi = mid;
            } else { 
                // nums[mid] is greater than at least one thing so...
                lo = mid+1; 
            }
        }
        // binary search for target - adjusting for circular array
        int pivot = lo;
        lo = 0;
        hi = nLen-1;
        while (lo<=hi) {
            int mid = (hi+lo)/2;
            int adjMid = (mid+pivot)%nLen;
            if (nums[adjMid]==target) {
                return adjMid;
            } else if (nums[adjMid]<target) {
                lo = mid+1;
            } else {
                hi = mid-1;
            }   
        }
        return -1;
    }

Remarks and Complexity Analysis: 
 * This question took me way too much time. 
 * I was trying to implement a ``O(log n)`` solution that would use very close to exactly ``log n`` operations, rather than 
   the ~2 log n operation solution (seen above). It was complicated and difficult to implement. Understanding the growth of functions, 
   if I can prove that a method has a certain big-O time, I should be not linger and struggle in attempt to reduce the constant factor.
 * **Time Complexity**: ``O(log n)`` where ``n=nums.length``
 * **Space Complexity**: ``O(1)`` 

.. code-block:: Java
    
    // attempt stopped in progress
    public int search(int[] nums, int target) {
            int numsLen = nums.length;
            int[] lo = new int[] {0, nums[0]}; // idx & value
            int[] hi = new int[] {numsLen/2, nums[numsLen/2]};
            
            if (lo[1]==target) {
                return lo[0];
            } else if (hi[1]==target) {
                return hi[0];
            }
            
            int[] cand = new int[] {-1, target-1};
            
            while (cand[1]!=target) {
                if (lo[1]<hi[1]) { // ascending
                    if (lo[1]<target && hi[1]>target) {
                        cand[0] = 
                    }
                }
            }
    
        }
    
    // similar idea but completed
    public int search(int[] A, int target) {
        int lo = 0;
        int hi = A.length - 1;
        while (lo < hi) {
            int mid = (lo + hi) / 2;
            if (A[mid] == target) return mid;

            if (A[lo] <= A[mid]) {
                if (target >= A[lo] && target < A[mid]) {
                    hi = mid - 1;
                } else {
                    lo = mid + 1;
                }
            } else {
                if (target > A[mid] && target <= A[hi]) {
                    lo = mid + 1;
                } else {
                    hi = mid - 1;
                }
            }
        }
        return A[lo] == target ? lo : -1;
    }

Question 58: Spiral Matrix
----------------------------------------------
Given an m x n matrix, return all elements of the matrix in spiral order.

Solution:

.. code-block:: Java
    :linenos:

    public List<Integer> spiralOrder(int[][] matrix) {
        
        List<Integer> res = new ArrayList<Integer>();
        
        if (matrix.length == 0) {
            return res;
        }
        
        int rowBegin = 0;
        int rowEnd = matrix.length-1;
        int colBegin = 0;
        int colEnd = matrix[0].length - 1;
        
        while (rowBegin <= rowEnd && colBegin <= colEnd) {
            // Traverse Right
            for (int j = colBegin; j <= colEnd; j ++) {
                res.add(matrix[rowBegin][j]);
            }
            rowBegin++;
            
            // Traverse Down
            for (int j = rowBegin; j <= rowEnd; j ++) {
                res.add(matrix[j][colEnd]);
            }
            colEnd--;
            
            if (rowBegin <= rowEnd) {
                // Traverse Left
                for (int j = colEnd; j >= colBegin; j --) {
                    res.add(matrix[rowEnd][j]);
                }
            }
            rowEnd--;
            
            if (colBegin <= colEnd) {
                // Traver Up
                for (int j = rowEnd; j >= rowBegin; j --) {
                    res.add(matrix[j][colBegin]);
                }
            }
            colBegin ++;
        }
        return res;
    }

    // faulty attempt 
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<Integer>();
        if (matrix==null || matrix.length==0) {
            return res;
        }
        
        int x_i, x_f, y_i, y_f;
        x_i = y_i = 0;
        x_f = matrix[0].length-1;
        y_f = matrix.length-1;
        while (x_i<=x_f || y_i<=y_f) {
            // top left -> top right
            for (int i = x_i; i <= x_f; i++) {
                res.add(matrix[y_i][i]);
                // System.out.println("here1");
            }
            y_i++;
            // right top -> right bottom
            for (int i = y_i; i <= y_f; i++) {
                res.add(matrix[i][x_f]);
                // System.out.println("here22");
            }
            x_f--;
            // bottom right -> bottom left
            if (y_i<=y_f) {
                for (int i = x_f; i >= x_i ; i--) {
                    res.add(matrix[y_f][i]);
                    // System.out.println("here3");
                }
            }
            y_f--;
            // left bottom -> left top
            if (x_i<=x_f) {
                for (int i = y_f; i >= y_i; i--) {
                    res.add(matrix[i][x_i]);
                    // System.out.println("here4");
                }
            }
            x_i++;
        }
        return res;
    }

    // Another solution
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new LinkedList<>(); 
        if (matrix == null || matrix.length == 0) return res;
        int n = matrix.length, m = matrix[0].length;
        int up = 0,  down = n - 1;
        int left = 0, right = m - 1;
        while (res.size() < n * m) {
            for (int j = left; j <= right && res.size() < n * m; j++)
                res.add(matrix[up][j]);
            
            for (int i = up + 1; i <= down - 1 && res.size() < n * m; i++)
                res.add(matrix[i][right]);
                     
            for (int j = right; j >= left && res.size() < n * m; j--)
                res.add(matrix[down][j]);
                        
            for (int i = down - 1; i >= up + 1 && res.size() < n * m; i--) 
                res.add(matrix[i][left]);
                
            left++; right--; up++; down--; 
        }
        return res;
    }

Remarks and Complexity Analysis: 
 * Had a difficult time with this...
 * **Time Complexity**: ``O(n)`` where ``n=total_elements``
 * **Space Complexity**: ``O(1)`` 
