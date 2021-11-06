************************
Week 4 [1-7 Nov 2021]
************************
Day 16 [1 Nov]
================
Question 31: Merge Two Sorted Lists
-----------------------------------
*Merge two sorted linked lists and return it as a sorted list. The list should be made by splicing 
together the nodes of the first two lists.*

My iterative solution: 

.. code-block:: python
    :linenos: 

    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        # list: no node => None
        # save head of the merged list to return
        resHead = None
        currNodeOne = l1
        currNodeTwo = l2
        prevNodeRes = None
        if l1 is None and l2 is None: 
            return resHead
        elif l1 is None: 
            return l2
        elif l2 is None: 
            return l1
        else: 
            if l1.val > l2.val: 
                resHead = l2
                currNodeTwo = l2.next
                prevNodeRes = l2
            else: 
                resHead = l1
                currNodeOne = l1.next
                prevNodeRes = l1

        while currNodeOne and currNodeTwo:  
            if currNodeOne.val > currNodeTwo.val: 
                prevNodeRes.next = currNodeTwo
                prevNodeRes = currNodeTwo
                currNodeTwo = currNodeTwo.next
            else: 
                prevNodeRes.next = currNodeOne
                prevNodeRes = currNodeOne
                currNodeOne = currNodeOne.next

        if currNodeOne is None: 
            prevNodeRes.next = currNodeTwo
        else: 
            prevNodeRes.next = currNodeOne
        
        return resHead

Remarks and Complexity Analysis: 
 * The code is quite long. I do think that there is a clear, readable structure to it but I am wondering if there would have been a way 
   to simplify the code. 
 * **Time Complexity**: ``O(n)`` - where ``n=len(l1)+len(l2)`` as all nodes of both lists must be traversed in the worst-case scenario.
 * **Space Complexity**: ``O(1)`` - we need to store ``resHead`` and ``prevNodeRes``.  

Neat recursive solution (cred: `yang <https://leetcode.com/problems/merge-two-sorted-lists/discuss/9715/Java-1-ms-4-lines-codes-using-recursion>`_):

.. code-block:: Java 
        
    public ListNode mergeTwoLists(ListNode l1, ListNode l2){
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        if(l1.val < l2.val){
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else{
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }   

My adaptation:: 

    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        if l1 is None: 
            return l2
        elif l2 is None: 
            return l1 
        else: 
            if l1.val < l2.val: 
                l1.next = self.mergeTwoLists(l1.next, l2)
                return l1
            else: 
                l2.next = self.mergeTwoLists(l1, l2.next)
                return l2

Iterative (in-place) solution (inspired by: `OldCodingFarmer <https://leetcode.com/problems/merge-two-sorted-lists/discuss/9735/Python-solutions-(iteratively-recursively-iteratively-in-place).>`_):: 

    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        sentinel = curr = ListNode(0)
        while l1 and l2: 
            if l1.val < l2.val: 
                curr.next = l1
                l1 = l1.next
            else: 
                curr.next = l2
                l2 = l2.next
            curr = curr.next
        curr.next = l1 or l2
        return sentinel.next

Remarks:
 * Although the actual runtime does not differ too significantly, I noticed interesting tools for implementation in the OldCodingFarmer's solution that I had 
   to give it a try myself:
   
1. ``sentinel = curr = ListNode(0)`` is a clever way to initialize both the ``curr`` traversing pointer-node and the ``sentinel`` node. I feel like I don't use 
   sentinel nodes as often as I should because it clearly simplifies the implementation of certain algorithms. 
2. Using the given parameters ``l1`` and ``l2`` instead of creating new variables would help with memory
3. ``curr.next = l1 or l2`` is a concise way of assigning the next node as the node that is not None (of the two)

Day 17 [2 Nov]
================
Question 32: Merge Sorted Array
-----------------------------------
*You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively. 
Merge nums1 and nums2 into a single array sorted in non-decreasing order. 
The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements 
denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.*

My solution (inspired by `chun <https://leetcode.com/problems/merge-sorted-array/discuss/29522/This-is-my-AC-code-may-help-you>`_): 

.. code-block:: python
    :linenos:

    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        pOne = m-1
        pTwo = n-1
        pRes = m+n-1
        
        while pTwo >= 0: 
            if pOne < 0: 
                nums1[pRes] =  nums2[pTwo]
                pRes -=1
                pTwo -=1
            else: 
                if nums2[pTwo] <= nums1[pOne]: 
                    nums1[pRes] = nums1[pOne]
                    pRes -=1
                    pOne -=1
                else: 
                    nums1[pRes] = nums2[pTwo]
                    pRes -=1
                    pTwo -=1

Remarks and Complexity Analysis: 
 * I am confident that this is the most time-efficient algorithm. The implementation can definitely be cleaned up more though!
 * **Time Complexity**: ``O(n+m)`` - the ``nums1`` is traversed only once from back to front
 * **Space Complexity**: ``O(1)`` - in-place modifications (still uncertain about space complexity) 

Elegant Java solution (cred: `chun <https://leetcode.com/problems/merge-sorted-array/discuss/29522/This-is-my-AC-code-may-help-you>`_):

.. code-block:: Java

    void merge(int A[], int m, int B[], int n) {
        int i=m-1;
		int j=n-1;
		int k = m+n-1;
		while(i >=0 && j>=0)
		{
			if(A[i] > B[j])
				A[k--] = A[i--];
			else
				A[k--] = B[j--];
		}
		while(j>=0)
			A[k--] = B[j--];
    }

Elegant Python Solution (cred: `cffls <https://leetcode.com/problems/merge-sorted-array/discuss/29503/Beautiful-Python-Solution>`_):: 

    def merge(self, nums1, m, nums2, n):
        while m > 0 and n > 0:
            if nums1[m-1] >= nums2[n-1]:
                nums1[m+n-1] = nums1[m-1]
                m -= 1                      # modify given param/variables
            else:
                nums1[m+n-1] = nums2[n-1]
                n -= 1
        if n > 0:
            nums1[:n] = nums2[:n]           # modify in bulk!


Day 18 [3 Nov]
================
Question 33: Valid Palindrome
-----------------------------------
*A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, 
it reads the same forward and backward. Alphanumeric characters include letters and numbers. 
Given a string s, return true if it is a palindrome, or false otherwise.*

My solution: 

.. code-block:: python
    :linenos:

    def isPalindrome(self, s: str) -> bool:
        strippedStr = "".join([c for c in s.lower() if c.isalnum()])
        return (strippedStr == strippedStr[::-1])

Remarks and Complexity Analysis: 
 * Pretty simple question - learned about ``.isalnum()``
 * **Time Complexity**: ``O(n)`` where ``n = len(s)`` each character is traversed at least once. 
 * **Space Complexity**: ``O(n)`` where ``n = len(s)`` - worst-case: the size of ``strippedStr`` is equal to that of ``s``. 

Day 19 [4 Nov]
================
Question 34: Top K Frequent Elements
--------------------------------------
*Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.*

My solution: 

.. code-block:: python
    :linenos: 

    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        myDict = {n:nums.count(n) for n in set(nums)}
        return [key for (key,v) in sorted(myDict.items(), key= lambda x: x[1], reverse = True)[:k]] 

Remarks and Complexity Analysis: 
 * Surprisingly slow performance (as compiled in leetcode server). Perhaps due to the sorting?
 * **Time Complexity**: ``O(nlog(n))`` where ``n = len(nums)`` - worst-case: sorting ``n`` items (all unique)
 * **Space Complexity**: ``O(n)`` where ``n = len(nums)`` - worst-case: ``myDict`` contains ``n`` items (all unique)

 LeetCode's solution 1: Heap

.. code-block:: python

   from collections import Counter
   def topKFrequent(self, nums: List[int], k: int) -> List[int]: 
       # O(1) time 
       if k == len(nums):
           return nums
       
       # 1. build hash map : character and how often it appears
       # O(N) time
       count = Counter(nums)   
       # 2-3. build heap of top k frequent elements and
       # convert it into an output array
       # O(N log k) time
       return heapq.nlargest(k, count.keys(), key=count.get) 

.. code-block:: Java

    // Java
    public int[] topKFrequent(int[] nums, int k) {
        // O(1) time
        if (k == nums.length) {
            return nums;
        }
        
        // 1. build hash map : character and how often it appears
        // O(N) time
        Map<Integer, Integer> count = new HashMap();
        for (int n: nums) {
          count.put(n, count.getOrDefault(n, 0) + 1);
        }

        // init heap 'the less frequent element first'
        Queue<Integer> heap = new PriorityQueue<>(
            (n1, n2) -> count.get(n1) - count.get(n2));

        // 2. keep k top frequent elements in the heap
        // O(N log k) < O(N log N) time
        for (int n: count.keySet()) {
          heap.add(n);
          if (heap.size() > k) heap.poll();    
        }

        // 3. build an output array
        // O(k log k) time
        int[] top = new int[k];
        for(int i = k - 1; i >= 0; --i) {
            top[i] = heap.poll();
        }
        return top;
    } 

Remarks and Complexity Analysis: 
 * Effective ``O(1)`` best-case implementation (first two lines)
 * In Python, library ``heapq`` provides a method ``nlargest``, which combines the last two steps under the hood and has 
   the same :math:`\mathcal{O}(N \log k)` time complexity.
 * **Time Complexity**: ``O(nlog(k))`` 
 * **Space Complexity**: ``O(n+k)`` to store a hash map with no more than ``n`` elements and a heap with ``k`` elements.

.. note:: 

    ``collections.Counter`` is a dictionary subclass for counting hashable objects. Elements are stored as dictionary keys 
    and their counts are stored as dictionary values. Counts are allowed to be any integer value including zero or 
    negative counts. The Counter class is similar to bags or multisets in other languages. Please refer to the summary I created 
    at :ref:`Counter`.
 

LeetCode's solution 2: Quickselect (Hoare's selection algorithm)

**From LeetCode:** Quickselect is a textbook algorthm typically used to solve the problems "find ``k`` th something": ``k`` th smallest, ``k`` th largest, ``k`` th most 
frequent, ``k`` th less frequent, etc. Like quicksort, quickselect was developed by Tony Hoare, and also known as Hoare's selection algorithm.

It has :math:`\mathcal{O}(N)` average time complexity and widely used in practice. It worth to note that its worth case time complexity 
is :math:`\mathcal{O}(N^2)`, although the probability of this worst-case is negligible.

As an output, we have an array where the pivot is on its perfect position in the ascending sorted array, sorted by the frequency. 
All elements on the left of the pivot are less frequent than the pivot, and all elements on the right are more frequent or have the same frequency.

Hence the array is now split into two parts. If by chance our pivot element took ``N - k`` th final position, then kk elements on the right are 
these top k frequent we're looking for. If not, we can choose one more pivot and place it in its perfect position.

If that were a quicksort algorithm, one would have to process both parts of the array. That would result in :math:`\mathcal{O}(N \log N)` 
time complexity. In this case, there is no need to deal with both parts since one knows in which part to search for N - kth less frequent 
element, and that reduces the average time complexity to :math:`\mathcal{O}(N)`.

Algorithm
 * Build a hash map element -> its frequency and convert its keys into the array unique of unique elements. Note that elements are 
   unique, but their frequencies are not. That means we need a partition algorithm that works fine with duplicates.
 * Work with unique array. Use a partition scheme (please check the next section) to place the pivot into its perfect position 
   pivot_index in the sorted array, move less frequent elements to the left of pivot, and more frequent or of the same frequency - to the right.
 * Compare pivot_index and N - k:
 * If pivot_index == N - k, the pivot is N - kth most frequent element, and all elements on the right are more frequent or of the same frequency. 
   Return these top k frequent elements.
 * Otherwise, choose the side of the array to proceed recursively.

.. code-block:: python

   from collections import Counter
   def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)
        unique = list(count.keys())
        
        def partition(left, right, pivot_index) -> int:
            pivot_frequency = count[unique[pivot_index]]
            # 1. move pivot to end
            unique[pivot_index], unique[right] = unique[right], unique[pivot_index]  
            
            # 2. move all less frequent elements to the left
            store_index = left
            for i in range(left, right):
                if count[unique[i]] < pivot_frequency:
                    unique[store_index], unique[i] = unique[i], unique[store_index]
                    store_index += 1

            # 3. move pivot to its final place
            unique[right], unique[store_index] = unique[store_index], unique[right]  
            
            return store_index
        
        def quickselect(left, right, k_smallest) -> None:
            """
            Sort a list within left..right till kth less frequent element
            takes its place. 
            """
            # base case: the list contains only one element
            if left == right: 
                return
            
            # select a random pivot_index
            pivot_index = random.randint(left, right)     
                            
            # find the pivot position in a sorted list   
            pivot_index = partition(left, right, pivot_index)
            
            # if the pivot is in its final sorted position
            if k_smallest == pivot_index:
                 return 
            # go left
            elif k_smallest < pivot_index:
                quickselect(left, pivot_index - 1, k_smallest)
            # go right
            else:
                quickselect(pivot_index + 1, right, k_smallest)
         
        n = len(unique) 
        # kth top frequent element is (n - k)th less frequent.
        # Do a partial sort: from less frequent to the most frequent, till
        # (n - k)th less frequent element takes its place (n - k) in a sorted array. 
        # All element on the left are less frequent.
        # All the elements on the right are more frequent.  
        quickselect(0, n - 1, n - k)
        # Return top k frequent elements
        return unique[n - k:]

.. code-block:: Java

    // Java
    int[] unique;
    Map<Integer, Integer> count;

    public void swap(int a, int b) {
        int tmp = unique[a];
        unique[a] = unique[b];
        unique[b] = tmp;
    }

    public int partition(int left, int right, int pivot_index) {
        int pivot_frequency = count.get(unique[pivot_index]);
        // 1. move pivot to end
        swap(pivot_index, right);
        int store_index = left;

        // 2. move all less frequent elements to the left
        for (int i = left; i <= right; i++) {
            if (count.get(unique[i]) < pivot_frequency) {
                swap(store_index, i);
                store_index++;
            }
        }

        // 3. move pivot to its final place
        swap(store_index, right);

        return store_index;
    }
    
    public void quickselect(int left, int right, int k_smallest) {
        /*
        Sort a list within left..right till kth less frequent element
        takes its place. 
        */

        // base case: the list contains only one element
        if (left == right) return;
        
        // select a random pivot_index
        Random random_num = new Random();
        int pivot_index = left + random_num.nextInt(right - left); 

        // find the pivot position in a sorted list
        pivot_index = partition(left, right, pivot_index);

        // if the pivot is in its final sorted position
        if (k_smallest == pivot_index) {
            return;    
        } else if (k_smallest < pivot_index) {
            // go left
            quickselect(left, pivot_index - 1, k_smallest);     
        } else {
            // go right 
            quickselect(pivot_index + 1, right, k_smallest);  
        }
    }
    
    public int[] topKFrequent(int[] nums, int k) {
        // build hash map : character and how often it appears
        count = new HashMap();
        for (int num: nums) {
            count.put(num, count.getOrDefault(num, 0) + 1);
        }
        
        // array of unique elements
        int n = count.size();
        unique = new int[n]; 
        int i = 0;
        for (int num: count.keySet()) {
            unique[i] = num;
            i++;
        }
        
        // kth top frequent element is (n - k)th less frequent.
        // Do a partial sort: from less frequent to the most frequent, till
        // (n - k)th less frequent element takes its place (n - k) in a sorted array. 
        // All element on the left are less frequent.
        // All the elements on the right are more frequent. 
        quickselect(0, n - 1, n - k);
        // Return top k frequent elements
        return Arrays.copyOfRange(unique, n - k, n);
    }

Remarks and Complexity Analysis: 
 * Effective ``O(n)`` average-case ``O(n)`` implementation (first two lines)
 * In Python, library ``heapq`` provides a method ``nlargest``, which combines the last two steps under the hood and has 
   the same :math:`\mathcal{O}(N \log k)` time complexity.
 * **Time Complexity**: average-case - ``O(n)`` ; worst-case - ``O(n^2)`` : In the worst-case of constantly bad chosen pivots, the 
   problem is not divided by half at each step, it becomes just one element less, that leads to :math:`\mathcal{O}(N^2)` time 
   complexity. It happens, for example, if at each step you choose the pivot not randomly, but take the rightmost element. For 
   the random pivot choice the probability of having such a worst-case is negligibly small.
 * **Space Complexity**: ``O(n)`` to store hash map and array of unique elements.

An ``O(n)`` solution: Bucket Sort (cred: `DBabi <https://leetcode.com/problems/top-k-frequent-elements/discuss/740374/Python-5-lines-O(n)-buckets-solution-explained>`_):: 

    def topKFrequent(self, nums, k):
        bucket = [[] for _ in range(len(nums) + 1)]
        Count = Counter(nums).items()  
        for num, freq in Count: bucket[freq].append(num) 
        flat_list = [item for sublist in bucket for item in sublist]
        return flat_list[::-1][:k]


Day 20 [5 Nov]
================
Question 35: Remove Duplicates from Sorted List II
----------------------------------------------------
*Given the head of a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct 
numbers from the original list. Return the linked list sorted as well.*

My failed solution:: 
    
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummyHead = ListNode()
        shift = dummyHead
        curr = head
        while curr: 
            if not curr.next: 
                shift.next = curr
                return dummyHead.next
            if curr.val != curr.next.val: 
                shift.next = curr
                shift = curr
                curr = curr.next
                continue
            else: 
                curr_val = curr.val
                while curr_val == curr.val: 
                    curr = curr.next
                    if not curr: 
                        break
        return dummyHead.next

LeetCode's Solution 1: Sentinel Head + Predecessor

*LeetCode:* Sentinel nodes are widely used for trees and linked lists as pseudo-heads, pseudo-tails, etc. They are 
purely functional and usually don't hold any data. Their primary purpose is to standardize the situation to avoid edge 
case handling. For example, let's use here pseudo-head with zero value to ensure that the situation "delete the list 
head" could never happen, and all nodes to delete are "inside" the list.

.. code-block:: python

    def deleteDuplicates(self, head: ListNode) -> ListNode:
        # attach sentinel head to the given list
        sentinel = ListNode(0, head)

        # predecessor = the last node before the sublist of duplicates
        pred = sentinel
        
        while head:
            # if it's a beginning of duplicates sublist 
            # skip all duplicates
            if head.next and head.val == head.next.val:
                # move till the end of duplicates sublist
                while head.next and head.val == head.next.val:
                    head = head.next
                # skip all duplicates
                pred.next = head.next 
            # otherwise, move predecessor
            else:
                pred = pred.next 
                
            # move forward
            head = head.next
            
        return sentinel.next

.. code-block:: Java
    
    // Java
    public ListNode deleteDuplicates(ListNode head) {
        // sentinel
        ListNode sentinel = new ListNode(0, head);

        // predecessor = the last node 
        // before the sublist of duplicates
        ListNode pred = sentinel;
        
        while (head != null) {
            // if it's a beginning of duplicates sublist 
            // skip all duplicates
            if (head.next != null && head.val == head.next.val) {
                // move till the end of duplicates sublist
                while (head.next != null && head.val == head.next.val) {
                    head = head.next;    
                }
                // skip all duplicates
                pred.next = head.next;     
            // otherwise, move predecessor
            } else {
                pred = pred.next;    
            }
                
            // move forward
            head = head.next;    
        }  
        return sentinel.next;
    }

Remarks and Complexity Analysis: 
 * It's always important to think about the algorithm critically and extensively before implementing it with code. The more 
   you take to think about the problem and your algorithm (without coding), the more structured, organized and efficient your 
   code will be. 
 * **Time Complexity**: ``O(n)`` - one pass through the list!
 * **Space Complexity**: ``O(1)`` - we don't allocate any additional data structure

