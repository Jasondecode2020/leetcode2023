### 1 509. Fibonacci Number

```python
class Solution:
    def fib(self, n: int) -> int:
        first, second = 0, 1
        for i in range(2, n + 1):
            second, first = second + first, second
        return second if n >= 1 else 0
```

### 2 1137. N-th Tribonacci Number

```python
class Solution:
    def tribonacci(self, n: int) -> int:
        first, second, third = 0, 1, 1
        for i in range(3, n + 1):
            third, second, first = third + second + first, third, second
        return third if n >= 1 else 0
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

### bottom up

2140. Solving Questions With Brainpower

```python
class Solution:
    def mostPoints(self, questions: List[List[int]]) -> int:
        dp, n = {}, len(questions)
        for i in range(n - 1, -1, -1):
            dp[i] = max(dp.get(i + 1, 0), questions[i][0] + dp.get(i + questions[i][1] + 1, 0))
        return dp[0]
```

1312. Minimum Insertion Steps to Make a String Palindrome

```python
class Solution:
    def minInsertions(self, s: str) -> int:
        @lru_cache(None)
        def dfs(i, j):
            if i >= j: return 0
            if s[i] == s[j]:
                return dfs(i + 1, j - 1)
            return min(dfs(i + 1, j) + 1, dfs(i, j - 1) + 1)
        return dfs(0, len(s) - 1)
```