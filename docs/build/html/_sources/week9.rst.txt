************************
Week 9 [06-13 Apr 2022]
************************
Day 44 [6 Apr]
================
Question 74: Kakao ID
------------------------------------------------
카카오에 입사한 신입 개발자 네오는 "카카오계정개발팀"에 배치되어, 카카오 서비스에 가입하는 유저들의 아이디를 생성하는 업무를 담당하게 되었습니다. "네오"에게 주어진 첫 업무는 새로 가입하는 유저들이 카카오 아이디 규칙에 맞지 않는 아이디를 입력했을 때, 입력된 아이디와 유사하면서 규칙에 맞는 아이디를 추천해주는 프로그램을 개발하는 것입니다.
다음은 카카오 아이디의 규칙입니다.
아이디의 길이는 3자 이상 15자 이하여야 합니다.
아이디는 알파벳 소문자, 숫자, 빼기(-), 밑줄(_), 마침표(.) 문자만 사용할 수 있습니다.
단, 마침표(.)는 처음과 끝에 사용할 수 없으며 또한 연속으로 사용할 수 없습니다.
"네오"는 다음과 같이 7단계의 순차적인 처리 과정을 통해 신규 유저가 입력한 아이디가 카카오 아이디 규칙에 맞는 지 검사하고 규칙에 맞지 않은 경우 규칙에 맞는 새로운 아이디를 추천해 주려고 합니다.
신규 유저가 입력한 아이디가 new_id 라고 한다면,

 * 1단계 new_id의 모든 대문자를 대응되는 소문자로 치환합니다.
 * 2단계 new_id에서 알파벳 소문자, 숫자, 빼기(-), 밑줄(_), 마침표(.)를 제외한 모든 문자를 제거합니다.
 * 3단계 new_id에서 마침표(.)가 2번 이상 연속된 부분을 하나의 마침표(.)로 치환합니다.
 * 4단계 new_id에서 마침표(.)가 처음이나 끝에 위치한다면 제거합니다.
 * 5단계 new_id가 빈 문자열이라면, new_id에 "a"를 대입합니다.
 * 6단계 new_id의 길이가 16자 이상이면, new_id의 첫 15개의 문자를 제외한 나머지 문자들을 모두 제거합니다. 만약 제거 후 마침표(.)가 new_id의 끝에 위치한다면 끝에 위치한 마침표(.) 문자를 제거합니다.
 * 7단계 new_id의 길이가 2자 이하라면, new_id의 마지막 문자를 new_id의 길이가 3이 될 때까지 반복해서 끝에 붙입니다.

My Solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;
    import java.util.stream.*;

    class Solution {
        public String solution(String new_id) {
            IntStream id_draft = new_id.chars();
            int[] intArr = id_draft
                .map(i -> this.upperToLower((char) i))
                .filter(i -> ('a'<=i && i<='z') || ('0'<=i && i<='9') || i==(int) '-' || i==(int) '_' || i==(int) '.')
                .toArray();
                // .boxed()
                // .collect(Collectors.toList());
            
            ArrayList<Integer> intList = new ArrayList<>();
            boolean prevWasDot = false;
            for (Integer i : intArr) {
                if (prevWasDot) {
                    if (i.intValue()==(int) '.') continue;
                    intList.add(i);
                    prevWasDot=false;
                    continue;
                } else {
                    intList.add(i);
                    if (i.intValue()==(int) '.') {
                        prevWasDot = true;
                    }
                }
            }
            
            if (intList.size()>0 && intList.get(0)== (int) '.') intList.remove(0);
            if (intList.size()>0 && intList.get(intList.size()-1)== (int) '.') intList.remove(intList.size()-1);
            
            if (intList.size()==0) return "aaa";
            
            if (intList.size()>=16) {
                intList = new ArrayList<Integer>(intList.subList(0,15));
            }
            
            if (intList.get(intList.size()-1)== (int) '.') intList.remove(intList.size()-1);
            while (intList.size()<3) {
                intList.add(intList.get(intList.size()-1));
            }
            
            StringBuilder sb = new StringBuilder();
            for (int i : intList) {
                sb.append((char) i);
            }
            return sb.toString();
            // IntStream.Builder builder = IntStream.Builder();
            // Integer prev = null;
            // for (Integer i : lst) {
            //     if (i.equals(prev)) continue;
            //     if (prev==null && i.intValue()==(int) '.') continue;
            //     prev = i;
            //     builder.add(i);
            // }
            // List<Integer> lstlst = builder
            //     .build()
            //     .collect(Collectors.toList);
                // .forEach(i -> System.out.println((char) i));
                // .boxed()
                // .collect(Collectors.toList());
        }
        
        public int upperToLower(char c) {
            if (Character.isUpperCase(c)) return Character.toLowerCase(c);
            return (int) c;
        }
    }

Remarks and Complexity Analysis: 
 * I really hate solving these questions. So much labor and they do not even deal with any algorithms. 
 * **Time Complexity**: ``O(n)`` 
 * **Space Complexity**: ``O(n)``

Beautiful REGEX Solution

.. code-block:: Java
    :linenos:

    class Solution {
        public String solution(String new_id) {
            String answer = "";
            String temp = new_id.toLowerCase();

            temp = temp.replaceAll("[^-_.a-z0-9]","");
            System.out.println(temp);
            temp = temp.replaceAll("[.]{2,}",".");
            temp = temp.replaceAll("^[.]|[.]$","");
            System.out.println(temp.length());
            if(temp.equals(""))
                temp+="a";
            if(temp.length() >=16){
                temp = temp.substring(0,15);
                temp=temp.replaceAll("^[.]|[.]$","");
            }
            if(temp.length()<=2)
                while(temp.length()<3)
                    temp+=temp.charAt(temp.length()-1);

            answer=temp;
            return answer;
        }
    }    

My only concern with the above solution is memory management as Strings are immutable and many String objects are being created.

Interesting OOP solution (clear description): 

.. code-block:: Java
    :linenos:

    class Solution {
        public String solution(String new_id) {

            String s = new KAKAOID(new_id)
                    .replaceToLowerCase()
                    .filter()
                    .toSingleDot()
                    .noStartEndDot()
                    .noBlank()
                    .noGreaterThan16()
                    .noLessThan2()
                    .getResult();
            return s;
        }

        private static class KAKAOID {
            private String s;

            KAKAOID(String s) {
                this.s = s;
            }

            private KAKAOID replaceToLowerCase() {
                s = s.toLowerCase();
                return this;
            }

            private KAKAOID filter() {
                s = s.replaceAll("[^a-z0-9._-]", "");
                return this;
            }

            private KAKAOID toSingleDot() {
                s = s.replaceAll("[.]{2,}", ".");
                return this;
            }

            private KAKAOID noStartEndDot() {
                s = s.replaceAll("^[.]|[.]$", "");
                return this;
            }

            private KAKAOID noBlank() {
                s = s.isEmpty() ? "a" : s;
                return this;
            }

            private KAKAOID noGreaterThan16() {
                if (s.length() >= 16) {
                    s = s.substring(0, 15);
                }
                s = s.replaceAll("[.]$", "");
                return this;
            }

            private KAKAOID noLessThan2() {
                StringBuilder sBuilder = new StringBuilder(s);
                while (sBuilder.length() <= 2) {
                    sBuilder.append(sBuilder.charAt(sBuilder.length() - 1));
                }
                s = sBuilder.toString();
                return this;
            }

            private String getResult() {
                return s;
            }
        }
    }


Question 75: Kth Number
------------------------------------------------
배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.
예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면
array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
2에서 나온 배열의 3번째 숫자는 5입니다.
배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

제한사항
 * array의 길이는 1 이상 100 이하입니다.
 * array의 각 원소는 1 이상 100 이하입니다.
 * commands의 길이는 1 이상 50 이하입니다.
 * commands의 각 원소는 길이가 3입니다.

My Solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;
    import java.util.stream.*;

    class Solution {
        public int[] solution(int[] array, int[][] commands) {
            int[] res = new int[commands.length];

            ArrayList<Integer> arrList = IntStream.of(array)
                .boxed()
                .collect(Collectors.toCollection(ArrayList::new));
            
            int i = 0;
            for (int[] command : commands) {
                res[i++] = this.helper(arrList, command);
            }
            return res;
        }
        
        public int helper(ArrayList<Integer> arrList, int[] command) {
            List<Integer> subList = arrList.subList(command[0]-1, command[1]);
            subList = subList.stream().sorted().collect(Collectors.toList());
            return subList.get(command[2]-1);
        }
    }

Alternative Solution (using copy of arrays): 

.. code-block:: Java
    :linenos:

    class Solution {
        public int[] solution(int[] array, int[][] commands) {
            int[] answer = new int[commands.length];

            for(int i=0; i<commands.length; i++){
                int[] temp = Arrays.copyOfRange(array, commands[i][0]-1, commands[i][1]);
                Arrays.sort(temp);
                answer[i] = temp[commands[i][2]-1];
            }

            return answer;
        }
    }

Question 76: Reorder List
------------------------------------------------
You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln
Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
You may not modify the values in the list's nodes. Only nodes themselves may be changed.

My Solution: 

.. code-block:: Java
    :linenos:

    import java.util.Stack;

    class Solution {
        public void reorderList(ListNode head) {
            if (head == null) return; 
            
            Stack<ListNode> stk = new Stack<>();
            ListNode p2 = head;
            ListNode p1 = head;
            
            while (p2!=null) {
                stk.push(p2);
                p2 = p2.next;
            }
            
            p2 = stk.pop();
            
            while (p1!=p2) {
                ListNode temp = p1.next;
                p1.next = p2;
                p1 = temp;
                if (p1==p2) break;
                p2.next = p1;
                p2 = stk.pop();
            }
            p1.next = null;
        }
    }

Remarks and Complexity Analysis: 
 * Wow solving a well-written, short, algorithm focused leetcode question for the first time in days is 
   gratifying. I'm slightly disappointed about the space complexity and I think there would have been a way 
   to solve this with one slow and one fast pointer (one starting halfway, other at the end) -- but I'm satisfied
 * **Time Complexity**: ``O(n)`` where ``n=list.size()``.
 * **Space Complexity**: ``O(n)`` thanks to the Stack

More space-efficient solution: 

.. code-block:: Java
    :linenos:

    public void reorderList(ListNode head) {
        if(head==null||head.next==null) return;
        
        //Find the middle of the list
        ListNode p1=head;
        ListNode p2=head;
        while(p2.next!=null&&p2.next.next!=null){ 
            p1=p1.next;
            p2=p2.next.next;
        }
        
        //Reverse the half after middle  1->2->3->4->5->6 to 1->2->3->6->5->4
        ListNode preMiddle=p1;
        ListNode preCurrent=p1.next;
        while(preCurrent.next!=null){
            ListNode current=preCurrent.next;
            preCurrent.next=current.next;
            current.next=preMiddle.next;
            preMiddle.next=current;
        }
        
        //Start reorder one by one  1->2->3->6->5->4 to 1->6->2->5->3->4
        p1=head;
        p2=preMiddle.next;
        while(p1!=preMiddle){
            preMiddle.next=p2.next;
            p2.next=p1.next;
            p1.next=p2;
            p1=p2.next;
            p2=preMiddle.next;
        }
    }

TRY **Reverse a linked list I & II** and **review this method once more**!

Day 45 [10 Apr]
================
Question 77: Palindromic Substrings
------------------------------------------------
Given a string s, return the number of palindromic substrings in it.
A string is a palindrome when it reads the same backward as forward.
A substring is a contiguous sequence of characters within the string.

My solution: 

.. code-block:: Java
    :linenos:

    // brute force
    class Solution {
        public int countSubstrings(String s) {
            int res = s.length();
            for (int n = 2; n <= s.length(); n++) {
                for (int i = 0; i<s.length()+1-n; i++) { 
                    res += this.isPalindrome(s,i,i+n-1);
                }
            }
            return res;
        }
        
        public int isPalindrome(String s, int lo, int hi) {
            while (lo<hi) {
                if (s.charAt(lo++)!=s.charAt(hi--)) return 0;
            }
            return 1;
        }
    }

    // optimized
    public int countSubstrings(String s) {
        int res = 0;
        for (int i = 0; i<s.length(); i++) {
            res+=this.extendPalindrome(s,i,i);
            res+=this.extendPalindrome(s,i,i+1);
        }
        return res;
    }
    
    private int extendPalindrome(String s, int lo, int hi) {
        int res = 0;
        while (lo>=0 && hi<s.length() && s.charAt(lo)==s.charAt(hi)) {
            res++;
            lo--;
            hi++;
        }
        return res;
    }

Remarks and Complexity Analysis: 
 * Requires smart analaysis and visual thinking!
 * **Time Complexity**: ``O(n^2)`` where ``n=s.length()``.
 * **Space Complexity**: ``O(1)``

Question 78: Longest Common Subsequence
------------------------------------------------
Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.
A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.
For example, "ace" is a subsequence of "abcde".
A common subsequence of two strings is a subsequence that is common to both strings.

My solution: 

.. code-block:: Java
    :linenos:

    public int longestCommonSubsequence(String text1, String text2) {
        int[][] memoMatrix = new int[text1.length()+1][text2.length()+1];
        for (int i = 1; i<text1.length()+1;i++) {
            for (int j = 1; j<text2.length()+1; j++) {
                if (text1.charAt(i-1)==text2.charAt(j-1)) {
                    memoMatrix[i][j] = memoMatrix[i-1][j-1] + 1;
                } else {
                    memoMatrix[i][j] = Math.max(memoMatrix[i-1][j], memoMatrix[i][j-1]);
                }
            }
        }
        return memoMatrix[text1.length()][text2.length()];
    }

Remarks and Complexity Analysis: 
 * Felt like a fluke because I didn't fully think of all the edge cases and wasn't comprehensive. 
 * Think about smaller sub-problems and always consider how to use data structures and divide-and-conquer to solve 
   the problem at hand!
 * **Time Complexity**: ``O(nm)`` where ``n=text1.length(); m=text2.length()``
 * **Space Complexity**: ``O(nm)`` where ``n=text1.length(); m=text2.length()``

Day 46 [12 Apr]
================
Question 79: 숫자 문자열과 영단어
------------------------------------------------
네오와 프로도가 숫자놀이를 하고 있습니다. 네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.

다음은 숫자의 일부 자릿수를 영단어로 바꾸는 예시입니다.
 * 1478 → "one4seveneight"
 * 234567 → "23four5six7"
 * 10203 → "1zerotwozero3"

이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 s가 매개변수로 주어집니다. s가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.

My solution: 

.. code-block:: Java
    :linenos:
    
    import java.util.*;

    class Solution {
        public int solution(String s) {
            HashMap<String, List<Object>> engToDigit = new HashMap<>();
            engToDigit.put("ze", new ArrayList<Object>(Arrays.asList('0', 3))); 
            engToDigit.put("on", new ArrayList<Object>(Arrays.asList('1', 2)));
            engToDigit.put("tw", new ArrayList<Object>(Arrays.asList('2', 2)));
            engToDigit.put("th", new ArrayList<Object>(Arrays.asList('3', 4)));
            engToDigit.put("fo", new ArrayList<Object>(Arrays.asList('4', 3)));
            engToDigit.put("fi", new ArrayList<Object>(Arrays.asList('5', 3)));
            engToDigit.put("si", new ArrayList<Object>(Arrays.asList('6', 2)));
            engToDigit.put("se", new ArrayList<Object>(Arrays.asList('7', 4)));
            engToDigit.put("ei", new ArrayList<Object>(Arrays.asList('8', 4)));
            engToDigit.put("ni", new ArrayList<Object>(Arrays.asList('9', 3)));
            
            StringBuilder res = new StringBuilder();
            
            for (int i = 0; i<s.length(); i++) {
                if (!Character.isDigit(s.charAt(i))) {
                    List<Object> numAndSkip = engToDigit.get(s.substring(i,i+2));
                    res.append((char) numAndSkip.get(0));
                    i+=(int) numAndSkip.get(1);
                } else {
                    res.append(s.charAt(i));
                }
            }
            
            return Integer.valueOf(res.toString());
        }
    }
    
Remarks and Complexity Analysis: 
 * Always think critically about your implementation before actually coding it! Think about the example cases. Else you will waste lots of time.
 * **Time Complexity**: ``O(n)`` where ``n=s.length()``.
 * **Space Complexity**: ``O(1)``

Alternative solution (less efficient in my opinion): 

.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {
        public int solution(String s) {
            String[] digits = {"0","1","2","3","4","5","6","7","8","9"};
            String[] alphabets = {"zero","one","two","three","four","five","six","seven","eight","nine"};

            for(int i=0; i<10; i++){
                s = s.replaceAll(alphabets[i],digits[i]);
            }

            return Integer.parseInt(s);
        }
    }

Question 80: [카카오 인턴] 키패드 누르기
------------------------------------------------
전화 키패드에서 왼손과 오른손의 엄지손가락만을 이용해서 숫자만을 입력하려고 합니다.
맨 처음 왼손 엄지손가락은 * 키패드에 오른손 엄지손가락은 # 키패드 위치에서 시작하며, 엄지손가락을 사용하는 규칙은 다음과 같습니다.

 #. 엄지손가락은 상하좌우 4가지 방향으로만 이동할 수 있으며 키패드 이동 한 칸은 거리로 1에 해당합니다.
 #. 왼쪽 열의 3개의 숫자 1, 4, 7을 입력할 때는 왼손 엄지손가락을 사용합니다.
 #. 오른쪽 열의 3개의 숫자 3, 6, 9를 입력할 때는 오른손 엄지손가락을 사용합니다.
 #. 가운데 열의 4개의 숫자 2, 5, 8, 0을 입력할 때는 두 엄지손가락의 현재 키패드의 위치에서 더 가까운 엄지손가락을 사용합니다. 4-1. 만약 두 엄지손가락의 거리가 같다면, 오른손잡이는 오른손 엄지손가락, 왼손잡이는 왼손 엄지손가락을 사용합니다. 순서대로 누를 번호가 담긴 배열 numbers, 왼손잡이인지 오른손잡이인 지를 나타내는 문자열 hand가 매개변수로 주어질 때, 각 번호를 누른 엄지손가락이 왼손인 지 오른손인 지를 나타내는 연속된 문자열 형태로 return 하도록 solution 함수를 완성해주세요.

[제한사항]
 * numbers 배열의 크기는 1 이상 1,000 이하입니다.
 * numbers 배열 원소의 값은 0 이상 9 이하인 정수입니다.
 * " * left"는 왼손잡이, "right"는 오른손잡이를 의미합니다.
 * hand는 "left" 또는 "right" 입니다.
 * "left"는 왼손잡이, "right"는 오른손잡이를 의미합니다.
 * 왼손 엄지손가락을 사용한 경우는 L, 오른손 엄지손가락을 사용한 경우는 R을 순서대로 이어붙여 문자열 형태로 return 해주세요.


.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {
        
        public String solution(int[] numbers, String hand) {
            char[] res = new char[numbers.length];
            
            HashMap<Integer, int[]> numToLoc = new HashMap<>();
            numToLoc.put(1, new int[] {0,0});
            numToLoc.put(2, new int[] {0,1});
            numToLoc.put(3, new int[] {0,2});
            numToLoc.put(4, new int[] {1,0});
            numToLoc.put(5, new int[] {1,1});
            numToLoc.put(6, new int[] {1,2});
            numToLoc.put(7, new int[] {2,0});
            numToLoc.put(8, new int[] {2,1});
            numToLoc.put(9, new int[] {2,2});
            numToLoc.put(0, new int[] {3,1});
            
            HashSet<Integer> leftNums = new HashSet<>(Arrays.asList(1,4,7));
            HashSet<Integer> rightNums = new HashSet<>(Arrays.asList(3,6,9));
            
            int[] leftLoc = {3,0};
            int[] rightLoc = {3,2};
            
            for (int i = 0 ; i < numbers.length ; i++) {
                if (leftNums.contains(numbers[i])) {
                    leftLoc = numToLoc.get(numbers[i]);
                    res[i] = 'L';
                } else if (rightNums.contains(numbers[i])) {
                    rightLoc = numToLoc.get(numbers[i]);
                    res[i] = 'R';
                } else {
                    int leftDist = this.calculateDist(leftLoc, numToLoc.get(numbers[i]));
                    int rightDist = this.calculateDist(rightLoc, numToLoc.get(numbers[i]));
                    
                    if (leftDist==rightDist) {
                        res[i] = hand.equals("right") ? 'R' : 'L';
                    } else if (leftDist<rightDist) {
                        res[i] = 'L';
                    } else {
                        res[i] = 'R';
                    }
                    
                    if (res[i]=='L') {
                        leftLoc = numToLoc.get(numbers[i]);
                    } else {
                        rightLoc = numToLoc.get(numbers[i]);
                    }
                }
            }
            
            return new String(res);
        }
        
        public int calculateDist(int[] fstLoc, int[] sndLoc) {
            return Math.abs(fstLoc[0]-sndLoc[0])+Math.abs(fstLoc[1]-sndLoc[1]);
        }
    }

Alternatively, using Array instead of HashMap (to reduce overhead): 

.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {
        public String solution(int[] numbers, String hand) {
            char[] res = new char[numbers.length];
            
            int[][] numToLoc = {
                    {3,1}, //0
                    {0,0}, //1
                    {0,1}, //2
                    {0,2}, //3
                    {1,0}, //4
                    {1,1}, //5
                    {1,2}, //6
                    {2,0}, //7
                    {2,1}, //8
                    {2,2}  //9
            };
            
            HashSet<Integer> leftNums = new HashSet<>(Arrays.asList(1,4,7));
            HashSet<Integer> rightNums = new HashSet<>(Arrays.asList(3,6,9));
            
            int[] leftLoc = {3,0};
            int[] rightLoc = {3,2};
            
            for (int i = 0 ; i < numbers.length ; i++) {
                if (leftNums.contains(numbers[i])) {
                    leftLoc = numToLoc[numbers[i]];
                    res[i] = 'L';
                } else if (rightNums.contains(numbers[i])) {
                    rightLoc = numToLoc[numbers[i]];
                    res[i] = 'R';
                } else {
                    int leftDist = this.calculateDist(leftLoc, numToLoc[numbers[i]]);
                    int rightDist = this.calculateDist(rightLoc, numToLoc[numbers[i]]);
                    if (leftDist==rightDist) {
                        res[i] = hand.equals("right") ? 'R' : 'L';
                    } else if (leftDist<rightDist) {
                        res[i] = 'L';
                    } else {
                        res[i] = 'R';
                    }
                    
                    if (res[i]=='L') {
                        leftLoc = numToLoc[numbers[i]];
                    } else {
                        rightLoc = numToLoc[numbers[i]];
                    }
                }
                
            }
            
            return new String(res);
        }
        
        public int calculateDist(int[] fstLoc, int[] sndLoc) {
            return Math.abs(fstLoc[0]-sndLoc[0])+Math.abs(fstLoc[1]-sndLoc[1]);
        }
    }

Question 81: 주차 요금 계산
------------------------------------------------
주차장의 요금표와 차량이 들어오고(입차) 나간(출차) 기록이 주어졌을 때, 차량별로 주차 요금을 계산하려고 합니다. 

어떤 차량이 입차된 후에 출차된 내역이 없다면, 23:59에 출차된 것으로 간주합니다.
0000번 차량은 18:59에 입차된 이후, 출차된 내역이 없습니다. 따라서, 23:59에 출차된 것으로 간주합니다.
00:00부터 23:59까지의 입/출차 내역을 바탕으로 차량별 누적 주차 시간을 계산하여 요금을 일괄로 정산합니다.
누적 주차 시간이 기본 시간이하라면, 기본 요금을 청구합니다.
누적 주차 시간이 기본 시간을 초과하면, 기본 요금에 더해서, 초과한 시간에 대해서 단위 시간 마다 단위 요금을 청구합니다.
초과한 시간이 단위 시간으로 나누어 떨어지지 않으면, 올림합니다.
⌈a⌉ : a보다 작지 않은 최소의 정수를 의미합니다. 즉, 올림을 의미합니다.
주차 요금을 나타내는 정수 배열 fees, 자동차의 입/출차 내역을 나타내는 문자열 배열 records가 매개변수로 주어집니다. 차량 번호가 작은 자동차부터 청구할 주차 요금을 차례대로 정수 배열에 담아서 return 하도록 solution 함수를 완성해주세요.

제한사항
 * fees의 길이 = 4
 * fees[0] = 기본 시간(분)
 * 1 ≤ fees[0] ≤ 1,439
 * fees[1] = 기본 요금(원)
 * 0 ≤ fees[1] ≤ 100,000
 * fees[2] = 단위 시간(분)
 * 1 ≤ fees[2] ≤ 1,439
 * fees[3] = 단위 요금(원)
 * 1 ≤ fees[3] ≤ 10,000
 * 1 ≤ records의 길이 ≤ 1,000
 * records의 각 원소는 "시각 차량번호 내역" 형식의 문자열입니다.
 * 시각, 차량번호, 내역은 하나의 공백으로 구분되어 있습니다.
 * 시각은 차량이 입차되거나 출차된 시각을 나타내며, HH:MM 형식의 길이 5인 문자열입니다.
 * HH:MM은 00:00부터 23:59까지 주어집니다.
 * 잘못된 시각("25:22", "09:65" 등)은 입력으로 주어지지 않습니다.
 * 차량번호는 자동차를 구분하기 위한, '0'~'9'로 구성된 길이 4인 문자열입니다.
 * 내역은 길이 2 또는 3인 문자열로, IN 또는 OUT입니다. IN은 입차를, OUT은 출차를 의미합니다.
 * records의 원소들은 시각을 기준으로 오름차순으로 정렬되어 주어집니다.
 * records는 하루 동안의 입/출차된 기록만 담고 있으며, 입차된 차량이 다음날 출차되는 경우는 입력으로 주어지지 않습니다.
 * 같은 시각에, 같은 차량번호의 내역이 2번 이상 나타내지 않습니다.
 * 마지막 시각(23:59)에 입차되는 경우는 입력으로 주어지지 않습니다.
 * 아래의 예를 포함하여, 잘못된 입력은 주어지지 않습니다.
 * 주차장에 없는 차량이 출차되는 경우
 * 주차장에 이미 있는 차량(차량번호가 같은 차량)이 다시 입차되는 경우

My solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {
        public int[] solution(int[] fees, String[] records) {
            HashMap<Integer, Integer> carNumToTotalTime = new HashMap<>();
            HashMap<Integer, Integer> carNumToEntryTime = new HashMap<>();
            
            for (String s : records) {
                String[] time_carNum_INOUT = s.split(" ");
                int time = this.timeConversion(time_carNum_INOUT[0]);
                int carNum = Integer.parseInt(time_carNum_INOUT[1]); 
                
                if ("IN".equals(time_carNum_INOUT[2])) {
                    carNumToEntryTime.put(carNum, time);
                } else { // OUT
                    int timeSpent = time - carNumToEntryTime.get(carNum);
                    carNumToTotalTime.put(carNum, carNumToTotalTime.getOrDefault(carNum,0)+timeSpent);
                    carNumToEntryTime.remove(carNum);
                }
            }
            
            for (int carNum : carNumToEntryTime.keySet()) {
                int moreTime = (23*60 + 59) - carNumToEntryTime.get(carNum);
                carNumToTotalTime.put(carNum, carNumToTotalTime.getOrDefault(carNum,0)+moreTime);
            }
            
            int[] res = new int[carNumToTotalTime.keySet().size()];
            int[] i = {0};
            carNumToTotalTime.keySet()
                .stream()
                .sorted()
                .forEach(carNum -> {
                    res[i[0]]=this.calcPrice(carNumToTotalTime.get(carNum), fees);
                    i[0]+=1;
                });
            return res;
        }
        
        public int timeConversion(String s) {
            int hour = Integer.parseInt(s.substring(0,2));
            int min = Integer.parseInt(s.substring(3,5));
            return (60*hour + min);
        }
        
        public int calcPrice(int TotalTime, int[] fees) {
            if (TotalTime <= fees[0]) return fees[1];
            int res = fees[1];
            int extraTime = TotalTime - fees[0];
            return res - Math.floorDiv(-extraTime, fees[2]) * fees[3];
        }
    }

Remarks and Complexity Analysis: 
 * Pretty simple but long (must think carefully about the implementation before actually coding!)
 * **Time Complexity**: ``O(n log n)`` where ``n=records.length`` as we sort through the keys.
 * **Space Complexity**: ``O(n)``

Alternative solution (that demonstrates the utility of TreeMap): 

.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {

        public int timeToInt(String time) {
            String temp[] = time.split(":");
            return Integer.parseInt(temp[0])*60 + Integer.parseInt(temp[1]);
        }
        public int[] solution(int[] fees, String[] records) {

            TreeMap<String, Integer> map = new TreeMap<>();

            for(String record : records) {
                String temp[] = record.split(" ");
                int time = temp[2].equals("IN") ? -1 : 1;
                time = timeToInt(temp[0]) * time;
                map.put(temp[1], map.getOrDefault(temp[1], 0) + time);
            }
            int idx = 0, ans[] = new int[map.size()];
            for(int time : map.values()) {
                if(time < 1) time += 1439;
                time -= fees[0];
                int cost = fees[1];
                if(time > 0)
                    cost += (time%fees[2] == 0 ? time/fees[2] : time/fees[2]+1)*fees[3];

                ans[idx++] = cost;
            }
            return ans;
        }
    }

.. note::
    **TreeMap** adds the key-value pairs in a sorted manner (with regards to the key). 

    Look-up / Insert = ``O(log n)``

    **HashMap:** Look-up / Insert = ``O(1)``
    **LinkedHashMap:** Look-up / Insert = ``O(1)`` (maintains insertion order; put 1,2,3 => int k: keySet() -> 1, 2, 3)

Question 82: 양궁대회
------------------------------------------------
카카오배 양궁대회가 열렸습니다. 
라이언은 저번 카카오배 양궁대회 우승자이고 이번 대회에도 결승전까지 올라왔습니다. 결승전 상대는 어피치입니다. 
카카오배 양궁대회 운영위원회는 한 선수의 연속 우승보다는 다양한 선수들이 양궁대회에서 우승하기를 원합니다. 따라서, 양궁대회 운영위원회는 결승전 규칙을 전 대회 우승자인 라이언에게 불리하게 다음과 같이 정했습니다.
어피치가 화살 n발을 다 쏜 후에 라이언이 화살 n발을 쏩니다.
점수를 계산합니다.
과녁판은 아래 사진처럼 생겼으며 가장 작은 원의 과녁 점수는 10점이고 가장 큰 원의 바깥쪽은 과녁 점수가 0점입니다. 

만약, k(k는 1~10사이의 자연수)점을 어피치가 a발을 맞혔고 라이언이 b발을 맞혔을 경우 더 많은 화살을 k점에 맞힌 선수가 k 점을 가져갑니다. 단, a = b일 경우는 어피치가 k점을 가져갑니다. k점을 여러 발 맞혀도 k점 보다 많은 점수를 가져가는 게 아니고 k점만 가져가는 것을 유의하세요. 또한 a = b = 0 인 경우, 즉, 라이언과 어피치 모두 k점에 단 하나의 화살도 맞히지 못한 경우는 어느 누구도 k점을 가져가지 않습니다.
예를 들어, 어피치가 10점을 2발 맞혔고 라이언도 10점을 2발 맞혔을 경우 어피치가 10점을 가져갑니다.
다른 예로, 어피치가 10점을 0발 맞혔고 라이언이 10점을 2발 맞혔을 경우 라이언이 10점을 가져갑니다.
모든 과녁 점수에 대하여 각 선수의 최종 점수를 계산합니다.
최종 점수가 더 높은 선수를 우승자로 결정합니다. 단, 최종 점수가 같을 경우 어피치를 우승자로 결정합니다.
현재 상황은 어피치가 화살 n발을 다 쏜 후이고 라이언이 화살을 쏠 차례입니다.
라이언은 어피치를 가장 큰 점수 차이로 이기기 위해서 n발의 화살을 어떤 과녁 점수에 맞혀야 하는지를 구하려고 합니다.
화살의 개수를 담은 자연수 n, 어피치가 맞힌 과녁 점수의 개수를 10점부터 0점까지 순서대로 담은 정수 배열 info가 매개변수로 주어집니다. 이때, 라이언이 가장 큰 점수 차이로 우승하기 위해 n발의 화살을 어떤 과녁 점수에 맞혀야 하는지를 10점부터 0점까지 순서대로 정수 배열에 담아 return 하도록 solution 함수를 완성해 주세요. 만약, 라이언이 우승할 수 없는 경우(무조건 지거나 비기는 경우)는 [-1]을 return 해주세요.

제한사항
 * 1 ≤ n ≤ 10
 * info의 길이 = 11
 * 0 ≤ info의 원소 ≤ n
 * info의 원소 총합 = n
 * info의 i번째 원소는 과녁의 10 - i 점을 맞힌 화살 개수입니다. ( i는 0~10 사이의 정수입니다.)
 * 라이언이 우승할 방법이 있는 경우, return 할 정수 배열의 길이는 11입니다.
 * 0 ≤ return할 정수 배열의 원소 ≤ n
 * return할 정수 배열의 원소 총합 = n (꼭 n발을 다 쏴야 합니다.)
 * return할 정수 배열의 i번째 원소는 과녁의 10 - i 점을 맞힌 화살 개수입니다. ( i는 0~10 사이의 정수입니다.)
 * 라이언이 가장 큰 점수 차이로 우승할 수 있는 방법이 여러 가지 일 경우, 가장 낮은 점수를 더 많이 맞힌 경우를 return 해주세요.
 * 가장 낮은 점수를 맞힌 개수가 같을 경우 계속해서 그다음으로 낮은 점수를 더 많이 맞힌 경우를 return 해주세요.
 * 예를 들어, [2,3,1,0,0,0,0,1,3,0,0]과 [2,1,0,2,0,0,0,2,3,0,0]를 비교하면 [2,1,0,2,0,0,0,2,3,0,0]를 return 해야 합니다.
 * 다른 예로, [0,0,2,3,4,1,0,0,0,0,0]과 [9,0,0,0,0,0,0,0,1,0,0]를 비교하면[9,0,0,0,0,0,0,0,1,0,0]를 return 해야 합니다.
 * 라이언이 우승할 방법이 없는 경우, return 할 정수 배열의 길이는 1입니다.
 * 라이언이 어떻게 화살을 쏘든 라이언의 점수가 어피치의 점수보다 낮거나 같으면 [-1]을 return 해야 합니다.

My incomplete solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;
    class Solution {
        public int[] solution(int n, int[] info) {
            int[] ryanHist = new int[11];
            //weighted Points => {target, num_of_shots_required}
            TreeMap<Double, int[]> weightedPointsMap = new TreeMap<>(Collections.reverseOrder());
            for (int i = 0; i < info.length; i++) {
                if (info[i]==0) {
                    weightedPointsMap.put((double) 10-i, new int[] {10-i, 1});
                } else {
                    weightedPointsMap.put(2.0*(10-i)/(info[i]+1), new int[] {10-i, info[i]+1});
                }
            }
            int arrowsLeft = n;
            int points = 0;
            for (int[] target_and_shots_needed : weightedPointsMap.values()) {
                if (target_and_shots_needed[1]<=arrowsLeft) {
                    points+= target_and_shots_needed[0];
                    arrowsLeft-= target_and_shots_needed[1];
                    ryanHist[10-target_and_shots_needed[0]]=target_and_shots_needed[1];
                }
                if (arrowsLeft==0) break;
            }
            ryanHist[10] = arrowsLeft;
            
            int apeachSum = 0;
            for (int i = 0; i<info.length ; i++) {
                if (info[i]>0 && ryanHist[i]==0) {
                    apeachSum+=(10-i);
                }
            }
            
            return apeachSum>=points ? new int[] {-1} : ryanHist;
        }
    }


https://programmers.co.kr/learn/courses/30/lessons/92342/solution_groups?language=java

**NOT DONE HERE** 

RETURN HERE
============

Day 47 [13 Apr]
================
Question 83: 로또의 최고 순위와 최저 순위
------------------------------------------------
로또를 구매한 민우는 당첨 번호 발표일을 학수고대하고 있었습니다. 하지만, 민우의 동생이 로또에 낙서를 하여, 일부 번호를 알아볼 수 없게 되었습니다. 당첨 번호 발표 후, 민우는 자신이 구매했던 로또로 당첨이 가능했던 최고 순위와 최저 순위를 알아보고 싶어 졌습니다. 
알아볼 수 없는 번호를 0으로 표기하기로 하고, 민우가 구매한 로또 번호 6개가 44, 1, 0, 0, 31 25라고 가정해보겠습니다. 당첨 번호 6개가 31, 10, 45, 1, 6, 19라면, 당첨 가능한 최고 순위와 최저 순위의 한 예는 아래와 같습니다

 * 순서와 상관없이, 구매한 로또에 당첨 번호와 일치하는 번호가 있으면 맞힌 걸로 인정됩니다.
 * 알아볼 수 없는 두 개의 번호를 각각 10, 6이라고 가정하면 3등에 당첨될 수 있습니다.
 * 3등을 만드는 다른 방법들도 존재합니다. 하지만, 2등 이상으로 만드는 것은 불가능합니다.
 * 알아볼 수 없는 두 개의 번호를 각각 11, 7이라고 가정하면 5등에 당첨될 수 있습니다.
 * 5등을 만드는 다른 방법들도 존재합니다. 하지만, 6등(낙첨)으로 만드는 것은 불가능합니다.
 
민우가 구매한 로또 번호를 담은 배열 lottos, 당첨 번호를 담은 배열 win_nums가 매개변수로 주어집니다. 이때, 당첨 가능한 최고 순위와 최저 순위를 차례대로 배열에 담아서 return 하도록 solution 함수를 완성해주세요.

제한사항
 * lottos는 길이 6인 정수 배열입니다.
 * lottos의 모든 원소는 0 이상 45 이하인 정수입니다.
 * 0은 알아볼 수 없는 숫자를 의미합니다.
 * 0을 제외한 다른 숫자들은 lottos에 2개 이상 담겨있지 않습니다.
 * lottos의 원소들은 정렬되어 있지 않을 수도 있습니다.
 * win_nums은 길이 6인 정수 배열입니다.
 * win_nums의 모든 원소는 1 이상 45 이하인 정수입니다.
 * win_nums에는 같은 숫자가 2개 이상 담겨있지 않습니다.
 * win_nums의 원소들은 정렬되어 있지 않을 수도 있습니다.

My solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {
        public int[] solution(int[] lottos, int[] win_nums) {
            HashSet<Integer> winNumSet = new HashSet<>();
            for (int win_num : win_nums) {
                winNumSet.add(win_num);
            }
            
            int correctCount = 0;
            int unknownCount = 0;
            for (int lottoNum : lottos) {
                if (lottoNum == 0) {
                    unknownCount++;
                }
                if (winNumSet.contains(lottoNum)) {
                    correctCount++;
                }
            }
            int bestRanking = correctCount+unknownCount==0 ? 6 : 7-(correctCount+unknownCount);
            int worstRanking = correctCount==0 ? 6 : 7-(correctCount);
            
            return new int[] {bestRanking, worstRanking};
        }

        //revised ending
        public int[] solution(int[] lottos, int[] win_nums) {
            HashSet<Integer> winNumSet = new HashSet<>();
            for (int win_num : win_nums) {
                winNumSet.add(win_num);
            }
            
            int correctCount = 0;
            int unknownCount = 0;
            for (int lottoNum : lottos) {
                if (lottoNum == 0) {
                    unknownCount++;
                }
                if (winNumSet.contains(lottoNum)) {
                    correctCount++;
                }
            }
            
            return new int[] {Math.min(6, 7-(correctCount+unknownCount)), Math.min(6, 7-(correctCount))};
        }
    }

Remarks and Complexity Analysis: 
 * Simple question.
 * **Time Complexity**: ``O(1)`` as ``lottos.length = win_nums.length = 6``.
 * **Space Complexity**: ``O(1)`` as ``lottos.length = win_nums.length = 6``.

Question 84: 괄호 변환
------------------------------------------------
카카오에 신입 개발자로 입사한 "콘"은 선배 개발자로부터 개발역량 강화를 위해 다른 개발자가 작성한 소스 코드를 분석하여 문제점을 발견하고 수정하라는 업무 과제를 받았습니다. 소스를 컴파일하여 로그를 보니 대부분 소스 코드 내 작성된 괄호가 개수는 맞지만 짝이 맞지 않은 형태로 작성되어 오류가 나는 것을 알게 되었습니다.
수정해야 할 소스 파일이 너무 많아서 고민하던 "콘"은 소스 코드에 작성된 모든 괄호를 뽑아서 올바른 순서대로 배치된 괄호 문자열을 알려주는 프로그램을 다음과 같이 개발하려고 합니다.

'(' 와 ')' 로만 이루어진 문자열이 있을 경우, '(' 의 개수와 ')' 의 개수가 같다면 이를 균형잡힌 괄호 문자열이라고 부릅니다.
그리고 여기에 '('와 ')'의 괄호의 짝도 모두 맞을 경우에는 이를 올바른 괄호 문자열이라고 부릅니다.
예를 들어, "(()))("와 같은 문자열은 "균형잡힌 괄호 문자열" 이지만 "올바른 괄호 문자열"은 아닙니다.
반면에 "(())()"와 같은 문자열은 "균형잡힌 괄호 문자열" 이면서 동시에 "올바른 괄호 문자열" 입니다.
'(' 와 ')' 로만 이루어진 문자열 w가 "균형잡힌 괄호 문자열" 이라면 다음과 같은 과정을 통해 "올바른 괄호 문자열"로 변환할 수 있습니다.

'(' 와 ')' 로만 이루어진 문자열 w가 "균형잡힌 괄호 문자열" 이라면 다음과 같은 과정을 통해 "올바른 괄호 문자열"로 변환할 수 있습니다.

1. 입력이 빈 문자열인 경우, 빈 문자열을 반환합니다. 
2. 문자열 w를 두 "균형잡힌 괄호 문자열" u, v로 분리합니다. 단, u는 "균형잡힌 괄호 문자열"로 더 이상 분리할 수 없어야 하며, v는 빈 문자열이 될 수 있습니다. 
3. 문자열 u가 "올바른 괄호 문자열" 이라면 문자열 v에 대해 1단계부터 다시 수행합니다. 3-1. 수행한 결과 문자열을 u에 이어 붙인 후 반환합니다. 
4. 문자열 u가 "올바른 괄호 문자열"이 아니라면 아래 과정을 수행합니다. 4-1. 빈 문자열에 첫 번째 문자로 '('를 붙입니다. 4-2. 문자열 v에 대해 1단계부터 재귀적으로 수행한 결과 문자열을 이어 붙입니다. 4-3. ')'를 다시 붙입니다. 
   4-4. u의 첫 번째와 마지막 문자를 제거하고, 나머지 문자열의 괄호 방향을 뒤집어서 뒤에 붙입니다. 4-5. 생성된 문자열을 반환합니다.
 
"균형잡힌 괄호 문자열" p가 매개변수로 주어질 때, 주어진 알고리즘을 수행해 "올바른 괄호 문자열"로 변환한 결과를 return 하도록 solution 함수를 완성해 주세요.

My solution: 

.. code-block:: Java
    :linenos:

    class Solution {
        public String solution(String p) {
            if (this.isProper(p)) return p;
            return this.helper(p);
        }

        private String helper(String p) {
            if ("".equals(p)) return "";
            int splitIdx = this.splitToUandV(p);
            String u = p.substring(0,splitIdx);
            String v = p.substring(splitIdx,p.length());
            if (this.isProper(u)) {
                return u + this.helper(v);
            } else {
                return "("+this.helper(v)+")"+this.stripAndInvert(u);
            }
        }
        
        private boolean isProper(String p) {
            int openCount = 0;
            for (char c : p.toCharArray()) {
                if (c=='(') {
                    openCount++;
                } else {
                    if (--openCount<0) return false;
                }
            }
            return true;
        }
        
        private int splitToUandV(String p) {
            int openCount = 0;
            int i = 0;
            do {
                if (p.charAt(i++)=='(') {
                    openCount++;
                } else { 
                    openCount--; 
                }
            } while (openCount!=0);
            return i; //exclusive upperbound
        }
        
        private String stripAndInvert(String p) {
            StringBuilder res = new StringBuilder();
            for (int i = 1; i<p.length()-1; i++) {
                res.append(p.charAt(i)=='('?')':'(');
            }
            return res.toString();
        }
    }

Remarks and Complexity Analysis: 
 * Quite simple when the instructions are read thoroughly
 * Really helped to use OOP principles (i.e. single responsbility principle) - designing each (helper) function to do 
   one task simplified a seemingly complex problem into a series of simple operations!
 * **Time Complexity**: ``>O(n^2)`` as ``substring()`` operation is ``O(n)``. If we used index pointers instead (**in-place**), it would have been much
   more efficient!
 * **Space Complexity**: ``O(n)``

.. note::
    Try implementing the above question using in-place modification (on ``char[]``) to optimize the time and space complexity!

Question XX: Longest Increasing Subsequence
------------------------------------------------

**REVIEW BACKTRACKING**
