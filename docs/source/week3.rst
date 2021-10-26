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
Question 33: XXX
--------------------------------
