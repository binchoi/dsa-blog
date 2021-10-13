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


**Take-aways**

* Difference between ``hashtable`` and ``dict`` data types (i.e. dict is larger umbrella)
* Increased time optimization comes at the cost of space (this is preferable as memory is cheaper than time) 

Question 2: Valid Parentheses
-------------------------------
*Given a string s containing just the characters '(', ')', '{', '}', 
'[' and ']', determine if the input string is valid.*

My solution: O(n)

* using stack and a helper function that compares a open parenthesis with a closed one

**Take-aways**

* No need to create class for stack when coding in Python (just use list operators)
* Could use dictionaries instead of helper function (would it help in memory/time?) 

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


