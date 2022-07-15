# cheetsheat

Concise notes to prepare for algorithm questions in technical interviews.
Writing this down also helps me internalise the solutions.

## Easy

## Medium

### Longest Common Subsequence (LCS)

Given to strings `s1` and `s2`.
Find the length of the LCS between them.

#### DP Bottom-Up

Keep a DP array `A` of size `|s1|*|s2|` where `A[i][j]` is the length of the LCS of `s1_i` and `s2_j`.
Let `Z` be some LCS of `s1` and `s2`
We have:
- If `s1[i] == s2[j]`, then `s1[i] = s2[j]` is the last letter in `Z`. Otherwise, pick any other LCS, we can add `s1[i] = s2[j]` to extend it. `A[i][j] = A[i-1][j-1] + 1`
- Otherwise, then `A[i][j] = max(A[i-1][j], A[i][j-1])` because at least one of the conditions below is true. If both are false, then contradiction since `s1[i] == s2[j]`.
  - If `s1[i]` is not the tail of `Z`, then LCS of `s1_i` and `s2_j` is the same as `s1_{i-1}` and `s2_j`.
  - If `s2[j]` is not the tail of `Z`, then LCS of `s1_i` and `s2_j` is the same as `s1_i` and `s2_{j-1}`.

Complexities:
- Time: `O(nm)`
- Space: `O(nm)`. Reducible to `O(max(m,n))` by using only 2 rows.

### Delete Operation for Two Strings

Use LCS. Answer is `m + n - 2*|LCS|`.

Complexities:
- Time: `O(nm)`
- Space: `O(nm)`. Reducible to `O(max(m,n))` by using only 2 rows.

## Medium-Hard

### Longest Increasing Subsequence (LIS)

`nums` is the array of input numbers.
Find length of longest LIS in `nums`.

#### DP Bottom Up

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

`A` is now a sorted array. Use binary search to find the right tail.
Answer is `A.size()`.

Complexities:
- Time: `O(n log n)`
- Space: `O(n)`

### Number of Longest Increasing Subsequence (LIS)

`nums` is the array of input numbers.
Find the number of LIS in `nums`.

#### DP Bottom Up

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
