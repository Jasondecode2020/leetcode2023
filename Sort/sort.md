1433. Check If a String Can Break Another String

```python
class Solution:
    def checkIfCanBreak(self, s1: str, s2: str) -> bool:
        l1, l2, n = sorted(list(s1)), sorted(list(s2)), len(s1)
        if all(a >= b for (a, b) in zip(l1, l2)) or all(a <= b for (a, b) in zip(l1, l2)):
            return True
        return False
```