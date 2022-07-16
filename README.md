# cheetsheat

Concise notes to prepare for algorithm questions in technical interviews.
Writing this down also helps me internalise the solutions.

## Dynamic Programming

### Longest Common Subsequence (LCS) \[Medium\]

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

### Delete Operation for Two Strings \[Medium\]

Use LCS. Answer is `m + n - 2*|LCS|`.

Complexities:
- Time: `O(nm)`
- Space: `O(nm)`. Reducible to `O(max(m,n))` by using only 2 rows.

### Longest Increasing Subsequence (LIS) \[Medium-Hard\]

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

### Number of Longest Increasing Subsequence (LIS) \[Medium-Hard\]

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

### Edit Distance \[Medium-Hard\]

Given two strings `s1` and `s2`, return the minimum number of operations required to convert `s1` to `s2`.
3 Actions: Insert, Delete and Replace.

#### Top-Down Memoization

Have a DP array `A` of size `(m+1)*(n+1)` where `A[i][j]` is the edit distance between `s1[:i]` and `s2[:j]`.

If `s1[-1] == s2[-1]`, no need for any action.
If not, then take the best (min) of insert, delete and replace.
Insert and delete corresponds to moving left and up in the DP array.
Replace corresponds to moving diagonally top left in the DP array.

Complexities:
- Time: `O(mn)`
- Space: `O(mn)`

#### Bottom-Up DP

Have a DP array `A` of size `(m+1)*(n+1)` where `A[i][j]` is the edit distance between `s1[:i]` and `s2[:j]`.

If `s1[i] == s2[j]`, then no need for any action.
If not, then take the best (min) of insert, delete and replace.
Insert and delete corresponds to moving right and down in the DP array.
Replace corresponds to moving diagonally bottom right in the DP array.

Complexities:
- Time: `O(mn)`
- Space: `O(mn)`

### Coin Change \[Medium\]

You are given an integer array `coins` representing coins of different denominations and an integer amount representing a total amount of money.
Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.
You may assume that you have an infinite number of each kind of coin.

#### Top-Down Memoization

Keep a DP array `A` of size `amount + 1` where `A[i]` is the min number of coins for amount `i`.
For each recursion call, we go through each coin value. We have
```
A[i] = min(A[i - j]) + 1 for each coin value
```

Complexities:
- Time: `O(n*amount)`
- Space: `O(amount)`

#### Bottom-Up Iterative
Keep a DP array `A` of size `amount + 1` where `A[i]` is the min number of coins for amount `i`.
`A[0] = 0` obviously.
We then again have
```
A[i] = min(A[i - j]) + 1 for each coin value
```

Complexities:
- Time: `O(n*amount)`
- Space: `O(amount)`

## Bit Manipulations

### Bitwise AND of Numbers Range \[Medium\]

Given two integers `left` and `right` that represent the range `[left, right]`, return the bitwise AND of all numbers in this range, inclusive.

#### Bit Scan Approach

Any bit difference between `left` and `right` means an element will have that bit to be 0.
So the answer is all the bits `left` and `right` share.
Since it is a range, they most likely share left-most bits.
So, scan from left to right and `OR` the current bit to the result until first diff bit.

Complexities:
- Time: `O(number of bits)`
- Space: `O(1)`

## Math

### Integer Break \[Medium\]

Given an integer `n`, break it into the sum of `k` positive integers, where `k >= 2`, and maximize the product of those integers.
Return the maximum product you can get.

#### Math way

For any factor `f >= 4`, `2(f-2) = 2f - 4 >= f` so we can break `f` up and get bigger.
We have `3*3*3 > 2*2*2*1` so `3` is better and `2` can be used at most twice.

For special cases where `n` is too small, since we need at least 2 factors, we have to break up `n` into 1s and 2s:
- `n = 2`: `1*1 = 1`
- `n = 3`: `1*2 = 2`

For normal cases, we have
- `n % 3 = 0`: Just split all into 3s => `3 ** (n/3)`
- `n % 3 = 1`: `2*2 > 3*1` and `n >= 4` => `2 * 2 * 3**((n-4)/3)`
- `n % 3 = 2`: `2 * 3**((n-2)/3)`

Complexities:
- Time: `O(log(n))`
- Space: `O(1)`
