************************
Week 1 [13-20 Oct 2021]
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
    ``Iter()`` returns an iterator for a given object (e.g. list, sets, tuples, ...). More on `iterator type <https://www.mygreatlearning.com/blog/iterator-in-python/>`_. 
    In our example, it is like a list whose elements we can see one-by-one (and we have to discard it after seeing it)

    ``all()`` takes an *iterable* (e.g. list, tuple, dictionary, iterator, etc...) and returns True if all items in the 
    iterable are True, else it returns False. Its counterpart is ``any()``, which returns True if any item in an iterable is True, else it returns False.


