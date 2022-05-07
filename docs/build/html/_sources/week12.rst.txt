************************
Week 12 [02-09 May 2022]
************************
Day 54 [02 May]
================
Question 99: 소수 찾기
------------------------------------------------
한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.
각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

제한사항
 * numbers는 길이 1 이상 7 이하인 문자열입니다.
 * numbers는 0~9까지 숫자만으로 이루어져 있습니다.
 * "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

My solution: 

.. code-block:: Java
    :linenos:
    
    // simple and clear solution
    import java.util.HashSet;
    class Solution {
        public int solution(String numbers) {
            HashSet<Integer> set = new HashSet<>();
            permutation("", numbers, set);
            int res = 0;
            for (int s : set){
                if (isPrime(s)) res++;
            }        
            return res;
        }

        public boolean isPrime(int n){
            if(n<=1) return false;
            for(int i=2; i<n; i++){
                if(n%i==0) return false;
            }
            return true;
        }

        public void permutation(String prefix, String str, HashSet<Integer> set) {
            int n = str.length();
            if(!prefix.equals("")) set.add(Integer.valueOf(prefix));
            for (int i = 0; i < n; i++)
            permutation(prefix + str.charAt(i), str.substring(0, i) + str.substring(i+1, n), set);
        }
    }

    // more time efficient (string concat removed)
    import java.util.stream.*;
    import java.util.Arrays;
    import java.util.ArrayList;

    class Solution {
        public int solution(String numbers) {
            int[] intArr = IntStream.range(0,numbers.length()).map(e -> (numbers.charAt(e)-'0')).toArray();        
            ArrayList<Integer> perm = new ArrayList<>();
            for (int i = 1; i <= intArr.length; i++) {
                this.backtrack(perm, new ArrayList<>(), intArr, new boolean[intArr.length], i);
            }
            
            int res = 0;
            for (int p : perm) {
                if (this.isPrime(p)) res++;
            }
            
            return res;
        }
        
        private void backtrack(ArrayList<Integer> perm,  ArrayList<Integer> tempList, int[] intArr, boolean[] used, int desiredLength) {
            if (tempList.size()==desiredLength) {
                int cand = this.convertIntList(tempList);
                if (!perm.contains(cand)) {
                    perm.add(cand);
                }
            } else {
                for (int i=0; i<intArr.length; i++) {
                    if (used[i]) continue;
                    tempList.add(intArr[i]);
                    used[i]=true;
                    this.backtrack(perm, tempList, intArr, used, desiredLength);
                    tempList.remove(tempList.size()-1);
                    used[i]=false;
                }
            }
        }
        
        private boolean isPrime(int num) {
            if (num<=1) return false;
            
            for (int i = 2; i<num; i++) {
                if (num%i==0) return false;
            }
            return true;
        }
        
        private int convertIntList(ArrayList<Integer> intList) {
            int cand = 0;
            for (Integer num : intList) {
                cand*=10;
                cand+=num;
            }
            return cand;
        }
    }

Remarks and Complexity Analysis: 
 * Not too difficult but was a good review of backtracking (for brute force method)
 * String concatenation in Java can be costly
 * I wonder if interviewers would prefer a more efficient method of computing primes.
 * **Time Complexity**: ``>O(n^2)`` (perhaps ``>O((n!)^2)``) where ``n=numbers.length()``. 
 * **Space Complexity**: ``O(n^2)``

Question 100: Lowest Common Ancestor of a Binary Search Tree
------------------------------------------------------------------
Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

My solution: 

.. code-block:: Java
    :linenos:

    class Solution {
        public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
            while ((root.val<p.val && root.val<q.val) || (root.val>p.val && root.val>q.val)) {
                root= root.val<p.val ? root.right : root.left;
            }
            return root;  
        }
    }

    // or 
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while ((root.val - p.val) * (root.val - q.val) > 0)
            root = p.val < root.val ? root.left : root.right;
        return root;
    }

Remarks and Complexity Analysis: 
 * Key was to leverage the characteristic of BST (in-order traversal is ascending order)
 * **Time Complexity**: ``O(log n)`` where ``n=num_of_tree_nodes_in_tree``. 
 * **Space Complexity**: ``O(1)``

Day 55 [03 May]
================
Question 101: 카펫
------------------------------------------------
Leo는 카펫을 사러 갔다가 아래 그림과 같이 중앙에는 노란색으로 칠해져 있고 테두리 1줄은 갈색으로 칠해져 있는 격자 모양 카펫을 봤습니다.

Leo는 집으로 돌아와서 아까 본 카펫의 노란색과 갈색으로 색칠된 격자의 개수는 기억했지만, 전체 카펫의 크기는 기억하지 못했습니다.

Leo가 본 카펫에서 갈색 격자의 수 brown, 노란색 격자의 수 yellow가 매개변수로 주어질 때 카펫의 가로, 세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.

제한사항
 * 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
 * 노란색 격자의 수 yellow는 1 이상 2,000,000 이하인 자연수입니다.
 * 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.

My solution: 

.. code-block:: Java
    :linenos:
    
    class Solution {
        public int[] solution(int brown, int yellow) {
            int area = brown+yellow;
            int s2; 
            for (int s1 = 2; s1 <= (int) Math.sqrt(area); s1++) {
                if (area%s1!=0) continue;
                s2 = area/s1;
                if (2*s1+2*s2-4==brown) {
                    return new int[] {s2,s1};
                }
            }
            return new int[] {};
        }
    }

Remarks and Complexity Analysis: 
 * Simple brute force question. 
 * Really nice when simple math can help solve coding questions
 * **Time Complexity**: ``O(sqrt(b+y))`` which is the maximum number of iterations
 * **Space Complexity**: ``O(1)``

Constant time solution using quadratic formula: 

.. code-block:: Java
    :linenos:

    public int[] solution(int brown, int yellow) {
        int s1PlusS2 = (brown+4)/2;
        int area = brown+yellow;
        int[] answer = {(int)(s1PlusS2+Math.sqrt(s1PlusS2*s1PlusS2-4*area))/2,(int)(s1PlusS2-Math.sqrt(s1PlusS2*s1PlusS2-4*area))/2};
        return answer;
    }

Question 102: 타겟 넘버
------------------------------------------------
n개의 음이 아닌 정수들이 있습니다. 이 정수들을 순서를 바꾸지 않고 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

.. code-block:: Java
    :linenos: 
 
    -1+1+1+1+1 = 3
    +1-1+1+1+1 = 3
    +1+1-1+1+1 = 3
    +1+1+1-1+1 = 3
    +1+1+1+1-1 = 3

사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

제한사항
 * 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
 * 각 숫자는 1 이상 50 이하인 자연수입니다.
 * 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

My solution: 

.. code-block:: Java
    :linenos:

    //recursive [much easier to read - at cost of overhead and risk of overflow]
    public int solution(int[] numbers, int target) {
        return this.helper(numbers, target, 0, 0);
    }
    
    private int helper(int[] numbers, int target, int sumSoFar, int i) {
        if (i>=numbers.length) {
            return (target==sumSoFar) ? 1 : 0; 
        } else {
            return this.helper(numbers, target, sumSoFar+numbers[i], i+1) + this.helper(numbers, target, sumSoFar-numbers[i], i+1);
        }
    }

    //iterative dfs
    public int solution(int[] numbers, int target) {
        if (numbers.length==0) return 0;
        
        Stack<Integer> stk = new Stack<>();
        int i = 0;
        int sum = 0;
        int res = 0;
        
        for (int n : numbers) {
            stk.add(n);
            i++;
            sum+=n;
        }
        
        if (sum<target) {
            return 0;
        }
        
        while (!stk.isEmpty()) {
            if (i<numbers.length) {
                int num = numbers[i++];
                sum+=num;
                stk.add(num);
            } else {
                if (sum==target) res++;
                // remove all the negative nums added
                while (!stk.isEmpty() && stk.peek()<0) {
                    i--;
                    sum-=stk.pop();
                } 
                if (!stk.isEmpty()) {
                    int posN = stk.pop();
                    sum-=(2*posN);
                    stk.add(-posN);
                }
            }

        }
        return res;
    }

    //initial disorganized
    import java.util.Stack;

    class Solution {
        public int solution(int[] numbers, int target) {
            if (numbers.length==0) return 0;
            Stack<Integer> stk = new Stack<>();
            
            int i = 0;
            int sum = 0;
            int res = 0;
            
            while (true) {
                if (i<numbers.length) {
                    int num = numbers[i++];
                    sum+=num;
                    stk.add(num);
                } else {
                    if (sum==target) {
                        res++;
                    } 
                    while (i>0 && stk.peek()<0) {
                        i--;
                        sum-=stk.pop();
                    }
                    if (i>0) {
                        int posN = stk.pop();
                        sum-=(2*posN);
                        stk.add(-posN);
                    } else {
                        return res;
                    }
                }
            }
        }
    }

Remarks and Complexity Analysis: 
 * Recursive solution is much more intuitive and easier to understand. It is always a trade-off between space vs. time and simplicity vs. minimizing overhead
 * I should not overlook the beauty of recursion.
 * **Time Complexity**: ``O(2^n)`` where ``n=numbers.length``. 
 * **Space Complexity**: ``O(1)``

Question 103: 네트워크
------------------------------------------------
네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

제한사항
 * 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
 * 각 컴퓨터는 0부터 n-1인 정수로 표현합니다.
 * i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
 * computer[i][i]는 항상 1입니다.

My solution: 

.. code-block:: Java
    :linenos: 

    class Solution {
        public int solution(int n, int[][] computers) {
            int res = 0;
            boolean[] visited = new boolean[n+1];
            for (int i = 1; i<=n; i++) {
                if (!visited[i]) {
                    res++;
                    this.dfs(computers, visited, i);
                }
            }
            return res;
        }
        
        private void dfs(int[][] computers, boolean[] visited, int compNum) {
            visited[compNum]=true;
            for (int i = 0; i<computers[0].length; i++) {
                if (computers[compNum-1][i]==1 && !visited[i+1]) { // connected & unvisited
                    this.dfs(computers, visited, i+1);
                }
            }
        }
    }

Day 56 [08 May]
================
Question 104: 단어 변환
------------------------------------------------
두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.

예를 들어 begin이 "hit", target가 "cog", words가 ["hot","dot","dog","lot","log","cog"]라면 "hit" -> "hot" -> "dot" -> "dog" -> "cog"와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

제한사항
 * 각 단어는 알파벳 소문자로만 이루어져 있습니다.
 * 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
 * words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
 * begin과 target은 같지 않습니다.
 * 변환할 수 없는 경우에는 0를 return 합니다.

My solution: 

.. code-block:: Java
    :linenos: 

    import java.util.*;
    import java.util.stream.*;

    class Solution {

        public int solution(String begin, String target, String[] words) {
            int res = this.helper(begin, target, new HashSet<String>(), words);
            if (res>=words.length+1) return 0;
            return res;
        }
        
        private int helper(String curr, String target, HashSet<String> seen, String[] words) {
            if (curr.equals(target)) {
                return seen.size();           
            }
            String[] options = this.getOptions(curr, words, seen);
            int best = words.length+1;
            for (String o : options) {
                seen.add(o);
                best = Math.min(best, this.helper(o, target, new HashSet<String>(seen), words));
                seen.remove(o);
            }
            return best;
        }
        
        private String[] getOptions(String curr, String[] words, HashSet<String> seen) {
            return Arrays.stream(words).filter(w -> this.diffByOneChar(curr,w) && !seen.contains(w)).toArray(String[]::new); 
        }
        
        private boolean diffByOneChar(String w1, String w2) {
            int diff = 0;
            for (int i=0; i<w1.length(); i++) {
                if (w1.charAt(i)!=w2.charAt(i)) diff++;
            }
            return (w1.length()==w2.length() && diff==1);
        }
    }

Remarks and Complexity Analysis: 
 * I got stuck for a long time because of a small bug in the helper method diffByOneChar... I wonder what is the best way to prevent
   this kind of mistake in real coding tests. How can I test the helper functions during interviews?
 * **Time Complexity**: ``O(n!)`` where ``n=words.length``. 
 * **Space Complexity**: ``O(n)``


Question 105: 나머지가 1이 되는 수 찾기
------------------------------------------------
자연수 n이 매개변수로 주어집니다. n을 x로 나눈 나머지가 1이 되도록 하는 가장 작은 자연수 x를 return 하도록 solution 함수를 완성해주세요. 답이 항상 존재함은 증명될 수 있습니다.

My solution: 

.. code-block:: Java
    :linenos: 
    
    class Solution {
        public int solution(int n) {
            for (int i=1; i<n; i++){
                if (n%i==1) return i;
            }
            return n-1;
        }
    }

Remarks and Complexity Analysis: 
 * Level 1 simple question
 * **Time Complexity**: ``O(n)``
 * **Space Complexity**: ``O(1)``