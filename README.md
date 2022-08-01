# cheetsheat

Concise notes to prepare for algorithm questions in technical interviews.
Writing this down also helps me internalise the solutions.

Credits: Leetcode

## Array

### Two Sum \[Easy\]

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.
You may not use the same element twice.

#### Hash Map

Use hash map.

Complexities:
- Time: `O(n)`
- Space: `O(n)`

### Merge Sorted Array \[Easy\]

You are given two integer arrays `nums1` and `nums2`, sorted in non-decreasing order, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively. Merge `nums1` and `nums2` into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array `nums1`. To accommodate this, nums1 has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to 0 and should be ignored. `nums2` has a length of `n`.

#### Linear Placement

Start placing the largest element between the two into `nums1`.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

### Intersection of Two Arrays II \[Easy\]

Given two integer arrays `nums1` and `nums2`, return an array of their intersection. Each element in the result must appear as many times as it shows in both arrays and you may return the result in any order.

#### Frequency Hash Map

If one array is big and one is small, put the small array in hash map.

Complexities:
- Time: `O(m + n)`
- Space: `O(m + n)`

#### Sort

Can use external sort if both arrays are too big.

Complexities:
- Time: `O(mlogm + nlogn)`
- Space: `O(m + n)`

### Best Time to Buy and Sell Stock \[Easy\]

Given an array of prices of stock on each day, we can choose ONE day to buy and ONE day to sell.
Find max profit.

#### Linear Scan

Scan for low prices to buy. 
If price is lower than lowest, buy then as any higher price that can make more profit with the old low can make even more profit with new low.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

### Best Time to Buy and Sell Stock II \[Medium\]

Given an array of prices of stock on each day, each day we can buy or sell.
We can't hold 2 stocks at the same time.
Selling and buying on the same day is allowed.
Find max profit.

#### Linear Scan

Just buy and sell if `prices[i] > prices[i-1]`.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

### Best Time to Buy and Sell Stock III and IV \[Hard\]

Given an array of prices of stock on each day, each day we can buy or sell.
We can't hold 2 stocks at the same time.
Selling and buying on the same day is allowed.
We can use upto `k` transactions.
Find max profit.

#### Dynamic Programming

```
A[k][i] = max profit on day i, using k transactions
```
If, for transaction `k`, we don't do anything on day `i`, then `A[k][i] = A[k][i-1]`.
If, we decide to sell on day `i`, then `A[k][i] = max(A[k-1][j-1] + prices[i] - prices[j])` for `j < i`, where `j` is the day we buy the stock we sell.
So we have
```
A[k][i] = max(A[k][i-1], A[k-1][j-1] + prices[i] - prices[j]) for j < i
```

Complexities:
- Time: `O(kn^2)`
- Space: `O(kn)`

#### Optimized DP

We can optimize this by using this formulae:
```
A[k][i] = max(A[k][i-1], prices[i] + max(A[k-1][j-1] - prices[j])) for j < i
```

This allows us to calculate `max(A[k-1][j-1] - prices[j])` without using a second linear loop.

Complexities:
- Time: `O(kn)`
- Space: `O(kn)`

#### Godly Optimized DP

Since our inner max value `max(A[k-1][j-1] - prices[j])` only required the lookback of 1 day, we can just keep information for
2 days.

Complexities:
- Time: `O(kn)`
- Space: `O(k)`

### Maximum Subarray \[Medium\]

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

#### Scan

If next number `nums[i+1]` is at least the Maximum Subarray Sum containing `nums[i]` (`nums[i+1] >= nums[i+1] + sum`), better to start a new subarray from `nums[i+1]`.
Otherwise, it's better to add `nums[i+1]` to current Subarray.
Remove leading negative elements to maximize sum.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

#### DP

`A` is the DP array of size `n`.
`A[i]` is the Maximum Subarray Sum for `nums[:i+1]` that includes `nums[i]`.
```
A[i] = nums[i] >= A[i-1] + nums[i] ? nums[i] : A[i-1] + nums[i];
```

Complexities:
- Time: `O(n)`
- Space: `O(n)`

## Linked List

### Linked List Cycle \[Easy\]

Detect cycle in a linked list.

#### Floyd's Cycle Detection Algorithm

Have a slow and fast pointer.

Do in a loop:
- Increment slow 1 step at a time.
- Increment fast by 2 steps at a time.
- If loop end is found => no loop
- If fast is same as slow => loop detected

Let `m` be the length of the loop.
So, `n-m` is the length before the loop.

If there is no cycle, it takes `O(n)` for fast to find the end of the loop.

If there is a cycle:
- It takes `n-m` steps for slow to enter the loop, by then, fast should be in the loop.
- Every iteration, the gap between fast and slow increases by 1.
- fast and slow will meet when the distance is `m` or divisible by `m`.
- It takes at most `m` steps for the distance to reach `m`.
- Hence, it takes `n - m + m = n = O(n)` for fast and slow to meet.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

## Tree

### Validate Binary Search Tree \[Medium\]

Given a Binary Tree. Check if it's a valid BST.

#### Approach

REMINDER: Keep track of the range of values the nodes can take.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

### Lowest Common Ancestor of a Binary Search Tree \[Easy-Medium\]

Given 3 BSTs `T`, `p` and `q`. `p` and `q` are subtrees of `T`.
Find the LCA of `p` and `q` in `T`.

#### Approach

Check if current `root` is `p` or `q`:

- If `root` is neither:
  - the LCA is lower. Search `root->left` and `root->right`.
  - `root->left` AND `root->right` returns something => `root` is the LCA
  - Only one returns => the returned value is the LCA.
  - If both returns, then current `root` is the LCA.
- If `root` is one: returns `root` as the LCA cannot be lower.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

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

## Others

### Shuffle Array \[Medium\]

Given an integer array `nums`, design an algorithm to randomly shuffle the array. All permutations of the array should be equally likely as a result of the shuffling.

#### Use Randomization Module

Initialize a RNG `rand`.
For each shuffle, for each number, just swap with a random index.

Complexities:
- Time: `O(n)`
- Space: `O(n)`

### Happy Number \[Easy\]

Write an algorithm to determine if a number `n` is happy.

A happy number is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
- Those numbers for which this process ends in 1 are happy.

Return `true` if `n` is a happy number, and `false` if not.

#### Cycle Detection

Just calculate the next number in the sequence and use Floyd's Cycle Detection to detect Cycle.

Complexities:
- Time: `O(|sequence + cycle if exists|)`
- Space: `O(1)`
