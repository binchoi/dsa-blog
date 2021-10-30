************************
Week 3 [25-31 Oct 2021]
************************
Day 13 [25 Oct]
================
Question 31: Word Break
--------------------------------
*Given a string ``s`` and a dictionary of strings ``wordDict``, return true if ``s``` can be segmented into a 
space-separated sequence of one or more dictionary words. Note that the same word in the dictionary may be 
reused multiple times in the segmentation.*

My failed attempts:: 
    
    def __init__(self): 
        self.res = False       

    def wordBreakHelper(self, remainingStr: str, wordDict: List[str]):
        if remainingStr.replace("|", "") == "": 
            self.res = True
            return
        else: 
            for word in wordDict: 
                if self.res: 
                    return
                foundIdx = remainingStr.find(word)
                if foundIdx == -1: 
                    continue
                self.wordBreakHelper((remainingStr[:foundIdx] + "|"+ remainingStr[foundIdx+ len(word):]), wordDict)
                        
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:        
        self.wordBreakHelper(s, wordDict)
        return self.res
        
    # second fail
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:        
        res = []
        def wordBreakHelper(remainingStr: str): 
            if remainingStr == "": 
                res.append(True)
                return
            else: 
                cands = [word for word in wordDict if remainingStr.find(word)==0]
                if cands == []: 
                    res.append(False)
                    return
                for c in cands: 
                    wordBreakHelper(remainingStr[len(c):])
        wordBreakHelper(s)
        return any(res)

.. note:: 

    Lexical Scoping in Python is different from that found in other programming languages. I realized this when a function call of the 
    ``wordBreakHelper()`` would not recognize ``res`` yet allows ``res.append()``. For more information, consider reading some articles 
    about Python's `scope and closure <https://medium.com/@dannymcwaves/a-python-tutorial-to-understanding-scopes-and-closures-c6a3d3ba0937>`_ 
    and its `LEGB Rule <https://medium.com/@ahmedfgad/python-scopes-and-the-legb-rule-d37bf3277e2c>`_. 

LeetCode's brute-force/backtracking solution:: 

    # Python
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        def wordBreakRecur(s: str, word_dict: Set[str], start: int):
            if start == len(s):
                return True
            for end in range(start + 1, len(s) + 1):
                if s[start:end] in word_dict and wordBreakRecur(s, word_dict, end):
                    return True
            return False

        return wordBreakRecur(s, set(wordDict), 0)

.. code-block:: Java

    // Java
    public boolean wordBreak(String s, List<String> wordDict) {
        return wordBreakRecur(s, new HashSet<>(wordDict), 0);
    }

    private boolean wordBreakRecur(String s, Set<String> wordDict, int start) {
        if (start == s.length()) {
            return true;
        }
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordDict.contains(s.substring(start, end)) && wordBreakRecur(s, wordDict, end)) {
                return true;
            }
        }
        return false;
    }

.. tip:: 

    Note how the backtracking method (in the if-branch in the for-loop) exits the recursive stack (curr_condition and recursive_call => True). ``wordDict`` is provided 
    as a parameter for the helper function simply because the program makes the given list in to a set prior to giving it to the helper function.  

**Both of the solutions above also fail with 'Time Limit Exceeded'.**

LeetCode's Recursion with memoization:: 

    # Python
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        @lru_cache
        def wordBreakMemo(s: str, word_dict: FrozenSet[str], start: int):
            if start == len(s):
                return True
            for end in range(start + 1, len(s) + 1):
                if s[start:end] in word_dict and wordBreakMemo(s, word_dict, end):
                    return True
            return False

        return wordBreakMemo(s, frozenset(wordDict), 0)

.. code-block:: Java

    // Java
    public boolean wordBreak(String s, List<String> wordDict) {
        return wordBreakMemo(s, new HashSet<>(wordDict), 0, new Boolean[s.length()]);
    }

    private boolean wordBreakMemo(String s, Set<String> wordDict, int start, Boolean[] memo) {
        if (start == s.length()) {
            return true;
        }
        if (memo[start] != null) {
            return memo[start];
        }
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordDict.contains(s.substring(start, end)) && wordBreakMemo(s, wordDict, end, memo)) {
                return memo[start] = true;
            }
        }
        return memo[start] = false;
    }

.. note:: 

    Implementation (python) did not change much at all except for two things. 
    
    1. Addition of ``@lru_cache`` annotation to 
    the helper function: The 'Least Recently Used Cache' (i.e. LRU cache) annotation **keeps references to the arguments 
    and return values** (of the annotated function) until they age out of the cache or until the cache is cleared. 
    The LRU cache works best when the most recent calls are the best predictors of upcoming calls. 

    2. replacing ``set()`` with ``frozenset()``: The ``frozenset()`` function returns an immutable frozenset object initialized 
    with elements from the given iterable. While elements of a set can be modified at any time, elements of the frozen set 
    remain the same after creation. Methods of ``frozenset`` type include ``fs1.union(fs2)``, ``fs1.intersection(fs2)``, 
    ``fs1.difference(fs2)``, ``fs1.isdisjoint(fs2)``, and ``fs1.issubset(fs2)``. The transition from ``set`` to ``frozenset`` was made 
    as ``@lru_cache`` **cannot function in the presence of mutable data-type arguments** as they are not hashable. Hence, when 
    using the above annotation, one must try to transform the datatypes of the arguments into their frozen counterparts. 

Remarks and Complexity Analysis: 
 * **Time Complexity**: ``O(n^3)`` - size of recursion tree can reach ``n^2``.
 * **Space Complexity**: ``O(n)`` - the depth of recursion tree can reach ``n``.

LeetCode's Breadth-First-Search Solution:: 

    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        word_set = set(wordDict)
        q = deque()
        visited = set()

        q.append(0)
        while q:
            start = q.popleft()
            if start in visited:
                continue
            for end in range(start + 1, len(s) + 1):
                if s[start:end] in word_set:
                    q.append(end)
                    if end == len(s):
                        return True
                visited.add(start)
        return False

.. code-block:: Java

    // Java
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet = new HashSet<>(wordDict);
        Queue<Integer> queue = new LinkedList<>();
        boolean[] visited = new boolean[s.length()];
        queue.add(0);
        while (!queue.isEmpty()) {
            int start = queue.remove();
            if (visited[start]) {
                continue;
            }
            for (int end = start + 1; end <= s.length(); end++) {
                if (wordDictSet.contains(s.substring(start, end))) {
                    queue.add(end);
                    if (end == s.length()) {
                        return true;
                    }
                }
            }
            visited[start] = true;
        }
        return false;
    }

Remarks and Complexity Analysis: 
 * **Time Complexity**: ``O(n^3)`` - For every starting index, the search can continue till the end of the given string.
 * **Space Complexity**: ``O(n)`` - Queue of at most ``n`` size is needed.

LeetCode's Dynamic Programming Approach:: 

    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        word_set = set(wordDict)
        dp = [False] * (len(s) + 1)
        dp[0] = True

        for i in range(1, len(s) + 1):
            for j in range(i):
                if dp[j] and s[j:i] in word_set:
                    dp[i] = True
                    break
        return dp[len(s)]

.. code-block:: Java

    // Java
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet = new HashSet<>(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && wordDictSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }

Remarks and Complexity Analysis: 
 * Incredible how concise these implementations are. (re-do soon!)
 * **Time Complexity**: ``O(n^3)`` - There are two nested loops, and substring computation at each iteration.
 * **Space Complexity**: ``O(n)`` - Length of ``p`` array is ``n+1``

Question 32: Same Tree
--------------------------------
*Given the roots of two binary trees ``p`` and ``q``, write a function to check if they are the same or not. 
Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.*

My recursive solution: 

.. code-block:: python
    :linenos:
        
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
            if p == q: 
                return True
            if p is None or q is None:
                return False
            if (p.val == q.val) & self.isSameTree(p.left, q.left) & self.isSameTree(p.right, q.right): 
                return True
            else: 
                return False

    # Made more concise (inspired by Stefan)
    def isSameTree(self, p, q):
        if p and q:
            return p.val == q.val and self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
        return p is q

Remarks and Complexity Analysis: 
 * Simple, easy-to-comprehend algorithm (not that elegant). Since it is recursive, rather than iterative, it must be more 
   expensive due to the additional overhead of method calls. 
 * **Time Complexity**: ``O(n)`` where ``n=size of the smaller tree``
 * **Space Complexity**: ``O(1)`` (or ``O(log n)`` average case and ``O(n)`` worst case) - need to revise space complexity!

LeetCode's recursive solution:: 

    def isSameTree(self, p, q):
        # p and q are both None
        if not p and not q:
            return True
        # one of p and q is None
        if not q or not p:
            return False
        if p.val != q.val:
            return False
        return self.isSameTree(p.right, q.right) and \
               self.isSameTree(p.left, q.left)

.. code-block:: Java

    // Java      
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // p and q are both null
        if (p == null && q == null) return true;
        // one of p and q is null
        if (q == null || p == null) return false;
        if (p.val != q.val) return false;
        return isSameTree(p.right, q.right) &&
                isSameTree(p.left, q.left);
    }


LeetCode's iterative solution:: 

    from collections import deque
    def isSameTree(self, p, q):
        def check(p, q):
            # if both are None
            if not p and not q:
                return True
            # one of p and q is None
            if not q or not p:
                return False
            if p.val != q.val:
                return False
            return True
        
        deq = deque([(p, q),])
        while deq:
            p, q = deq.popleft()
            if not check(p, q):
                return False
            
            if p:
                deq.append((p.left, q.left))
                deq.append((p.right, q.right))
        return True

.. note:: 

    Need to review ``deque`` from ``collections``. 

.. code-block:: Java

    // Java      
    public boolean check(TreeNode p, TreeNode q) {
        // p and q are null
        if (p == null && q == null) return true;
        // one of p and q is null
        if (q == null || p == null) return false;
        if (p.val != q.val) return false;
        return true;
    }

    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) return true;
        if (!check(p, q)) return false;

        // init deques
        ArrayDeque<TreeNode> deqP = new ArrayDeque<TreeNode>();
        ArrayDeque<TreeNode> deqQ = new ArrayDeque<TreeNode>();
        deqP.addLast(p);
        deqQ.addLast(q);

        while (!deqP.isEmpty()) {
        p = deqP.removeFirst();
        q = deqQ.removeFirst();

        if (!check(p, q)) return false;
        if (p != null) {
            // in Java nulls are not allowed in Deque
            if (!check(p.left, q.left)) return false;
            if (p.left != null) {
            deqP.addLast(p.left);
            deqQ.addLast(q.left);
            }
            if (!check(p.right, q.right)) return false;
            if (p.right != null) {
            deqP.addLast(p.right);
            deqQ.addLast(q.right);
            }
        }
        }
        return true;
    }

Alternative iterative solution (cred: `home <https://leetcode.com/problems/same-tree/solution/>`_):

.. code-block:: Java

    // Java
    public boolean isSameTree(TreeNode p, TreeNode q) {
        Queue<TreeNode> queue = new LinkedList<>();
        if (p == null && q == null)
            return true;
        else if (p == null || q == null)
            return false;
        if (p != null && q != null) {
            queue.offer(p);
            queue.offer(q);
        }
        while (!queue.isEmpty()) {
            TreeNode first = queue.poll();
            TreeNode second = queue.poll();
            if (first == null && second == null)
                continue;
            if (first == null || second == null)
                return false;
            if (first.val != second.val)
                return false;
            queue.offer(first.left);
            queue.offer(second.left);
            queue.offer(first.right);
            queue.offer(second.right);
        }
        return true;
    }

Day 14 [26 Oct]
================
Question 33: K-th Symbol in Grammar
-------------------------------------
*We build a table of* ``n`` *rows (1-indexed). We start by writing* ``0`` *in the 1st row. Now in every subsequent row, we look at the previous row 
and replace each occurrence of* ``0`` *with* ``01``  *, and each occurrence of* ``1``  *with* ``10`` *. For example, for* ``n = 3`` *, the 1st row is* ``0`` *, the 
2nd row is* ``01`` *, and the 3rd row is* ``0110`` *. Given two integer* ``n`` *and* ``k`` *, return the kth (1-indexed) symbol in the nth row of a table of n rows.*

My recursive solution (inspired by `grandyang <https://leetcode.com/problems/k-th-symbol-in-grammar/discuss/113697/My-3-lines-C%2B%2B-recursive-solution>`_):

.. code-block:: python
    :linenos: 

    def kthGrammar(self, n: int, k: int) -> int:
        if (n == 1 and k == 1): 
            return 0
        # else we determine by considering the parent's value and 
        # knowing if node is left/right branch
        elif (k%2==0): # even => right child
            return 1 if (self.kthGrammar(n-1, k/2)==0) else 0
        else: # odd => left child
            return 0 if (self.kthGrammar(n-1, (k+1)/2)==0) else 1

Remarks and Complexity Analysis: 
 * Initially, I tried to write down the first few rows in hopes of finding a pattern that will allow me to implement a constant time 
   solution. Such pattern did not exist (or, more accurately, I was unable to identify it).
 * The key to the solution above is realizing that each row can be represented as a level in a binary tree. One could notice this 
   from how the number of digits/nodes double in every successive row. Furthermore, the second key lies in understanding that 
   the ``k`` parameter provides us with the information of whether the node is a left or right child (from the parity of 1-indexed ``k`` parameter). 
   Finally, knowing that a ``0``-parent node has a ``0`` left-child and a ``1`` right-child (while vice versa for ``1``-parent), we can then deduce the value 
   of a given node by the identity of its parent node -- *Recursion*!
 * **Time Complexity**: ``O(n)`` - we would make ``n-1`` recursive calls for any parameter ``()n, k)``. 
 * **Space Complexity**: ``O(1)`` - (uncertain!)

.. note:: 

    A *further observation* would have made the algorithm more concise and efficient: Realize that the parity of ``k`` (in the main and recursive calls of 
    ``kthGrammar``) is the critical information that determines the output of our function. If we were to trace our value at ``(n,k)`` from the root 
    node, we realize that our value *switches* every time we move down to the right-child (from ``0`` => ``1`` or ``1`` => ``0``).  Hence, given parameter ``k``, 
    we can repeatedly (ceil) divide it while keeping track of the number of times it is odd (i.e. right-child) before we reach 0. If we find this number to be, say ``y``, 
    we can simply switch our initial value 0 ``y`` times to compute our output (i.e. return ``y%2``). This process can be simplified by converting our decimal value ``k`` 
    into its binary counterpart (! due to indexing, we convert ``k-1`` !) which are conveniently base-2. 

    For instance, for ``k=4``, the binary conversion (of 3) would be ``b'11'``. This is exactly the route that we take to reach ``k=4`` (regardless of ``n``) - i.e. only two 
    right-child path is taken. Granted there could be a long trail of left-child nodes (e.g. for params: ``(100,4)``), but there will only be exactly two right-child path taken 
    - i.e. exactly two *switches* (``0`` => ``1`` => ``0``). Hence, for any ``k``, we can simply find the binary representation of ``k-1`` and count the number of ``1``'s in it, 
    finally return its modulo 2. 

    Learn more about Python's bit-wise operators `here <https://realpython.com/python-bitwise-operators/>`_!

Sample solution (cred: `tyuan <https://leetcode.com/problems/k-th-symbol-in-grammar/discuss/113705/JAVA-one-line>`_): 

.. code-block:: Java

    // Java
    public int kthGrammar(int N, int K) {
        return Integer.bitCount(K-1) & 1;
    }

My adaptation:: 

    def countSetBits(self,n):
        count = 0
        while (n):
            count += n & 1
            n >>= 1
        return count
    
    def kthGrammar(self, n: int, k: int) -> int:
        return self.countSetBits(k-1) % 2

Day 15 [27 Oct]
================
Question 34: Remove Duplicates from Sorted List
------------------------------------------------
*Given the head of a sorted linked list, delete all duplicates such that each element appears only 
once. Return the linked list sorted as well.*

My iterative solution: 

.. code-block:: python
    :linenos: 

    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        headNode = head
        if headNode is None: 
            return headNode
    
        prevNode = head
        currNode = head.next
        while currNode is not None: 
            if prevNode.val == currNode.val: 
                prevNode.next = currNode.next
            else: 
                prevNode = currNode
            currNode = currNode.next
        return headNode

Remarks and Complexity Analysis: 
 * The solution above is the first implementation that came to mind. It's quite simple (and far from *incisive*), but 
   fairly efficient and easy-to-understand. 
 * **Time Complexity**: ``O(n)`` - where ``n=len(linked_list)`` as all nodes have to be traversed once.
 * **Space Complexity**: ``O(1)`` - the only memory we store is the object ``prevNode`` (which is updated as neccessary)

Similar Java solution (cred: `san89 <https://leetcode.com/problems/remove-duplicates-from-sorted-list/discuss/28614/My-pretty-solution.-Java.>`_):

.. code-block:: Java
    
    public ListNode deleteDuplicates2(ListNode head) {
        if(head == null || head.next == null) return head;
        
        ListNode list = head;
         while(list.next != null) 
         {
             if (list.val == list.next.val)
                 list.next = list.next.next;
             else 
                 list = list.next;
         }

         return head;
    }

Question 35: Pascal's Triangle
------------------------------------------------
*Given an integer numRows, return the first numRows of Pascal's triangle. In Pascal's triangle, each number 
is the sum of the two numbers directly above it.*

My iterative solution: 

.. code-block:: python
    :linenos: 

    import math
    def getNthRow(self, nRow: int) -> List[int]: 
        def calcBinom(nVal, rVal): 
            return int((math.factorial(nVal))/(math.factorial(rVal)*math.factorial(nVal-rVal)))
        res = [] 
        for i in range(nRow): 
            res.append(calcBinom(nRow-1, i)) # nVal = nRow - 1 => b/c first row nVal=0
        return res
    
    def generate(self, numRows: int) -> List[List[int]]:
        res = []
        for i in range(1,numRows+1): 
            res.append(self.getNthRow(i))
        return res

Remarks and Complexity Analysis: 
 * My concern is whether the use of the Binomial Theorem (which involves factorials) instead of memoization simplifies the algorithm at the cost of 
   time complexity. My understanding is that ``math.factorial`` is implemented using 
   lower-level languages like C (and by result, is faster) - how would I describe the time complexity of algorithms which use such 
   features?
 * **Time Complexity**: ``O(n^2)`` - where ``n=numRows`` thanks to the nested for-loops.
 * **Space Complexity**: ``O(n^2)`` - (uncertain)

Alternative solution (cred: `rheaxu <https://leetcode.com/problems/pascals-triangle/discuss/38141/My-concise-solution-in-Java>`_): 

.. code-block:: Java

    public List<List<Integer>> generate(int numRows)
    {
        List<List<Integer>> allrows = new ArrayList<List<Integer>>();
        ArrayList<Integer> row = new ArrayList<Integer>();
        for(int i=0;i<numRows;i++)
        {
            row.add(0, 1);
            for(int j=1;j<row.size()-1;j++)
                row.set(j, row.get(j)+row.get(j+1));
            allrows.add(new ArrayList<Integer>(row));
        }
        return allrows;
    }

Another very creative solution (cred: `sherlock <https://leetcode.com/problems/pascals-triangle/discuss/38128/Python-4-lines-short-solution-using-map.>`_)::

    def generate(self, numRows):
        res = [[1]]
        for i in range(1, numRows):
            res += [map(lambda x, y: x+y, res[-1] + [0], [0] + res[-1])]
        return res[:numRows]

The reason the above algorithm works is that the offset sum of the previous pascal row can directly produce the next pascal row (i.e. ``1 3 3 1 0`` + ``0 1 3 3 1`` => ``1 4 6 4 1``). 


Day 16 [28 Oct]
================
Question 35: XXXXXX
-----------------------