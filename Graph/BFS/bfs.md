1765. Map of Highest Peak

```python
class Solution:
    def highestPeak(self, isWater: List[List[int]]) -> List[List[int]]:
        q, visited = deque(), set()
        ROWS, COLS = len(isWater), len(isWater[0])
        res = [[0] * COLS for i in range(ROWS)]
        # put water points inside res, q, and visited
        for i in range(ROWS):
            for j in range(COLS):
                if isWater[i][j] == 1:
                    res[i][j] = 0
                    q.append((i, j, 0))
                    visited.add((i, j))
        # bfs
        while q:
            for i in range(len(q)):
                r, c, n = q.popleft()
                for dr, dc in [[0, 1], [0, -1], [1, 0], [-1, 0]]:
                    row, col = r + dr, c + dc
                    if 0 <= row < ROWS and 0 <= col < COLS and (row, col) not in visited:
                        visited.add((row, col))
                        q.append((row, col, n + 1))
                        res[row][col] = n + 1
        return res
```