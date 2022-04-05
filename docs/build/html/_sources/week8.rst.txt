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

Day 42 [4 Apr]
================
Question 70: Marathon
------------------------------------------------
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.
마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

제한사항
 * 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
 * completion의 길이는 participant의 길이보다 1 작습니다.
 * 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
 * 참가자 중에는 동명이인이 있을 수 있습니다.


My Solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {
        public String solution(String[] participant, String[] completion) {
            HashMap<String,Integer> partCount = new HashMap<>();
            // iterate through all participants
            for (String s:participant) {
                partCount.put(s,partCount.getOrDefault(s,0)+1);
            }
            // iterate through all completers
            for (String s:completion) {
                partCount.put(s,partCount.get(s)-1);
                if (partCount.get(s)==0) partCount.remove(s);
            }
        
            return partCount.isEmpty() ? "" : partCount.keySet().iterator().next();
        }
    }

Remarks and Complexity Analysis: 
 * Easy question
 * Time complexity is our friend.
 * **Time Complexity**: ``O(n)`` where ``n=participant.size()``.
 * **Space Complexity**: ``O(n)``

Question 71: Phone Book
------------------------------------------------
전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

제한 사항
 * phone_book의 길이는 1 이상 1,000,000 이하입니다.
 * 각 전화번호의 길이는 1 이상 20 이하입니다.
 * 같은 전화번호가 중복해서 들어있지 않습니다.

My Solution: 

.. code-block:: Java
    :linenos:

    import java.util.HashSet;
    import java.util.HashMap;
    import java.util.Arrays;

    class Solution {
        public boolean solution(String[] phone_book) {
            HashMap<Integer, HashSet<String>> prefixCandsByLen = new HashMap<>();
            Arrays.sort(phone_book, (a,b) -> Integer.compare(a.length(), b.length()));
            
            for (String num : phone_book) {
                for (int i : prefixCandsByLen.keySet()) {
                    if (prefixCandsByLen.get(i).contains(num.substring(0,i))) return false;
                }
                HashSet<String> prefixSet = prefixCandsByLen.getOrDefault(num.length(), new HashSet<String>());
                prefixSet.add(num);
                prefixCandsByLen.put(num.length(), prefixSet);            
            }
            return true;
        }
    }

Remarks and Complexity Analysis: 
 * Not bad at all -- but I wonder if there is a better way to implement this (e.g. prefix trie?)
 * **Time Complexity**: ``O(n^2)`` where ``n=phone_book.length``.
 * **Space Complexity**: ``O(n)``

Question 72: Spy Wardrobe
------------------------------------------------
스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.
예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

제한 사항
 * clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
 * 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
 * 같은 이름을 가진 의상은 존재하지 않습니다.
 * clothes의 모든 원소는 문자열로 이루어져 있습니다.
 * 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
 * 스파이는 하루에 최소 한 개의 의상은 입습니다.


My Solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;
    class Solution {
        public int solution(String[][] clothes) {
            HashMap<String,Integer> typeCount = new HashMap<>();
            for (String[] itemAndType : clothes) {
                typeCount.put(itemAndType[1], typeCount.getOrDefault(itemAndType[1],1)+1);
            }
            int res = 1;
            for (int i : typeCount.values()) {
                res*=i;
            }
            return res-1;
        }

        // Alternatively, using reduce (similar to fold)
        public int solution(String[][] clothes) {
            HashMap<String,Integer> typeCount = new HashMap<>();
            for (String[] itemAndType : clothes) {
                typeCount.put(itemAndType[1], typeCount.getOrDefault(itemAndType[1],1)+1);
            }
            return typeCount.values()
                .stream()
                .reduce(1,(acc, i) -> acc*i) - 1;
        }

        // alternatively, relying heavily on stream
        public int solution(String[][] clothes) {
            return Arrays.stream(clothes)
                    .collect(groupingBy(p -> p[1], mapping(p -> p[0], counting())))
                    .values()
                    .stream()
                    .collect(reducing(1L, (x, y) -> x * (y + 1))).intValue() - 1;
        }
    }
    
Remarks and Complexity Analysis: 
 * The key was to realize that the problem is much easier when we reduce our scenario into a simpler one and then modify the result 
   afterwards (i.e. include notWearing as one option for each group, compute all the possible combinations where we take one 
   member from each of the groups, then subtract one to account for the non-valid zero clothes option)
 * **Time Complexity**: ``O(n^2)`` where ``n=phone_book.length``.
 * **Space Complexity**: ``O(n)``

Day 43 [5 Apr]
================
Question 73: Kakao Lawsuit
------------------------------------------------
신입사원 무지는 게시판 불량 이용자를 신고하고 처리 결과를 메일로 발송하는 시스템을 개발하려 합니다. 무지가 개발하려는 시스템은 다음과 같습니다.
각 유저는 한 번에 한 명의 유저를 신고할 수 있습니다.
신고 횟수에 제한은 없습니다. 서로 다른 유저를 계속해서 신고할 수 있습니다.
한 유저를 여러 번 신고할 수도 있지만, 동일한 유저에 대한 신고 횟수는 1회로 처리됩니다.
k번 이상 신고된 유저는 게시판 이용이 정지되며, 해당 유저를 신고한 모든 유저에게 정지 사실을 메일로 발송합니다.
유저가 신고한 모든 내용을 취합하여 마지막에 한꺼번에 게시판 이용 정지를 시키면서 정지 메일을 발송합니다.
다음은 전체 유저 목록이 ["muzi", "frodo", "apeach", "neo"]이고, k = 2(즉, 2번 이상 신고당하면 이용 정지)인 경우의 예시입니다.


My Solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {
        public int[] solution(String[] id_list, String[] report, int k) {
            int[] res = new int[id_list.length];
            
            HashMap<String, Integer> idToIndex = new HashMap<>();
            HashMap<String, Set<String>> idToSuerSet = new HashMap<>();
            
            for (int i = 0; i<id_list.length ; i++) {
                idToIndex.put(id_list[i], i);
                idToSuerSet.put(id_list[i], new HashSet<String>());
            }
            
            for (String suerToSued : report) {
                idToSuerSet.get(suerToSued.split(" ")[1]).add(suerToSued.split(" ")[0]);
            }
            
            for (String id : idToSuerSet.keySet()) {
                if (idToSuerSet.get(id).size() >= k) {
                    for (String suer : idToSuerSet.get(id)) {
                        res[idToIndex.get(suer)] += 1;
                    }
                }
            }
            return res;
        }
    }