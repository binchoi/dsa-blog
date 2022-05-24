************************
Week 13 [18-25 May 2022]
************************
Day 57 [18 May]
================
Question 106: 후보키
------------------------------------------------
프렌즈대학교 컴퓨터공학과 조교인 제이지는 네오 학과장님의 지시로, 학생들의 인적사항을 정리하는 업무를 담당하게 되었다.

그의 학부 시절 프로그래밍 경험을 되살려, 모든 인적사항을 데이터베이스에 넣기로 하였고, 이를 위해 정리를 하던 중에 후보키(Candidate Key)에 대한 고민이 필요하게 되었다.

후보키에 대한 내용이 잘 기억나지 않던 제이지는, 정확한 내용을 파악하기 위해 데이터베이스 관련 서적을 확인하여 아래와 같은 내용을 확인하였다.

관계 데이터베이스에서 릴레이션(Relation)의 튜플(Tuple)을 유일하게 식별할 수 있는 속성(Attribute) 또는 속성의 집합 중, 다음 두 성질을 만족하는 것을 후보 키(Candidate Key)라고 한다.
유일성(uniqueness) : 릴레이션에 있는 모든 튜플에 대해 유일하게 식별되어야 한다.
최소성(minimality) : 유일성을 가진 키를 구성하는 속성(Attribute) 중 하나라도 제외하는 경우 유일성이 깨지는 것을 의미한다. 즉, 릴레이션의 모든 튜플을 유일하게 식별하는 데 꼭 필요한 속성들로만 구성되어야 한다.
제이지를 위해, 아래와 같은 학생들의 인적사항이 주어졌을 때, 후보 키의 최대 개수를 구하라.


제한사항
 * relation은 2차원 문자열 배열이다.
 * relation의 컬럼(column)의 길이는 1 이상 8 이하이며, 각각의 컬럼은 릴레이션의 속성을 나타낸다.
 * relation의 로우(row)의 길이는 1 이상 20 이하이며, 각각의 로우는 릴레이션의 튜플을 나타낸다.
 * relation의 모든 문자열의 길이는 1 이상 8 이하이며, 알파벳 소문자와 숫자로만 이루어져 있다.
 * relation의 모든 튜플은 유일하게 식별 가능하다.(즉, 중복되는 튜플은 없다.)

My solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;
    import java.util.stream.*;

    class Solution {
        public int solution(String[][] relation) {
            int res = 0;
            List<Set<Integer>> found = new ArrayList<>();
            Set<Integer> pool = IntStream.range(0,relation[0].length)
                .boxed().collect(Collectors.toSet());
            for (int size = 1; size<=relation.length; size++) {
                // System.out.println("POOL:" + pool);
                List<List<Integer>> candidates = this.findCombinations(new ArrayList<>(pool), size);
                for (List<Integer> candKey : candidates) {
                    // System.out.println(">"+candKey);
                    if (this.isValidKey(candKey, relation) && this.isMinimalKey(new HashSet<>(candKey), found)) {
                        res++;
                        found.add(new HashSet<>(candKey));
                    }
                }
            }
            if (res==0) return 1;
            return res;
        }
        
        private boolean isMinimalKey(Set<Integer> candKey, List<Set<Integer>> found) {
            for (Set<Integer> key : found) {
                if (candKey.containsAll(key)) {
                    return false;
                }
            }
            return true;
        }
        
        private boolean isValidKey(List<Integer> attributeIdxs, String[][] relation) {
            HashSet<String> seen = new HashSet<>();
            for (String[] stringArr : relation) {
                StringBuilder cand = new StringBuilder();
                for (int idx : attributeIdxs) {
                    cand.append(stringArr[idx]);
                }
                String candStr = cand.toString();
                if (seen.contains(candStr)) {
                    return false;
                }
                seen.add(candStr);
            }
            return true;
        }
        
        private List<List<Integer>> findCombinations(ArrayList<Integer> pool, int size) {
            List<List<Integer>> res = new ArrayList<>();
            this.backtrack(res, new ArrayList<>(), pool, size, 0);
            return res;
        }
        
        private void backtrack(List<List<Integer>> list, List<Integer> tempList, ArrayList<Integer> pool, int remaining, int start) {
            if (remaining<0) return;
            if (remaining==0) {
                list.add(new ArrayList<>(tempList));
            } else {
                for (int i = start; i<pool.size(); i++) {
                    tempList.add(pool.get(i));
                    this.backtrack(list, tempList, pool, remaining-1, i+1);
                    tempList.remove(tempList.size()-1);
                }
            }
        }
    }


My initial (close) solution: 

.. code-block:: Java
    :linenos:
    
    import java.util.*;
    import java.util.stream.*;

    class Solution {
        public int solution(String[][] relation) {
            int res = 0;
            Set<Integer> pool = IntStream.range(0,relation[0].length)
                .boxed().collect(Collectors.toSet());
            for (int size = 1; size<=relation.length; size++) {
                System.out.println("POOL:" + pool);
                List<List<Integer>> candidates = this.findCombinations(new ArrayList<>(pool), size);
                for (List<Integer> candKey : candidates) {
                    System.out.println(">"+candKey);
                    if (this.isValidKey(candKey, relation)) {
                        res++;
                        for (int attributeIdx : candKey) {
                            if (pool.contains(attributeIdx)) {
                                pool.remove(attributeIdx);
                            }
                        }
                    }
                }
            }
            if (res==0) return 1;
            return res;
        }
        
        // System.out.println("result is : " + this.isValidKey(new ArrayList<>(Arrays.asList(1,2)), relation));
        
        private boolean isValidKey(List<Integer> attributeIdxs, String[][] relation) {
            HashSet<String> seen = new HashSet<>();
            for (String[] stringArr : relation) {
                StringBuilder cand = new StringBuilder();
                for (int idx : attributeIdxs) {
                    cand.append(stringArr[idx]);
                }
                String candStr = cand.toString();
                if (seen.contains(candStr)) {
                    return false;
                }
                seen.add(candStr);
            }
            return true;
        }
        
        private List<List<Integer>> findCombinations(ArrayList<Integer> pool, int size) {
            List<List<Integer>> res = new ArrayList<>();
            this.backtrack(res, new ArrayList<>(), pool, size, 0);
            return res;
        }
        
        private void backtrack(List<List<Integer>> list, List<Integer> tempList, ArrayList<Integer> pool, int remaining, int start) {
            if (remaining<0) return;
            if (remaining==0) {
                list.add(new ArrayList<>(tempList));
            } else {
                for (int i = start; i<pool.size(); i++) {
                    tempList.add(pool.get(i));
                    this.backtrack(list, tempList, pool, remaining-1, i+1);
                    tempList.remove(tempList.size()-1);
                }
            }
        }
    }

Remarks and Complexity Analysis: 
 * I made a critical error -- an inconsiderate assumption -- which made me miss the full mark on this question. I assumed that once an attribute has been used in a key once, it can no longer appear in future keys. However, this is an incorrect assumption as the following is possible: [2,3], [3,4], [2,4,5].
 * Dismissing these possibilities without thoughtful consideration led to my confusion and struggle
 * I also took much longer than I should have for this question... How could I have increased my speed (other than regular practice, that is)?
 * Using the backtracking standard structure was helpful in implementing the combinations helper function.
 * **Time Complexity**: ``>O(m*n^2)`` (perhaps ``>O(m*(n!)^2)``) where ``n=relation[0].length()``. 
 * **Space Complexity**: ``O(n^2)``

Question 107: Random TestDome Question
------------------------------------------------

My solution: 

.. code-block:: Java
    :linenos:
    
    public class UserInput {
    
        public static class TextInput {
            
            private StringBuilder inputBuffer = new StringBuilder();
            
            protected StringBuilder getInputBuffer() {
                return inputBuffer;
            }
            
            public String getValue() {
                return inputBuffer.toString();
            }
            
            public void add(char c) {
                inputBuffer.append(c);
            }
        }

        public static class NumericInput extends TextInput {
                
            @Override
            public void add(char c) {
                if (Character.isDigit(c)) {
                    getInputBuffer().append(c);
                }
            }
        }

        public static void main(String[] args) {
            TextInput input = new NumericInput();
            input.add('1');
            input.add('a');
            input.add('0');
            System.out.println(input.getValue());
        }
    }

Remarks and Complexity Analysis: 
 * Good practice of OOP!

Day 58 [19 May]
================
Question 108: 땅따먹기
------------------------------------------------

땅따먹기 게임을 하려고 합니다. 땅따먹기 게임의 땅(land)은 총 N행 4열로 이루어져 있고, 모든 칸에는 점수가 쓰여 있습니다. 1행부터 땅을 밟으며 한 행씩 내려올 때, 각 행의 4칸 중 한 칸만 밟으면서 내려와야 합니다. 단, 땅따먹기 게임에는 한 행씩 내려올 때, 같은 열을 연속해서 밟을 수 없는 특수 규칙이 있습니다.

제한사항
 * 행의 개수 N : 100,000 이하의 자연수
 * 열의 개수는 4개이고, 땅(land)은 2차원 배열로 주어집니다.
 * 점수 : 100 이하의 자연수

My solution: 

.. code-block:: Java
    :linenos:
    
    import java.util.Arrays;

    class Solution {
        int solution(int[][] land) {
            for (int r = 1; r < land.length; r++) {
                land[r][0] += Math.max(Math.max(land[r-1][1], land[r-1][2]), land[r-1][3]);
                land[r][1] += Math.max(Math.max(land[r-1][0], land[r-1][2]), land[r-1][3]);
                land[r][2] += Math.max(Math.max(land[r-1][0], land[r-1][1]), land[r-1][3]);
                land[r][3] += Math.max(Math.max(land[r-1][0], land[r-1][1]), land[r-1][2]);
            }
            return Arrays.stream(land[land.length-1]).max().getAsInt();
        }
    }

Remarks and Complexity Analysis: 
 * I had the right approach initially (looking up, rather than looking down) but lacked the ability to find pattern and divide the question into smaller, digestable bits. As a result it took me way longer than it should have
 * Next time, when I am stuck I should consider smaller test cases and solve them by hand with various methods and gradually grow the test cases such that the pattern can evolve
 * **Time Complexity**: ``O(n)`` where ``n = land.length`` as ``m=4`` is fixed -- one iteration down the matrix
 * **Space Complexity**: ``O(1)`` - we use the provided array input as our main data structure

Question 109: 카카오 프렌즈 컬러링북
------------------------------------------------

출판사의 편집자인 어피치는 네오에게 컬러링북에 들어갈 원화를 그려달라고 부탁하여 여러 장의 그림을 받았다. 여러 장의 그림을 난이도 순으로 컬러링북에 넣고 싶었던 어피치는 영역이 많으면 색칠하기가 까다로워 어려워진다는 사실을 발견하고 그림의 난이도를 영역의 수로 정의하였다. (영역이란 상하좌우로 연결된 같은 색상의 공간을 의미한다.)
그림에 몇 개의 영역이 있는지와 가장 큰 영역의 넓이는 얼마인지 계산하는 프로그램을 작성해보자.

입력 형식
 * 입력은 그림의 크기를 나타내는 m과 n, 그리고 그림을 나타내는 m × n 크기의 2차원 배열 picture로 주어진다. 제한조건은 아래와 같다.
 * 1 <= m, n <= 100
 * picture의 원소는 0 이상 2^31 - 1 이하의 임의의 값이다.
 * picture의 원소 중 값이 0인 경우는 색칠하지 않는 영역을 뜻한다.

출력 형식

리턴 타입은 원소가 두 개인 정수 배열이다. 그림에 몇 개의 영역이 있는지와 가장 큰 영역은 몇 칸으로 이루어져 있는지를 리턴한다.

My solution: 

.. code-block:: Java
    :linenos:
    
    class Solution {
        
        private int[][] visited;
        
        public int[] solution(int m, int n, int[][] picture) {
            visited = new int[m][n];
            
            int numberOfArea = 0;
            int maxSizeOfOneArea = 0;

            for (int y = 0; y < m; y++) {
                for (int x = 0; x < n; x++) {
                    if (visited[y][x]==0 && picture[y][x]!=0) {
                        numberOfArea++;
                        int size = dfs(y, x, picture);
                        maxSizeOfOneArea = Math.max(maxSizeOfOneArea, size);
                    }
                }
            }
            
            int[] answer = new int[2];
            answer[0] = numberOfArea;
            answer[1] = maxSizeOfOneArea;
            return answer;
        }
        
        private int dfs(int y, int x, int[][] picture) {
            int size = 1; 
            //color own square
            visited[y][x] = 1;
            //if up exists
            if (y-1>=0 && visited[y-1][x]==0 && picture[y][x]==picture[y-1][x]) {
                size += dfs(y-1, x, picture);
            }
            if (y+1<picture.length && visited[y+1][x]==0 && picture[y][x]==picture[y+1][x]) {
                size += dfs(y+1, x, picture);
            }
            if (x-1>=0 && visited[y][x-1]==0 && picture[y][x]==picture[y][x-1]) {
                size += dfs(y, x-1, picture);
            }
            if (x+1<picture[0].length && visited[y][x+1]==0 && picture[y][x]==picture[y][x+1]) {
                size += dfs(y, x+1, picture);
            }
            return size;  
        }
    }

Remarks and Complexity Analysis: 
 * Solved this question faster than expected (which made me very glad!)
 * Standard dfs with visited matrix and iterating throw all cells sequentially. Pretty simple question compared to leetcode dfs problems. 
 * **Time Complexity**: ``O(m*n)``
 * **Space Complexity**: ``O(m*n)`` for visited table

Day 59 [24 May]
================
Question 110: 124 나라의 숫자
------------------------------------------------
124 나라가 있습니다. 124 나라에서는 10진법이 아닌 다음과 같은 자신들만의 규칙으로 수를 표현합니다.

124 나라에는 자연수만 존재합니다.
124 나라에는 모든 수를 표현할 때 1, 2, 4만 사용합니다.
예를 들어서 124 나라에서 사용하는 숫자는 다음과 같이 변환됩니다.

.. list-table:: 
    :header-rows: 1

    * - 10진법	
      - 124 나라	
      - 10진법	
      - 124 나라
    * - 1	
      - 1	
      - 6	
      - 14
    * - 2	
      - 2	
      - 7	
      - 21
    * - 3	
      - 4	
      - 8	
      - 22
    * - 4	
      - 11	
      - 9	
      - 24
    * - 5	
      - 12	
      - 10	
      - 41

자연수 n이 매개변수로 주어질 때, n을 124 나라에서 사용하는 숫자로 바꾼 값을 return 하도록 solution 함수를 완성해 주세요.
    

My solution: 

.. code-block:: Java
    :linenos:

    import java.util.HashMap;

    class Solution {
        public String solution(int n) {
            int curr = n;
            StringBuilder res = new StringBuilder();
            HashMap<Integer, Character> remainderToCode = new HashMap<>();
            remainderToCode.put(0,'1');
            remainderToCode.put(1,'2');
            remainderToCode.put(2,'4');
            
            while (curr>0) {
                res.append(remainderToCode.get((curr-1)%3));
                curr=(curr-1)/3;
            }
            
            return res.reverse().toString();
        }
    }

Remarks and Complexity Analysis: 
 * What a clean, satisfying problem-solving experience! I used pen and paper to quickly notice that it is similar to how we convert
   decimal to binary by repeated division where the unit is 3 (because there are three possibilities for the value in each place)
 * So this is actually simply base-3 representation of numbers
 * I also like how I used the hashtable to simplify my code!
 * **Time Complexity**: ``O(log_3 n)``
 * **Space Complexity**: ``O(1)`` 

    
Question 111: RoutePlanner
------------------------------------------------
As a part of the route planner, the routeExists method is used as a quick filter if the destination is reachable, before using more computationally intensive procedures for finding the optimal route.

The roads on the map are rasterized and produce a matrix of boolean values - true if the road is present or false if it is not. The roads in the matrix are connected only if the road is immediately left, right, below or above it.

Finish the routeExists method so that it returns true if the destination is reachable or false if it is not. The fromRow and fromColumn parameters are the starting row and column in the mapMatrix. The toRow and toColumn are the destination row and column in the mapMatrix. The mapMatrix parameter is the above mentioned matrix produced from the map.

My solution: 

.. code-block:: Java
    :linenos:

    import java.util.*;

    public class RoutePlanner {

        static boolean[][] visited;

        public static boolean routeExists(int fromRow, int fromColumn, int toRow, int toColumn,
                                        boolean[][] mapMatrix) {
            if (mapMatrix.length<1) return true;
            visited = new boolean[mapMatrix.length][mapMatrix[0].length];
            return routeHelper(fromRow, fromColumn, toRow, toColumn, mapMatrix);
        }

        public static boolean routeHelper(int fromRow, int fromColumn, int toRow, int toColumn,
                                        boolean[][] mapMatrix) {
            visited[fromRow][fromColumn] = true;
            if (fromRow==toRow && fromColumn==toColumn) return true;

            boolean res = false;

            if (fromRow>0 && mapMatrix[fromRow-1][fromColumn] && !visited[fromRow-1][fromColumn]) {
                res = res || routeHelper(fromRow-1, fromColumn, toRow, toColumn, mapMatrix);
            }
            if (!res && fromRow<mapMatrix.length-1 && mapMatrix[fromRow+1][fromColumn] && !visited[fromRow+1][fromColumn]) {
                res = res || routeHelper(fromRow+1, fromColumn, toRow, toColumn, mapMatrix);
            }
            if (!res && fromColumn>0 && mapMatrix[fromRow][fromColumn-1] && !visited[fromRow][fromColumn-1]) {
                res = res || routeHelper(fromRow, fromColumn-1, toRow, toColumn, mapMatrix);
            }
            if (!res && fromColumn<mapMatrix[0].length-1 && mapMatrix[fromRow][fromColumn+1] && !visited[fromRow][fromColumn+1]) {
                res = res || routeHelper(fromRow, fromColumn+1, toRow, toColumn, mapMatrix);
            }

            return res;
        }
        
        public static void main(String[] args) {
            boolean[][] mapMatrix = {
                    {true,  false, false},
                    {true,  true,  false},
                    {false, true,  true}
            };

            System.out.println(routeExists(0, 0, 2, 2, mapMatrix));
        }
    }

Remarks and Complexity Analysis: 
 * Pretty easy question! Classic dfs.
 * **Time Complexity**: ``O(m*n)``
 * **Space Complexity**: ``O(m*n)`` for visited matrix