## Description
92. Backpack

Given n items with size Ai, an integer m denotes the size of a backpack. How full you can fill this backpack?

Example  

    Example 1:  
      Input:  [3,4,8,5], backpack size=10  
      Output:  9  

    Example 2:  
      Input:  [2,3,5,7], backpack size=12  
      Output:  12  
	
Challenge   
O(n x m) time and O(m) memory.

O(n x m) memory is also acceptable if you do not know how to optimize memory.

Notice  
You can not divide any item into small pieces.

## Solution
Time: O(n * m), Space: O(n * m) (空间可用滚动数组优化到 O(m))

对于每个重量 M, 如果可以知道能不能凑成，就可以知道最大的可以凑成的重量。
确定状态：需要知道 n 个物品能否拼出重量 w。最后一步：最后一个物品 A[n - 1] 能否入包。
对于每个重量 w，按元素顺序循环：
1. 前 n - 1 个元素可拼出 w，第 n 个元素也能拼出 w。
2. 如果前 n - 1 个元素可拼出 w - A[n]，则第 n 个元素也能拼出 w。

转移方程：dp[n][m] = dp[n - 1][m] or dp[n - 1][m - A[n - 1]]。
        
## Code

    class Solution:
        """
        @param m: An integer m denotes the size of a backpack
        @param A: Given n items with size A[i]
        @return: The maximum size
        """
        def backPack(self, m, A):
            # write your code here
            if not A: 
                return False
            n = len(A)
            if len(A) == 0 and m == 0:
                return False

            dp = [[False for _ in range(m + 1)] for _ in range(n + 1)]
            dp[0][0] = True
            for i in range(1, n + 1):
                for j in range(m + 1):
                    if j == 0:
                        dp[i][j] = True
                        continue
                    dp[i][j] = dp[i - 1][j]
                    if j - A[i - 1] >= 0:  
                        if dp[i - 1][j - A[i - 1]]:
                            dp[i][j] = True
            for i in reversed(range(m + 1)):
                if dp[n][i]:
                    return i
                
