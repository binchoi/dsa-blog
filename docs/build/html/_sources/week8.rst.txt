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


My Solution (one day later): 

.. code-block:: Java
    :linenos:

    Map<Integer,Integer> iMap = new HashMap<>();
    int preorderIdx = 0;
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i=0; i<inorder.length; i++) {
            iMap.put(inorder[i], i);
        }
        return this.buildTreeHelper(preorder,0,preorder.length-1);
    }
    
    public TreeNode buildTreeHelper(int[] preorder, int lo, int hi) {
        if (hi<lo) return null;
        //else
        int rootVal = preorder[preorderIdx++];
        TreeNode myTree = new TreeNode(rootVal);
        myTree.left = this.buildTreeHelper(preorder, lo, iMap.get(rootVal)-1);
        myTree.right = this.buildTreeHelper(preorder, iMap.get(rootVal)+1, hi);
        
        return myTree;
    }

The key that I didn't catch previous to viewing the solution is that the recursive calls will increment the shared variable ``preorderIdx`` such that by the time ``myTree.right`` is computed, the ``preorderIdx`` will point to 
the root of the right subtree. Brilliant! 

Day 39 [29 Mar]
================

Question 67: First Unique Character in a String
------------------------------------------------
Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1.

My Solution: 

.. code-block:: Java
    :linenos:

    public int firstUniqChar(String s) {
        HashMap<Character, Boolean> charUniq = new HashMap<>();
        char[] charArr = s.toCharArray();
        for (char c:charArr) {
            if (charUniq.containsKey(c)) {
                charUniq.put(c, Boolean.valueOf(false));
            } else {
                charUniq.put(c, Boolean.valueOf(true));
            }
        }
        for (int i=0;i<charArr.length;i++) {
            if (charUniq.get(charArr[i])) return i;
        }
        return -1;
    }

    // Alternative approach
    public int firstUniqChar(String s) {
        HashSet<Character> charSeen = new HashSet<>();
        HashSet<Character> charSeenTwice = new HashSet<>();
        char[] charArr = s.toCharArray();
        for (char c:charArr) {
            if (charSeen.contains(c)) {
                charSeenTwice.add(c);
            } else {
                charSeen.add(c);
            }
        }
        for (int i=0;i<charArr.length;i++) {
            if (!charSeenTwice.contains(charArr[i])) return i;
        }
        return -1;
    }

    // revised
    public int firstUniqChar(String s) {
        HashMap<Character, Integer> charCount = new HashMap<>();
        for (int i=0; i<s.length(); i++) {
            charCount.put(s.charAt(i), charCount.getOrDefault(s.charAt(i), 0)+1);
        }
        for (int i=0;i<s.length();i++) {
            if (charCount.get(s.charAt(i))==1) return i;
        }
        return -1;
    }

Remarks and Complexity Analysis: 
 * Easy question! If only my future interviews will be like this.
 * I wonder if using one HashMap<Character, Boolean> is more space efficient than using two HashSet<Character>. 
 * ``getOrDefault()`` is a useful method!
 * **Time Complexity**: ``O(n)`` where ``n=s.length()``.
 * **Space Complexity**: ``O(n)`` where ``n=s.length()`` for both implementations


Question 68: First Unique Character in a String
------------------------------------------------
Implement the myAtoi(string s) function, which converts a string to a 32-bit signed integer (similar to C/C++'s atoi function).

The algorithm for myAtoi(string s) is as follows:
 * Read in and ignore any leading whitespace.
 * Check if the next character (if not already at the end of the string) is '-' or '+'. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.
 * Read in next the characters until the next non-digit character or the end of the input is reached. The rest of the string is ignored.
 * Convert these digits into an integer (i.e. "123" -> 123, "0032" -> 32). If no digits were read, then the integer is 0. Change the sign as necessary (from step 2).
 * If the integer is out of the 32-bit signed integer range [-231, 231 - 1], then clamp the integer so that it remains in the range. Specifically, integers less than -231 should be clamped to -231, and integers greater than 231 - 1 should be clamped to 231 - 1.
 * Return the integer as the final result.

Note:

Only the space character ' ' is considered a whitespace character.
Do not ignore any characters other than the leading whitespace or the rest of the string after the digits.

My overflow solution: 

.. code-block:: Java
    :linenos:

    public int myAtoi(String s) {
        StringBuilder intString = new StringBuilder();
        boolean pos = true;
        int idx = 0;
        
        if (s.length()==0) {
            return 0;
        }
        
        while (idx <s.length() && s.charAt(idx)==' ') {
            idx++;
        }
        
        if (idx <s.length() && (s.charAt(idx)=='-' || s.charAt(idx)=='+')) {
            if (s.charAt(idx)=='-') {
                pos = false;
            }
            idx++;
        }
        
        HashSet<Character> digitSet = new HashSet<Character>(Arrays.asList('0', '1', '2', '3', '4', '5', '6', '7', '8', '9'));
        
        while (idx<s.length() && digitSet.contains(s.charAt(idx))) {
            intString.append(s.charAt(idx++));
        }
        
        String res = intString.toString();
        
        if (res.equals("")) {
            return 0;
        }
        
        Long resLong = pos ? Long.parseLong(res) : -Long.parseLong(res);
        
        if (resLong>Integer.MAX_VALUE) {
            return Integer.MAX_VALUE;
        } else if (resLong<Integer.MIN_VALUE) {
            return Integer.MIN_VALUE;
        } else {
            return resLong.intValue();
        }
    }

Better solution:

.. code-block:: Java
    :linenos:

    public int myAtoi(String str) {
        int index = 0;
        int total = 0;
        int sign = 1;
        
        // Check if empty string
        if(str.length() == 0)
            return 0;
        
        // remove white spaces from the string
        while(index < str.length() && str.charAt(index) == ' ')
            index++;
        
        if (index == str.length()) return 0;
        
        // get the sign
        if(str.charAt(index) == '+' || str.charAt(index) == '-') {
            sign = str.charAt(index) == '+' ? 1 : -1;
            index++;
        }
        
        // convert to the actual number and make sure it's not overflow
        while(index < str.length()) {
            int digit = str.charAt(index) - '0';
            if(digit < 0 || digit > 9) break;
            
            // check for overflow
            if(Integer.MAX_VALUE / 10 < total || Integer.MAX_VALUE / 10 == total && Integer.MAX_VALUE % 10 < digit)
                return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            
            total = total*10 + digit;
            index++; // don't forget to increment the counter
        }
        return total*sign;
    }

Remarks and Complexity Analysis: 
 * Not too familiar with overflow prevention techniques!
 * **Time Complexity**: ``O(n)`` where ``n=s.length()``.
 * **Space Complexity**: ``O(1)``

Day 40 [1 Apr]
================
Question 68: Game Tournament
------------------------------------------------
△△ 게임대회가 개최되었습니다. 이 대회는 N명이 참가하고, 토너먼트 형식으로 진행됩니다. N명의 참가자는 각각 1부터 N번을 차례대로 배정받습니다. 그리고, 1번↔2번, 3번↔4번, ... , N-1번↔N번의 참가자끼리 게임을 진행합니다. 각 게임에서 이긴 사람은 다음 라운드에 진출할 수 있습니다. 이때, 다음 라운드에 진출할 참가자의 번호는 다시 1번부터 N/2번을 차례대로 배정받습니다. 만약 1번↔2번 끼리 겨루는 게임에서 2번이 승리했다면 다음 라운드에서 1번을 부여받고, 3번↔4번에서 겨루는 게임에서 3번이 승리했다면 다음 라운드에서 2번을 부여받게 됩니다. 게임은 최종 한 명이 남을 때까지 진행됩니다.
이때, 처음 라운드에서 A번을 가진 참가자는 경쟁자로 생각하는 B번 참가자와 몇 번째 라운드에서 만나는지 궁금해졌습니다. 게임 참가자 수 N, 참가자 번호 A, 경쟁자 번호 B가 함수 solution의 매개변수로 주어질 때, 처음 라운드에서 A번을 가진 참가자는 경쟁자로 생각하는 B번 참가자와 몇 번째 라운드에서 만나는지 return 하는 solution 함수를 완성해 주세요. 단, A번 참가자와 B번 참가자는 서로 붙게 되기 전까지 항상 이긴다고 가정합니다.

My Solution: 

.. code-block:: Java
    :linenos:

    public int solution(int n, int a, int b) {
        int answer = 0;
        
        while (a!=b) {
            a= a%2==1 ? (a+1)/2 : a/2;
            b=b%2==1 ? (b+1)/2 : b/2;
            answer++;
        }
        return answer;
    }
    
Day 41 [3 Apr]
================
Question 69: Valid Anagram
------------------------------------------------
Given two strings s and t, return true if t is an anagram of s, and false otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

My Solution: 

.. code-block:: Java
    :linenos:

    public boolean isAnagram(String s, String t) {
        if (s.length()!=t.length()) return false;
        
        int charAtoInt = (int) 'a'; 
        int[] charCount = new int[26]; 
        
        for (char c : s.toCharArray()) {
            charCount[c-charAtoInt]++;
        }
        for (char c : t.toCharArray()) {
            charCount[c-charAtoInt]--;
        }
        for (int i : charCount) {
            if (i!=0) return false;
        }
        return true;
    }

Remarks and Complexity Analysis: 
 * Easy question! 
 * Thinking about time complexity is useful while brainstorming the solution.
 * **Time Complexity**: ``O(n)`` where ``n=s.length()=t.length()``.
 * **Space Complexity**: ``O(1)``



