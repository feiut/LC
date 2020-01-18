## Description
394. Coins in a Line

There are n coins in a line. Two players take turns to take one or two coins from right side until there are no more coins left. The player who take the last coin wins.

Could you please decide the first player will win or lose?

If the first player wins, return true, otherwise return false.

Example
Example 1:

Input: 1  
Output: true  
Example 2:

Input: 4  
Output: true  
Explanation:  
The first player takes 1 coin at first. Then there are 3 coins left.  
Whether the second player takes 1 coin or two, then the first player can take all coin(s) left.  
Challenge  
O(n) time and O(1) memory

## Solution

博弈类的题目关键在找必胜/必败情况。

此类题要从 0 开始分析，而非一般 dp 问题从最后一步入手。

本题转移方式：上一步(对手操作造成的局面)如果全部是必胜情况，那下一步必败，如果前一步(对手操作)有失败的情况，那下一步必胜。因为可以取 1 或 2 颗，所以更新 dp[i] 时 ，看上一步对手所有的操作，即为 dp[i - 1] 和 dp[i - 2]。 

## Code
Time: O(n), Space:O(n)
    class Solution:
        """
        @param n: An integer
        @return: A boolean which equals to true if the first player will win
        """
        def firstWillWin(self, n):
            # write your code here
            if n == 0:
                return False
            if n < 3:
                return True
            dp = [False for _ in range(n)]
            dp[0] = True
            dp[1] = True
            for i in range(3, n):
                if dp[i - 1] and dp[i - 2]:
                    dp[i] = False
                    continue
                if dp[i - 1] or dp[i - 2]:
                    dp[i] = True
                    continue
                dp[i] = True
            return dp[n - 1]
