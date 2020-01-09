## Description
### 1582. Minimum Path Sum II

Given an n * m matrix, each point has a weight, from the bottom left of the matrix to the top right (you can go in four directions), let you find a path so that the path through the minimum sum of weights and return the minimum sum.

Example  

Example 1:

Input：[[1,1,1],[1,1,1],[1,1,1]]  
Output：5  
Explanation：(2,0)->(2,1)->(2,2)->(1,2)->(0,2),the minimum sum is 5.

Example 2:  

Input：[[2,3][3,2]]  
Output：8  
Explanation：(1,0)->(1,1)->(0,1),the minimum sum is 8.  

## Solution
DFS + DP

dp[i][j] 数组保存由起点到 [i][j] 点的路径 sum 最小值, 初始值为 float('inf'). 

dfs 遍历时更新 dp 数组, dp[cur_x][cur_y] = dp[pre_x][pre_y] (path_sum_until_now) + matrix[i][j].

当数组越界, 或 dp[i][j] 存的值已经小于将更新的新路径的 sum 值时停止递归.

## Code

    class Solution:
        """
        @param matrix: a matrix
        @return: the minimum height
        """
        def minPathSumII(self, matrix):
            # Write your code here
            if not matrix or len(matrix) == 0 or len(matrix[0]) == 0:
                return 0
            n = len(matrix)
            m = len(matrix[0])
            dp = [[float('inf') for _ in range(m)] for _ in range(n)]
            # dfs
            direction_x = [0, 1, 0, -1]
            direction_y = [1, 0, -1, 0]
            self.dfs(n - 1, 0, direction_x, direction_y, matrix, dp, matrix[n - 1][0])
            return dp[0][m - 1]

        def dfs(self, x, y, direction_x, direction_y, matrix, dp, cursum):
            # 需要传入一个数据 cursum 记录截止到目前的总 sum
            dp[x][y] = cursum
            n = len(matrix)
            m = len(matrix[0])
            for i in range(4):
                next_x = x + direction_x[i]
                next_y = y + direction_y[i]
                # 当越界以及 dp[next_x][next_x] 小于将更新的值时需要跳出递归并 return
                if(next_x < 0 or next_x > n - 1 or next_y < 0 or next_y > m - 1 or dp[next_x][next_y] < dp[x][y] + matrix[next_x][next_y]):
                    continue
                self.dfs(next_x, next_y, direction_x, direction_y, matrix, dp, dp[x][y] + matrix[next_x][next_y])
            return
