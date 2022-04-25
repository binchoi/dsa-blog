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
    
    Iterable itr = itb.iterator(); // (?) Check type declaration syntax
    if (itr.hasNext()) itr.next();

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

    if (col.isEmpty()) return "wow";

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

mapToInt() [returns IntStream]
-------------------------------
.. code-block:: Java
    :linenos:

    IntStream intStream = Stream.of(1,2,3,4)
            .mapToInt(e->e);
    int[] intArr = Stream.of(1,2,3,4)
            .mapToInt(e -> (int) e)
            .toArray();


Stream to List vs. Array
-------------------------------
.. code-block:: Java
    :linenos:

    List<String> lst = Stream.of("123", "456")
            .map(e -> e.toUpperCase())
            .collect(Collectors.toList());
    String[] arr = Stream.of("123", "456")
            .map(e -> e.toUpperCase())
            .toArray(String[]::new);

List vs. Array to Stream
-------------------------------
.. code-block:: Java
    :linenos:

    lst.stream();
    Arrays.stream(arr);

    // ex
    lst.stream()
            .forEach(System.out::println);
    Arrays.stream(arr)
            .forEach(System.out::println);

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

Arrays.asList() vs. ArrayList(Arrays.asList(array))
--------------------------------------------------------
former puts a wrapper on vs. latter actually takes each element and inserts them into a new ArrayList

.. code-block:: Java
    :linenos:

    String[] stringArray = new String[] { "A", "B", "C", "D" }; 
    List<String> stringList = Arrays.asList(stringArray); // works ONLY FOR OBJECTS (int[] will cause error)
    stringList.set(0, "E");
    stringList.add("F"); // ERROR THIS DOES NOT WORK - FIXED LENGTH

    String[] stringArray = new String[] { "A", "B", "C", "D" }; 
    ArrayList<String> stringList = new ArrayList<>(Arrays.asList(stringArray));
    stringList.set(0, "E");
    stringList.add("F"); // :-)

Arrays.asList() applications
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

int[] to ArrayList<Integer>
---------------------------
.. code-block:: Java
    :linenos:

    ArrayList<Integer> arrList = IntStream.of(array)
                                    .boxed()
                                    .collect(Collectors.toCollection(ArrayList::new));
    HashSet<String> hashSet = lstOfString.stream()
                                    .collect(Collectors.toCollection(HashSet::new));
    //to demonstrate...
    IntStream.of(new int[] {1,2,3,4,5})
        .boxed()
        .collect(Collectors.toCollection(HashSet::new))
        .forEach(System.out::println);

    List<String> lstOfString = Arrays.asList("AHEHEF", "AHEHEF", "EFEF");

    HashSet<String> hashSet = lstOfString.stream().collect(Collectors.toCollection(HashSet::new));
    hashSet.forEach(System.out::println);
    
    ArrayList<String> arrList = lstOfString.stream().collect(Collectors.toCollection(ArrayList::new));
    arrList.forEach(System.out::println);

[Static method] Arrays.sort()
-------------------------------
.. code-block:: Java
    :linenos:

    Arrays.sort(temp);

[Static method] Arrays.copyOfRange()
-------------------------------------
.. code-block:: Java
    :linenos:

    int[] temp = Arrays.copyOfRange(array, start, end);


String
======================
char[] to String
---------------- 
.. code-block:: Java
    :linenos:

    char[] hello = {'c', 'o', 'd'}; // char[] is essentially String, so... 
    String helloo = new String(hello);

charAt()
---------------- 
.. code-block:: Java
    :linenos:

    string.charAt(0);

concat()
---------------- 
.. code-block:: Java
    :linenos:

    strOne.concat(strTwo); // strOne+strTwo 
    String abc = "abc";
    String cde = "cde";
    System.out.println(abc.concat(cde)); //abccde
    System.out.println(abc);             //abc

StringBuilder
---------------- 
.. code-block:: Java
    :linenos:

    // if regular Strings were used => O(xn^2) time
    public static String joinWords(String[] words) {
        StringBuilder builder = new StringBuilder(); //resizable String / CharArray
        for (String w: words) {
            builder.append(w);
        }
        return builder.toString();
    }


length()
---------------- 
.. code-block:: Java
    :linenos:

    String cde = "cde";
    cde.length(); // for ARRAY -> .length and for COLLECTIONS -> .size()

replace() && replaceAll() && replaceFirst()
-------------------------------------------- 
.. code-block:: Java
    :linenos:

    String cde = "cdefghijklmnop";
    //both replace all occurences of the pattern but replaceAll uses regular expression
    cde.replace("cd", "  ");
    cde.replaceAll("[^a-f]", "");
    //replaces only first occurance - uses regex
    cde.replaceFirst("[^a-f]", "");

split()
---------------- 
.. code-block:: Java
    :linenos:

    String str = "cdKef gh_ijKkl mnK*op";

    String[] strArr = str.split(" ");
    Arrays.stream(strArr).forEach(System.out::println);

    String[] strArr2 = str.split("[K*-_]");
    Arrays.stream(strArr2).forEach(System.out::println);

    //cdKef
    //gh_ijKkl
    //mnK*op

    //cd
    //ef gh
    //ij
    //kl mn
    //    <- empty String
    //op

startsWith()
---------------- 
.. code-block:: Java
    :linenos:

    String str = "cdKef gh_ijKkl mnK*op";
    
    str.startsWith("cdK"); //true 

substring()
---------------- 
.. code-block:: Java
    :linenos:

    String str = "abcdefghijk";
    str.substring(3,6); // incl, excl -> "def"

toCharArray()
---------------- 
.. code-block:: Java
    :linenos:

    String str = "abcdefghijk";
    str.toCharArray();
    
toLowerCase() && toUpperCase()
-------------------------------------------- 
.. code-block:: Java
    :linenos:

    String str = "abcdefghijk";
    String lower = str.toLowerCase();
    String upper = lower.toUpperCase();

trim()
-------------------------------------------- 
.. code-block:: Java
    :linenos:

    String str = " abcdefghijk   ";
    String trim = str.trim(); // "abcdefghijk"

[Static method] valueOf()
-------------------------------------------- 
.. code-block:: Java
    :linenos:

    String.valueOf(1); // "1"
    String.valueOf(false); // "false"
    String.valueOf('a'); // "a"
    String.valueOf(3L); // "3"
    
repeat()
------------------------------------------- 
.. code-block:: Java
    :linenos:

    // Equivalent to Python's 3*"a" = "aaa"
    String str = "asdsa";
    String longerStr = str.repeat(5);
    

Integer
======================
Integer to int (vice versa)
----------------------------- 
.. code-block:: Java
    :linenos:

    Integer b = Integer.valueOf(5); // int to Integer
    b.intValue(); // Integer to int

    // auto boxing and auto unboxing works most of the time
    int a = integer; 
    Integer c = a;

Integer to Binary (vice versa)
---------------------------------
.. code-block:: Java
    :linenos:

    // int to binString
    Integer.toBinaryString(someInt); // no padding in the binary representation

    // binString to int
    int foo = Integer.parseInt("1001", 2);


ArrayList
======================
Resizable arrays, unlike arrays which are of fixed length. 

Still ``O(1)`` access, amortized insertion ``O(1)`` (think of time it takes to add ``n`` elements - doubling operations contribute 
n/2, n/4, ..., 1 which sum to ``O(n)`` which averages to ``O(1)`` per single insertion operation). Deletion is ``O(1)``.

add() && remove() && get()
----------------------------- 
.. code-block:: Java
    :linenos:

    ArrayList<Integer> arrList = new ArrayList<Integer>(Arrays.asList(1,2,3,4));
    arrList.add(8);
    arrList.remove(0); // idx
    arrList.add(99);
    arrList.get(0);

set()
----------------------------- 
.. code-block:: Java
    :linenos:

    ArrayList<Integer> arrList = new ArrayList<Integer>(Arrays.asList(1,2,3,4));
    arrList.set(0, 100) // param: idx, new_element

removeIf()
----------------------------- 
.. code-block:: Java
    :linenos:

    ArrayList<Integer> arrList = new ArrayList<Integer>(Arrays.asList(1,2,3,4));
    arrList.add(8);
    arrList.set(0, 100) // param: idx, new_element

**EDIT THIS**


Math
======================
import Math
------------
.. code-block:: Java
    :linenos:
    
    import static java.lang.Math.*;

Math.abs()
----------------------------- 
.. code-block:: Java
    :linenos:

    int res = Math.abs(a - b); //param: int, long, float, double

Math.max()
----------------------------- 
.. code-block:: Java
    :linenos:

    int res = Math.max(a, b); //param: int, long, float, double

Math.min()
----------------------------- 
.. code-block:: Java
    :linenos:

    int res = Math.min(a, b); //param: int, long, float, double

Math.floorDiv()
----------------------------- 
.. code-block:: Java
    :linenos:

    int res = Math.floorDiv(4, 3); //param: int, long ; 1

Math.round()
----------------------------- 
.. code-block:: Java
    :linenos:

    int res = Math.round(7.43f); //param: float, double
    // Returns the closest int to the argument, with ties rounding to positive infinity

Math.signum()
----------------------------- 
.. code-block:: Java
    :linenos:

    int res = Math.signum(7.43f); //param: float, double -> 1
    int res = Math.signum(-1); // -> -1
    // Returns the signum function of the argument; zero if the argument is zero, 1.0 if 
    // the argument is greater than zero, -1.0 if the argument is less than zero.

Math.ceil() && Math.floor()
----------------------------- 
.. code-block:: Java
    :linenos:

    int res = Math.ceil(x); //param: double
    int res = Math.floor(x); //param: double

Math.pow() && Math.sqrt()
----------------------------- 
.. code-block:: Java
    :linenos:

    int res = Math.pow(5,2); //25
    int a = Math.sqrt(res); //5

double & float both represent floating point numbers, but double is more precise



Stack
======================
import Stack
---------------
.. code-block:: Java
    :linenos:

    import java.util.Stack;

push() && pop() && peek()
--------------------------
.. code-block:: Java
    :linenos:

    Stack<Integer> stk = new Stack<>();
    stk.push(10);
    int a = stk.peek(); 
    int b = stk.pop();
    assert(a == b);
    assert(stk.isEmpty());

Queue
======================
import Queue
---------------
.. code-block:: Java
    :linenos:

    import java.util.LinkedList;

add() & remove() & peek()
--------------------------
.. code-block:: Java
    :linenos:

    Queue<Integer> q = new LinkedList<>();
    q.add(10); // enqueue; add to the END of the list
    q.add(11); 
    int a = stk.peek(); // return the FRONT/TOP of the list - i.e. 10
    int b = stk.remove(); // dequeue ; remove th FRONT of the list - i.e. 10
    assert(a == b);
    assert(!stk.isEmpty());


https://leetcode.com/problems/reverse-linked-list/




Conversions
======================
T[] to List<T>
-----------------------
.. code-block:: Java
    :linenos:

    Integer[] intArr = {1,2,3}; // needs to be boxed
    List<Integer> intList = Arrays.asList(intArr); // cannot add elements
    HashList<Integer> intList2 = new ArrayList<>(Arrays.asList(intArr)); // fully functioning arraylist
    Stream.of(strArray)
        .collect(Collectors.toList());
    
**RETURN**


IntStream
======================
Creating IntStream: IntStream.of(...)
------------------------------------------
.. code-block:: Java
    :linenos:

    IntStream intStream = IntStream.of(1,2,3,4,5,6);
    IntStream intStreamSingleton = IntStream.of(1);

    intStream.forEach(System.out::println);
    intStreamSingleton.forEach(System.out::println);
    
Creating IntStream: IntStream.range(...)
------------------------------------------
.. code-block:: Java
    :linenos:

    // IntStream.range(lo, hi);
    IntStream intStreamRange = IntStream.range(0,11);

    intStreamRange.forEach(System.out::println);
    
Creating IntStream: IntStream.iterate(int seed, IntUnaryOperator f)
---------------------------------------------------------------------
.. code-block:: Java
    :linenos:

    IntStream intStreamIterate = IntStream.iterate(10, e -> e+2).limit(10);
    intStreamIterate.forEach(System.out::println); // 10, 12, 14, ... , 28

    IntStream intStreamIterate2 = IntStream.iterate(1, e < 100, e -> e*2);
    // identical to for (int i = 1; i < 100; i*=2*) {...}

    
toArray() -> returns int[]
-----------------------------
.. code-block:: Java
    :linenos:

    //toArray() -> int[]
    IntStream intStream = IntStream.range(1, 11);
    int[] intArr = intStream.toArray();
    
boxed().collect(Collectors.toList()) -> returns List<Integer>
--------------------------------------------------------------
.. code-block:: Java
    :linenos:

    List<Integer> intList = intStream.boxed().collect(Collectors.toList());


switch
=======
.. code-block:: Java
    :linenos:

    switch(expression) {
        case x:
            // code block
            break;
        case y:
            // code block
            break;
        default:
            // code block
    }


HashMap
========
containsKey()
--------------
.. code-block:: Java
    :linenos:

    boolean bool = map.containsKey(key);

containsValue()
----------------
.. code-block:: Java
    :linenos:

    boolean bool = map.containsValue(val);

get() / getOrDefault()
----------------------
.. code-block:: Java
    :linenos:

    map.get(key);
    map.getOrDefault(key, defaultVal);

map.isEmpty()
----------------------
.. code-block:: Java
    :linenos:

    map.isEmpty();

keySet()
----------------------
.. code-block:: Java
    :linenos:

    for (String s : map.keySet()) {...}

values()
----------------------
.. code-block:: Java
    :linenos:

    Collection<V> values = map.values();

put() & putIfAbsent()
----------------------
.. code-block:: Java
    :linenos:

    map.put(key,val);
    map.putIfAbsent(key,val);

remove()
----------------------
.. code-block:: Java
    :linenos:

    map.remove(key);

replace(k,v) & replace(k,oldVal,newVal)
--------------------------------------------
.. code-block:: Java
    :linenos:

    map.replace(k, val2);
    map.replace(k, val1, val3); // doesn't replace as val!=predictedVal
    map.replace(k, val2, val3); // replace successful

map.size()
----------------------
.. code-block:: Java
    :linenos:

    map.size(); // number of kv-pair

entrySet()
--------------
.. code-block:: Java
    :linenos:

    import java.util.Map; 

    Set<Map.Entry<K,V>> EntrySet = map.entrySet();

    for (Map.Entry<String, String> book: bookMap.entrySet()) {
        System.out.println("key: " + book.getKey() + " value: " + book.getValue());
    }

    // example
    HashMap<String, String> strMap = new HashMap<>();
    strMap.put("123", "131");
    
    for (Map.Entry<String, String> kv : strMap.entrySet()) {
        System.out.println("key:"+ kv.getKey());
        System.out.println("value:"+ kv.getValue());
    }


Sorting Operations
======================
Sorting Arrays: Arrays.sort(arr)
------------------------------------------
.. code-block:: Java
    :linenos:

    // ASCENDING
    Arrays.sort(arr); 

    // DESCENDING
    int[] descendingArr = IntStream.of(arr)
                .map(i -> -i)
                .sorted()
                .map(i -> -i)
                .toArray();


Sorting part of Arrays: Arrays.sort()
------------------------------------------
.. code-block:: Java
    :linenos:

    // ASCENDING
    int[] arr2 = {100,21,3,4,5,6,62,1,2,12121};
    Arrays.sort(arr2, 0, 5); // inclusive lower bound, exclusive upper bound
    for (int i : arr2) System.out.println(i);

https://www.baeldung.com/java-sorting

