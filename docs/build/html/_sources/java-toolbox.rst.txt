****************
My Java Toolbox
****************

.. image:: img/col-img.png
  :width: 1000

Iterable
======================
iterator()
-----------
.. code-block:: Java
    :linenos:

    if (itb.iterator().hasNext()) iterator.next();

forEach()
-----------
.. code-block:: Java
    :linenos:

    itb.forEach(i -> System.out.println(i));

Collection
=============
add() & addAll()
------------------
.. code-block:: Java
    :linenos:

    col.add(e); 
    col.addAll(c);

remove() & removeAll() & removeIf()
------------------------------------
.. code-block:: Java
    :linenos:

    col.remove(o); 
    col.removeAll(c);
    //remove all elements that satisfy the given predicate
    col.removeIf(num -> (num%3==0)); 

contains(o) & containsAll(c)
-----------------------------
.. code-block:: Java
    :linenos:

    col.contains(o); 
    col.containsAll(c);

equals(o)
------------------
.. code-block:: Java
    :linenos:

    col.equals(o); 

isEmpty()
------------------
.. code-block:: Java
    :linenos:

    if (col.isEmpty) return "wow";

hashCode()
------------------
.. code-block:: Java
    :linenos:

    System.out.println("The collection " + col " has the hashcode: " + col.hashCode());

retainAll()
------------------
.. code-block:: Java
    :linenos:

    col1.retainAll(col2);
    //remove all elements from col1 that are not in col2
    //used often with ArrayList, HashSet, etc...

size()
------------------
.. code-block:: Java
    :linenos:

    col1.size();

toArray()
------------------
.. code-block:: Java
    :linenos:

    Object[] arr = col.toArray();
    

Collection.sort(List, Comparator)
---------------------------------------
Similar to ``Arrays.sort()`` but for sorting objects in linked lists, arraylists, queues, etc...

.. code-block:: Java
    :linenos:

    Collections.sort(alphabetList); //default is ascending order
    Collections.sort(alphabetList, Collections.reverseOrder()); //descending order
    Collections.sort(alphabetList, (a,b)->Integer.compare(a[0],b[0])); 
    Collections.sort(alphabetList, (a,b)->a[0]-b[0]);
    list.sort(Comparator.comparing(n -> n.length()));
    

stream() & parallelStream()
----------------------------
.. code-block:: Java
    :linenos:

    int res = col
                .stream()
                .reduce(0, (acc, e) -> acc+e);
    //parallelStream() returns a possibly parallel Stream with this collection as its source. 
    //it may return a sequential stream like .stream()

Stream
=============
A sequence of elements supporting sequential and parallel aggregate operations (e.g. see below). Similar to collection, but they 
do not store their own data, immutable, and lazy. 

.. code-block:: Java
    :linenos:

    int sum = col.stream()
                .filter(e -> e.getColor() == RED)
                .mapToInt(e -> e.getWeight())
                .sum();

Stream.of()
------------------
.. code-block:: Java
    :linenos:

    Stream<Integer> stream = Stream.of(1,2,3,4,5);
    Stream<String> stream = Stream.of("one","two");
    Stream<Integer> stream = Stream.of(myArray);

    //alternatively, directly from list...
    list.stream();

.toArray()
------------------
.. code-block:: Java
    :linenos:

    String[] myArray = stream.toArray(String[]::new); //::new is constructor reference

collect()
------------------
.. code-block:: Java
    :linenos:

    List<String> myList = stream.collect(Collectors.toList());
    Set<String> mySet = stream.collect(Collectors.toSet());

map()
-----
.. code-block:: Java
    :linenos:

    Stream<String> res = list.stream().map(w->w.toLowerCase());
    Stream<String> res = list.stream().map(String::toLowerCase); //equivalent

    res = Stream.of(1,2,3,4,5).map(i -> i+1); //> 2 3 4 5 6

    //without map()
    List<InternetAddress> listOfEmail = new ArrayList<>();
    for (Person p: people) {
        listOfEmail.add(new InternetAddress(p.getEmailAddress()));
    }

    //with map()
    people.stream()
        .map(Person::getEmailAddress)
        .map(InternetAddress::new)
        .collect(Collectors.toList());
    
    //let's see what map() can do...
    assertEquals(Optional.of(Optional.of("STRING")), 
                                                Optional
                                                .of("string")
                                                .map(s -> Optional.of("STRING")));
    //nested optional? that's cumbersome, let's use flatMap()

flatMap()
----------
.. code-block:: Java
    :linenos:

    //useful for flattening
    assertEquals(Optional.of("STRING"), Optional
                                            .of("string")
                                            .flatMap(s -> Optional.of("STRING")));
    

filter()
--------
.. code-block:: Java
    :linenos:

    //filter all strings whose first letter is "f" (i.e. all remaining start with "f")
    Stream<String> res = list.stream().filter(str->str.substring(0,1).equals("f"));
    Stream<Integer> res = list.stream().filter(e -> e<10); //only elements under 10 remain

    List<Character> lst = Arrays.asList('C','a','0', 'B');
    lst = lst.stream().filter(Character::isUpperCase).collect(Collectors.toList());
    lst.forEach(System.out::println);

reduce()
--------
.. code-block:: Java
    :linenos:

    //similar to fold
    int res = stream.reduce(0, (acc, e) -> acc+e);
    String res = stream.reduce("", (acc, e) -> acc+"|"+e);

    //another ex: finding max
    listOfPeople.stream()
        .map(Person::getAge)
        .reduce((max, age) -> age > max ? age : max)
    
    //another ex: concating names
    listOfPeople.stream()
        .map(Person::getName)
        .reduce("Names: ", String::concat)

limit()
--------
.. code-block:: Java
    :linenos:

    res = stream.limit(3);
    //the first 3 elements

distinct()
----------------------------
.. code-block:: Java
    :linenos:

    res = stream.distinct();
    //remove duplicated elements 

sorted()
--------
.. code-block:: Java
    :linenos:

    res = stream.sorted();

allMatch() & anyMatch() & noneMatch()
------------------------------------------------------
.. code-block:: Java
    :linenos:

    boolean isTrue = boolList.stream().anyMatch(e -> e.equals("CHINA")); //if any true -> true
    boolean isTrue = boolList.stream().allMatch(node -> node!=null); //if all true -> true
    boolean res = boolList.stream().noneMatch(e -> e.equals("USA")); //if none true -> true
    
findAny() & findFirst() 
------------------------------------------------------
.. code-block:: Java
    :linenos:

    Optional<Integer> res = list.stream().filter(num->num<4).findAny();
    return res.isPresent() ? res.get() : 0;

    Optional<Integer> res = list.stream().filter(num->num<4).findFirst();

Collectors.groupingBy()
-----------------------------
.. code-block:: Java
    :linenos:

    Map<Integer, List<String>> groups = stream
                                            .collect(Collectors.groupingBy(w -> w.length()));
    //> 4=[Some], 5=[Somee], 6=[Someee, Someee], ...

    Map<Integer, Set<String>> groups = stream
                                            .collect(Collectors.groupingBy(w -> w.length(), Collectors.toSet()));
    //> 4=[Some], 5=[Somee], 6=[Someee], ...

    Map<Integer,Set<String>> map = Stream.of("Some", "Somee", "Someee", "Someee", "Someeeee").collect(Collectors.groupingBy(e -> e.length(), Collectors.toSet()));
    for (int i : map.keySet()) {
        map.get(i).forEach(System.out::println);
    }
    

Array
======================
initialization
---------------
.. code-block:: Java
    :linenos:

    //empty array
    int[] intArr = new int[10];

    //with values
    int[] intArr = new int[] {1,2,3};
    int[] intArr = {1,2,3};

    //with values pt.2 using IntStream
    int[] intArr = IntStream.of(1,7,5,3).toArray();
    int[] intArr = IntStream.of(1,7,5,3).sorted().toArray(); //ascending by default

    //filled with one value
    int[] intArr = new int[10];
    Arrays.fill(intArr, 1); // import java.util.Arrays

    //filled with consecutive values
    int[] intArr = IntStream.range(1,11).toArray(); // import java.util.stream.IntStream

array.asList() vs. ArrayList(Arrays.asList(array))
--------------------------------------------------------
former puts a wrapper on vs. latter actually takes each element and inserts them into a new ArrayList

.. code-block:: Java
    :linenos:

    String[] stringArray = new String[] { "A", "B", "C", "D" };
    List stringList = Arrays.asList(stringArray);
    stringList.set(0, "E");
    stringList.add("F"); // ERROR THIS DOES NOT WORK - FIXED LENGTH

    String[] stringArray = new String[] { "A", "B", "C", "D" }; 
    List stringList = new ArrayList<>(Arrays.asList(stringArray));
    stringList.set(0, "E");
    stringList.add("F"); // :-)

asList() applications
------------------------------------------------------------------------------

**To initialize HashSets, ArrayList, and Queues with value**

.. code-block:: Java
    :linenos:

    List<String> strList = Arrays.asList("1", "3", "5"); //wrapper

    ArrayList<String> arrLst = new ArrayList<>(Arrays.asList("1", "3", "5"));
    HashSet<String> hSet = new HashSet<>(Arrays.asList("1", "3", "5"));
    Queue<String> q = new ArrayDeque<>(Arrays.asList("1", "3", "5"));

common use of toArray()
-------------------------
.. code-block:: Java
    :linenos:

    int[] intArr = intList.toArray(new int[intList.size()]);
    Person[] people = valid.toArray(new Person[valid.size()]);







