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
 * **Space Complexity**: ``O(n)`` where ``n = len(s)`` -- worst-case: the size of ``strippedStr`` is equal to that of ``s``. 

 