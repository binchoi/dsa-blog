************************
Week 11 [25-30 Apr 2022]
************************
Day 52 [25 Apr]
================
Question 92: Linked List Cycle
------------------------------------------------
Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

My final solution: 

.. code-block:: Java
    :linenos:

    public boolean hasCycle(ListNode head) {
        if (head==null) return false;
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next!=null && fast.next.next!=null) { // if null is ever reached => no Cycle!
            fast=fast.next.next;
            slow=slow.next;
            if (fast.equals(slow)) {
                return true;
            }
        }
        return false;
    }

My first ``O(n)`` solution: 

.. code-block:: Java
    :linenos:

    public boolean hasCycle(ListNode head) {
        HashSet<ListNode> seenNodes = new HashSet<>();
        while (head!=null) {
            if (seenNodes.contains(head)) {
                return true;
            }
            seenNodes.add(head);
            head=head.next;
        }
        return false;
    }


Remarks and Complexity Analysis: 
 * Easy question but really interesting way to solve with constant space-complexity (Floyd’s Cycle-Finding Algorithm)
 * **Time Complexity**: ``O(n)`` where ``n=list.size()``. 
 * **Space Complexity**: ``O(1)`` for final solution & ``O(n)`` for naive solution

Question 93: 오픈채팅방
------------------------------------------------
카카오톡 오픈채팅방에서는 친구가 아닌 사람들과 대화를 할 수 있는데, 본래 닉네임이 아닌 가상의 닉네임을 사용하여 채팅방에 들어갈 수 있다.
신입사원인 김크루는 카카오톡 오픈 채팅방을 개설한 사람을 위해, 다양한 사람들이 들어오고, 나가는 것을 지켜볼 수 있는 관리자창을 만들기로 했다. 채팅방에 누군가 들어오면 다음 메시지가 출력된다.
"[닉네임]님이 들어왔습니다."
채팅방에서 누군가 나가면 다음 메시지가 출력된다.
"[닉네임]님이 나갔습니다."
채팅방에서 닉네임을 변경하는 방법은 다음과 같이 두 가지이다.
채팅방을 나간 후, 새로운 닉네임으로 다시 들어간다.
채팅방에서 닉네임을 변경한다.
닉네임을 변경할 때는 기존에 채팅방에 출력되어 있던 메시지의 닉네임도 전부 변경된다.
예를 들어, 채팅방에 "Muzi"와 "Prodo"라는 닉네임을 사용하는 사람이 순서대로 들어오면 채팅방에는 다음과 같이 메시지가 출력된다.
"Muzi님이 들어왔습니다."
"Prodo님이 들어왔습니다."
채팅방에 있던 사람이 나가면 채팅방에는 다음과 같이 메시지가 남는다.
"Muzi님이 들어왔습니다."
"Prodo님이 들어왔습니다."
"Muzi님이 나갔습니다."
Muzi가 나간후 다시 들어올 때, Prodo 라는 닉네임으로 들어올 경우 기존에 채팅방에 남아있던 Muzi도 Prodo로 다음과 같이 변경된다.
"Prodo님이 들어왔습니다."
"Prodo님이 들어왔습니다."
"Prodo님이 나갔습니다."
"Prodo님이 들어왔습니다."
채팅방은 중복 닉네임을 허용하기 때문에, 현재 채팅방에는 Prodo라는 닉네임을 사용하는 사람이 두 명이 있다. 이제, 채팅방에 두 번째로 들어왔던 Prodo가 Ryan으로 닉네임을 변경하면 채팅방 메시지는 다음과 같이 변경된다.
"Prodo님이 들어왔습니다."
"Ryan님이 들어왔습니다."
"Prodo님이 나갔습니다."
"Prodo님이 들어왔습니다."
채팅방에 들어오고 나가거나, 닉네임을 변경한 기록이 담긴 문자열 배열 record가 매개변수로 주어질 때, 모든 기록이 처리된 후, 최종적으로 방을 개설한 사람이 보게 되는 메시지를 문자열 배열 형태로 return 하도록 solution 함수를 완성하라.

제한사항
 * record는 다음과 같은 문자열이 담긴 배열이며, 길이는 1 이상 100,000 이하이다.
 * 다음은 record에 담긴 문자열에 대한 설명이다.
 * 모든 유저는 [유저 아이디]로 구분한다.
 * [유저 아이디] 사용자가 [닉네임]으로 채팅방에 입장 - "Enter [유저 아이디] [닉네임]" (ex. "Enter uid1234 Muzi")
 * [유저 아이디] 사용자가 채팅방에서 퇴장 - "Leave [유저 아이디]" (ex. "Leave uid1234")
 * [유저 아이디] 사용자가 닉네임을 [닉네임]으로 변경 - "Change [유저 아이디] [닉네임]" (ex. "Change uid1234 Muzi")
 * 첫 단어는 Enter, Leave, Change 중 하나이다.
 * 각 단어는 공백으로 구분되어 있으며, 알파벳 대문자, 소문자, 숫자로만 이루어져있다.
 * 유저 아이디와 닉네임은 알파벳 대문자, 소문자를 구별한다.
 * 유저 아이디와 닉네임의 길이는 1 이상 10 이하이다.
 * 채팅방에서 나간 유저가 닉네임을 변경하는 등 잘못 된 입력은 주어지지 않는다.

My solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {
        public String[] solution(String[] record) {
            HashMap<String, ArrayList<Integer>> uidToMsgIdx = new HashMap<>();
            HashMap<String, String> uidToNickname = new HashMap<>();
            ArrayList<String> msgList = new ArrayList<>();
            int msgListIdx = 0;
            
            for (String r : record) {
                String[] rArr = r.split(" ");

                if (rArr[0].equals("Enter") || rArr[0].equals("Leave")) {
                    if (rArr[0].equals("Enter")) {
                        msgList.add("님이 들어왔습니다.");
                    } else {
                        msgList.add("님이 나갔습니다.");
                    }
                    this.addMsgIdxToMap(uidToMsgIdx, rArr[1], msgListIdx++);
                }
                
                if (rArr.length==3) { // nickname change
                    uidToNickname.put(rArr[1], rArr[2]);
                }
            }
            
            for (String uid: uidToMsgIdx.keySet()) {
                for (int msgIdx : uidToMsgIdx.get(uid)) {
                    msgList.set(msgIdx, (uidToNickname.get(uid)+msgList.get(msgIdx)));
                }
            }
            
            return msgList.toArray(new String[msgList.size()]);
        }
        
        private void addMsgIdxToMap(HashMap<String, ArrayList<Integer>> map, String uid, Integer idx) {
            if (map.containsKey(uid)) {
                map.get(uid).add(idx);
            } else {
                map.put(uid, new ArrayList<Integer>(Arrays.asList(idx)));
            }
        }
    }

Remarks and Complexity Analysis: 
 * Very satisfying problem solving experience. I spent a good amount of time planning and writing down the things I need to do at each step which
   helped me a lot when actually implementing the solution.
 * There were no difficult edge cases involved in this question and it was fairly straight forward. 
 * Compartmentalizing repeated processes as a seperate private function was helpful. 
 * Using Javatool box was also good!
 * **Time Complexity**: ``O(n)`` where ``n=record.length``. 
 * **Space Complexity**: ``O(n)``


Question 94: 기능개발
------------------------------------------------
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.
또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.
먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

제한 사항
 * 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
 * 작업 진도는 100 미만의 자연수입니다.
 * 작업 속도는 100 이하의 자연수입니다.
 * 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

My solution: 

.. code-block:: Java
    :linenos:   

    import java.util.ArrayList;
    import java.util.stream.*;

    class Solution {
        public int[] solution(int[] progresses, int[] speeds) {
            
            if (progresses.length==0) {
                return new int[] {};
            }
            
            int[] daysRemaining = new int[progresses.length];
            for (int i = 0; i<progresses.length; i++) {
                daysRemaining[i] = -Math.floorDiv(-(100-progresses[i]),speeds[i]);
            }
            
            ArrayList<Integer> deploysPerDay = new ArrayList<>(); 
            
            int count = 1;
            int prev = daysRemaining[0];
            for (int i = 1; i<daysRemaining.length; i++) {
                if (daysRemaining[i]<=prev) {
                    count++;
                } else {
                    deploysPerDay.add(count);
                    prev = daysRemaining[i];
                    count = 1;
                }
            }
            // add last
            deploysPerDay.add(count);

            return deploysPerDay.stream().mapToInt(i -> i).toArray();
        }
    }

Remarks and Complexity Analysis: 
 * Pretty simple. Could have used another data structure like stack, but using a good old ArrayList was intuitive to me
 * **Time Complexity**: ``O(n)`` where ``n=progresses.length``. 
 * **Space Complexity**: ``O(n)``


Optimal space-complexity solution: 

.. code-block:: Java
    :linenos:   

    import java.util.ArrayList;
    import java.util.Arrays;
    class Solution {
        public int[] solution(int[] progresses, int[] speeds) {
            int[] dayOfend = new int[100];
            int day = -1;
            for(int i=0; i<progresses.length; i++) {
                while(progresses[i] + (day*speeds[i]) < 100) {
                    day++;
                }
                dayOfend[day]++;
            }
            return Arrays.stream(dayOfend).filter(i -> i!=0).toArray();
        }
    }


Question 95: 멀쩡한 사각형
------------------------------------------------
가로 길이가 Wcm, 세로 길이가 Hcm인 직사각형 종이가 있습니다. 종이에는 가로, 세로 방향과 평행하게 격자 형태로 선이 그어져 있으며, 모든 격자칸은 1cm x 1cm 크기입니다. 이 종이를 격자 선을 따라 1cm × 1cm의 정사각형으로 잘라 사용할 예정이었는데, 누군가가 이 종이를 대각선 꼭지점 2개를 잇는 방향으로 잘라 놓았습니다. 그러므로 현재 직사각형 종이는 크기가 같은 직각삼각형 2개로 나누어진 상태입니다. 새로운 종이를 구할 수 없는 상태이기 때문에, 이 종이에서 원래 종이의 가로, 세로 방향과 평행하게 1cm × 1cm로 잘라 사용할 수 있는 만큼만 사용하기로 하였습니다.
가로의 길이 W와 세로의 길이 H가 주어질 때, 사용할 수 있는 정사각형의 개수를 구하는 solution 함수를 완성해 주세요.

My incomplete solution: 

.. code-block:: Java
    :linenos:

    import static java.lang.Math.*;

    class Solution {
        public long solution(int w, int h) {
            if (h>w) {
                return this.helper(w,h);
            } else {
                return this.helper(h,w);
            }
        }
        
        private long helper(int w, int h) {
            long res = w*h-h;
            double slope = (double) h/w;
            double yVal = 0.0;
            for (int i = 1; i<=w; i++) {
                yVal+=(slope);
                if (Math.floor(yVal)!=Math.ceil(yVal)) {
                    System.out.println(yVal);
                    res--;
                }
            }
            return res;
        }
    }
            
Remarks: 
 * I thought that the minimum number of blocks rendered unusable for any given plot would be the ``(longer_side_length)`` when the two sides are equal
   (which is true) and that I coud simply determine the number of usuable blocks by tracing the y-value of the line for each x increment to check if it is a whole number or not (in which case an additional block is rendered unusable on top of that).

Day 53 [01 May]
================
Question 96: 가장 큰 수
------------------------------------------------
0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

My final solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;
    import java.util.stream.*;

    class Solution {
        public String solution(int[] numbers) {
            StringBuilder res = new StringBuilder();
            String[] strArr = Arrays.stream(numbers).mapToObj(e -> String.valueOf(e)).toArray(String[]::new);
            
            Arrays.sort(strArr, new Comparator<String>() {
                @Override
                public int compare(String o1, String o2) {
                    return (o2+o1).compareTo(o1+o2);
                }
            });
            
            Arrays.stream(strArr).forEach(s -> res.append(s));

            return res.toString().charAt(0)=='0' ? "0" : res.toString();
        }
    }

Remarks and Complexity Analysis: 
 * It wasn't a difficult question but a good one to review sorting!
 * **Time Complexity**: ``O(n log n)`` where ``n=numbers.length``. 
 * **Space Complexity**: ``O(n log n)`` - could be more depending on String concat method

.. note:: 
    implement an annonymous Comparator class and override the ``compare`` method.

Similar, alternative solution: 

.. code-block:: Java
    :linenos:

    class Solution {
        public String solution(int[] numbers) {
            StringBuilder res = new StringBuilder();
            String[] strArr = Arrays.stream(numbers).mapToObj(e -> String.valueOf(e)).toArray(String[]::new);
            
            Arrays.sort(strArr, new Comparator<String>() {
                @Override
                public int compare(String o1, String o2) {
                    return -Integer.compare(Integer.valueOf(o1+o2), Integer.valueOf(o2+o1));
                }
            });
            
            Arrays.stream(strArr).forEach(s -> res.append(s));

            return res.toString().charAt(0)=='0' ? "0" : res.toString();
        }
    }


Question 97: H-Index
------------------------------------------------
H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과1에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었다면 h의 최댓값이 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

제한사항
 * 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
 * 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

My final solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {
        public int solution(int[] citations) {
            Arrays.sort(citations);        
            for (int h = citations.length-1 ; h >= 0; h--) {
                if (citations[h]<citations.length-h) return citations.length-h-1;
            }
            return citations.length;
        }
    }

    // sol2. when I insisted on sorting the list in descending order..
    import java.util.*;

    class Solution {
        public int solution(int[] citations) {
            // sort in descending
            Arrays.sort(citations);
            this.reverse(citations);
            
            for (int h = 1 ; h <= citations.length; h++) {
                if (citations[h-1]<h) return h-1;
            }
            
            return citations.length;
        }
        
        public static void reverse(int[] input) { 
            int last = input.length - 1; 
            int middle = input.length / 2; 
            for (int i = 0; i <= middle; i++) { 
                int temp = input[i]; 
                input[i] = input[last - i]; 
                input[last - i] = temp; 
            } 
        }
    }

    //initial sol that also sorted the list in descending order but with linear space complexity
    import java.util.*;

    class Solution {
        public int solution(int[] citations) {
            Integer[] hIdx = Arrays.stream(citations) // not satisfied because this increases space complexity from O(1) -> O(n)
                .boxed()
                .sorted(Collections.reverseOrder())
                .toArray(Integer[]::new);
                        
            for (int h = 1 ; h <= citations.length; h++) {
                if (hIdx[h-1]<h) return h-1;
            }
            
            return citations.length;
        }
    }

Remarks and Complexity Analysis: 
 * Good practice and revision. Improving by the question - let's go!!
 * If we are using for-loop after sorting, we don't have to forcefully sort in descending order, we can sort ascending and then traverse opposite direction
 * **Time Complexity**: ``O(n log n)`` where ``n=citations.length``. 
 * **Space Complexity**: ``O(1)``


Question 98: 모의고사
------------------------------------------------
수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

제한 조건
 * 시험은 최대 10,000 문제로 구성되어있습니다.
 * 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
 * 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

My solution: 

.. code-block:: Java
    :linenos:

    //clean solution
    import java.util.ArrayList;
    class Solution {
        public int[] solution(int[] answer) {
            int[] a = {1, 2, 3, 4, 5};
            int[] b = {2, 1, 2, 3, 2, 4, 2, 5};
            int[] c = {3, 3, 1, 1, 2, 2, 4, 4, 5, 5};
            int[] points = new int[3];
            for(int i=0; i<answer.length; i++) {
                if(answer[i] == a[i%a.length]) {points[0]++;}
                if(answer[i] == b[i%b.length]) {points[1]++;}
                if(answer[i] == c[i%c.length]) {points[2]++;}
            }
            int maxScore = Math.max(points[0], Math.max(points[1], points[2]));
            ArrayList<Integer> list = new ArrayList<>();
            if(maxScore == score[0]) list.add(1);
            if(maxScore == score[1]) list.add(2);
            if(maxScore == score[2]) list.add(3);
            return list.stream().mapToInt(i->i.intValue()).toArray();
        }
    }

    //unclean solution
    import java.util.*;
    import java.util.stream.*;

    class Solution {
        
        private int[] p1, p2, p3;
        private HashMap<Integer,Integer> points;
        
        public int[] solution(int[] answers) {
            p1 = new int[] {1, 2, 3, 4, 5};
            p2 = new int[] {2, 1, 2, 3, 2, 4, 2, 5};
            p3 = new int[] {3, 3, 1, 1, 2, 2, 4, 4, 5, 5};
                    
            points = new HashMap<>();
            
            for (int i = 0; i < answers.length; i++) {
                this.helper(1, answers, i);
                this.helper(2, answers, i);
                this.helper(3, answers, i);
            }
            
            int maxPoints = 0;
            for (int k : points.keySet()) {
                maxPoints = Math.max(maxPoints, points.getOrDefault(k, 0));
            }
            
            LinkedList<Integer> list = new LinkedList<>();

            for (int k : points.keySet()) {
                if (points.get(k)==maxPoints) list.add(k);
            }
            
            return list.stream().mapToInt(e -> (int) e).toArray();
        }
        
        private void helper(int playerNum, int[] answers, int idx) {
            int[] playerChoices = new int[answers.length];
            switch (playerNum) {
                case 1: 
                    playerChoices = p1;
                    break;
                case 2:
                    playerChoices = p2;
                    break;
                case 3:
                    playerChoices = p3;
                    break;
            }
            if (playerChoices[idx%playerChoices.length]==answers[idx]) {
                points.put(playerNum, points.getOrDefault(playerNum,0)+1);
            }
        }
    }

Remarks and Complexity Analysis: 
 * Conversions got very icky.
 * **Time Complexity**: ``O(n)`` where ``n=answers.length``. 
 * **Space Complexity**: ``O(1)``