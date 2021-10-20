************************
Week 2 [17-24 Oct 2021]
************************

Day 6 [18 Oct]
========================
Question 16: Group Anagrams
----------------------------
*Given an array of strings strs, group the anagrams together. You can return the answer in any order. An Anagram is a 
word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original 
letters exactly once.*

My solution: 

.. code-block:: python
    :linenos: 

    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        resDict = {}
        for s in strs: 
            sorted_s = "".join(sorted(s))
            if sorted_s not in resDict.keys(): 
                resDict[sorted_s] = [s]
            else: 
                resDict[sorted_s].append(s)
        return list(resDict.values())

Remarks and Complexity Analysis: 
 * A simple problem that did not take much time to solve. 
 * I keep forgetting the commonly used methods of different data structures (e.g. ``dict``: ``.values()``, ``.keys()``, 
   ``list``: ``sorted()`` vs. ``.sort()``). Perhaps, I should make a sub-page for my personal reminders
 * **Time Complexity**: ``O(n)`` where ``n=len(strs)``. More precisely, ``O(n KlogK)`` where ``K=max(len(s in strs))`` as 
   the built-in ``sorted()`` method has a time complexity of ``O(nlogn)``.
 * **Space Complexity**: ``O(nK)`` - total information stored in ``resDict``

LeetCode's simpler solution:: 

    def groupAnagrams(self, strs):
        ans = collections.defaultdict(list)
        for s in strs:
            ans[tuple(sorted(s))].append(s)
        return ans.values()

* ``tuple(sorted(s))`` is still a hashable key! I wonder if there is any benefit to using the tuple as a key rather than the 
  sorted string? 


.. note:: 

    ``collections`` is a module that implements specialized container datatypes providing alternatives to Python’s general 
    purpose built-in containers, dict, list, set, and tuple. ``collections.defaultdict(default_factory=None, /[,...])`` can 
    take a parameter ``defaultdict`` which is called without arguments to initialize the values for keys. Hence, 
    ``collections.defaultdict(list)`` initializes the values to have a default type of ``list``. 


LeetCode's Java solution: 

.. code-block:: Java
    :linenos:

    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs.length == 0) return new ArrayList();
        Map<String, List> ans = new HashMap<String, List>();
        for (String s : strs) {
            char[] ca = s.toCharArray();
            Arrays.sort(ca);
            String key = String.valueOf(ca);
            if (!ans.containsKey(key)) ans.put(key, new ArrayList());
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }

Alternative Approach (Categorize by Count): 

This method is more efficient as character count has a time complexity of ``O(n)`` where ``n=len(string)``. In other words, 
counting each string is linear in the size of the string. This is thanks to how Python and Java store strings. 
In Java, the hashable representation of our count will be a string delimited with *'#'* characters. For example, 
``abbccc`` will be ``#1#2#3#0#0#0...#0`` where there are 26 entries total. In python, the representation will be a tuple of 
the counts. For example, ``abbccc`` will be ``(1, 2, 3, 0, 0, ..., 0)``, where again there are 26 entries total.

.. code-block:: python
    :linenos: 

    def groupAnagrams(strs):
        ans = collections.defaultdict(list)
        for s in strs:
            count = [0] * 26
            for c in s:
                count[ord(c) - ord('a')] += 1
            ans[tuple(count)].append(s)
        return ans.values()

.. code-block:: Java
    :linenos: 

    // Java 
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs.length == 0) return new ArrayList();
        Map<String, List> ans = new HashMap<String, List>();
        int[] count = new int[26];
        for (String s : strs) {
            Arrays.fill(count, 0);
            for (char c : s.toCharArray()) count[c - 'a']++;

            StringBuilder sb = new StringBuilder("");
            for (int i = 0; i < 26; i++) {
                sb.append('#');
                sb.append(count[i]);
            }
            String key = sb.toString();
            if (!ans.containsKey(key)) ans.put(key, new ArrayList());
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }

* **Time Complexity**: ``O(nK)`` where ``n=len(strs)`` and ``K=max(len(s in strs))``
* **Space Complexity**: ``O(nK)`` - total information stored in ``resDict``

Another interesting solution (cred: `kit  <https://leetcode.com/problems/group-anagrams/discuss/19202/5-line-Python-solution-easy-to-understand>`_):: 

    def groupAnagrams(self, strs):
        d = {}
        for w in strs:
            key = tuple(sorted(w))
            d[key] = d.get(key, []) + [w]
        return list(d.values())

Question 17: Longest Substring Without Repeating Characters
-------------------------------------------------------------
*Given a string s, find the length of the longest substring without repeating characters.*

My brute-force solution: 

.. code-block:: python 
    :linenos:

    def lolsHelper(self, s): 
        res = 0
        seen = set()
        for c in s: 
            if c in seen: 
                return res
            else: 
                seen.add(c)
                res += 1
        return res
    
    def lengthOfLongestSubstring(self, s: str) -> int:
        if s is "" or None: 
            return 0
        longestFromHere = [0]
        for i in range(len(s)): 
            longestFromHere.append(self.lolsHelper(s[i:]))
        return max(longestFromHere)

Remarks and Complexity Analysis: 
 * brute-force algorithm was easy to devise but had difficulty optimizing it
 * **Time Complexity**: ``O(n^2)``
 * **Space Complexity**: ``O(n)``


LeetCode's brute-force solution (same intuition):

.. code-block:: Java
    
    // Java
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();

        int res = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                if (checkRepetition(s, i, j)) {
                    res = Math.max(res, j - i + 1);
                }
            }
        }

        return res;
    }

    private boolean checkRepetition(String s, int start, int end) {
        int[] chars = new int[128];

        for (int i = start; i <= end; i++) {
            char c = s.charAt(i);
            chars[c]++;
            if (chars[c] > 1) {
                return false;
            }
        }

        return true;
    }

LeetCode's sliding window solution:: 

    def lengthOfLongestSubstring(self, s: str) -> int:
        chars = [0] * 128

        left = right = 0

        res = 0
        while right < len(s):
            r = s[right]
            chars[ord(r)] += 1

            while chars[ord(r)] > 1:
                l = s[left]
                chars[ord(l)] -= 1
                left += 1

            res = max(res, right - left + 1)

            right += 1
        return res

.. code-block:: Java

    // Java
    public int lengthOfLongestSubstring(String s) {
        int[] chars = new int[128];

        int left = 0;
        int right = 0;

        int res = 0;
        while (right < s.length()) {
            char r = s.charAt(right);
            chars[r]++;

            while (chars[r] > 1) {
                char l = s.charAt(left);
                chars[l]--;
                left++;
            }

            res = Math.max(res, right - left + 1);

            right++;
        }
        return res;
    }

* **Time Complexity**: ``O(2n) = O(n)`` as the worst case is if a character is visited twice
* **Space Complexity**: ``O(min(m,n))`` -- determined by the size of the set which depends on either size of the string 
  or size of the charset
  

LeetCode's optimized sliding window solution:: 

    def lengthOfLongestSubstring(self, s: str) -> int:
        n = len(s)
        ans = 0
        # mp stores the current index of a character
        mp = {}

        i = 0
        # try to extend the range [i, j]
        for j in range(n):
            if s[j] in mp:
                i = max(mp[s[j]], i)

            ans = max(ans, j - i + 1)
            mp[s[j]] = j + 1

        return ans

.. code-block:: Java

    // Java
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }

Alternative version of above code (cred: `rohan <https://leetcode.com/problems/longest-substring-without-repeating-characters/solution/>`_):

.. code-block:: Java

    // Java
    public int lengthOfLongestSubstring(String s) {
        
        Map<Character, Integer> map= new HashMap<>();
        int start=0, len=0;
        
        // abba
        for(int i=0; i<s.length(); i++) {
            char c = s.charAt(i);
            if (map.containsKey(c)) {
                if (map.get(c) >= start) 
                    start = map.get(c) + 1;
            }
            len = Math.max(len, i-start+1);
            map.put(c, i);
        }
        return len;
    }

Question 18: Remove Element
------------------------------
*Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The relative order of the elements may be changed. 
Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the 
array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. 
It does not matter what you leave beyond the first k elements. Return k after placing the final result in the first k slots of nums.*

My solution: 

.. code-block:: python
    :linenos:

    def removeElement(self, nums: List[int], val: int) -> int:
        for i, n in reversed(list(enumerate(nums))): 
            if n==val: 
                nums.pop(i)
        return len(nums)

.. tip:: 

    If you want to iterate through an iterator in reverse order but still keep the original indices, you can apply ``enumerate`` on the iterator, before 
    calling ``reversed()``. 

Remarks and Complexity Analysis: 
 * This was my cheat question of the day. I did learn more about for-loops from it though!
 * **Time Complexity**: ``O(n)``
 * **Space Complexity**: ``O(1)``

Day 7 [19 Oct]
========================
Question 19: Combination Sum
-----------------------------

*Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations 
of candidates where the chosen numbers sum to target. You may return the combinations in any order. The same number may be chosen 
from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers 
is different. It is guaranteed that the number of unique combinations that sum up to target is less than 150 combinations for 
the given input.*

My brute-force solution: 

.. code-block:: python
    :linenos:

    def combinationSumHelper(self, candidates, target, acc, resSet): 
        if target == 0: 
            resSet.add(tuple(sorted(acc)))
        else: 
            for c in candidates: 
                if c <= target: 
                    newAcc = acc + [c]
                    self.combinationSumHelper(candidates, target-c, newAcc, resSet)

    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        resSet = set()
        self.combinationSumHelper(candidates, target, [], resSet)
        return list(list(s) for s in resSet)

Remarks and Complexity Analysis: 
 * Brute-force, recursive algorithm... I couldn't think of other ways. 
 * Disappointing that I had to call operations like ``list()`` and ``tuple()`` so much (undermining the inefficiency)
 * Depth First Search (DFS) algorithm
 * **Time Complexity**: ``O(n^2)`` (not certain)
 * **Space Complexity**: ``O(n^2)`` (not certain)

Alternative solution (cred: `oldCodingFarmer <https://leetcode.com/problems/combination-sum/discuss/16510/Python-dfs-solution./509814>`_): 

.. code-block:: python
    :linenos:

    def combinationSum(self, candidates, target):
        ret = []
        self.dfs(candidates, target, [], ret)
        return ret
    
    def dfs(self, nums, target, path, ret):
        if target < 0:
            return 
        if target == 0:
            ret.append(path)
            return 
        for i in range(len(nums)):
            self.dfs(nums[i:], target-nums[i], path+[nums[i]], ret)

* Duplicates are taken into account as the candidates (nums) are sliced as the for-loop proceeds. This nullifies the use of a set (and reduces related overhead).

.. tip:: 

    To prevent duplicate elements in DFS algorithms, consider iterating through increasingly smaller sub-sets (excluding prev). Consider the example above! 

Question 20: Next Greater Element I
-------------------------------------
*The next greater element of some element x in an array is the first greater element that is to the right of x in the same array. 
You are given two distinct 0-indexed integer arrays nums1 and nums2, where nums1 is a subset of nums2. For each 0 <= i < nums1.length, 
find the index j such that nums1[i] == nums2[j] and determine the next greater element of nums2[j] in nums2. If there is no next greater 
element, then the answer for this query is -1. Return an array ans of length nums1.length such that ans[i] is the next 
greater element as described above.*

My brute-force solution: 

.. code-block:: python
    :linenos:

    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
            res = []
            for n in nums1:
                nIdx = nums2.index(n)
                found = False
                for cand in nums2[nIdx:]: 
                    if cand > n: 
                        res.append(cand)
                        found = True
                        break
                if not found: 
                    res.append(-1)
            return res 

Remarks and Complexity Analysis: 
 * Brute-force, iterative algorithm... I couldn't think of other ways. 
 * **Time Complexity**: ``O(n*m*)`` where ``n=len(nums1), m=len(nums2)``
 * **Space Complexity**: ``O(n)`` 

Alternative, linear-time solution (cred: `yuxiang <https://leetcode.com/problems/next-greater-element-i/discuss/97595/Java-10-lines-linear-time-complexity-O(n)-with-explanation>`_):

.. code-block:: Java

    // Java
    public int[] nextGreaterElement(int[] findNums, int[] nums) {
        Map<Integer, Integer> map = new HashMap<>(); // map from x to next greater element of x
        Stack<Integer> stack = new Stack<>();
        for (int num : nums) {
            while (!stack.isEmpty() && stack.peek() < num)
                map.put(stack.pop(), num);
            stack.push(num);
        }   
        for (int i = 0; i < findNums.length; i++)
            findNums[i] = map.getOrDefault(findNums[i], -1);
        return findNums;
    }

.. code-block:: python

    # Python
    def nextGreaterElement(self, findNums, nums):
        st, d = [], {}
        for n in nums:
            while st and st[-1] < n:
                d[st.pop()] = n
            st.append(n)
        
        return [d.get(x, -1) for x in findNums]

* Very clever solution (come back to it!)
* Learn about "monotone stack" 


Question 21: Length of Last Word
-------------------------------------
*Given a string s consisting of some words separated by some number of spaces, return the length of the last word in the string. 
A word is a maximal substring consisting of non-space characters only.*

.. code-block:: python
   :linenos:

    def lengthOfLastWord(self, s: str) -> int:
            for cand in s.split(" ")[::-1]:
                if len(cand)>0: 
                    return len(cand)
    
    # more simple
    def lengthOfLastWord(self, s: str) -> int:
        return len(s.strip().split(" ")[-1])



Remarks and Complexity Analysis: 
 * Very simple question
 * **Time Complexity**: ``O(1)``
 * **Space Complexity**: ``O(1)`` 

Day 8 [20 Oct]
========================
Question 22: Add Two Numbers
-----------------------------
*You are given two non-empty linked lists representing two non-negative integers. The digits are stored in 
reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as 
a linked list. You may assume the two numbers do not contain any leading zero, except the number 0 itself.*

My solution: 

.. code-block:: python
    :linenos:
        
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        topNode = l1
        botNode = l2
        
        firstIt = True
        carryOver = 0
        while  topNode is not None or botNode is not None: 
            if topNode is None: 
                topVal = 0
                botVal = botNode.val
            elif botNode is None: 
                botVal = 0 
                topVal = topNode.val
            else: 
                topVal = topNode.val 
                botVal = botNode.val
            
            newNode = ListNode((topVal+botVal+carryOver)%10)
            carryOver = (topVal+botVal+carryOver) // 10
            if firstIt: 
                retNode = newNode
                firstIt = False
            else: 
                prevNode.next = newNode
            prevNode = newNode
            
            if topNode is None: 
                botNode = botNode.next
            elif botNode is None: 
                topNode = topNode.next
            else:                 
                topNode = topNode.next
                botNode = botNode.next
        if carryOver != 0: 
            newNode = ListNode(carryOver)
            prevNode.next = newNode
        return retNode

Remarks and Complexity Analysis: 
 * Not too difficult to solve using the elementary math method
 * **Time Complexity**: ``O(n)`` where ``n=max(len(l1), len(l2))``
 * **Space Complexity**: ``O(n)`` where ``n=max(len(l1), len(l2))`` 

LeetCode's solution:

.. code-block:: Java

    // Java
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0);
        ListNode p = l1, q = l2, curr = dummyHead;
        int carry = 0;
        while (p != null || q != null) {
            int x = (p != null) ? p.val : 0;
            int y = (q != null) ? q.val : 0;
            int sum = carry + x + y;
            carry = sum / 10;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
            if (p != null) p = p.next;
            if (q != null) q = q.next;
        }
        if (carry > 0) {
            curr.next = new ListNode(carry);
        }
        return dummyHead.next;
    }

* Same algorithm, more concise!

Revised solution: 

.. code-block:: python
    :linenos:

    def addTwoNumbers(self, l1, l2):
        sentinelNode = ListNode(0)
        result_tail = sentinelNode
        carryOver = 0
                
        while l1 or l2 or carryOver:            
            val1  = (l1.val if l1 else 0)
            val2  = (l2.val if l2 else 0)
            carryOver, out = divmod(val1+val2 + carryOver, 10)    
                      
            result_tail.next = ListNode(out)
            result_tail = result_tail.next                      
            
            l1 = (l1.next if l1 else None)
            l2 = (l2.next if l2 else None)
               
        return sentinelNode.next

* Useful function ``divmod()``!

.. tip:: 

    Always consider using a sentinel node/element when implementing a linked list!


Question 23: Intersection of Two Arrays
----------------------------------------
*Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in 
the result must be unique and you may return the result in any order.*

My solution: 

.. code-block:: python
    :linenos:

    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
            set1 = set(nums1)
            set2 = set(nums2)
            
            resSet = set()
            for s in set1: 
                if s in set2: 
                    resSet.add(s)
            return list(resSet)

Remarks and Complexity Analysis: 
 * Simple algorithm
 * Thought about optimizing by iterating over the set with less elements but concluded that it would not help the time complexity significantly
 * **Time Complexity**: ``O(n)`` where ``n=max(len(nums1), len(nums2))``. ``in`` / ``contain`` operators are ``O(1)`` average case in python sets.
 * **Space Complexity**: ``O(n)`` where ``n=min(len(set1), len(set2))``

LeetCode's implementation::

    def set_intersection(self, set1, set2):
        return [x for x in set1 if x in set2]
        
    def intersection(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """  
        set1 = set(nums1)
        set2 = set(nums2)
        
        if len(set1) < len(set2):
            return self.set_intersection(set1, set2)
        else:
            return self.set_intersection(set2, set1)

.. code-block:: Java

    // Java
    public int[] set_intersection(HashSet<Integer> set1, HashSet<Integer> set2) {
        int [] output = new int[set1.size()];
        int idx = 0;
        for (Integer s : set1)
        if (set2.contains(s)) output[idx++] = s;

        return Arrays.copyOf(output, idx);
    }

    public int[] intersection(int[] nums1, int[] nums2) {
        HashSet<Integer> set1 = new HashSet<Integer>();
        for (Integer n : nums1) set1.add(n);
        HashSet<Integer> set2 = new HashSet<Integer>();
        for (Integer n : nums2) set2.add(n);

        if (set1.size() < set2.size()) return set_intersection(set1, set2);
        else return set_intersection(set2, set1);
    }

LeetCode's implementation using **built-in** operator::

    def intersection(self, nums1, nums2):
        set1 = set(nums1)
        set2 = set(nums2)
        return list(set2 & set1)

.. code-block:: Java

    // Java
    public int[] intersection(int[] nums1, int[] nums2) {
        HashSet<Integer> set1 = new HashSet<Integer>();
        for (Integer n : nums1) set1.add(n);
        HashSet<Integer> set2 = new HashSet<Integer>();
        for (Integer n : nums2) set2.add(n);

        set1.retainAll(set2);

        int [] output = new int[set1.size()];
        int idx = 0;
        for (int s : set1) output[idx++] = s;
        return output;
    }

Alleged Facebook Solution (cred: `hasan <https://leetcode.com/problems/intersection-of-two-arrays/solution/>`_)::

    function intersections(l1, l2) {
        l1.sort((a, b) => a - b) // assume sorted
        l2.sort((a, b) => a - b) // assume sorted
        const intersections = []
        let l = 0, r = 0;
        while ((l2[l] && l1[r]) !== undefined) {
        const left = l1[r], right = l2[l];
            if (right === left) {
                intersections.push(right)
                while (left === l1[r]) r++;
                while (right === l2[l]) l++;
                continue;
            }
            if (right > left) while (left === l1[r]) r++;
            else while (right === l2[l]) l++;   
            
        }
        return intersections;
    }

Question 24: Longest Common Prefix
----------------------------------------
*Write a function to find the longest common prefix string amongst an array of strings. If there is no common prefix, 
return an empty string* ``""``.

My iterative solution: 

.. code-block:: python
    :linenos:
    
    def longestCommonPrefix(self, strs: List[str]) -> str:
        res = ""
        i = 0
        maxIdx = min(len(s) for s in strs) - 1
        while True: 
            if i > maxIdx: 
                return res
            if all(c==strs[0][i] for c in [s[i] for s in strs]): 
                res += strs[0][i]
                i += 1
            else: 
                return res

Remarks and Complexity Analysis: 
 * Pleased to have applied some of the skills I have learnt through LeetCode in an unseen problem
 * **Time Complexity**: ``O(n*m)`` where ``n=min(len(s))`` - i.e. minimum number of characters in string elements of ``strs``, 
   and ``m=len(strs)``
 * **Space Complexity**: ``O(n)`` where ``n=min(len(s))`` (uncertain)
 * LeetCode solution is not that helpful and is mostly overkill (with worse time/space-complexity)


Day 9 [21 Oct]
========================
Question 25: XXXX
-----------------------------
