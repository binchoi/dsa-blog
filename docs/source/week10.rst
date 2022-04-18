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
