## Description
563. Backpack V

Given n items with size nums[i] which an integer array and all positive numbers. An integer target denotes the size of a backpack. Find the number of possible fill the backpack.

Each item may only be used once

Example   

    Given candidate items [1,2,3,3,7] and target 7,

    A solution set is: 
    [7]
    [1, 3, 3]
    return 2
## Solution
Time: O(n * target), Space: O(n * target) / O(target)

确定状态：需要知道 n 个物品有多少种方式拼出重量 w (w = 0, 1, ..., target)。

dp[i][w] = 用前 i 个物品有多少种方式拼出重量 w。 

最后一步：第 n 个物品(重量 nums[n])能否入包。  
    情况 1. 前 n - 1 个物品可以拼出 w。  
    情况 2. 前 n - 1 个物品拼出 w - nums[n]，加上最后一个重量 nums[n] 刚好到 w。 
总情况数 = 情况 1 + 情况 2

转移方程：dp[i][w] = dp[i - 1][w] + dp[i - 1][w - nums[i - 1]]

注意：初始条件 dp[0][0] = 1：0 个物品有一种方式拼出重量 0，因为转移方程第二项 w = nums[i - 1] 时有一种方法拼出来，即自己拼出自己，为保持形式统一需要把该初始值设为 1。


## Code
### S1. Space: O(n * target)
    class Solution:
        """
        @param nums: an integer array and all positive numbers
        @param target: An integer
        @return: An integer
        """
        # Time: O(n * target), Space: O(n * target)
        def backPackV(self, nums, target):
            # write your code here
            # i 个物品凑成 j 重量有 dp[i][j] 种方法，等于 i - 1 个物品凑成重量 j 的方法 + i - 1 个物品凑成 j - nums[i] 的方法。
            # dp[0][j] = 0
            if not nums or len(nums) == 0:
                return 0
            n = len(nums)
            dp = [[0 for _ in range(target + 1)] for _ in range(n + 1)]
            # dp[0][0] = 1
            for i in range(1, n + 1):
                for j in range(target + 1):
        此处可以通过定义 dp[0][0] = 1 来避免判断 j 是否与 nums[i - 1][j] 相等
                    if j == nums[i - 1]:
                        dp[i][j] += 1
                    dp[i][j] += dp[i - 1][j]
                    if j - nums[i - 1] >= 0:
                        dp[i][j] += dp[i - 1][j - nums[i - 1]]
            return dp[n][target]
### S2. Space: O(target)
        class Solution:
            """
            @param nums: an integer array and all positive numbers
            @param target: An integer
            @return: An integer
            """
            def backPackV(self, nums, target):
                # write your code here
                # 空间优化：更新第 i 层的 dp[i][j] 时只与第 i - 1 层的 dp[i - 1][j] 和 dp[i - 1][j - nums[i - 1]] 有关，可以只用一个数组进行更新，但是由于更新后面的数据需要前面的数据，因此要从后向前更新。
                if not nums or len(nums) == 0:
                    return 0
                n = len(nums)
                dp = [0 for _ in range(target + 1)]
                dp[0] = 1  # 因为后续要计算 j - nums[i - 1]。当 j = nums[i - 1] 时，即只用 nums[i - 1] 自己就能拼出重量 j，应该是 1 种情况，而转移方程为 dp[i][j] += dp[i - 1][0]，即上一层拼出重量 0 的数值，为了保持格式一致，可以让 dp[0][0] = 1
                for i in range(1, n + 1):
                    for j in reversed(range(target + 1)):
                        if j - nums[i - 1] >= 0:
                            dp[j] += dp[j - nums[i - 1]]
                    # print(dp[i % 2])
                print(dp)
                return dp[target]
