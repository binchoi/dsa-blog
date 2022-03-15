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

Day 27 [10 Mar]
================
Question 42: Container With Most Water
---------------------------------------
You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]). 
Find two lines that together with the x-axis form a container, such that the container contains the most water. 
Return the maximum amount of water a container can store. Notice that you may not slant the container.

My Inefficient (Time Limit Exceeded) Bruteforce Solution: 

.. code-block:: Java
    :linenos: 

    import java.util.*;

    class Solution {
        
        public Integer convertIdx(int idx, int arrSize, boolean isStart) {
            if (isStart) {
                return Integer.valueOf(idx);
            } else {
                return Integer.valueOf(arrSize-idx-1);
            }
        }
        
        public HashMap<Integer,Integer> find_candidates(int[] height, boolean isStart) {
            HashMap<Integer,Integer> candidates = new HashMap<>();
            
            candidates.put(convertIdx(0, height.length, isStart), Integer.valueOf(height[0]));

            int highestSoFar = height[0];
            for (int i=1; i<height.length; i++) {
                if (height[i]>highestSoFar) {
                    candidates.put(convertIdx(i,height.length,isStart), Integer.valueOf(height[i]));
                    highestSoFar = height[i];
                }
            }
            return candidates;
        }
        
        public int maxArea(int[] height) {
            HashMap<Integer,Integer> start_candidates = find_candidates(height, true);
            
            int[] revHeight = new int[height.length];
            int j = height.length;
            for (int i = 0; i < height.length; i++) {
                revHeight[j - 1] = height[i];
                j--;
            }
            HashMap<Integer,Integer> end_candidates = find_candidates(revHeight, false);      
            int bestSoFar = 0;
            for (Map.Entry<Integer, Integer> start_set:start_candidates.entrySet()) {
                for (Map.Entry<Integer, Integer> end_set:end_candidates.entrySet()) {
                    int temp = Math.min(end_set.getValue(), start_set.getValue())*(end_set.getKey() - start_set.getKey());
                    if (temp>bestSoFar) {
                        bestSoFar = temp;
                    }
                }
            }
            return bestSoFar;
        }
    }

Remarks and Complexity Analysis: 
 * Again, not the cleanest code I've written
 * I learned a lot about Java as a programming language. For one, why is it so difficult to reverse an array? Well to be fair, it is mostly 
   a problem with LeetCode not permitting me to import certain packages like ArrayUtil or Collections (which really got me thinking that I should 
   use Python for any algo interview) but I feel like there are more choices in Java. And navigating them wisely seems to require 
   experience and understanding of what each offers or doesn't offer. 
 * **Time Complexity**: ``O(n^2)`` - worst case scenario is probably triangle shape (increasing then decreasing lines) which would correspond to ~``O(1/4 n^2)``
 * **Space Complexity**: ``O(n)``

Two Pointer O(n) solutions: 
 * Using two pointer is extremely efficient and powerful in many algorithm problems
 * continue working on algorithms and coding tests!!


Python Two-pointer Solution::

    class Solution:
        def maxArea(self, height):
            i, j = 0, len(height) - 1
            water = 0
            while i < j:
                water = max(water, (j - i) * min(height[i], height[j]))
                if height[i] < height[j]:
                    i += 1
                else:
                    j -= 1
            return water

Java Two-pointer Solution:: 

    public int maxArea(int[] height) {
        int left = 0, right = height.length - 1;
        int maxArea = 0;

        while (left < right) {
            maxArea = Math.max(maxArea, Math.min(height[left], height[right])
                    * (right - left));
            if (height[left] < height[right])
                left++;
            else
                right--;
        }

        return maxArea;
    }

Day 28 [14 Mar]
================
Question 43: Valid Parentheses
---------------------------------------

My Solution: 

.. code-block:: Java
    :linenos: 

    class Solution {
        public boolean isValid(String s) {
            Stack<Character> stk = new Stack();
            HashMap<Character, Character> parsMatch = new HashMap<>();
            parsMatch.put(']', '[');
            parsMatch.put(')', '(');
            parsMatch.put('}', '{');
            for (char c : s.toCharArray()) {
                if (parsMatch.containsValue(c)) {
                    stk.push(c);
                } else {
                    if (stk.isEmpty() || stk.pop() != (parsMatch.get(c))) {
                        return false;
                    }
                }
            }
            return (stk.isEmpty());
        }
    }

Remarks and Complexity Analysis: 
 * Had a little struggle with reference vs. primitive types. I now understand the casting between primitives and wrappers better.
 * **Time Complexity**: ``O(n)`` 
 * **Space Complexity**: ``O(n)``

Casting to-and-fro primitive/wrapper::

    int i = 5; //5 is a primitive int
    Integer iRef = i; // convert to wrapper

    Integer iRef = new Integer(5); // starts life as wrapper
    int j = i; //ok, unboxed to primitive

Question 44: Climbing Stairs
---------------------------------------
You are climbing a staircase. It takes n steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

My solution:

.. code-block:: Java
    :linenos: 

    private HashMap<Integer, Integer> memoTable = new HashMap<>();
    
    public int climbStairs(int n) {
        if (n==1) {
            return 1;
        } else if (n==2) {
            return 2;
        } else if (this.memoTable.containsKey(n)) {
            return this.memoTable.get(n);
        } else {
            int sol = this.climbStairs(n-2) + this.climbStairs(n-1);
            this.memoTable.put(n,sol);
            return (this.climbStairs(n-2) + this.climbStairs(n-1));
        }
    }

Even better solution:

.. code-block:: Java
    :linenos: 

    public int climbStairs(int n) {
        if (n==1) {
            return 1;
        } else if (n==2) {
            return 2;
        }
        
        int a = 1;
        int b = 2;
        while (n-->2) {
            int tempB = b;
            b = a+b;
            a = tempB;
        }
        return b;
    }


Remarks and Complexity Analysis: 
 * The key was realizing the pattern and the recursive nature (it's fibonacci!)
 * **Time Complexity**: ``O(n)`` 
 * **Space Complexity**: ``O(1)``


Question 45: Valid Palindrome
---------------------------------------
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers. Given a string s, return true if it is a palindrome, or false otherwise.

My solution:

.. code-block:: Java
    :linenos:

    public boolean isPalindrome(String s) {
        ArrayList<Character> filteredArr = new ArrayList<>();
        for (Character c : s.toCharArray()) {
            if (Character.isLetter(c) || Character.isDigit(c)) {
                filteredArr.add(Character.toLowerCase(c));
            }
        }
        if (filteredArr.isEmpty()) {
            return true;
        }
        ArrayList<Character> origArr = new ArrayList<>(filteredArr);
        Collections.reverse(filteredArr);
        return origArr.equals(filteredArr);
    }

Alternative solutions:

.. code-block:: Java

    public boolean isPalindrome(String s) {
        String actual = s.replaceAll("[^A-Za-z0-9]", "").toLowerCase();
        String rev = new StringBuffer(actual).reverse().toString();
        return actual.equals(rev);
    }

    public boolean isPalindrome(String s) {
        if (s.isEmpty()) {
        	return true;
        }
        int head = 0, tail = s.length() - 1;
        char cHead, cTail;
        while(head <= tail) {
        	cHead = s.charAt(head);
        	cTail = s.charAt(tail);
        	if (!Character.isLetterOrDigit(cHead)) {
        		head++;
        	} else if(!Character.isLetterOrDigit(cTail)) {
        		tail--;
        	} else {
        		if (Character.toLowerCase(cHead) != Character.toLowerCase(cTail)) {
        			return false;
        		}
        		head++;
        		tail--;
        	}
        }
        
        return true;
    }

Remarks and Complexity Analysis: 
 * I think the better way to solve this problem is not regex (or my solution) but using two-pointers. It is more space-efficient and seems more approprite.
 * **Time Complexity**: ``O(n)`` 
 * **Space Complexity**: ``O(1)`` for two-pointer; ``O(n)`` otherwise


Day 29 [15 Mar]
================
For a change, I tried out the 283rd LeetCode Weekly Contest. I managed to solve two questions: one perfectly and one imperfectly. 

Question 46: Cells in a Range on an Excel Sheet
------------------------------------------------------
A cell (r, c) of an excel sheet is represented as a string "<col><row>" where:

<col> denotes the column number c of the cell. It is represented by alphabetical letters.
For example, the 1st column is denoted by 'A', the 2nd by 'B', the 3rd by 'C', and so on.
<row> is the row number r of the cell. The rth row is represented by the integer r.
You are given a string s in the format "<col1><row1>:<col2><row2>", where <col1> represents the column c1, <row1> represents the row r1, <col2> represents the column c2, and <row2> represents the row r2, such that r1 <= r2 and c1 <= c2. 
Return the list of cells (x, y) such that r1 <= x <= r2 and c1 <= y <= c2. The cells should be represented as strings in the format mentioned above and be sorted in non-decreasing order first by columns and then by rows.

My solution: 

.. code-block:: Java
    :linenos:

    class Solution {
        public List<String> cellsInRange(String s) {
            List<String> res = new ArrayList<String>();
            char[] alphabets = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".toCharArray();
            
            Character x_i = Character.toUpperCase(s.charAt(0));
            char x_f = Character.toUpperCase(s.charAt(3));
            int row_i = Character.getNumericValue(s.charAt(1));
            int row_f = Character.getNumericValue(s.charAt(4));
            
            int downStep = row_f-row_i;
            int idx = 0;
            while (alphabets[idx]!=x_i) {idx++;}
            do {
                String letter = Character.toString(alphabets[idx]);
                for (int j = row_i; j<=row_f; j++) {
                    res.add(letter+j);
                }
            } while (alphabets[idx++]!=x_f);
            return res;
        }
    }

Remarks and Complexity Analysis: 
 * Still trying to get familiar with the methods of Java's classes (e.g. arr.length, Character.toUpperCase(), string.charAt(), Integer.valueOf(), etc...)
 * **Time Complexity**: ``O(n*m)`` - where ``n,m`` are the vertical and horizontal distance between the two given positions
 * **Space Complexity**: ``O(1)`` for two-pointer; ``O(n)`` otherwise
 * I found out that in Java, you can do ``char c++;`` to increment the value of char variable ``c`` from 'a' to 'b' or from 'D' to 'E' or even from '3' to '4'.

My revised solution: 

.. code-block:: Java
    :linenos:

    public List<String> cellsInRange(String s) {
        List<String> res = new ArrayList<String>();
        for (char c = s.charAt(0); c<=s.charAt(3); c++) {
            for (int i=Character.getNumericValue(s.charAt(1)); i<=Character.getNumericValue(s.charAt(4));i++) {
              res.add(Character.toString(c)+i);
          }
        }
        return res;
    }
    
    // Version 3
    public List<String> cellsInRange(String s) {
        List<String> res = new ArrayList<String>();
        for (char c = s.charAt(0); c<=s.charAt(3); c++) {
            for (char i=s.charAt(1); i<=s.charAt(4);i++) {
              res.add(""+c+i);
          }
        }
        return res;
    }

Question 47: Append K Integers With Minimal Sum
------------------------------------------------------
You are given an integer array nums and an integer k. Append k unique positive integers that do not appear in nums to nums such that the resulting total sum is minimum. Return the sum of the k integers appended to nums

My (faulty) solution: 

.. code-block:: Java
    :linenos:

    class Solution {
        public long minimalKSum(int[] nums, int k) {
            long res = 0;
            
            Arrays.sort(nums);
            int numChecking = 1;
            int arrIdx = 0;
            while (k>0) {
                if (arrIdx>=nums.length) {
                    break;
                }
                while (numChecking < nums[arrIdx] && k>0) {
                    res+=numChecking;
                    numChecking++;
                    k--;
                }
                arrIdx++;
                numChecking++;
            }
            if (k==0) {
                return res;
            } else {
                return res + (k*(2*numChecking+k-1)/2);
            }
        }
    }

    // Second try
    public long minimalKSum(int[] nums, int k) {
        long res = (k*(k+1))/2;
        Set<Integer> seen = new HashSet<>();
        for (int n:nums) {
            if (n<=k) {
                if (seen.contains(Integer.valueOf(n))) {
                    continue;
                } else {
                    res+=((++k)-n);
                    seen.add(Integer.valueOf(n));
                }
            }
        }
        return res;
    }

Remarks and Complexity Analysis: 
 * Second try is much closer but not quite there. 
 * **Time Complexity**: ``O(n)`` - where ``n=nums.length``
 * **Space Complexity**: ``O(1)``

Alternative (correct) solution: 

.. code-block:: Java
    :linenos:

    public long minimalKSum(int[] nums, int k) {
        Arrays.sort(nums);
        Set<Integer> set = new HashSet<>();
        long sum = 0;

        for (int num: nums) {
            if (!set.contains(num) && num <= k) {
                k++;
                sum += num;        
            }            
            set.add(num);
        }

        long res = (long)(1 + k) * k / 2 - sum;
        return res;   
    }

Question 48: Counting Bits
------------------------------------------------------
Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.

I am not familiar with bit operations in Java, so I looked at the discuss earlier!

Solution:

.. code-block:: Java
    :linenos:

    public int[] countBits(int num) {
        int[] f = new int[num + 1];
        for (int i=1; i<=num; i++) f[i] = f[i >> 1] + (i & 1);
        return f;
    }

Remarks and Complexity Analysis: 
 * line 2 creates the return array, line 3 starts from ``i=1`` and by doing so initialized ``f[0]=0``
 * ``i >> 1`` is equivalent to ``i/2`` (i.e. f[i/2])
 * ``i & 1`` is equivalent to ``i%2`` -> if the number is odd, we add one to f[i >> 1] as one 1 is removed when shifting to right
 * **Time Complexity**: ``O(n)``
 * **Space Complexity**: ``O(1)`` apart from the returned array (of size ``num.length``)