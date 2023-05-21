1857. Largest Color Value in a Directed Graph

```python
class Solution:
    def largestPathValue(self, colors: str, edges: List[List[int]]) -> int:
        g = defaultdict(list)
        for u, v in edges:
            g[u].append(v)

        def dfs(node):
            if node in path:
                return inf
            if node in visited:
                return 0
            
            visited.add(node)
            path.add(node)
            colorIndex = ord(colors[node]) - ord('a')
            count[node][colorIndex] += 1
            for adj in g[node]:
                if dfs(adj) == inf:
                    return inf
                for i in range(26):
                    count[node][i] = max(count[node][i], count[adj][i] + (1 if colorIndex == i else 0))
            path.remove(node)
            return max(count[node])

        visited, path, n = set(), set(), len(colors)
        count, res = [[0] * 26 for i in range(n)], 0
        for i in range(n):
            if i not in visited:
                res = max(res, dfs(i))
        return res if res != inf else -1
```

332. Reconstruct Itinerary

```python
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        def dfs(city):
            while len(g[city]):
                dfs(g[city].pop(0))
            res.insert(0, city)

        res, g = [], defaultdict(list)
        for u, v in sorted(tickets):
            g[u].append(v)

        dfs('JFK')
        return res
```