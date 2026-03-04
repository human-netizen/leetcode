# Sliding Window Maximum

## Problem

Given an array of integers `nums` and a sliding window of size `k`, return an array of the maximum value in each window as it slides from left to right across the array.

## Approach: Monotonic Deque

We use a **monotonic decreasing deque** that tracks candidate indices.

### Key Idea

At any point, if a newer element is larger than an older one, the older one can never be the answer for the current or any future window. So we can safely discard it from the deque. This keeps the deque sorted in decreasing order of values, meaning the front always holds the index of the current window's maximum.

### Algorithm

1. Iterate through the array with index `i`.
2. **Pop from back:** While the deque is not empty and `nums[deque.back()] <= nums[i]`, pop from the back. These elements are now useless since `nums[i]` is bigger and will outlive them in the window.
3. **Push current index** onto the back of the deque.
4. **Pop from front:** If the front index has fallen out of the window (`deque.front() <= i - k`), remove it.
5. **Record result:** Once the window is fully formed (`i >= k - 1`), the front of the deque is the max for this window.

### Complexity

| | |
|---|---|
| **Time** | O(n). Each element is pushed and popped from the deque at most once. |
| **Space** | O(k). The deque holds at most `k` elements. |

## Solution (C++)

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> dq;
        vector<int> res;

        for (int i = 0; i < (int)nums.size(); i++) {
            while (!dq.empty() && nums[dq.back()] <= nums[i])
                dq.pop_back();

            dq.push_back(i);

            if (dq.front() <= i - k)
                dq.pop_front();

            if (i >= k - 1)
                res.push_back(nums[dq.front()]);
        }

        return res;
    }
};
```