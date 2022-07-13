# cheetsheat

Concise notes to prepare for algorithm questions in technical interviews.

## Easy

## Medium-Hard

### Longest Increasing Subsequence (LIS)

#### Naive Approach

`nums` is the array of input numbers.
Keep a DP array `A` where `A[i]` is the length of the LIS of `nums[:i+1]` (excluding `i+1`).
We then have
```
A[i] = max(A[j]) for 0 <= j < i such that nums[j] < nums[i]
```

- Time: `O(n^2)`
- Space: `O(n)`
