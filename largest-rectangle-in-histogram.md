# Largest Rectangle in Histogram

## Problem

Given an array of integers `heights` where `heights[i]` represents the height of a bar with width `1`, return the area of the largest rectangle that can be formed among the bars.

## Approach: Monotonic Stack

We use a **monotonically increasing stack** of indices to efficiently find the left and right boundaries for each bar.

### Key Idea

For each bar, the largest rectangle using that bar's height extends left and right until it hits a shorter bar. A monotonically increasing stack tracks this efficiently. When a new bar is shorter than the top, the top bar has found its right boundary (the current index) and its left boundary (the new stack top after popping, or the start of the array if the stack is empty). So every pop gives us both boundaries and we can compute the area immediately. A sentinel bar of height 0 is appended at the end to flush all remaining bars from the stack.

### Algorithm

1. Append a bar of height `0` at the end of the array to act as a sentinel.
2. Iterate through the array with index `i`.
3. **Pop and compute:** While the stack is not empty and `heights[stack.top()] > heights[i]`, pop the top. The popped bar's height is `h`. The width is `i - stack.top() - 1` if the stack is not empty, or `i` if it is. Update the answer with `h * width`.
4. **Push current index** onto the stack.
5. Return the maximum area found.

### Complexity

| | |
|---|---|
| **Time** | O(n). Each index is pushed and popped at most once. |
| **Space** | O(n). The stack holds at most n elements. |

## Solution (C++)

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        stack<int> pref;
        int ans = 0;
        heights.push_back(0);
        for (int i = 0; i <= n; i++) {
            while (!pref.empty() && heights[pref.top()] > heights[i]) {
                int h = heights[pref.top()];
                int width = i - pref.top();
                pref.pop();
                if (!pref.empty()) width = i - pref.top() - 1;
                else width = i;
                ans = max(ans, width * h);
            }
            pref.push(i);
        }
        return ans;
    }
};
```