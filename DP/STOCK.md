## Description
* 买卖一次: 121. Best Time to Buy and Sell Stock  
* 不限买卖次数: 122. Best Time to Buy and Sell Stock II  
* 买卖两次: 123. Best Time to Buy and Sell Stock III  
* 买卖k次:188. Best Time to Buy and Sell Stock IV  
//  * 带有冷却的股票买卖问题:309. Best Time to Buy and Sell Stock with Cooldown  
* 带有交易费用的股票买卖问题:714. Best Time to Buy and Sell Stock with Transaction Fee  

## Solution 

#### 买卖 1 次:   
*    `max - min  `

#### 买卖无限次:     
*    `if prices[i] > prices[i - 1]:  
    ans += prices[i] - prices[i - 1]`  

#### 买卖 k 次:   
*    开两个数组:   
        dp0[i][j]: 第i天，已经买卖过K次, 且手中无股票时的利润  
        dp1[i][j]: 第i天，已经买卖过K次, 且手中有股票时的利润  
*    注意数组大小: n * (K + 1)
*    初始值:  
         第零天 i == 0, 手中只能买入第 0 天的股票:  
         `dp0[0][j] = 0, dp1[0][j] = -price[0]`  
         还没进行完第一次买卖 j == 0, 手中只能买入第 i 天及其以前最低价格的股票:   
         `dp1[i][0] = max(dp1[i - 1][0], -prices[i])`
*    转移方程:  
        dp0: 与第 i - 1 天保持一致 / 在前一天手中有股票的情况下卖出了手中持有的股票:  
        `dp0[i][j] = max(dp0[i - 1][j], dp1[i - 1][j] + prices[i])`   
        dp1: 与第 i - 1 天保持一致 / 在前一天手中没股票的情况下买入新的的股票:   
        `dp1[i][j] = max(dp1[i - 1][j], dp0[i - 1][j - 1] - prices[i])`
    
## Code
    class Solution:
        """
        @param K: An integer
        @param prices: An integer array
        @return: Maximum profit
        """
        def maxProfit(self, K, prices):
            # write your code here
            if not prices or len(prices) < 2 or K < 1:
                return 0
            n = len(prices)
            # 当 k > n / 2 时, 相当于无限多次买卖
            if K > n / 2:
                ans = 0
                for i in range(1, n):
                    if prices[i] > prices[i - 1]:
                        ans += prices[i] - prices[i - 1]
                return ans

            # dp0[i][j]: 第i天时, 已经买卖过j次, 且手里没股票时的利润
            dp0 = [[0 for _ in range(K + 1)] for _ in range(n)]
            # dp0[i][j]: 第i天时, 已经买卖过j次, 且手里有股票时的利润
            dp1 = [[-float('inf') for _ in range(K + 1)] for _ in range(n)]
            for i in range(n):
                for j in range(K + 1):
                    if i == 0:
                        dp1[i][j] = -prices[0]
                        continue
                    if j == 0:
                        dp1[i][j] = max(dp1[i - 1][j], -prices[i])
                        continue
                    dp0[i][j] = max(dp0[i - 1][j], dp1[i - 1][j] + prices[i])
                    dp1[i][j] = max(dp1[i - 1][j], dp0[i - 1][j - 1] - prices[i])
            # print(dp0, dp1)
            return dp0[n - 1][K]

        
## Link
https://zhuanlan.zhihu.com/p/92908822
