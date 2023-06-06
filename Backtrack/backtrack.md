1219. Path with Maximum Gold

```python
class Solution:
    def getMaximumGold(self, grid: List[List[int]]) -> int:
        def dfs(r, c, cur):
            if r < 0 or r > R - 1 or c < 0 or c > C - 1 or grid[r][c] == 0 or (r, c) in visited:
                return
            self.res = max(self.res, cur + grid[r][c])
            visited.add((r, c))
            for dr, dc in directions:
                dfs(r + dr, c + dc, cur + grid[r][c])
            visited.remove((r, c))

        directions = [(1, 0), (-1,0), (0, 1), (0,-1)]
        R, C, self.res = len(grid), len(grid[0]), 0
        for i in range(R):
            for j in range(C):
                if grid[i][j] != 0:
                    visited = set()
                    dfs(i, j, 0)
        return self.res
```