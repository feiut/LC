## Description
437. Copy Books

Given n books and the i-th book has pages[i] pages. There are k persons to copy these books.

These books list in a row and each person can claim a continous range of books. For example, one copier can copy the books from i-th to j-th continously, but he can not copy the 1st book, 2nd book and 4th book (without 3rd book).

They start copying books at the same time and they all cost 1 minute to copy 1 page of a book. What's the best strategy to assign books so that the slowest copier can finish at earliest time?

Return the shortest time that the slowest copier spends.

Example
Example 1:

Input: pages = [3, 2, 4], k = 2
Output: 5
Explanation: 
    First person spends 5 minutes to copy book 1 and book 2.
    Second person spends 4 minutes to copy book 3.
Example 2:

Input: pages = [3, 2, 4], k = 3
Output: 4
Explanation: Each person copies one of the books.
Challenge
O(nk) time

Notice
The sum of book pages is less than or equal to 2147483647

## Solution

  时间复杂度：O(K*N^2)

  最后一步：k - 1 个人抄了 0 ～ j 本书，且总时间最小。第 k 人抄 j ～ k - 1 本书，使总时间最少。    
  dp[i][j] 表示前 i 个人抄前 j 本书需要的最少时间。    
  在前 i - 1 人已经抄了 j 本时，第 i 人可以抄 0 ～ k - j 本。  
  转移到 dp[i][j] 的情况：前 i - 1 人抄了前 0 ～ j 本书，dp[i][j] = min(max(dp[i - 1][0 ~ j - 1], pages[j] + ...))。即枚举前 i - 1 个人抄写的书数量 x，每个 x 的抄写总时间是 dp[i - 1][x] 与 A[x] + A[x + 1] + ... + A[j] 两者的最大值。再对所有 x 取最小值。

  转移方程：`dp[i][j] = min(max(dp[i - 1][x], pages[x] +...+ pages[j - 1])`, 其中 x = 0, ..., j。    
  初始值：`dp[0][0] = 0, dp[0][j] = inf, dp[i][0] = 0`  
  
## Code

    class Solution:
        """
        @param pages: an array of integers
        @param k: An integer
        @return: an integer
        """
        def copyBooks(self, pages, k):
            # write your code here
            # 最后一步：k - 1 个人抄了 0 ～ j 本书，且总时间最小。第 k 人抄 j ～ k - 1 本书，使总时间最少。
            # dp[i][j] 表示前 i 个人抄前 j 本书需要的最少时间。
            # 在前 i - 1 人已经抄了 j 本时，第 i 人可以抄 0 ～ k - j 本，
            # 转移到 dp[i][j] 的情况：
            #   前 i - 1 人抄了前 0 ～ j 本书，dp[i][j] = min(max(dp[i - 1][0 ~ j - 1], A[j] + ...))。
            #       即枚举前 i - 1 个人抄写的书数量 x，每个 x 的抄写总时间是 dp[i - 1][x] 与 A[x] + A[x + 1] + ... + A[j] 两者的最大值。再对所有 x 取最小值。

            # 转移方程：dp[i][k] = min(max(dp[i - 1][x], A[x] +...+ A[j])(x: 0 ~ k)
            # 初始值：dp[0][j] = inf, dp[i][0] = 0
            # 时间复杂度：O(K*N^2)
            if not pages or k == 0 or len(pages) < 1:
                return 0
            n = len(pages)

            res = 0
            if n <= k:
                for page in pages:
                    res = max(res, page)
                return res

            # p stores page sum, p[i] is the sum of pages from book 0 to i
            # p = [0 for _ in range(n + 1)]
            # p[0] = 0
            # for i in range(n):
            #     p[i + 1] = p[i] + pages[i]

            dp = [[float('inf') for _ in range(n + 1)] for _ in range(k + 1)]
            dp[0][0] = 0
            # i 人， j 书
            for i in range(1, k + 1):
                for j in range(n + 1):
                    if j == 0:
                        dp[i][j] = 0
                        continue
                    s = 0
                    print(dp[i - 1][j])
                    dp[i][j] = dp[i - 1][j]  
                    for m in reversed(range(j + 1)):   # 此处必须是从第 j 本书开始，因为 sum 要算上当前第 j 本书
                        dp[i][j] = min(max(dp[i - 1][m], s), dp[i][j])
                        if m > 0:
                            s += pages[m - 1]

            return dp[k][n]
