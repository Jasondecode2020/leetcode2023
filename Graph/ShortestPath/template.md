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