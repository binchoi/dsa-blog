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


Day 62 [18 Jan]
================
Redirect to Study Repo
-----------------------

I've been recording a lot of my solutions to algorithm problems in my ``study`` repository (`link <https://github.com/binchoi/study/tree/master/python/05-top-leet-questions>`_).


As of 2023/02/16, the repository contains my solutions to the following problems.

.. code-block:: Yaml

    best-time-to-buy-sell-stock-2.py
    contains-duplicates.py
    intersection-of-two-arrays-ii.py
    move-zeros.py
    plus-one.py
    remove-duplicates-from-sorted-array.py
    rotate-array.py
    rotate-image.py
    single-number.py
    two-sum.py
    valid-sudoku.py
    first-unique-character-in-a-string.py
    implement-strstr.py
    longest-common-prefix.py
    reverse-integer.py
    reverse-string.py
    string-to-integer-atoi.py
    valid-anagram.py
    valid-palindrome.py
    linked-list-cycle.py
    merge-two-sorted-list.py
    reverse-linked-list.py
    binary-tree-level-order-traversal.py
    convert-sorted-array-to-bst.py
    maximum-depth-of-binary-tree.py
    tree-traversal-guide.py
    util.py
    validate-binary-search-tree.py
    best-time-to-buy-and-sell-stock.py
    climbing-stairs.py
    maximum-subarray.py
    count-primes.py
    fizzbuzz.py
    power-of-three.py
    first-bad-version.py
    min-stack.py
    shuffle-an-array.py
    number-of-1-bits.py
    pascals-triangle.py
    reverse-bits.py
    valid-parenthesis.py
    group-anagrams.py
    longest-palindromic-substring.py
    longest-substring-without-repeating-chars.py
    add-two-numbers.py
    intersection-of-two-linked-lists.py
    binary-tree-in-order-traversal.py
    number-of-islands.py
    populating-next-right-pointers-in-each-node.py
    generate-parenthesis.py
    letter-combinations-of-a-phone-number.py
    palindrome-partitioning.py
    permutations-ii.py
    permutations.py
    subsets.py
    merge-intervals.py
    search-for-a-range.py
    search-in-rotated-array.py
    sort-colors.py
    unique-paths.py
    excel-sheet-column-number.py
    majority-element.py
    pow(x,n).py
    scientific-notation.py
    choosing-houses.py
    continguous-subarray-twosum.py
    digit-anagrams.py
    pattern-substrings.py