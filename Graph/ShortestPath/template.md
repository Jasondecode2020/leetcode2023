### Floyd-Warshall

#### 1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance

```python
# 9 line Floyd-Warshall Template
# init
dist = [[inf] * n for i in range(n)]
for u, v, w in edges:
    dist[u][v] = dist[v][u] = w
for u in range(n):
    dist[u][u] = 0
for k in range(n):
    for i in range(n):
        for j in range(n):
            dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
# add edge
def addEdge(self, edge) -> None:
    u, v, c = edge
    if c < self.dist[u][v]:
        self.dist[u][v] = c
        # Partial Floyd-Warshall for updating involved edges only
        for k in [u, v]:
            for i in range(self.n):
                for j in range(self.n):
                    self.dist[i][j] = min(self.dist[i][j], self.dist[i][k] + self.dist[k][j])
```

1976. Number of Ways to Arrive at Destination

```python
class Solution:
    def countPaths(self, n: int, roads: List[List[int]]) -> int:
        #1. graph
        g, mod = defaultdict(list), 10 ** 9 + 7
        for u, v, t in roads:
            g[u].append((v, t))
            g[v].append((u, t))
            
        #2. times, ways array initializer
        times, ways = [inf] * n, [0] * n
        times[0], ways[0] = 0, 1
        
        #3. dijkstra
        pq = [(0, 0)] # time, node
        while pq:
            old_t, u = heappop(pq) # current time, current node
            for v, t in g[u]:
                new_t = old_t + t
                # casual logic: update shorter path
                if new_t < times[v]:
                    times[v] = new_t
                    ways[v] = ways[u]
                    heapq.heappush(pq, (new_t, v))
                # if find same time path... update ways only
                elif new_t == times[v]:
                    ways[v] += ways[u]
        return ways[n-1] % mod
```

1786. Number of Restricted Paths From First to Last Node

```python
class Solution:
    def countRestrictedPaths(self, n: int, edges: List[List[int]]) -> int:
        g, mod = defaultdict(list), 10 ** 9 + 7
        # dfs + memo
        @lru_cache(None)
        def dfs(cur):
            if cur == n:
                return 1
            res = 0
            for (nei, w) in g[cur]:
                # no need  to maintain set, it's impossible to go back
                if dist[cur] > dist[nei]: 
                    res += dfs(nei)
            return res
        # dijkstra
        for u, v, w in edges:
            g[u].append((v, w))
            g[v].append((u, w))
        
        dist = [inf] * (n + 1)
        dist[n] = 0
        visited = set()
        pq = [(0, n)]
        while pq:
            d, cur = heappop(pq)
            visited.add(cur)
            for (nei, w) in g[cur]:
                if nei not in visited:
                    new_dist = d + w
                    if new_dist < dist[nei]:
                        heappush(pq, (new_dist, nei))
                        dist[nei] = new_dist
        res = dfs(1)
        return res % mod
```

2290. Minimum Obstacle Removal to Reach Corner

```python
class Solution:
    def minimumObstacles(self, grid: List[List[int]]) -> int:
        ROWS, COLS = len(grid), len(grid[0])
        steps = 1 if grid[0][0] else 0
        pq = [(steps, 0, 0)]
        visited = set([(0, 0)])
        while pq:
            n, x, y = heappop(pq)
            if x == ROWS - 1 and y == COLS - 1:
                return n
            for dr, dc in [[0, 1], [0, -1], [1, 0], [-1, 0]]:
                r, c = x + dr, y + dc
                if 0 <= r < ROWS and 0 <= c < COLS and (r, c) not in visited:
                    visited.add((r, c))
                    if grid[r][c]:
                        heappush(pq, (n + 1, r, c))
                    else:
                        heappush(pq, (n, r, c))
```