# Summoners Incantation

## Solution

```python
input_text = input()  

nums = list(map(int, input_text.strip("[]").split(",")))

def max_sum_non_adjacent(nums):
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]
    

    prev_max = nums[0]
    curr_max = max(nums[0], nums[1])
    
    for i in range(2, len(nums)):
        new_max = max(curr_max, prev_max + nums[i])
        
        prev_max = curr_max
        curr_max = new_max
    
    return curr_max


result = max_sum_non_adjacent(nums)
print(result)
```

