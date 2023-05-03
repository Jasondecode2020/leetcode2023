### 1 560. Subarray Sum Equals K
```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        for i in range(1, len(nums)):
            nums[i] += nums[i - 1]

        d, res = defaultdict(int), 0
        d[0] = 1
        for n in nums:
            prev = n - k
            if prev in d:
                res += d[prev]
            d[n] += 1
        return res
```