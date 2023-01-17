************************
Week 15 [17-22 Jan 2023]
************************
Reignite!

Day 61 [17 Jan]
================
Question 113: Almost Increasing Sequence
------------------------------------------------
Given a sequence of integers as an array, determine whether it is possible to obtain a strictly increasing sequence by removing no more than one element from the array.

Note: sequence a0, a1, ..., an is considered to be a strictly increasing if a0 < a1 < ... < an. Sequence containing only one element is also considered to be strictly increasing.

My naive solution: 

.. code-block:: Python
    :linenos:

    def solution(sequence):
        if len(sequence) < 3:  # base case 
            return True
            
        skip_idx = -1
        for i in range(len(sequence)-1):
            prev = sequence[i-1] if skip_idx == i else sequence[i]
            if sequence[i+1]<=prev:
                if skip_idx != -1:
                    return False
                skip_idx = i if (sequence[i-1]>=sequence[i+1]) else i+1
        return True

Doesn't pass inputs like ``[1, 2, 1, 2]`` and ``[1, 2, 3, 4, 5, 3, 5, 6]``.

My solution: 

.. code-block:: Python
    :linenos:

    def solution(sequence):
        stk = []
        skipped_idx = -1
        
        curr = 0
        while curr < len(sequence):
            if curr != skipped_idx:
                if curr + 1 < len(sequence):
                    if sequence[curr] < sequence[curr+1]:
                        stk.append(sequence[curr])
                    elif skipped_idx != -1:
                        return False
                    else:
                        skipped_idx, item_to_push = get_idx_to_skip(sequence, curr)
                        if skipped_idx == -1:
                            return False
                        stk.append(item_to_push)
                else:
                    stk.append(sequence[curr])
            curr += 1
        return True
                
            
        
    def get_idx_to_skip(sequence, idx) -> (int, int):
        option_one = sequence[idx]
        option_two = sequence[idx + 1]
        left = sequence[idx - 1] if idx >= 1 else -1000000
        right = sequence[idx + 2] if idx < len(sequence) - 2 else 1000000
        
        if left < option_one < right:
            # option one is valid option
            return idx + 1, option_one
        elif left < option_two < right:
            return idx, option_two
        else:
            return -1, -1 # bad

Remarks and Complexity Analysis: 
 * I added the stk for extension of the question (not necessary)
 * **Time Complexity**: ``O(n)`` where ``n=len(sequence)``
 * **Space Complexity**: ``O(1)`` (considering the stack unnecessary and negligible)