```java
class Solution {

    private int dfs(int ind, int mask, boolean leadingZero, int[][] dp){
        if(ind < 0) return 1;

        if(!leadingZero && dp[ind][mask] != -1) return dp[ind][mask];

        int count = 0;

        for(int i = 0; i <= 9; i++){
            if(!leadingZero && ((mask >> i) & 1) == 1) continue;
            if(leadingZero && i == 0) {
                count += dfs(ind - 1, mask, true, dp);
            } 
            else{
                count += dfs(ind -1, (mask | (1 << i)), false, dp);
            }
        }

        return count;
    }
    public int countNumbersWithUniqueDigits(int n) {
        int[][] dp = new int[n][1 << 10];
        for(int i = 0; i < n; i++){
            Arrays.fill(dp[i], -1);
        }
        return dfs(n-1, 0, true, dp);
    }

}
```


  

[357. Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

Given an integer `n`, return the count of all numbers with unique digits, `x`, where `0 <= x < 10n`.

**Example 1:**

**Input:** n = 2
**Output:** 91
**Explanation:** The answer should be the total numbers in the range of 0 ≤ x < 100, excluding 11,22,33,44,55,66,77,88,99

**Example 2:**

**Input:** n = 0
**Output:** 1

**Constraints:**

- `0 <= n <= 8`