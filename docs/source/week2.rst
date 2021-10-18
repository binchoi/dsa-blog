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
Question 19: XYZ
----------------------------