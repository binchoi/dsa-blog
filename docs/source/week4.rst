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

        while currNodeOne is not None and currNodeTwo is not None:  
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