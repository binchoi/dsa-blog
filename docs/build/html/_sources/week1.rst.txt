************************
Week 1 [13-17 Oct 2021]
************************

Day 1 [13 Oct]
========================
Question 1: Two Sum
--------------------
*Given an array of integers nums and an integer target, return indices of 
the two numbers such that they add up to target.*

My initial solution: O(n^2)
Optimal solution:

*option 1 (using dictionary)*

.. code-block:: python
   :linenos:

    class Solution(object):
        def twoSum(self, nums, target):
            buffer_dictionary = {}
            for i in rangenums.__len()):
                if nums[i] in buffer_dictionary:
                    return [buffer_dictionary[nums[i]], i] 
                else: 
                    buffer_dictionary[target - nums[i]] = i 

*option 2 (more concise)*

.. code-block:: python
   :linenos:

    class Solution(object):
        def twoSum(self, nums, target):
            dic = {}
            for i, n in enumerate(nums): 
                if n in dic:
                    return [dic[n], i]
                dic[target-n] = i


.. note::
    The difference between ``hashtable`` and ``dict`` datatypes (i.e. ``dict`` is larger umbrella)
    
    Increased time optimization comes at the cost of space complexity (this is preferable as memory is cheaper than time) 

Question 2: Valid Parentheses
-------------------------------
*Given a string s containing just the characters '(', ')', '{', '}', 
'[' and ']', determine if the input string is valid.*

My solution: O(n)
 * using stack and a helper function that compares a open parenthesis with a closed one

.. tip::
    No need to create class for stack when coding in Python (just use list operators)! 
    
    You can use data structures like dictionaries and hashtables instead of helper functions to increase efficiency! 

Question 3: Search Insert Position
-----------------------------------
*Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, 
return the index where it would be if it were inserted in order.*

*You must write an algorithm with O(log n) runtime complexity.*

My solution:

.. code-block:: python
   :linenos:

    def searchInsert(self, nums: List[int], target: int) -> int:
        l , r = 0, len(nums)-1
        while l <= r:
            mid=(l+r)//2
            if nums[mid] < target:
                l = mid+1
            else:
                if nums[mid]== target and nums[mid-1]!=target:
                    return mid
                else:
                    r = mid-1
        return l


* Struggled a lot for some reason
* Regular binary search but nuances/details are critical

**Take-aways**
 * Review binary search (trees)! 



Day 2 [14 Oct]
========================
Question 4: Pow(x,n)
---------------------
*Implement pow(x, n), which calculates x raised to the power n*

Notes: 
 * Was a little too simple than expected

My solution [O(1)]:

.. code-block:: python
   :linenos:

    class Solution(object):
        def myPow(self, x: float, n: int) -> float:
            return x**n

Two more do-it-myself solutions (Python):

*Recursive*::
    
    class Solution:
        def myPow(self, x, n):
            if not n:
                return 1
            if n < 0:
                return 1 / self.myPow(x, -n)
            if n % 2:
                return x * self.myPow(x, n-1)
            return self.myPow(x*x, n/2)

*Iterative*::

    class Solution:
        def myPow(self, x, n):
            if n < 0:
                x = 1 / x
                n = -n
            pow = 1
            while n:
                if n & 1:
                    pow *= x
                x *= x
                n >>= 1
            return pow


Solution in Java (for future reference)::

    public class Solution {
        public double pow(double x, int n) {
            if(n == 0)
                return 1;
            if(n<0){
                n = -n;
                x = 1/x;
            }
            return (n%2 == 0) ? pow(x*x, n/2) : x*pow(x*x, n/2);
        }
    }

.. tip::
    Python is an efficient language for coding tests!

Question 5: Maximum Subarray
-----------------------------
*Given an integer array nums, find the contiguous subarray (containing at least one number) 
which has the largest sum and return its sum.*

*A subarray is a contiguous part of an array.*

Notes: 
* Implemented using the classic, basic form of Maximum Sub-array problem
* I doubted if I fully understand the algorithm at heart but now I feel more comfortable with it

My solution [O(n log(n))]:

.. code-block:: python
   :linenos:

    def maxCrossingSubArray(nums, low, mid, high): 
        lsum = -float('inf')
        sum = 0
        for i in range(mid, low-1, -1): 
            sum = sum + nums[i]
            if sum > lsum: 
                lsum = sum 
                maxLIdx = i
        rsum = -float('inf')
        sum = 0
        for i in range(mid+1, high+1): 
            sum = sum + nums[i]
            if sum > rsum: 
                rsum = sum 
                maxRIdx = i
        return (maxLIdx, maxRIdx, lsum+rsum)

    def maxSubArrayHelper(nums: List[int], low, high):
        if high == low:
            return (low, high, nums[low])
        else: 
            mid = (low + high) // 2
            (lLow, lHigh, lsum) = maxSubArrayHelper(nums, low, mid)
            (rLow, rHigh, rsum) = maxSubArrayHelper(nums, mid+1, high)
            (cLow, cHigh, csum) = maxCrossingSubArray(nums, low, mid, high)
            if lsum >= rsum and lsum >= csum: 
                return (lLow, lHigh, lsum)
            elif lsum <= rsum and rsum >= csum: 
                return (rLow, rHigh, rsum) 
            else: 
                return (cLow, cHigh, csum) 
                
    class Solution:
        def maxSubArray(self, nums: List[int]) -> int:
            (low, high, maxSum) = maxSubArrayHelper(nums, 0, len(nums)-1)
            return maxSum

My Python adaptation of a O(n) solution (credit: `cbmbbz <https://leetcode.com/problems/maximum-subarray/discuss/20211/Accepted-O(n)-solution-in-java>`_)::

    class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        maxSoFar = nums[0]
        maxIncludingThis = nums[0]
        for i in range(1,len(nums)): 
            # maxIncludingThis added not maxSoFar b/c contiguous sub-array
            maxIncludingThis = max(maxIncludingThis + nums[i], nums[i]) 
            maxSoFar = max(maxSoFar, maxIncludingThis)
        return maxSoFar


Kadane's algorithm (Python):: 

    for i in range(1, len(nums)):
        if nums[i-1] > 0:
            nums[i] += nums[i-1]
    return max(nums)


Java solution (for later) by `FujiwaranoSai <https://leetcode.com/problems/maximum-subarray/discuss/20193/DP-solution-and-some-thoughts>`_::

    public int maxSubArray(int[] A) {
            int n = A.length;
            int[] dp = new int[n];//dp[i] means the maximum subarray ending with A[i];
            dp[0] = A[0];
            int max = dp[0];
            
            for(int i = 1; i < n; i++){
                dp[i] = A[i] + (dp[i - 1] > 0 ? dp[i - 1] : 0);
                max = Math.max(max, dp[i]);
            }
            
            return max;
    }

.. tip::
    Critically analyze the problem and search for ways to optimize the solution (efficiency is key)! Dynamic Programming 
    is a wonderful tool to increase the efficiency of many algorithms.

Question 6: Is Subsequence
-----------------------------
*Given two strings s and t, return true if s is a subsequence of t, or false otherwise.*

*A subsequence of a string is a new string that is formed from the original string by 
deleting some (can be none) of the characters without disturbing the relative positions 
of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).*

My quick solution [O(n) where n is len(t)]: 

.. code-block:: python
   :linenos:

    class Solution:
        def isSubsequence(self, s: str, t: str) -> bool:
            tList = list(t)
            tIdx = 0
            for charS in list(s):
                if tIdx == len(tList): 
                        return False
                while tList[tIdx] != charS:
                    tIdx += 1
                    if tIdx == len(tList): 
                        return False
                tIdx += 1
            return True

* Doesn't feel as elegant or organized as I hope

Really short solution (cred: `StefanPochmann <https://leetcode.com/problems/is-subsequence/discuss/87258/2-lines-Python>`_)::

    def isSubsequence(self, s, t):
        t = iter(t)
        return all(c in t for c in s)
             
.. tip::
    ``Iter()`` returns an iterator for a given object (e.g. list, sets, tuples, ...). 
    More on `iterator type <https://www.mygreatlearning.com/blog/iterator-in-python/>`_. 
    In our example, it is like a list whose elements we can see one-by-one (and we have to discard it after seeing it)

    ``all()`` takes an *iterable* (e.g. list, tuple, dictionary, iterator, etc...) and returns True if all items in the 
    iterable are True, else it returns False. Its counterpart is ``any()``, which returns True if any item in an iterable is True, else it returns False.


Day 3 [15 Oct]
========================
Question 7: Path Sum
---------------------
*Given the root of a binary tree and an integer targetSum, return true if the tree 
has a root-to-leaf path such that adding up all the values along the path equals targetSum. A leaf is a node with no children.*

Remarks: 
 * I'm realizing the importance of spending more time thinking about the algorithm and problem and not rushing to code the solution
 * Solution took a lots of tries to get right

.. code-block:: python
    :linenos:

    class Solution:
        def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
            if (root is None): 
                return False # in case else-branch called on leaf-node
            elif (targetSum == root.val and not root.left and not root.right): 
                return True # leaf node reached with pathSum==targetSum ; key=leaf node has no children
            else: 
                return self.hasPathSum(root.left, targetSum-root.val) or self.hasPathSum(root.right, targetSum-root.val)

.. tip::
    Reading and understanding every definition and instruction is critical to problem solving (e.g. 'A leaf is a node with no children'). 

    Think about test cases first before attempting to devise a solution! 

    "Premature optimization is the root of all evil" - Donald Knuth

Different Approaches (cred: `OldCodingFarmer <https://leetcode.com/problems/path-sum/discuss/36486/Python-solutions-(DFS-recursively-DFS%2Bstack-BFS%2Bqueue)>`_)::

    # Recursive
    def hasPathSum1(self, root, sum):
        if not root:
            return False
        if not root.left and not root.right and root.val == sum:
            return True
        return self.hasPathSum(root.left, sum-root.val) or self.hasPathSum(root.right, sum-root.val)
    
    # DFS + stack   
    def hasPathSum(self, root, sum):
        stack = [(root, sum)]
        while stack:
            node, value = stack.pop()
            if node:
                if not node.left and not node.right and node.val == value:
                    return True
                stack.append((node.right, value-node.val))
                stack.append((node.left, value-node.val))
        return False

    # BFS with queue
    def hasPathSum(self, root, sum):
        if not root:
            return False
        queue = [(root, sum-root.val)]
        while queue:
            curr, val = queue.pop(0)
            if not curr.left and not curr.right and val == 0:
                return True
            if curr.left:
                queue.append((curr.left, val-curr.left.val))
            if curr.right:
                queue.append((curr.right, val-curr.right.val))
        return False

* May be slightly over-engineered, but for reference!

Question 8: Best Time to Buy and Sell Stock
--------------------------------------------
*You are given an array prices where ``prices[i]`` is the price of a given stock on the ith day. You want to maximize your 
profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.*

*Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.*

Remarks: 
 * Much easier after having done :ref:`Question 5: Maximum Subarray`. 
 * I used one of the optimized maximum subarray algorithms (Kadane's algorithm) I read in the discussion forum

.. code-block:: python
    :linenos:

    class Solution:
        def maxProfit(self, prices: List[int]) -> int:
            # no profit can be gained from one or zero days
            if len(prices) <= 1: 
                return 0

            #transform list -> change in price
            priceChange = [0] # initialize -> no profit
            for i in range(len(prices)-1): 
                priceChange.append(prices[i+1] - prices[i])
            
            maxSoFar = priceChange[0]
            maxEndingHere = priceChange[0]
            for delta in priceChange[1:]: 
                maxEndingHere = max(maxEndingHere + delta, delta)
                maxSoFar = max(maxEndingHere, maxSoFar)
            return maxSoFar
            # day to buy and sell, could be recorded and returned too

Using Kadane's algorithm (adapted):: 

    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) <= 1: 
            return 0
        priceChange = [0] # initialize -> no profit
        for i in range(len(prices)-1): 
            priceChange.append(prices[i+1] - prices[i])
        
        # Kadaane's algorithm    
        for i in range(1,len(priceChange)): 
            if priceChange[i-1] > 0: 
                priceChange[i] += priceChange[i-1]
        return max(priceChange)

Question 9: Reverse Linked List
-------------------------------------
*Given the head of a singly linked list, reverse the list, and return the reversed list.* 

*Follow up: A linked list can be reversed either iteratively or recursively. Could you implement both?*

My solution (iterative program):

.. code-block:: python
   :linenos:

    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None: 
            return None
        
        nextNodeRev = ListNode(head.val, None)
        currNode = head.next
        headNodeRev = nextNodeRev
        
        while currNode is not None: 
            headNodeRev = ListNode(currNode.val, nextNodeRev)
            nextNodeRev = headNodeRev
            currNode = currNode.next
            
        return headNodeRev

Remarks:
 * I felt quite confident with the above implementation. 
   Credit to careful and critical reading!
 * The iterative logic came more naturally than the recursive algorithm logic.

Simple and elegant solution (cred: `tusizi <https://leetcode.com/problems/reverse-linked-list/discuss/58127/Python-Iterative-and-Recursive-Solution>`_)::
     
    # iterative
    def reverseList(self, head):
    prev = None
    while head:
        curr = head
        head = head.next
        curr.next = prev
        prev = curr
    return prev

    # recursive 
    def reverseList(self, head):
        return self._reverse(head)

    def _reverse(self, node, prev=None):
        if not node:
            return prev
        n = node.next
        node.next = prev
        return self._reverse(n, node)


.. admonition:: Recursive vs. Iterative Programs

    **Recursion** is a method call in which the method being called is the same as the one 
    making the call (i.e. when an entity *calls itself*). **Iteration** is when a *loop* is 
    repeatedly executed until a certain condition is met. 

    * Infinite recursion can lead to system crash vs. Infinite iteration consumes CPU cycles.
    * Unlike iteration, recursion repeatedly invokes the mechanism, and thereby the overhead, 
      of method calls. This can be expensive in processor time and memory space. 


Day 4 [16 Oct]
========================
Question 10: Unique Paths
--------------------------
*A robot is located at the top-left corner of a* ``m x n`` *grid. 
The robot can only move either down or right at any point in time. The robot is trying to reach the 
bottom-right corner of the grid (marked 'Finish' in the diagram below).*

*How many possible unique paths are there?*

My solution (iterative - ``O(m x n)``):

.. code-block:: python
    :linenos:

    import numpy as np

    def uniquePaths(self, m: int, n: int) -> int:
        # initialize the memoization matrix
        memoMatrix = np.ones((m,n))
        for rowIdx in range(m-2, -1, -1): 
            for colIdx in range(n-2, -1, -1): 
                memoMatrix[rowIdx][colIdx] = memoMatrix[rowIdx+1][colIdx] + memoMatrix[rowIdx][colIdx +1]  
        return int(memoMatrix[0][0])

Remarks:
 * I am happy that I recognized the presence of sub-problems that could be solved and stored 
   through memoization (Dynamic Programming)
 * I am dissatisfied because I knew there is a mathematical formula that could compute the solution 
   in ``O(1)`` time. 
 * Numpy is not all that neccessary here (could have done ``[[1] * n] * m`` instead of ``np.ones((m,n))``)

Math Solution (``O(1)``) using permutation (cred: `whitehat <https://leetcode.com/problems/unique-paths/discuss/22958/Math-solution-O(1)-space>`_)::

    import math
    
    def uniquePaths(self, m: int, n: int) -> int:
        # (m+n-2)! / [(m-1)! (n-1)!]
        res = math.factorial(m+n-2)/(math.factorial(m-1)*math.factorial(n-1))
        return int(res)

If importing math is prohibited:: 

    def uniquePaths(self, m: int, n: int) -> int:
        # (m+n-2)! / [(m-1)! (n-1)!]
        res = 1
        for i in range(m, m+n-1): 
            res *= i
        for j in range(2, n): 
            res /= j
        return int(res) 

Question 11: Move Zeroes
--------------------------
*Given an integer array nums, move all 0's to the end of it while maintaining the relative order 
of the non-zero elements. Note that you must do this in-place without making a copy of the array.*

My solution (``O(n)``):

.. code-block:: python
    :linenos:

    def moveZeroes(self, nums: List[int]) -> None:
        popIdx = []
        zeros = 0
        
        for i, n in enumerate(nums): 
            if n==0: 
                popIdx = [i] + popIdx
                zeros += 1
        for idx in popIdx:
            nums.pop(idx)
        nums += [0] * zeros

Remarks:
 * Could be more concise and could maybe cut down the constant factor (on time complexity), but I am 
   satisfied with a ``O(n)`` solution

Revised solution (``O(n)`` with reduced constant factor; cred: `Kurteck <https://leetcode.com/problems/move-zeroes/discuss/72011/Simple-O(N)-Java-Solution-Using-Insert-Index>`_):: 

    def moveZeroes(self, nums: List[int]) -> None:
        if (nums is None or len(nums)==1): 
            return
        
        insertPos = 0 # for non-Zero elements
        for i in range(len(nums)): 
            if nums[i] != 0:
                nums[insertPos] = nums[i]
                insertPos += 1
        for j in range(insertPos, len(nums)): 
            nums[j] = 0


Question 12: Unique Email Addresses
------------------------------------
*Every valid email consists of a local name and a domain name, separated by the '@' sign. Besides lowercase 
letters, the email may contain one or more '.' or '+'. For example, in "alice@leetcode.com", "alice" is 
the local name, and "leetcode.com" is the domain name. If you add periods '.' between some characters in 
the local name part of an email address, mail sent there will be forwarded to the same address without 
dots in the local name.* 

*Note that this rule does not apply to domain names. For example, 
"alice.z@leetcode.com" and "alicez@leetcode.com" forward to the same email address. If you add a plus '+' 
in the local name, everything after the first plus sign will be ignored. This allows certain emails to 
be filtered. Note that this rule does not apply to domain names. For example, "m.y+name@email.com" will 
be forwarded to "my@email.com". It is possible to use both of these rules at the same time.*

*Given an array of strings emails where we send one email to each email[i], return the number of different 
addresses that actually receive mails.*

My initial solution (``O(n)``):

.. code-block:: python
    :linenos:

    def numUniqueEmails(self, emails: List[str]) -> int:
        emailDict = {}
        for e in emails: 
            [local, domain] = e.split("@")
            
            local = local.split("+")[0].replace(".", "")
            
            if domain in list(emailDict): 
                emailDict[domain].add(local)
            else: 
                emailDict[domain] = {local}
                
        res = 0
        for s in list(emailDict.values()): 
            res += len(s)
        return res

My revised solution (``O(n)`` and better space complexity):

.. code-block:: python
    :linenos:

    def numUniqueEmails(self, emails: List[str]) -> int:
        emailSet = set()
        for e in emails: 
            [local, domain] = e.split("@")
            local = local.split("+")[0].replace(".", "")
            emailSet.add(local + "@" + domain)
        return len(emailSet)

Remarks: 
 * There are so many useful python methods (built-in) that can help optimize the performance of many algorithms. 
   Make sure to be familiar with them so that you don't have to search on Google every time you are doing a question.
 * I don't think there are many ways to make the algorithm more efficient. 

.. tip:: 

    Consider the nature of the problem carefully and think about the various data structures that can be 
    used to solve the problem. Which one is most efficient? Which is most natural or easy to understand? Which 
    one is most compatible with the devised algorithm? 

Day 5 [17 Oct]
========================
Question 13: Generate Parentheses
-----------------------------------
*Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.*

My recursive solution (``O(n)``... I think?):

.. code-block:: python
    :linenos:

    def genParHelper(self,openPar, closePar, acc, currString, res): 
        if openPar + closePar != 0: 
            if acc > 0:
                if openPar > 0:
                    self.genParHelper(openPar-1, closePar, acc+1, currString+"(", res)
                if closePar > 0: 
                    self.genParHelper(openPar, closePar-1, acc-1, currString+")", res)
            else: 
                self.genParHelper(openPar-1, closePar, acc+1, currString+"(", res)
        else: 
            res.append("(" + currString + ")")
        
    def generateParenthesis(self, n: int) -> List[str]:
        res = []
        self.genParHelper(n-1, n-1, 1, "", res)
        return res

Remarks: 
 * It helped to study the test cases provided and to walk through the steps I would take to solve the problem. 
   Then, I tried to implement as many know-hows discovered into my code. 
 * I considered a math (combination/permutation solution) but decided against it as I would have to provide 
   a list of strings rather than a numerical count of them. 
 * Time complexity is difficult to compute.

    * According to LeetCode, the time/space complexity is approximately ``O(4^n / sqrt(n))`` because of Catalan 
      numbers' asymptotic bound ... yeah.

LeetCode's Brute Force solution (``O(2^(2n) * n)``)::

    def generateParenthesis(self, n):
        def generate(A = []):
            if len(A) == 2*n:
                if valid(A):
                    ans.append("".join(A))
            else:
                A.append('(')
                generate(A)
                A.pop()
                A.append(')')
                generate(A)
                A.pop()

        def valid(A):
            bal = 0
            for c in A:
                if c == '(': bal += 1
                else: bal -= 1
                if bal < 0: return False
            return bal == 0

        ans = []
        generate()
        return ans

LeetCode's Backtracking solution (``O(2^(2n) * n)``)::

    # Python
    def generateParenthesis(self, n: int) -> List[str]:
        ans = []
        def backtrack(S = [], left = 0, right = 0):
            if len(S) == 2 * n:
                ans.append("".join(S))
                return
            if left < n:
                S.append("(")
                backtrack(S, left+1, right)
                S.pop()
            if right < left:
                S.append(")")
                backtrack(S, left, right+1)
                S.pop()
        backtrack()
        return ans

LeetCode's Backtracking solution (``O(2^(2n) * n)``):

.. code-block:: Java

    // Java
    public void backtrack(List<String> ans, StringBuilder cur, int open, int close, int max){
        if (cur.length() == max * 2) {
            ans.add(cur.toString());
            return;
        }

        if (open < max) {
            cur.append("(");
            backtrack(ans, cur, open+1, close, max);
            cur.deleteCharAt(cur.length() - 1);
        }
        if (close < open) {
            cur.append(")");
            backtrack(ans, cur, open, close+1, max);
            cur.deleteCharAt(cur.length() - 1);
        }
    }


Question 14: ZigZag Conversion
-----------------------------------
*Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.*

*The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this*::

   P   A   H   N
   A P L S I I G
   Y   I   R

*And then read line by line: "PAHNAPLSIIGYIR". Write the code that will take a string and make this 
conversion given a number of rows*

My solution:

.. code-block:: python
    :linenos:

    def convert(self, s: str, numRows: int) -> str:
        if numRows <= 1: 
            return s
        
        rows = [[] for i in range(numRows)]
        strLen = len(s)
        pointer = 0
        
        while strLen > pointer: 
            for i in range(2*numRows - 2): 
                if pointer == strLen: 
                    break
                    
                if i < numRows: 
                    rows[i].append(s[pointer])
                    pointer += 1
                else: 
                    rows[numRows-2-i].append(s[pointer])
                    pointer += 1
        flatten = [item for sublist in rows for item in sublist]
        return "".join(flatten)

Remarks & Complexity Analysis: 
 * Once again, drawing it out made me realize that a data structure that categorizes the characters by their 
   row would be effective. 
 * Thinking creatively and mathematically in the beginning of the process can make a problem so much easier 
   to solve. Don't rush into typing your code (you might be stuck for a while!). Instead take some time to think 
   and devise a mission plan!
 * Could have been more conservative by creating only the neccessary number of sub-arrays (``min(len(s), numRows)``)
 * **Time Complexity**: ``O(n)`` where ``n=len(s)``.

Similar LeetCode solution:

.. code-block:: Java

    // Java
    public String convert(String s, int numRows) {

        if (numRows == 1) return s;

        List<StringBuilder> rows = new ArrayList<>();
        for (int i = 0; i < Math.min(numRows, s.length()); i++)
            rows.add(new StringBuilder());

        int curRow = 0;
        boolean goingDown = false;

        for (char c : s.toCharArray()) {
            rows.get(curRow).append(c);
            if (curRow == 0 || curRow == numRows - 1) goingDown = !goingDown;
            curRow += goingDown ? 1 : -1;
        }

        StringBuilder ret = new StringBuilder();
        for (StringBuilder row : rows) ret.append(row);
        return ret.toString();
    }

Another interesting LeetCode solution (visit by row):

.. code-block:: Java

    // Java
    public String convert(String s, int numRows) {

        if (numRows == 1) return s;

        StringBuilder ret = new StringBuilder();
        int n = s.length();
        int cycleLen = 2 * numRows - 2;

        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j + i < n; j += cycleLen) {
                ret.append(s.charAt(j + i));
                if (i != 0 && i != numRows - 1 && j + cycleLen - i < n)
                    ret.append(s.charAt(j + cycleLen - i));
            }
        }
        return ret.toString();
    }

The above algorithm works as, for all whole numbers :math:`k`,
 * Characters in row 0 are located at indexes :math:`k \; (2 \cdot \text{numRows} - 2)`
 * Characters in row :math:`\text{numRows}-1` are located at indexes :math:`k \; (2 \cdot \text{numRows} - 2) + \text{numRows} - 1`
 * Characters in inner row :math:`i` are located at indexes :math:`k \; (2 \cdot \text{numRows}-2)+i` and :math:`(k+1)(2 \cdot \text{numRows}-2)- i`.

Question 15: Palindrome Number
-----------------------------------
*Given an integer x, return true if x is palindrome integer. An integer is a palindrome when it reads the same backward 
as forward. For example, 121 is palindrome while 123 is not.*

My Solution: 

.. code-block:: python
    :linenos:

    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]

Remarks & Complexity Analysis: 
 * Remarkably simple question, I purposely chose this because I am low on time... (everyone needs a cheat day)
 * Didn't complete the follow-up challenge (do it without converting the ``int`` into a ``str``)
 * **Time Complexity**: ``O(n)`` where ``n=len(str(x))``

LeetCode's Solution:

.. code-block:: C++
    
    // C++
    public bool IsPalindrome(int x) {
        // Special cases:
        // As discussed above, when x < 0, x is not a palindrome.
        // Also if the last digit of the number is 0, in order to be a palindrome,
        // the first digit of the number also needs to be 0.
        // Only 0 satisfy this property.
        if(x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int revertedNumber = 0;
        while(x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }

        // When the length is an odd number, we can get rid of the middle digit by revertedNumber/10
        // For example when the input is 12321, at the end of the while loop we get x = 12, revertedNumber = 123,
        // since the middle digit doesn't matter in palidrome(it will always equal to itself), we can simply get rid of it.
        return x == revertedNumber || x == revertedNumber/10;
    }

