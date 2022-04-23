************************
Week 5 [08-14 Nov 2021]
************************
Day 23 [8 Nov]
================
Question 38: Excel Sheet Column Number
---------------------------------------
*Given a string columnTitle that represents the column title as appear in an Excel sheet, return its corresponding column number.*

My solution:

.. code-block:: python
    :linenos:

    def titleToNumber(self, columnTitle: str) -> int:
        order = len(columnTitle) - 1
        res = 0
        for c in columnTitle:
            res += (26**(order) * (ord(c)-64))
            order -= 1
        return res

Remarks and Complexity Analysis: 
 * Simple question. Significantly easier than implementing the converse operation: :ref:`Question 36: Excel Sheet Column Title`.
 * **Time Complexity**: ``O(n)`` - where ``n=len(columnTitle)-1``
 * **Space Complexity**: ``O(1)`` - only the ``res`` integer field is used for storage. 

Day 24 [9 Nov]
================
Question 39: XXXX
---------------------------------------