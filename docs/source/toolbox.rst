************************
Python Toolkit
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

Performing ``list(d)`` on a dictionary returns a list of all the keys used in the dictionary, in insertion order (if you 
want it sorted, just use ``sorted(d)`` instead). To check whether a single key is in the dictionary, use the ``in`` keyword.


``d.keys()``
    Returns iterator of the dictionary keys (different from list of keys given by ``list(d)``) 

``d.values()``
    Returns iterator of the dictionary values (to get list of values -> ``list(d.values())``) 

``d.get(...)``

List
-----------------------



Specialized Built-in Containers
=========================================
Collections
-------------
Fill in soon `link1 <https://docs.python.org/3/library/collections.html#module-collections>`_


Counter 
----------
Fill in soon `link2 <https://docs.python.org/3/library/collections.html#collections.Counter>`_


