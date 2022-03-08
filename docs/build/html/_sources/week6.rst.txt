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

Question 39: MinSubArrayLen
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

Day 25 [5 Feb]
===============
Well that's a little embarrassing. It's okay -- what matters is that I am back :-) Today I tried my first 
leetcode question in Java. 

Question 40: Two Sum
---------------------

.. code-block:: Java
    :linenos: 

    public int[] twoSum(int[] nums, int target) {
        
        int[] res = new int[2];
        HashMap<Integer,Integer> lookingFor = new HashMap(); // val, idx
        for (int i=0; i<nums.length; i++) {
            if (lookingFor.containsKey(nums[i])) {
                res[0] = i;
                res[1] = lookingFor.get(nums[i]);
                return res;
            }
            lookingFor.put(target-nums[i], i);
        }
        return new int[] {0,0};
    }


Remarks and Complexity Analysis: 
 * Didn't see the ``O(n)`` solution at first!
 * **Time Complexity**: ``O(n)`` - where ``n=nums.length`` 
 * **Space Complexity**: ``O(n)``.
 * Wow, Java is verbose!

Day 26 [9 Mar]
===============
Well that's very embarrassing. Let's keep learning Java

Question 41: Longest Palindromic Substring
-------------------------------------------
Given a string s, return the longest palindromic substring in s.

.. code-block:: Java
    :linenos: 

    class Solution {
        public String longestPalindrome(String s) {
            
            if (new StringBuilder(s).reverse().toString().equals(s)) {
                return s;
            }
            int bestFrom, bestTo;
            bestFrom = bestTo = 0;
            int highestSoFar = 1;
            int record, lower, upper;
            for (int i = 1; i< s.length()-1; i++) {
                record = 1;
                lower = upper = i;
                while (lower-1>=0 && upper+1<=s.length()-1) {
                    if (s.charAt(lower-1) == s.charAt(upper+1)) {
                        record+=2;
                        lower--;
                        upper++;
                    } else {
                        break;
                    }
                }
                if (record>highestSoFar) {
                    highestSoFar = record;
                    bestFrom = lower;
                    bestTo = upper;
                }
            }
            
            for (int i=0; i<s.length()-1; i++) {
                if (s.charAt(i)==s.charAt(i+1)) {
                    record = 2;
                    lower = i;
                    upper = i+1;
                    while (lower-1>=0 && upper+1<=s.length()-1) {
                        if (s.charAt(lower-1) == s.charAt(upper+1)) {
                            record+=2;
                            lower--;
                            upper++;
                        } else {
                            break;
                        }
                    }
                    if (record>highestSoFar) {
                        highestSoFar = record;
                        bestFrom = lower;
                        bestTo = upper;
                    }
                }
            }
            if (highestSoFar==1) {
                return s.substring(0,1);
            } else {
                return s.substring(bestFrom, bestTo+1);
            }
        }
    }

A cleaner version of my solution (provided by LeetCode): 

.. code-block:: Java
    :linenos: 

    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) return "";
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            int len1 = expandAroundCenter(s, i, i);
            int len2 = expandAroundCenter(s, i, i + 1);
            int len = Math.max(len1, len2);
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
        return s.substring(start, end + 1);
    }

    private int expandAroundCenter(String s, int left, int right) {
        int L = left, R = right;
        while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
            L--;
            R++;
        }
        return R - L - 1;
    }

Remarks and Complexity Analysis: 
 * Wow, not the cleanest code I've written (lot's of repetition)
 * **Time Complexity**: ``O(n^2)``
 * **Space Complexity**: ``O(1)``
 * I am practicing Java's syntax with these coding questions, but I am wondering if I should later focus on strictly using python.

