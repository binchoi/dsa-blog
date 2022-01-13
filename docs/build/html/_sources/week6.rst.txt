************************
Week 6 [10-16 Jan 2022]
************************

Day 24 [13 Jan]
================
Wow. It has been a hot minute! Before we jump into the next question, please allow me to explain the reason behind my 
tremendously long hiatus from Algo-practice. A lot has happened since the last practice session. 
I have prepared for and completed the exams for my first semester, flew back to Qatar for 
winter break with my family, enjoyed a wonderful three-to-four weeks, traveled to see Freddy 
in Sweden, flew to Helsinki to begin my exchange semester at Aalto University School of Computer Science, and 
successfully completed my first week of school in Finland. Now that you're all caught up - let the grind the 
continue. 

Question 39: 
---------------------------------------
Failed Bruteforce attempt (Time Limit Exceeded)::

    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
            arr_len = len(nums)
            min_len = arr_len + 1
            for i in range(arr_len): 
                sum = nums[i]
                if (sum>=target): 
                    return 1
                for j in range(i+1, arr_len): 
                    sum += nums[j]
                    if (sum>=target): 
                        min_len = min((j-i+1), min_len)
                        break
                    if (j-i+1>=min_len): 
                        break
            return (min_len if min_len != arr_len+1 else 0)

Two-pointer solution

.. code-block:: python

    :linenos: 

    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        arr_len = len(nums)
        
        if nums is None or len(nums)==0: 
            return 0 
        
        i = 0 
        j = 0 
        sum = 0
        res = arr_len+1
        
        while (j<arr_len): 
            sum += nums[j]
            j += 1
            while (sum >= target): 
                res = min(res, j-i)
                sum -= nums[i]
                i += 1
        return 0 if (res==arr_len+1) else res

Remarks and Complexity Analysis: 
 * Took quite some time to grasp the appropriate method to approach the problem. 
 * **Time Complexity**: ``O(n^2)`` - where ``n=len(nums)`` 
 * **Space Complexity**: ``O(1)``.
 * overall, I am feeling real rusty. Let's keep up the consistency now!

 
