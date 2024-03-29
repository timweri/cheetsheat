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

### Single Number \[Easy\]

Given an array of numbers where each number occurs twice except for one number.
Find this single number.

#### XOR

Just XOR the whole array.
The duplicates will cancel themselves out.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

### Majority Element \[Easy-Medium\]

Find the majority element in an array.
The majority element is the element that appears more than `floor(n / 2)` times. You may assume that the majority element always exists in the array.

#### Boyer-Moore Majority Vote Algorithm

Keep track of the current majority number and the vote count.

If the next number is the current majority number, increment the vote count.
If not:
- if the vote count is 0, we have a new majority candidate
- else, decreemnt the vote count and keep the current majority candidate

This works because there is enough vote for the majority number to fight all other numbers.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

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

### 3Sum \[Medium\]

Given an integer array `nums`. Find all triplets `[nums[i], nums[j], nums[k]]` such that `i`, `j` and `k` are all distinct and `nums[i] + nums[j] + nums[k] == 0`.
Avoid duplicates.

#### Two Pointer

Sort the array since sorting is only `O(n log n)`. This will not affect our overall runtime of `O(n^2)`.
Fix the first number to be each number from left to right.
Run two pointer on the remaining portion of the array.

To avoid duplicates, skip the subsequent contiguous duplicated elements for each position.

Optimizations:
- If `nums[i] > 0`, there's no way sum is 0 because `nums` is sorted.

Complexities:
- Time: `O(n^2)`
- Space: `O(1)`

#### Set

Partition `nums` into 3 separate arrays: zero, negative and positive.
Make 2 lookup sets: negative and positive.

Cases: 
- If `|zero| >= 3`, add `[0,0,0]` to answer.
- If there is at least 1 zero, whatever exists in both negative and positive sets form an answer.
- For all pairs of negatives, the last element must be in the positive
- For all pairs of positives, the last element must be in the negative

Complexities
- Time: `O(n^2)`
- Space: `O(n)`

### Maximum Subarray Sum \[Medium\]

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

### Maximum Subarray Product \[Medium\]

Given an integer array `nums` (numbers can be negative), find a contiguous non-empty subarray within the array that has the largest product, and return the product.

#### Scan

Keep track of the best negative `neg` and best positive product `pos`.
Since this is multiplying, unless the new num is `0`, the magnitude of `neg` and `pos` is non-decreasing.
Once the new num is `0`, both have to reset to `0` since we only count subarrays.

If current `num` is:
- `num = 0`: then reset both `neg` and `pos` to `0`.
- `num > 0`:
  - signs of `neg * num` and `pos * num` won't change
  - if `pos = 0`, it's better to set `pos := num`
  - no matter what, `neg *= num`
- `num < 0`:
  - `neg * num` and `pos * num` switch sign
  - if `neg = 0`, it's better to set `neg := num`
  - no matter what, `neg = pos * num`

Complexities:
- Time: `O(n)`
- Space: `O(1)`

### Merge Intervals \[Medium\]

Given an array `intervals`, merge all overlapping intervals and return the final array.

#### Sort

Sort by the starting time.
Compare the last interval in the result with every interval in `intervals`.
If not overlapping, since sorted by start, all subsequent intervals cannot overlap => new element in the result array.
If overlap, update the end time of the last element in the result.

Complexities:
- Time: `O(n log n)`
- Space: `O(sort)`

### Search a 2D Matrix II \[Medium\]

Search for `target` in a `m x n` matrix where each row and column are sorted in ascending order.

#### Binary Search

For each row that may contain `target`, perform binary search.

Complexities:
- Time: `O(m log n)`
- Space: `O(1)`

#### Greedy

Search from top right.
If `matrix[i][j] < target`, move down because everything on the left is even smaller (eliminated row).
If `matrix[i][j] > target`, move left because everything below is even bigger (eliminated column);

Complexities:
- Time: `O(m + n)`
- Space: `O(1)`

### Non-Overlapping Intervals

Given an array of intervals, find the minimum number of intervals we have to delete to obtain the maximum number of non-overlapping intervals.

#### Greedy 1 (Sort by start time)

Sort intervals by start time.
We keep track of the last interval we chose, named `cur`.
Then we look for the next interval to be added.
Prefer ones that end earlier as anything that does not overlap with the later end will also not overlap with the earlier end but not the other way around.
We have 3 cases:
- No overlapping: `f_cur <= s_next`. No need to delete anything, `cur = next`.
- Partial overlapping: `s_next < f_cur <= f_next`. Delete later end.
- Total overlapping: `next` contained within `cur`. Delete cur. `cur = next`.

Complexities:
- Time: `O(n log n)`
- Space: `O(sort)`

#### Greedy 2 (Sort by end time)

It's the number of intervals minus answer of the Scheduling Interval problem, which is the maximum number of non-overlapping intervals.

Complexities:
- Time: `O(n log n)`
- Space: `O(sort)`

### Subarray Sum Equals K \[Medium\]

Given an array of integers `nums` and integer `k`, return the number of subarrays whose sum is `k`.

#### HashMap and Prefix Sum

HashMap `A` maps the value of prefix sum to the number of occurences of that sum.
Loop from left, keep track of the current prefix sum.
For each element, update the prefix sum and then the HashMap.

If current sum is `sum`, we need some prefix sum of value `sum - target`.
Just look up in `A` to see how many prefix sums of this value we have and increase count.

Complexities:
- Time: `O(n)`
- Space: `O(n)`

### Increasing Triplet Subsequence \[Medium-Hard\]

Given an integer array `nums`. Determine whether `nums` contains an increasing subsequence of size 3.

#### LIS

Use LIS algorithm. Terminates when we find a subsequence of size 3.

Complexities:
- Time: `O(n * log 3) = O(n)`
- Space: `O(3) = O(1)`

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

### Linked List Cycle II \[Medium\]

Find the entry point of cycle in a linked list.

#### Floyd's CDA with a Twist

Use Floyd's Cycle Detection Algorithm to detect the cycle and find the meeting point of
the fast and slow pointer.

Let `M` be the length from head to entry point.
Let `N` be the length from entry point to meeting point.
Let `C` be the length of the cycle.

The fast pointer travels `M + N + C` to reach the meeting point.
The slow pointer travels `M + N` to reach the meeting point.
We have `M + N + C = 2(M + N)`, hence `M = C - N`.
This means the length from head to entry point is the same as meeting point to entry point.

So we just need to traverse from head and from meeting point till we meet at entry point.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

### Intersection of Two Linked Lists \[Medium\]

Given two linked lists `l1` and `l2`, find the start of the merge of two lists.

#### Hash Map

Hash Map every linked list node of `l1`. Traverse `l2` and look up the map.

Complexities:
- Time: `O(m + n)`
- Space: `O(m)`

#### Find length first

Traverse both list and find the length of each.
The goal is to start from the same distance away from the merge.

If the merge exists, then it has length at most `min(m,n)`.
So we just have to start traversing from length `min(m,n)`.

Complexities:
- Time: `O(m + n)`
- Space: `O(1)`

#### Simultaneous traverses

Say the merge has length `k`.
Then the last `k` nodes of both `l1` and `l2` are the same.

We have `l1 + l2 = l1 + (l2 - k) + k` and `l2 + l1 = l2 + (l1 - k) + k`.
Since `l1 + (l2 - k) = l2 + (l1 - k)`, if we traverses two lists `l1->l2` and `l2->l1`,
we will reach the merge at the same time.

Complexities:
- Time: `O(m + n)`
- Space: `O(1)`

### Merge K Sorted List \[Medium-Hard\]

#### Merge 2 Lists at a time

We can merge two lists at at a time.

Let the average length of each list be `n`.

- First round, we have `k` lists of length `n`. Merging takes `O(k/2 * 2n) = O(kn)`.
- Second round, we have `k/2` lists of length `2n`. Merging takes `O(k/4 * 4n) = O(kn)`.
- We have `log k` rounds.

Complexities:
- Time: `O(kn log k)`
- Space: `O(kn)`

#### Min Heap

Build a min heap containing the min item in each list. 
Each time we add the min element to our result list and replace this min element in the min heap with the next element.

Complexities:
- Time: `O(n log k)`. `n` is the total number of elements in `k` lists. For each element, we insert and pop it into the heap, which is `O(log k)`.
- Space: `O(k)`

## String

### Group Anagrams \[Medium\]

Given an array of strings, return the array of all groups of anagrams.

#### HashMap + Count Sort

Use hash map to map string to index of anagram group.
To make all anagrams use the same lookup string, use count sort.

Complexities:
- Time: `O(number of groups * size of string)`
- Space: `O(number of groups)`

### Multiply Strings \[Medium\]

Given 2 non-negative integers as strings `s1` and `s2`, multiply them.

#### Digit by Digit

Make an array of size `|s1| + |s2|`.
Multiply each pair of digits in `s1` and `s2`, store the result and overflow at the appropriate indices.

Complexities:
- Time: `O(|s1| * |s2|)`
- Space: `O(|s1| + |s2|)`

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

### Two Sum IV (BST) \[Medium\]

Given `k`, find two nodes in a BST that sum up to `k`.

#### Hashmap

Just visit every node and keep track of things in a hashmap like normal two sum.

Complexities:
- Time: `O(n)`
- Space: `O(n)`

#### Iterators

BST is kind of equivalent to a sorted array.
Write an inorder iterator and reverse inorder iterator for the BST and use the two pointer method.

Complexities:
- Time: `O(n)`
- Space: `O(log n)`

## Stack/Queue

### Minimum Remove to Make Valid Parentheses \[Medium\]

Given a string with letters and parentheses, remove the min amount of parentheses to make it valid.

#### Stack

Keep stack of index of brackets to delete.
If we see `(`, just push. Any leftover `(` at the end will stay and be deleted.
If we see `)`, if the top is a `(`, then pop a `(`. These two brackets are not in the stack so will not be deleted.

Go through the stack and replace all invalid bracket with `#`.
Build a new string and only add non-`#` characters.

Complexities:
- Time: `O(n)`
- Space: `O(n)`


#### Delete

Go from left to right and count the amount of unclosed parentheses (`unclosed`).
If `unclosed = 0`, then if we see `)`, there is nothing to close => mark `)` for deletion by changing it to `#`.

Do the same from right to left.

Build a string and only add non-`#` characters.

Complexities:
- Time: `O(n)`
- Space: `O(1)`

## Backtracking

### Combination Sum \[Medium\]

Given an array of distcint integers `candidates`.
Return a list of all unique combinations of candidates where the sum is `target`.

#### Backtracking

We traverse the tree of all solutions using DFS.
The maximum number of children for each tree is `N = |candidates|`.
The target `target = T` and `M = min(candidates)`.

The tree is an `N`-ary tree with depth `T/M`.
This tree has a maximal size of `N^(T/M + 1)`.

Complexities:
- Time: `O(N^(T/M+1))` which is the upper bound on the total number of nodes in the tree. 
- Space: `O(T/M)` which because of the deepest traversal using the smallest candidates.

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

## Monotonic Stack

### Daily Temperatures

Given an array of integers `temperatures` represents the daily temperatures, return an array answer such that `answer[i]` is the number of days you have to wait after the ith day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

#### Monotonic Stack

Use a monotonic stack to store all the indices of the days where we haven't found a day with higher temperature.

Complexities:
- Time: We do at most `n` push and `n` pops. `O(n)`
- Space: `O(n)`

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

### Josephus Problem (Find the Winner of the Circular Game) \[Hard\]

Given a circle of `n` people and a number `k`.
Starting with person `1`, the person `i` will kill person `i + k - 1` in the circle.
Then, continue with the next person after `i + k - 1`.
Find the last survivor.

#### Simulation

We can keep a vector of remaining people.
We simulate the problem and remove the person who is killed.
Last person in the vector is the survivor.

We run `n-1` iterations.
For each iteration, it's `O(1)` to find the next victim and the next killer.
However, deletion in the vector is `O(n)`.

Complexities:
- Time: `O(n^2)`
- Space: `O(n)`

#### Recursion Simulation

Here, we will use `0`-indexing for numbering people.

Induction Proof:

Hypothesis: If `f(n, k)` is the last survivor for `(n,k)`, then `f(n+1, k) = (f(n,k) + k) % (n+1)` is the last survivor for `(n+1, k)`.

Base Case: `f(1, k) = 0`. (`0`th person is the survivor)

Inductive Step: Assume `f(n,k)` is the last survivor for `(n,k)`. We need to prove `f(n+1,k) = (f(n,k) + k) % (n+1)`.

In `(n+1,k)`, we have that `k % (n+1)` is the next killer, moving the game to the state of `(n,k)`.
This next killer is the starter of `(n,k)`, so in this new game state, the next killer is number `0`.

In this new game state `(n,k)`, `f(n,k)` is the survivor.
`f(n,k)` is numbered using this new numbering system.
So `f(n,k)` is actually `(k + f(n,k)) % (n+1)` in `(n+1, k)`.

Since `f(n,k) <= n - 1`, the person was first killed in `(n+1, k)`, which is `(k-1) % (n+1)` cannot be the survivor as
`f(n,k)` is not big enough to go around to reach this killed person.

Hence, `(k + f(n,k)) % (n + 1)` is the last survivor of `(n+1,k)`. QED?

So, our recursive solution is:
```
f(n,k) = (f(n-1,k) + k) % n
```

Complexities:
- Time: `O(n)`
- Space: `O(n)`

#### Iterative Simulation

Same proof as the recursive case.
However, we can write a bottom-up iterative solution.

Complexities:
- Time: `O(n)`
- Space: `O(1)`
