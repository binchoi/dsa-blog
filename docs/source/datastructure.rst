********************
Data Structure
********************

**Data Structures**
 * Data structure is a way of organizing data so that it can be used effectively.
 * Cleaner code, faster algorithm, better management and organization of data

Abstract Data Type (ADT)
=========================
An abstract data type is an abstraction of a data structure which provides only the interface to which a data 
structure must adhere to. 

.. list-table:: ADT vs. DS
   :widths: 25 75
   :header-rows: 1

   * - Abstraction (ADT)
     - Implementation (DS)
   * - List
     - Dynamic Array, Linked List
   * - Queue
     - Linked List / Array / Stack based Queue
   * - Map
     - Tree Map, Hash Map/Table

Static and Dynamic Arrays
===========================

Static Array: A fixed length container containing n elements indexable from the range [0,n-1]
Dynamic Array: An array that can shrink or grow in length

Uses: 
 * Storing and accessing sequential data
 * Templorarily storing objects
 * Used by IO routines as buffers
 * Lookup tables and inverse lookup tables
 * Can be used to return multiple values from a function
 * Used in dynamic programming to cache answers to subproblems

.. list-table:: Complexity
   :widths: 25 25 25
   :header-rows: 1

   * - 
     - Static Array
     - Dynamic Array
   * - Access
     - O(1)
     - O(1)
   * - Search
     - O(n)
     - O(n)
   * - Insertion
     - N/A
     - O(n)
   * - Appending
     - N/A
     - O(1)
   * - Deletion
     - N/A
     - O(1)

Dynamic Array Implementation
 1. Create a static array with an initial capacity
 2. Add elements to the underlying static array, keeping track of the number of elements
 3. When capacity is exceeded, create an array double the size and migrate!

Linked List 
============

.. list-table:: Singly vs. Doubly Linked List
   :widths: 33 33 33
   :header-rows: 1

   * - 
     - Pros
     - Cons
   * - Singly Linked
     - Use less memory ; Simpler implementation
     - Cannot easily access previous elements
   * - Doubly Linked
     - Can be traversed backwards
     - 2x memory

.. list-table:: Complexity
   :widths: 25 25 25
   :header-rows: 1

   * - 
     - Singly Linked
     - Doubly Linked
   * - Search
     - O(n)
     - O(n)
   * - Insert at head
     - O(1)
     - O(1)
   * - Insert at tail
     - O(1) or O(n)
     - O(1)
   * - Deletion at head
     - O(1)
     - O(1)
   * - Remove at tail 
     - O(n)
     - O(1)
   * - Remove in middle
     - O(n)
     - O(n)

