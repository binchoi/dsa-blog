************************
Python Toolbox
************************
General Purpose Built-in Containers
=========================================
Dictionary
------------
[From Python Documentation:] Dictionaries are sometimes found in other languages as “associative memories” or “associative arrays”. Unlike sequences, 
which are indexed by a range of numbers, dictionaries are indexed by keys, which can be any *immutable type*; strings and 
numbers can always be keys. Tuples can be used as keys if they contain only strings, numbers, or tuples; if a tuple 
contains any mutable object either directly or indirectly, it cannot be used as a key. You can’t use lists as keys, 
since lists can be modified in place using index assignments, slice assignments, or methods like ``append()`` and ``extend()``.

It is best to think of a dictionary as a *set of key: value pairs*, with the requirement that the keys are unique 
(within one dictionary). A pair of braces creates an empty dictionary: ``{}``. Placing a comma-separated list of key:value 
pairs within the braces adds initial key:value pairs to the dictionary; this is also the way dictionaries are written on 
output.

The main operations on a dictionary are storing a value with some key and extracting the value given the key. It is also 
possible to delete a key:value pair with ``del``. If you store using a key that is already in use, the old value associated 
with that key is forgotten. It is an error to extract a value using a non-existent key.

Performing ``list(d)`` on a dictionary returns a **list of all the keys** used in the dictionary, in insertion order (if you 
want it sorted, just use ``sorted(d)`` instead). To check whether a single key is in the dictionary, use the ``in`` keyword.


.. code-block:: python
    :linenos: 

    # to instantiate (common)
    tel = {'jack': 4098, 'sape': 4139, 'john': 123, 'cena': 0}
    dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])  # from a sequence of kv pairs
    {x: x**2 for x in (2, 4, 6)}  # dict comprehension

    # to instantiate (variety)
    a = dict(one=1, two=2, three=3)
    b = {'one': 1, 'two': 2, 'three': 3}
    c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
    d = dict([('two', 2), ('one', 1), ('three', 3)])
    e = dict({'three': 3, 'one': 1, 'two': 2})
    f = dict({'one': 1, 'three': 3}, two=2)
    a == b == c == d == e == f  # returns True

    # to get key list
    list(tel.keys())
    list(tel)    # ['jack', 'sape', 'john', 'cena']
    sorted(tel)  # ['cena', 'jack', 'john', 'sape']

    # to get key view/iterator
    d.keys()

    # to get val list
    list(d.values())

    # to get val view/iterator
    d.values()

    # to add kv-pair
    tel['guido'] = 4128

    # to get - 2 ways
    tel['jack']      # if non-existant, raises KeyError
    tel.get('jack2') # if non-existant, returns None

    # to delete
    del tel['sape']  # if non-existant, raises KeyError

    # to pop (delete & return val)
    tel.pop('jack2')  # if non-existant, raises KeyError
    tel.pop('jack2', default=0)  # returns default

    # to stack pop (LIFO) [from version 3.7: LIFO order is now guaranteed]
    tel['just_pushed'] = 1
    tel.popitem()  # returns ('just_pushed', 1) tuple; raises KeyError if empty

    # to check if key in dict
    'guido' in tel
    'jack' not in tel

    # to get (key, value) pair view/iterator
    for k, v in tel.items():
        print(k, v, sep=" -> ")

    # to set default
    tel.setdefault('oh', default=1)  # if 'oh' exists, return its val, else set val to 1
    tel.setdefault('asdf')  # default value is None


List
-----------------------

.. code-block:: python
    :linenos: 

    # to instantiate (common)
    fruits = ['orange', 'apple', 'pear', 'banana', 'kiwi', 'apple', 'banana']

    # to count (freq of particular item)
    fruits.count('apple')  # 2

    # to find the index of item using list.index(x[, start[, end]])
    fruits.index('banana')  # first occurance -> 3, raise ValueError if absent
    fruits.index('banana', 4)  # find occurance starting at position 4 -> 6, raise ValueError if absent

    # to reverse list in-place
    fruits.reverse()
    fruits  # ['banana', 'apple', 'kiwi', 'banana', 'pear', 'apple', 'orange']

    # to append
    fruits.append('grape')  # ['banana', 'apple', 'kiwi', 'banana', 'pear', 'apple', 'orange', 'grape']

    # to insert (first arg = idx of element before which to insert)
    fruits.insert(0, "jackfruit")  # insert "jackfruit" to beginning of list
    fruits.insert(len(fruits), "passionfruit")  # identical to append
    
    # to sort in-place (default: ascending)
    fruits.sort()
    fruits  # ['apple', 'apple', 'banana', 'banana', 'grape', 'kiwi', 'orange', 'pear']
    fruits.sort(reverse=True)  # in descending
    fruits.sort(key=lambda x: len(x))  # define custom key (e.g. length of str)

    # to pop
    fruits.pop()  # remove the item at end of list, and return it
    fruits.pop(1) # remove the item at index 1, and return it

    # to remove (by element, not idx)
    fruits.remove("banana")  # remove first instance of "banana", raise ValueError if absent

    # to extend
    fruits.extend(["more", "fruits", "to", "add"])  # identical to a[len(a):] = iterable

    # to clear
    fruits.clear()
    del fruits[:]  # identical to above


When creating multi-dimensional arrays (i.e. matrices), be cautious of using multiplication:

.. code-block:: python
    :linenos: 

    # multiplication is okay when dealing with immutables (e.g. int)
    a = [0] * 5  # [0, 0, 0, 0, 0]
    a[1] = 1
    a   # [0, 1, 0, 0, 0] 

    # but when with mutables (like list)...
    t = [[]] * 5
    t  # [[], [], [], [], []]
    t[0].append(5)
    t  # [[5], [5], [5], [5], [5]]  - Uh Oh!

    # CORRECT WAY
    matrix = [[0]*5 for _ in range(5)]
    [[0 for _ in range(2)] for _ in range(5)]  # also correct

    mat = np.array([0]*size).reshape(rows,cols)  # using numpy

.. note:: 

    The runtime complexity of the len() function on your Python list is O(1). Because the list object maintains an integer counter that increases and decreases as you add and remove list elements. 
 


Using Lists as Stacks
-----------------------
.. code-block:: python
    :linenos: 

    stack = [3, 4, 5]
    stack.append(6)
    stack.append(7)
    stack        # [3, 4, 5, 6, 7]
    stack.pop()  # 7
    stack        # [3, 4, 5, 6]
    stack.pop()  # 6
    stack.pop()  # 5
    stack        # [3, 4]


Using Lists as Queues
-----------------------
It is also possible to use a list as a queue, where the first element added is the first element retrieved (“first-in, first-out”); however, **lists are not efficient for this purpose**. While appends and pops from the end of list are fast, doing **inserts or pops from the beginning of a list is slow** (because all of the other elements have to be shifted by one).

To implement a queue, use collections.deque which was designed to have fast appends and pops from both ends. For example:

.. code-block:: python
    :linenos: 

    from collections import deque
    queue = deque(["Eric", "John", "Michael"])
    queue.append("Terry")           # Terry arrives
    queue.append("Graham")          # Graham arrives
    queue.popleft()                 # The first to arrive now leaves
    # 'Eric'
    queue.popleft()                 # The second to arrive now leaves
    # 'John'
    queue                           # Remaining queue in order of arrival
    # deque(['Michael', 'Terry', 'Graham'])




Specialized Built-in Containers
=========================================
Collections
-------------
Fill in soon `link1 <https://docs.python.org/3/library/collections.html#module-collections>`_


Counter 
----------
Fill in soon `link2 <https://docs.python.org/3/library/collections.html#collections.Counter>`_


