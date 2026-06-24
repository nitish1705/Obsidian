
```java
class Solution {

    private int dfs(int index, int mask, boolean leadingZero, int tight, int[][][] dp, int n, String number){
        if (index == n) {
            return leadingZero ? 0 : 1; 
        }   

        if(!leadingZero && dp[index][mask][tight] != -1){
            return dp[index][mask][tight];
        }

        int count = 0;
        int limit = (tight == 1)? number.charAt(index) - '0' : 9;
        
        for(int i = 0; i <= limit; i++){
            if(!leadingZero && ((mask >> i) & 1) == 1){
                continue;
            }
            int nextTight = (tight == 1 && limit == i)? 1 : 0;

            if(leadingZero && i == 0){
                count += dfs(index + 1, mask, true, 0, dp, n, number);
            }
            else{
                count += dfs(index + 1, (mask | (1 << i)), false, nextTight, dp, n, number);
            }
        }
        
        if (!leadingZero) {
            dp[index][mask][tight] = count;
        }

        return count;
    }
    public int countSpecialNumbers(int n) {
        String number = String.valueOf(n);
        int[][][] dp = new int[number.length()][1 << 10][2];
        for(int[][] row : dp){
            for(int[] col :  row){
                Arrays.fill(col, -1);
            }
        }
        return dfs(0, 0, true, 1, dp, number.length(), number);
    }
}
```


[2376. Count Special Integers](https://leetcode.com/problems/count-special-integers/)

We call a positive integer **special** if all of its digits are **distinct**.

Given a **positive** integer `n`, return _the number of special integers that belong to the interval_ `[1, n]`.

**Example 1:**

**Input:** n = 20
**Output:** 19
**Explanation:** All the integers from 1 to 20, except 11, are special. Thus, there are 19 special integers.

**Example 2:**

**Input:** n = 5
**Output:** 5
**Explanation:** All the integers from 1 to 5 are special.

**Example 3:**

**Input:** n = 135
**Output:** 110
**Explanation:** There are 110 integers from 1 to 135 that are special.
Some of the integers that are not special are: 22, 114, and 131.

**Constraints:**

- `1 <= n <= 2 * 109`
