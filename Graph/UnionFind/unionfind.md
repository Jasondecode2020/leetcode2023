### 990. Satisfiability of Equality Equations
```python
class Solution:
    def equationsPossible(self, equations: List[str]) -> bool:
        uf = {c: c for c in string.ascii_lowercase}
        def find(x):
            while x != uf[x]:
                x = uf[x]
            return x
        # union first
        for a, e, _, b in equations:
            if e == "=":
                p1 = find(a)
                p2 = find(b)
                uf[p2] = p1
        # check union second
        for a, e, _, b in equations:
            if e == "!" and find(a) == find(b):
                return False
        return True
```