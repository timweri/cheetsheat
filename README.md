# cheetsheat

Concise notes to prepare for algorithm questions in technical interviews.
Writing this down also helps me internalise the solutions.

## Easy

## Medium-Hard

### Longest Increasing Subsequence (LIS)

`nums` is the array of input numbers.
Find length of longest LIS in `nums`.

#### Naive Approach

Keep a DP array `A` where `A[i]` is the length of the LIS ending with `nums[i]`.
We then have
```
A[i] = max(A[j]) + 1 for 0 <= j < i such that nums[j] < nums[i]
=> Answer is max(A[i])
```

Complexities:
- Time: `O(n^2)`
- Space: `O(n)`

#### Smart Tail List Approach

Build a list `A` where `A[i]` is the smallest tail of all increasing subsequences seen so far of length `i+1` 
(essentially keeping track of the tail of the best subsequence for each length).
Iterate through `nums`, for each `v`:
-  If `A[A.size() - 1] < v`, append `v` to the longest **active** subsequence.
-  Otherwise, find the longest subsequence where we can replace the tail where `v < tail` because having `v` as tail guarantee longer subsequence with the same prefix.

A is now a sorted array. Use binary search to find the right tail.

Complexities:
- Time: `O(n log n)`
- Space: `O(n)`

### Number of Longest Increasing Subsequence (LIS)

`nums` is the array of input numbers.
Find the number of LIS in `nums`.

#### Naive Approach

Keep two DP arrays `L` and `Cnt` where
- `A[i]` is the length of the LIS ending with `nums[i]`.
- `Cnt[i]` is the number of LIS ending with `nums[i]`.
We then have
```
A[i] = max(A[j]) + 1 for 0 <= j < i such that nums[j] < nums[i]
Cnt[i] = sum(Cnt[j]) for 0 <= j < i such that nums[j] < nums[i] and A[j] + 1 == A[i]
=> Answer is sum(Cnt[i]) for all i such that A[i] == max(A[j])
```

Complexities:
- Time: `O(n^2)`
- Space: `O(n)`
