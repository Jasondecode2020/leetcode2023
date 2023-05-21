713. Subarray Product Less Than K


```python
class Solution:
    def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
        l, res, product = 0, 0, 1
        for r in range(len(nums)):
            product *= nums[r]
            while product >= k and l <= r:
                product //= nums[l]
                l += 1
            res += r - l + 1
        return res
```