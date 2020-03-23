## 464. Sort Integers II

Given an integer array, sort it in ascending order in place. Use quick sort, merge sort, heap sort or any O(nlogn) algorithm.

Example. 

Example1:

Input: [3, 2, 1, 4, 5],   
Output: [1, 2, 3, 4, 5].

Example2:

Input: [2, 3, 1],   
Output: [1, 2, 3].


## Solution
Two pointers 实现快排。  

注意：
- 取当前数组中间 index 的值作为基准比较两边大小，把 左边大于基准的值 和 右边小于基准的值 对调（如果只有一边有就将其和基准对调）
- 循环条件中 left 和 right 的关系要用 <= 号，保证循环结束时有 left > right，从而保证两个字问题中没有重叠部分

    class Solution:
        # @param {int[]} A an integer array
        # @return nothing
        def sortIntegers2(self, A):
            # Write your code here
            self.quickSort(A, 0, len(A) - 1)

        def quickSort(self, A, start, end):
            if start >= end:
                return

            left, right = start, end
            # key point 1: pivot is the value, not the index. Use // in stead of /.
            pivot = A[(start + end) // 2];

            # key point 2: every time you compare left & right, it should be 
            # left <= right not left < right
            while left <= right:
                while left <= right and A[left] < pivot:
                    left += 1

                while left <= right and A[right] > pivot:
                    right -= 1

                if left <= right:
                    A[left], A[right] = A[right], A[left]

                    left += 1
                    right -= 1

            self.quickSort(A, start, right)
            self.quickSort(A, left, end)
