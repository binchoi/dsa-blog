************************
Week 9 [6-13 Mar 2022]
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

https://leetcode.com/problems/longest-common-subsequence/discuss/351689/JavaPython-3-Two-DP-codes-of-O(mn)-and-O(min(m-n))-spaces-w-picture-and-analysis