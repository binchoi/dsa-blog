************************
Week 14 [13-20 Jul 2022]
************************
I took a long hiatus from posting questions. The reason for the break from posting is my job search process. I've done at least 30 algorithm / data structure questions in the past 5-6 weeks in testing situation. And I am happy to report that my practice and work to improve my problem solving abilities have paid off. I think I have on average successfully solved 90% of all questions I came across. After landing ~7 job offers, I made my decision to go to Buzzvil. 

But the journey doesn't stop here. I've experienced the benefit of learning by doing and engaging my creative and problem-solving mindset through these questions. As I am pushed to learn new skills/languages, I am confident that consistent DSA practice will be tremendously beneficial. So here we go again. 

Let's start by learning some Go!

Day 60 [13 Jul]
================
Question 112: Two Sum
------------------------------------------------
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

My solution: 

.. code-block:: Go
    :linenos:

    func twoSum(nums []int, target int) []int {
        myHashMap := map[int]int{}
        for idx, val := range nums {
            if compIdx, ok := myHashMap[val]; ok {
                return []int{idx, compIdx}   
            }
            myHashMap[target-val] = idx
        }
        return []int{0,0}
    }

Remarks and Complexity Analysis: 
 * Classic first leetcode question!
 * Familiarizing myself with using hashmaps and for-loops in Golang
 * **Time Complexity**: ``O(n)`` where ``n=len(nums)``
 * **Space Complexity**: ``O(n)`` where ``n=len(nums)``