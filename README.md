# Equal Subset Sum Partition

Given a set of positive numbers, find if we can partition it into two subsets such that the sum of elements in both the subsets is equal.

Example 1: #
```
Input: {1, 2, 3, 4}
Output: True
Explanation: The given set can be partitioned into two subsets with equal sum: {1, 4} & {2, 3}
```

Example 2: #
```
Input: {1, 1, 3, 4, 7}
Output: True
Explanation: The given set can be partitioned into two subsets with equal sum: {1, 3, 4} & {1, 7}
```

Example 3: #
```
Input: {2, 3, 4, 6}
Output: False
Explanation: The given set cannot be partitioned into two subsets with equal sum.
```

We can break down the problem statement as

Assume if S represents the total sum of all the given numbers, then the two equal subsets must have a sum equal to S/2. This essentially transforms our problem to: 
"Find a subset of the given numbers that has a total sum of S/2".


Lets solve this problem using dynamic programming

Consider the input {1,2,3,4}
Lets build our 2D table as below

S = 1+2+3+4 = 10


Let’s try to populate our dp[][] array from the above solution, working in a bottom-up fashion. Essentially, we want to find if we can make all possible sums with every subset.   
**This means, dp[i][s] will be ‘true’ if we can make sum ‘s’ from the first ‘i’ numbers.** 

So, for each number at index ‘i’ (0 <= i < num.length) and sum ‘s’ (0 <= s <= S/2), we have two options:
1. Exclude the number. In this case, we will see if we can get ‘s’ from the subset excluding this number: dp[i-1][s]
2. Include the number if its value is not more than ‘s’. In this case, we will see if we can find a subset to get the remaining sum: dp[i-1][s-num[i]]

If either of the two above scenarios is true, we can find a subset of numbers with a sum equal to ‘s’.

Let’s start with our base case of empty set capacity:

| set\sum | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| {} | T |F|F|F|F|F|
| {1}| T|||||
| {1,2}| T|||||
| {1,2,3}| T|||||
| {1,2,3,4}| T|||||

Since sum=0 can be found via an empty set we have set the columns above to T

For the set containing {1}, our entries will look like

| set\sum   | 0 | 1 | 2 | 3 | 4 | 5 |
|-----------|---|---|---|---|---|---|
| {}        | T | F | F | F | F | F |
| {1}       | T | T | F | F | F | F |
| {1,2}     | T |   |   |   |   |   |
| {1,2,3}   | T |   |   |   |   |   |
| {1,2,3,4} | T |   |   |   |   |   |



For set {1,2}:  
* For sum=1, since dp[index-1][sum] = T, we have dp[index][sum] = T, ie we can exclude this new item in set to get reqd sum  
* For sum=2, we can just have the new item in set to get the result, hence dp[index][sum] = T
* For sum=3, we check if
  * previous row is T (since the sum can be created by ignoring the new item in set) 
  * previous row's sum without the new element (2 in this case) is T

ie dp[index][sum] = dp[index-1][sum] || dp[index-1][sum - new_item]

* For sum=4, hence is F, so is sum=5

| set\sum   | 0 | 1 | 2 | 3 | 4 | 5 |
|-----------|---|---|---|---|---|---|
| {}        | T | F | F | F | F | F |
| {1}       | T | T | F | F | F | F |
| {1,2}     | T | T | T | T | F | F |
| {1,2,3}   | T |   |   |   |   |   |
| {1,2,3,4} | T |   |   |   |   |   |


For set {1,2,3}:  
* sum=0,1,2,3 = T since dp[index-1][sum] = T  
* sum[4] = T since dp[index-1][sum-3] = T  
* sum[5] = T since dp[index-1][sum-3] = T  

| set\sum   | 0 | 1 | 2 | 3 | 4 | 5 |
|-----------|---|---|---|---|---|---|
| {}        | T | F | F | F | F | F |
| {1}       | T | T | F | F | F | F |
| {1,2}     | T | T | T | T | F | F |
| {1,2,3}   | T | T | T | T | T | T |
| {1,2,3,4} | T |   |   |   |   |   |


For set {1,2,3,4}:  
* sum=0,1,2,3,4,5 = T since dp[index-1][sum] = T  

| set\sum   | 0 | 1 | 2 | 3 | 4 | 5 |
|-----------|---|---|---|---|---|---|
| {}        | T | F | F | F | F | F |
| {1}       | T | T | F | F | F | F |
| {1,2}     | T | T | T | T | F | F |
| {1,2,3}   | T | T | T | T | T | T |
| {1,2,3,4} | T | T | T | T | T | T |


Hence, it is possible to partition to set {1,2,3,4} into two subsets with equal sums.




Consider the set {2, 2, 4, 6}

S=2+2+4+6 = 14

| set\sum   | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|-----------|---|---|---|---|---|---|---|---|
| {}        | T | F | F | F | F | F | F | F |
| {2}       | T | F | T | F | F | F | F | F |
| {2,2}     | T | F | T | F | T | F | F | F |
| {2,2,4}   | T | F | T | F | T | F | T | F |
| {2,2,4,6} | T | F | T | F | T | F | T | F |  

Hence, the set {2, 2, 4, 6| cannot be partitioned into two subsets with equal sum.







