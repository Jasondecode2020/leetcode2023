### 1 416. Partition Equal Subset Sum

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total = sum(nums)
        if total % 2: return False
        total //= 2
        dp = [0] * (total + 1)
        for i in range(len(nums)):
            for j in range(total, nums[i] - 1, -1):
                dp[j] = max(dp[j], dp[j - nums[i]] + nums[i])
        return dp[-1] == total
```

### 2 823. Binary Trees With Factors

```python
class Solution:
    def numFactoredBinaryTrees(self, arr: List[int]) -> int:
        dp = defaultdict(int)
        MOD = 10 ** 9 + 7
        for a in arr:
            dp[a] = 1

        n = len(arr)
        arr.sort()
        for i in range(1, n):
            for j in range(i):
                if arr[i] / arr[j] in dp:
                    dp[arr[i]] += dp[arr[i] / arr[j]] * dp[arr[j]]
        return sum(dp.values()) % MOD
```

### 3 70. Climbing Stairs

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        first, second = 1, 2
        for i in range(3, n + 1):
            second, first = second + first, second
        return second if n >= 2 else first
```

### 4 746. Min Cost Climbing Stairs

- cost[i] means the min cost in ith step

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        for i in range(2, len(cost)):
            cost[i] = min(cost[i - 1], cost[i - 2]) + cost[i]
        return min(cost[-2:])
```

### 5 63. Unique Paths II

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # # method 1: dp
        '''
        ROWS, COLS = m, n
        dp = [[1] * COLS for i in range(ROWS)]
        for i in range(1, ROWS):
            for j in range(1, COLS):
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        return dp[-1][-1]
        '''
        
        # method 2: dfs
        '''
        @lru_cache(None)
        def dfs(row, col):
            if row == m or col == n:
                return 0
            if row == m - 1 and col == n - 1:
                return 1
            return dfs(row + 1, col) + dfs(row, col + 1)
        return dfs(0, 0)
        '''

        ### method 3: math
        return factorial(m + n - 2) // (factorial(n - 1) * factorial(m - 1))
```

### 6 63. Unique Paths II

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        # method 1: dfs + memo
        '''
        m, n = len(obstacleGrid), len(obstacleGrid[0])
        @lru_cache(None)
        def dfs(row, col):
            if row == m or col == n or obstacleGrid[row][col]:
                return 0
            if row == m - 1 and col == n - 1:
                if obstacleGrid[row][col]:
                    return 0
                return 1
            return dfs(row + 1, col) + dfs(row, col + 1)
        return dfs(0, 0)
        '''
        
        # method 2: dp
        ROWS, COLS = len(obstacleGrid), len(obstacleGrid[0])
        dp = [[1] * COLS for i in range(ROWS)]
        if obstacleGrid[0][0]: return 0
        for j in range(1, COLS):
            dp[0][j] = 0 if obstacleGrid[0][j] else dp[0][j - 1]
        for i in range(1, ROWS):
            dp[i][0] = 0 if obstacleGrid[i][0] else dp[i - 1][0]
        for i in range(1, ROWS):
            for j in range(1, COLS):
                if obstacleGrid[i][j]:
                    dp[i][j] = 0
                else:
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        return dp[-1][-1]
```

### 7 343. Integer Break

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        @lru_cache(None)
        def dfs(num):
            if num == 1:
                return 1
            res = 0 if num == n else num
            for i in range(1, num):
                res = max(res, dfs(i) * dfs(num - i))
            return res
        return dfs(n)
```

### 8 96. Unique Binary Search Trees

```python
class Solution:
    def numTrees(self, n: int) -> int:
        @lru_cache(None)
        def dfs(num):
            if num <= 1:
                return 1
            res = 0
            for i in range(num):
                res += dfs(i) * dfs(num - i - 1)
            return res
        return dfs(n)
```