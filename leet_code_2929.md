# LEET CODE Problem 2929 #
You are given two positive integers n and limit.  

Return the total number of ways to distribute n candies among 3 children such that no child gets more than limit candies.

 

Example 1:

Input: n = 5, limit = 2  
Output: 3  
Explanation: There are 3 ways to distribute 5 candies such that no child gets more than 2 candies: (1, 2, 2), (2, 1, 2) and (2, 2, 1).  
Example 2:  

Input: n = 3, limit = 3  
Output: 10  
Explanation: There are 10 ways to distribute 3 candies such that no child gets more than 3 candies: (0, 0, 3), (0, 1, 2), (0, 2, 1), (0, 3, 0), (1, 0, 2), (1, 1, 1), (1, 2, 0), (2, 0, 1), (2, 1, 0) and (3, 0, 0).  
 

Constraints:

1 <= n <= 10^6  
1 <= limit <= 10^  
## Brute Force ##

- Brute Force Idea:
The brute force approach is to enumerate all possible values of 
𝑎
,
𝑏
,
𝑐  
a,b,c that satisfy the above two conditions.

- Step-by-step Approach:
We fix two variables (a and b) and compute the third (c = n - a - b), then check if c is valid:  

``` python
count = 0
for a in range(0, min(limit, n) + 1):
    for b in range(0, min(limit, n - a) + 1):
        c = n - a - b
        if 0 <= c <= limit:
            count += 1
return count
```
## Time Complexity ##
Time Complexity:
- Worst case: 
𝑂
(
limit
^
2
# 🏆 Optimized Approach

## 💡 Core Idea
We’re still solving:
$$ A + B + C = n $$
with constraints:
$$ 0 \leq A, B, C \leq limit $$

Instead of brute-forcing over two variables (A and B), we **fix B** and compute the number of valid (A, C) pairs that satisfy:
$$ A + C = n - B $$

## ✅ Strategy
1. **Loop over B** from `0` to `min(n, limit)`.
2. For each B:
   - Set `remaining = n - B`.
   - Find valid `(A, C)` pairs satisfying:
     $$ A + C = remaining $$ 
     with constraints:
     $$ 0 \leq A, C \leq limit $$

### 🔍 Breaking it Down:
For each `remaining`, the number of valid `(A, C)` pairs follows the condition:
$$ \max(0, remaining - limit) \leq A \leq \min(limit, remaining) $$

## 🔄 Combining All Conditions
Gathering constraints:
- $$ A \geq 0 $$
- $$ A \leq limit $$
- $$ A \leq remaining $$
- $$ A \geq remaining - limit $$

### 📌 Final Range for A
Combining lower bounds:
$$ A \geq \max(0, remaining - limit) $$

Combining upper bounds:
$$ A \leq \min(limit, remaining) $$

Thus, the valid range for A is:
$$ A \in [\max(0, remaining - limit), \min(limit, remaining)] $$

✔ This interval satisfies all four constraints **tightly**. 🚀
``` python
def count_distributions(n, limit):
    count = 0
    for B in range(min(limit, n) + 1):
        remaining = n - B
        min_A = max(0, remaining - limit)
        max_A = min(limit, remaining)
        if max_A >= min_A:
            count += (max_A - min_A + 1)
    return count
```
