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