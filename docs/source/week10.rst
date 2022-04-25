************************
Week 10 [17-23 Apr 2022]
************************
Day 48 [17 Apr]
================
Question 85: 크레인 인형뽑기 게임
------------------------------------------------
게임 화면은 "1 x 1" 크기의 칸들로 이루어진 "N x N" 크기의 정사각 격자이며 위쪽에는 크레인이 있고 오른쪽에는 바구니가 있습니다. (위 그림은 "5 x 5" 크기의 예시입니다). 각 격자 칸에는 다양한 인형이 들어 있으며 인형이 없는 칸은 빈칸입니다. 모든 인형은 "1 x 1" 크기의 격자 한 칸을 차지하며 격자의 가장 아래 칸부터 차곡차곡 쌓여 있습니다. 게임 사용자는 크레인을 좌우로 움직여서 멈춘 위치에서 가장 위에 있는 인형을 집어 올릴 수 있습니다. 집어 올린 인형은 바구니에 쌓이게 되는 데, 이때 바구니의 가장 아래 칸부터 인형이 순서대로 쌓이게 됩니다. 다음 그림은 [1번, 5번, 3번] 위치에서 순서대로 인형을 집어 올려 바구니에 담은 모습입니다.

만약 같은 모양의 인형 두 개가 바구니에 연속해서 쌓이게 되면 두 인형은 터뜨려지면서 바구니에서 사라지게 됩니다. 위 상태에서 이어서 [5번] 위치에서 인형을 집어 바구니에 쌓으면 같은 모양 인형 두 개가 없어집니다.

크레인 작동 시 인형이 집어지지 않는 경우는 없으나 만약 인형이 없는 곳에서 크레인을 작동시키는 경우에는 아무런 일도 일어나지 않습니다. 또한 바구니는 모든 인형이 들어갈 수 있을 만큼 충분히 크다고 가정합니다. (그림에서는 화면표시 제약으로 5칸만으로 표현하였음)
게임 화면의 격자의 상태가 담긴 2차원 배열 board와 인형을 집기 위해 크레인을 작동시킨 위치가 담긴 배열 moves가 매개변수로 주어질 때, 크레인을 모두 작동시킨 후 터트려져 사라진 인형의 개수를 return 하도록 solution 함수를 완성해주세요.

[제한사항]
 * board 배열은 2차원 배열로 크기는 "5 x 5" 이상 "30 x 30" 이하입니다.
 * board의 각 칸에는 0 이상 100 이하인 정수가 담겨있습니다.
 * 0은 빈 칸을 나타냅니다.
 * 1 ~ 100의 각 숫자는 각기 다른 인형의 모양을 의미하며 같은 숫자는 같은 모양의 인형을 나타냅니다.
 * moves 배열의 크기는 1 이상 1,000 이하입니다.
 * moves 배열 각 원소들의 값은 1 이상이며 board 배열의 가로 크기 이하인 자연수입니다.

My solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;

    class Solution {
        public int solution(int[][] board, int[] moves) {
            HashMap<Integer, Stack<Integer>> boardByCol = new HashMap<>();
            Stack<Integer> bucket = new Stack<Integer>();
            
            // fill in boardByCol
            for (int i=0; i < board[0].length; i++) {
                for (int j=board.length-1; j>=0; j--) {
                    if (board[j][i]==0) continue;
                    if (boardByCol.get(i+1)==null) {
                        Stack<Integer> newStk = new Stack<>();
                        newStk.push(board[j][i]);
                        boardByCol.put(i+1, newStk);
                    } else {
                        boardByCol.get(i+1).push(board[j][i]);
                    }
                }
            }
            
            int res = 0;
            // now we iterate through the moves
            for (int move : moves) {
                Stack<Integer> col = boardByCol.get(move);
                if (!col.isEmpty()) {
                    int e = col.pop();
                    if (!bucket.isEmpty() && bucket.peek()==e) {
                        bucket.pop();
                        res+=2;
                    } else {
                        bucket.push(e);
                    }
                }
            }
            return res;
        }
    }

Remarks and Complexity Analysis: 
 * Disappointing performance and problem-solving experience. I feel rusty with my Java syntax. 
 * **Time Complexity**: ``O(m*n + p)`` where ``m=board.length, n=board[0].length, p=move.lenth``.
 * **Space Complexity**: ``O(max(m*n, p))``

Day 49 [18 Apr]
================
Question 86: 실패율
------------------------------------------------
슈퍼 게임 개발자 오렐리는 큰 고민에 빠졌다. 그녀가 만든 프랜즈 오천성이 대성공을 거뒀지만, 요즘 신규 사용자의 수가 급감한 것이다. 원인은 신규 사용자와 기존 사용자 사이에 스테이지 차이가 너무 큰 것이 문제였다.
이 문제를 어떻게 할까 고민 한 그녀는 동적으로 게임 시간을 늘려서 난이도를 조절하기로 했다. 역시 슈퍼 개발자라 대부분의 로직은 쉽게 구현했지만, 실패율을 구하는 부분에서 위기에 빠지고 말았다. 오렐리를 위해 실패율을 구하는 코드를 완성하라.
실패율은 다음과 같이 정의한다.
스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수
전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 하도록 solution 함수를 완성하라.

제한사항
 * 스테이지의 개수 N은 1 이상 500 이하의 자연수이다.
 * stages의 길이는 1 이상 200,000 이하이다.
 * stages에는 1 이상 N + 1 이하의 자연수가 담겨있다.
 * 각 자연수는 사용자가 현재 도전 중인 스테이지의 번호를 나타낸다.
 * 단, N + 1 은 마지막 스테이지(N 번째 스테이지) 까지 클리어 한 사용자를 나타낸다.
 * 만약 실패율이 같은 스테이지가 있다면 작은 번호의 스테이지가 먼저 오도록 하면 된다.
 * 스테이지에 도달한 유저가 없는 경우 해당 스테이지의 실패율은 0 으로 정의한다.

My solution:

.. code-block:: Java
    :linenos:

    import java.util.*;
    class Solution {
        public int[] solution(int N, int[] stages) {
            int attempters = stages.length; 
            int[] stuckers = new int[N+1];
            for (int s : stages) {
                if (s<N+1) stuckers[s]++;
            }
            ArrayList<Stage> stagesArr = new ArrayList<>();
            for (int i = 1; i < N+1; i++) {
                if (attempters==0 || stuckers[i]==0) {
                    stagesArr.add(new Stage(i, 0.0));
                } else {
                    stagesArr.add(new Stage(i, (double) stuckers[i]/attempters));
                    attempters-=stuckers[i];
                }
            }
            
            stagesArr.sort(Collections.reverseOrder());
            stagesArr.stream().forEach(System.out::println);
            
            int[] res = new int[N];
            int j = 0;
            for (Stage s : stagesArr) {
                res[j++] = s.stage;
            }
            return res;
        }
        
        private class Stage implements Comparable<Stage> {
            int stage;
            double failure; 
            
            public Stage(int stage_, double failure_) {
                this.stage = stage_;
                this.failure = failure_;        
            }
            
            @Override
            public int compareTo(Stage s2) {
                if (failure<s2.failure) {
                    return -1;
                } else if (failure>s2.failure) {
                    return 1;
                } else if (stage>s2.stage) {
                    return -1; // we will reverse this such that if the failure is same
                            // we put the lower stage first
                } else {
                    return 1;
                }
            }
            
            @Override
            public String toString() {
                return "stage " + this.stage + " has failure rate: " + this.failure;
            }
        }
    }

Depressing attempts:

.. code-block:: Java
    :linenos:
    
    // previous attempts
    public int[] solution(int N, int[] stages) {
        TreeMap<Integer, Integer> hist = new TreeMap<>(Collections.reverseOrder());
        
        for (int s : stages) {
            hist.put(s, hist.getOrDefault(s, 0)+1);
        }
        
        int acc = 0;
        TreeMap<Double, Set<Integer>> failureToStage = new TreeMap<>(Collections.reverseOrder());
        for (int key : hist.keySet()) {
            System.out.println(key);
            if (key==N+1) {
                
            } else if (acc==0) {
                HashSet<Integer> zeroSet = new HashSet<>();
                zeroSet.add(key);
                failureToStage.put((double) 0, zeroSet);
                acc+=hist.get(key);
            } else {
                double failure =(double) hist.get(key)/acc;
                if (failureToStage.get(failure)==null) {
                    TreeSet<Integer> newSet = new TreeSet<Integer>();
                    newSet.add(key);
                    failureToStage.put(failure, newSet);
                } else {
                    failureToStage.get(failure).add(key);
                }
            }
            acc+=hist.get(key); 
        }
        
        int[] res = new int[hist.keySet().size()];
        int idx = 0;
        for (Double d : failureToStage.keySet()) {
            for (int i : failureToStage.get(d)) {
                res[idx++] = i;
            }
        }
            
        return res;
    }

    //...

    int[] passers;
    int[] stuckers;
    
    public int[] solution(int N, int[] stages) {
        int[] passers = new int[N+1];
        int[] stuckers = new int[N+1];
        for (int s : stages) {
            // increment stuckers
            if (s<N+1) stuckers[s]++;
            
            // increment passers
            int j = 1;
            while (j<s) {
                passers[j++]++;
            }
        }
        
        List<Integer> myList = IntStream.range(1, N+1).boxed().collect(Collectors.toList());
        myList.sort((a,b) -> Double.compare((double) stuckers[a]/passers[a], (double) (stuckers[b]/passers[b])));
        myList.forEach(System.out::println);
        // ...

    }

    import java.util.*;
    class Solution {
        public int[] solution(int N, int[] stages) {
            int attempters = stages.length; 
            int[] stuckers = new int[N+1];
            for (int s : stages) {
                if (s<N+1) stuckers[s]++;
            }
            ArrayList<Stage> stagesArr = new ArrayList<>();
            for (int i = 1; i < N+1; i++) {
                if (attempters==0 || stuckers[i]==0) {
                    stagesArr.add(new Stage(i, 0.0));
                    // System.out.println(">" + i + " has failure rate: 0.0") ;
                    // attempters-=stuckers[i]; // could be deleted?
                } else {
                    stagesArr.add(new Stage(i, (double) stuckers[i]/attempters));
                    // System.out.println(">" + i + " has failure rate: " + (double) stuckers[i]/attempters);
                    attempters-=stuckers[i];
                }
            }
            
            stagesArr.sort(Collections.reverseOrder());
            stagesArr.stream().forEach(System.out::println);
            
            int[] res = new int[N];
            int j = 0;
            for (Stage s : stagesArr) {
                res[j++] = s.stage;
            }
            return res;
            
            
        }
        
        private class Stage implements Comparable<Stage> {
            int stage;
            double failure; 
            
            public Stage(int stage_, double failure_) {
                this.stage = stage_;
                this.failure = failure_;        
            }
            
            @Override
            public int compareTo(Stage s2) {
                if (failure<s2.failure) {
                    return -1;
                } else if (failure>s2.failure) {
                    return 1;
                } else if (stage>s2.stage) {
                    return -1; // we will reverse this such that if the failure is same
                            // we put the lower stage first
                } else {
                    return 1;
                }
            }
            
            @Override
            public String toString() {
                return "stage " + this.stage + " has failure rate: " + this.failure;
            }
        }
    }

Remarks and Complexity Analysis: 
 * This question was such a Java turn off moment. I could think of the algorithm to solve it but couldn't get Java to work with me.
 * If it had been on python, I would have simply used a dictionary and sorted by value and output the keys using lambda expression. In java, 
   I struggled with the fact that such process is arduous and requires so many conversions and state changes. Additionally, I was eagerly searching
   for a way to store two elements of different types in a pair/tuple-like structure and struggled becuase I couldn't find an appropriate one.
 * **Time Complexity**: ``O(n log n)`` where ``n=N`` or - ``O(max(n log n, m))`` where ``m=stage.length``
 * **Space Complexity**: ``O(N)``



Question 87: Reverse a Linked List
------------------------------------------------
Given the head of a singly linked list, reverse the list, and return the reversed list.

My first attempt solution:

.. code-block:: Java
    :linenos:

    import java.util.Stack;
    class Solution {
        public ListNode reverseList(ListNode head) {
            if (head==null) return head;
            Stack<ListNode> nodes = new Stack<>();
            ListNode curr = head;
            while (curr!=null) {
                nodes.push(curr);
                curr = curr.next;
            }
            if (!nodes.isEmpty()) {
                head = nodes.pop();
            }
            ListNode prev = head;
            while (!nodes.isEmpty()) {
                prev.next = nodes.pop();
                prev = prev.next;
            }
            prev.next = null;
            return head;
        }
    }

My in-place solution:

.. code-block:: Java
    :linenos:

    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode next;
        while (head!=null) {
            next = head.next;
            head.next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }

Recursive solution (just for practice): 

.. code-block:: Java
    :linenos:

    public ListNode reverseList(ListNode head) {
        return this.reverseListHelper(null, head);
    }
    
    private ListNode reverseListHelper(ListNode prev, ListNode head) {
        if (head==null) return prev;
        ListNode next = head.next;
        head.next = prev;
        return this.reverseListHelper(head, next);
    }


Remarks and Complexity Analysis: 
 * First attempt was not as elegant as I hoped but it is optimized in terms of Big-O time complexity so I was happy. 
 * Second attempt hit what I was looking for! Much faster and space efficient as well.
 * **Time Complexity**: ``O(n)`` where ``n=List.size()``
 * **Space Complexity**: ``O(n)``

Question 88: 비밀지도
------------------------------------------------
네오는 평소 프로도가 비상금을 숨겨놓는 장소를 알려줄 비밀지도를 손에 넣었다. 그런데 이 비밀지도는 숫자로 암호화되어 있어 위치를 확인하기 위해서는 암호를 해독해야 한다. 다행히 지도 암호를 해독할 방법을 적어놓은 메모도 함께 발견했다.
지도는 한 변의 길이가 n인 정사각형 배열 형태로, 각 칸은 "공백"(" ") 또는 "벽"("#") 두 종류로 이루어져 있다.
전체 지도는 두 장의 지도를 겹쳐서 얻을 수 있다. 각각 "지도 1"과 "지도 2"라고 하자. 지도 1 또는 지도 2 중 어느 하나라도 벽인 부분은 전체 지도에서도 벽이다. 지도 1과 지도 2에서 모두 공백인 부분은 전체 지도에서도 공백이다.
"지도 1"과 "지도 2"는 각각 정수 배열로 암호화되어 있다.
암호화된 배열은 지도의 각 가로줄에서 벽 부분을 1, 공백 부분을 0으로 부호화했을 때 얻어지는 이진수에 해당하는 값의 배열이다.

네오가 프로도의 비상금을 손에 넣을 수 있도록, 비밀지도의 암호를 해독하는 작업을 도와줄 프로그램을 작성하라.
입력 형식
입력으로 지도의 한 변 크기 n 과 2개의 정수 배열 arr1, arr2가 들어온다.
1 ≦ n ≦ 16
arr1, arr2는 길이 n인 정수 배열로 주어진다.
정수 배열의 각 원소 x를 이진수로 변환했을 때의 길이는 n 이하이다. 즉, 0 ≦ x ≦ 2n - 1을 만족한다.

My solution: 

.. code-block:: Java
    :linenos:

    public String[] solution(int n, int[] arr1, int[] arr2) {
        String[] res = new String[n];
        for (int i = 0; i < n; i++) {
            String binStr =Integer.toBinaryString(arr1[i] | arr2[i]);
            res[i] = " ".repeat(n-binStr.length())+(binStr.replaceAll("1", "#").replaceAll("0", " "));
        }
        return res;
    }

My first naive solution:

.. code-block:: Java
    :linenos:
    
    import java.util.*;
    class Solution {
        public String[] solution(int n, int[] arr1, int[] arr2) {
            
            String[] arr1Str = this.helper(arr1, n);
            String[] arr2Str = this.helper(arr2, n);
            
            String[] res = new String[n];
            for (int i = 0; i < n ; i++) {          
                StringBuilder line = new StringBuilder();
                for (int j = 0 ; j < n ; j++) {
                    if (arr1Str[i].charAt(j)=='0' && arr2Str[i].charAt(j)=='0') {
                        line.append(' ');
                    } else {
                        line.append('#');
                    }
                }
                res[i] = line.toString();
            }
            
            return res;
        }
        
        private String[] helper(int[] input, int n) {
            String[] output = new String[n];
            for (int i=0; i<input.length; i++) {
                String binStr = (Integer.toBinaryString(input[i]));
                output[i] = "0".repeat(n-binStr.length())+binStr;
            }
            return output;
        }
    }

Remarks and Complexity Analysis: 
 * Pretty simple and good reminder of the power that bit-wise operators contain. 
 * **Time Complexity**: ``O(n^2)`` -- I think the String operations / manipulations would actually have added exponential time
 * **Space Complexity**: ``O(n)``

Question 89: 다트 게임
------------------------------------------------
카카오톡 게임별의 하반기 신규 서비스로 다트 게임을 출시하기로 했다. 다트 게임은 다트판에 다트를 세 차례 던져 그 점수의 합계로 실력을 겨루는 게임으로, 모두가 간단히 즐길 수 있다.
갓 입사한 무지는 코딩 실력을 인정받아 게임의 핵심 부분인 점수 계산 로직을 맡게 되었다. 다트 게임의 점수 계산 로직은 아래와 같다.
다트 게임은 총 3번의 기회로 구성된다.
점수와 함께 Single(S), Double(D), Triple(T) 영역이 존재하고 각 영역 당첨 시 점수에서 1제곱, 2제곱, 3제곱 (점수1 , 점수2 , 점수3 )으로 계산된다.
옵션으로 스타상(*) , 아차상(#)이 존재하며 스타상(*) 당첨 시 해당 점수와 바로 전에 얻은 점수를 각 2배로 만든다. 아차상(#) 당첨 시 해당 점수는 마이너스된다.
스타상(*)은 첫 번째 기회에서도 나올 수 있다. 이 경우 첫 번째 스타상(*)의 점수만 2배가 된다. (예제 4번 참고)
스타상(*)의 효과는 다른 스타상(*)의 효과와 중첩될 수 있다. 이 경우 중첩된 스타상(*) 점수는 4배가 된다. (예제 4번 참고)
스타상(*)의 효과는 아차상(#)의 효과와 중첩될 수 있다. 이 경우 중첩된 아차상(#)의 점수는 -2배가 된다. (예제 5번 참고)
Single(S), Double(D), Triple(T)은 점수마다 하나씩 존재한다.
스타상(*), 아차상(#)은 점수마다 둘 중 하나만 존재할 수 있으며, 존재하지 않을 수도 있다.
0~10의 정수와 문자 S, D, T, \*, #로 구성된 문자열이 입력될 시 총점수를 반환하는 함수를 작성하라.
입력 형식
"점수|보너스|[옵션]"으로 이루어진 문자열 3세트.
예) 1S2D*3T
점수는 0에서 10 사이의 정수이다.
보너스는 S, D, T 중 하나이다.
옵선은 \*이나 # 중 하나이며, 없을 수도 있다.
출력 형식
3번의 기회에서 얻은 점수 합계에 해당하는 정수값을 출력한다.
예) 37


My solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;
    import java.util.stream.IntStream;

    class Solution {
        public int solution(String dartResult) {   
            // identify borders
            int[] borders = new int[3];
            int j = 1;
            for (int i = 1; i<dartResult.length(); i++) {
                if (Character.isDigit(dartResult.charAt(i))&&!Character.isDigit(dartResult.charAt(i-1))) {
                    borders[j++] = i;
                }
            }
            
            int[] pointsList = new int[3];
            
            this.pointCalc(dartResult, 0, borders[1], pointsList, 0);
            this.pointCalc(dartResult, borders[1], borders[2], pointsList, 1);
            this.pointCalc(dartResult, borders[2], dartResult.length(), pointsList, 2);

            return IntStream.of(pointsList).sum();
        }
        
        private void pointCalc(String dartResult, int lo, int hi, int[] pointsList, int pointIdx) {
            int res;
            int pointer = lo;
            if (Character.isDigit(dartResult.charAt(lo+1))) {
                res = Integer.parseInt(dartResult.substring(lo, lo+2));
                pointer+=2;
            } else {
                res = Character.getNumericValue(dartResult.charAt(lo));
                pointer+=1;
            }
            
            switch (dartResult.charAt(pointer)) {
                case 'D': 
                    res*=res;
                    break;
                case 'T': 
                    res*=(res*res);
                    break;
                default: 
                    break;
            }
            pointer++;
            
            if (pointer<hi) {
                switch (dartResult.charAt(pointer)) {
                    case '*':
                        res*=2;
                        if (pointIdx>0) {
                            pointsList[pointIdx-1]*=2;
                        }
                        break;
                    case '#': 
                        res*=-1;
                        break;
                }
            }
            pointsList[pointIdx] = res;
        }
    }

    //alternative fun solution
    import java.util.*;
    class Solution {
        public int solution(String dartResult) {
            Stack<Integer> stack = new Stack<>();
            int sum = 0;
            for (int i = 0; i < dartResult.length(); ++i) {
                char c = dartResult.charAt(i);
                if (Character.isDigit(c)) {
                    sum = (c - '0');
                    if (sum == 1 && i < dartResult.length() - 1 && dartResult.charAt(i + 1) == '0') {
                        sum = 10;
                        i++;
                    }
                    stack.push(sum);
                } else {
                    int prev = stack.pop();
                    if (c == 'D') {
                        prev *= prev;
                    } else if (c == 'T') {
                        prev = prev * prev * prev;
                    } else if (c == '*') {
                        if (!stack.isEmpty()) {
                            int val = stack.pop() * 2;
                            stack.push(val);
                        }
                        prev *= 2;
                    } else if (c == '#') {
                        prev *= (-1);
                    }
                    // System.out.println(prev);
                    stack.push(prev);
                }
            }
            int totalScore = 0;
            while (!stack.isEmpty()) {
                totalScore += stack.pop();
            }
            return totalScore;
        }
    }

Remarks and Complexity Analysis: 
 * Not too difficult apart from java syntax (converting String to int and Character to int - i.e. ``Integer.valueOf(str)`` and ``Character.getNumericValue(char)``)
 * I could optimize this further but it is 2am -- will get back to it later!
 * **Time Complexity**: ``O(n)``
 * **Space Complexity**: ``O(n)``

**RECORD CHARACTER METHODS**

Day 50 [20 Apr]
================
Question 90: Subsets
------------------------------------------------
Given an integer array nums of unique elements, return all possible subsets (the power set).
The solution set must not contain duplicate subsets. Return the solution in any order.

My solution: 

.. code-block:: Java
    :linenos: 

    import java.util.ArrayList;
    class Solution {
        public List<List<Integer>> subsets(int[] nums) {
            List<List<Integer>> res = new ArrayList<>();
            ArrayList<Integer> tempList = new ArrayList<>();
            this.backtrack(res, tempList, 0, nums);
            return res;
        }
        
        public void backtrack(List<List<Integer>> res, ArrayList<Integer> tempList, int idx, int[] nums) {
            res.add(new ArrayList<Integer>(tempList));
            for (int i = idx; i < nums.length; i++) {
                tempList.add(nums[i]);
                this.backtrack(res, tempList, i+1, nums);
                tempList.remove(tempList.size()-1);
            }
        }
    }

Remarks and Complexity Analysis: 
 * Pretty intuitive after reading the general approach to backtracking!
 * **Time Complexity**: ``O(n^2)`` - common nested recursion/for-loop
 * **Space Complexity**: ``O(n)`` - for ``tempList``

Day 51 [22 Apr]
================
Question 91: 문자열 압축
------------------------------------------------
데이터 처리 전문가가 되고 싶은 "어피치"는 문자열을 압축하는 방법에 대해 공부를 하고 있습니다. 최근에 대량의 데이터 처리를 위한 간단한 비손실 압축 방법에 대해 공부를 하고 있는데, 문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 알고리즘을 공부하고 있습니다.
간단한 예로 "aabbaccc"의 경우 "2a2ba3c"(문자가 반복되지 않아 한번만 나타난 경우 1은 생략함)와 같이 표현할 수 있는데, 이러한 방식은 반복되는 문자가 적은 경우 압축률이 낮다는 단점이 있습니다. 예를 들면, "abcabcdede"와 같은 문자열은 전혀 압축되지 않습니다. "어피치"는 이러한 단점을 해결하기 위해 문자열을 1개 이상의 단위로 잘라서 압축하여 더 짧은 문자열로 표현할 수 있는지 방법을 찾아보려고 합니다.
예를 들어, "ababcdcdababcdcd"의 경우 문자를 1개 단위로 자르면 전혀 압축되지 않지만, 2개 단위로 잘라서 압축한다면 "2ab2cd2ab2cd"로 표현할 수 있습니다. 다른 방법으로 8개 단위로 잘라서 압축한다면 "2ababcdcd"로 표현할 수 있으며, 이때가 가장 짧게 압축하여 표현할 수 있는 방법입니다.
다른 예로, "abcabcdede"와 같은 경우, 문자를 2개 단위로 잘라서 압축하면 "abcabc2de"가 되지만, 3개 단위로 자른다면 "2abcdede"가 되어 3개 단위가 가장 짧은 압축 방법이 됩니다. 이때 3개 단위로 자르고 마지막에 남는 문자열은 그대로 붙여주면 됩니다.
압축할 문자열 s가 매개변수로 주어질 때, 위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라 압축하여 표현한 문자열 중 가장 짧은 것의 길이를 return 하도록 solution 함수를 완성해주세요.

제한사항
 * s의 길이는 1 이상 1,000 이하입니다.
 * s는 알파벳 소문자로만 이루어져 있습니다.

입출력 예
 * s	result
 * "aabbaccc"	7
 * "ababcdcdababcdcd"	9
 * "abcabcdede"	8
 * "abcabcabcabcdededededede"	14
 * "xababcdcdababcdcd"	17

입출력 예에 대한 설명
 * 입출력 예 #1 문자열을 1개 단위로 잘라 압축했을 때 가장 짧습니다.
 * 입출력 예 #2 문자열을 8개 단위로 잘라 압축했을 때 가장 짧습니다.
 * 입출력 예 #3 문자열을 3개 단위로 잘라 압축했을 때 가장 짧습니다.
 * 입출력 예 #4 문자열을 2개 단위로 자르면 "abcabcabcabc6de" 가 됩니다. 문자열을 3개 단위로 자르면 "4abcdededededede" 가 됩니다. 문자열을 4개 단위로 자르면 "abcabcabcabc3dede" 가 됩니다. 문자열을 6개 단위로 자를 경우 "2abcabc2dedede"가 되며, 이때의 길이가 14로 가장 짧습니다.
 * 입출력 예 #5 문자열은 제일 앞부터 정해진 길이만큼 잘라야 합니다. 따라서 주어진 문자열을 x / ababcdcd / ababcdcd 로 자르는 것은 불가능 합니다. 이 경우 어떻게 문자열을 잘라도 압축되지 않으므로 가장 짧은 길이는 17이 됩니다.


My solution: 

.. code-block:: Java
    :linenos: 

    class Solution {
        public int solution(String s) {
            int answer = Integer.MAX_VALUE;
            int len = s.length();
            
            if(s.length()==1) return 1;
            
            for(int r=1; r<=len/2; r++) {
                String pattern  = s.substring(0,r);
                int compLen =0;
                int cnt =1;
                String reStr="";
                for(int i=r; i<=s.length()-r; i+=r){
                    if(pattern.equals(s.substring(i,i+r))){
                        cnt++;
                    }else {
                        if(cnt>1) {
                            reStr += cnt+"";
                        }
                        reStr += pattern;
                        pattern = s.substring(i,i+r);
                        cnt=1;
                    }
                }
                
                if(cnt>1) {
                    reStr+= ""+cnt;
                }
                reStr+= pattern;
                
                int div = s.length()%r;
                answer = Math.min(answer, reStr.length()+div);
            }
            return answer;
        }
    }

    class Solution {
        public int solution(String s) {
            int min = s.length();
            int len = s.length()/2+1;
            for(int i = 1; i < len; i++) {
                String before = "";
                int sum = 0;
                int cnt = 1;
                for(int j = 0; j < s.length();) {               
                    int start = j;
                    j = (j+i > s.length()) ? s.length():j+i;
                    String temp = s.substring(start, j);
                    if(temp.equals(before)) {
                        cnt++;
                    } else {
                        if(cnt != 1) {
                            sum += (int)Math.log10(cnt)+1;
                        }
                        cnt = 1;
                        sum+=before.length();
                        before = temp;
                    }
                }
                sum+=before.length();
                if(cnt != 1) {
                    sum += (int)Math.log10(cnt)+1;
                }
                min = (min > sum) ? sum : min;
            }

            return min;
        }
    }

    class Solution {
        public int solution(String s) {
            int res = s.length();
            for (int unit = 1; unit <= s.length()/2; unit++) {
                int count = 1;
                String prev = "A";
                int saved = 0;
                for (int i=0; i+unit < s.length()+1; i+=unit) {
                    String found = s.substring(i, i+unit);
                    if (prev.equals(found)) {
                        // System.out.println("prev: " + prev + " vs. found: " + found + "count increased");
                        count++;
                    } else {
                        if (count>1) {
                            saved+=(unit*(count-1)-(1+Math.floorDiv(count,10)));
                        }
                        prev=found;
                        count=1;
                    }
                }
                if (count>1) {
                    saved+=(unit*(count-1)-1);
                }
                res = Math.min(res, s.length()-saved);
            }
            return res;
        }
    }