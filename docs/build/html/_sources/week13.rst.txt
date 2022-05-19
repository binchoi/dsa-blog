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

