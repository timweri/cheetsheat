# cheetsheat

Concise notes to prepare for algorithm questions in technical interviews.
Writing this down also helps me internalise the solutions.

## Easy

## Medium-Hard

### Longest Increasing Subsequence (LIS)

`nums` is the array of input numbers.
Find length of longest LIS in `nums`.

#### Naive Approach

Keep a DP array `A` where `A[i]` is the length of the LIS of `nums[:i+1]` (excluding `i+1`).
We then have
```
A[i] = max(A[j]) for 0 <= j < i such that nums[j] < nums[i]
```

Complexities:
- Time: `O(n^2)`
- Space: `O(n)`

#### Smart Tail List Approach

Build a list `A` where `A[i]` is the smallest tail of all increasing subsequences seen so far of length `i+1` 
(essentially keeping track of the tail of the best subsequence for each length).
Iterate through `nums`, for each `v`:
-  Append to the longest **active** subsequence by checking `tail < v`.
-  Otherwise, find the longest subsequence where we can replace the tail where `v < tail` because having `v` as tail guarantee longer subsequence with the same prefix.

Complexities:
- Time: `O(n log n)`
- Space: `O(n)`
