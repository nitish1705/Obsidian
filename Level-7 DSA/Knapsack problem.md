
Captain Jack Sparrow and his crew have discovered a hidden treasure cave on a mysterious island.Inside the cave are N treasure chests. Each chest contains gold coins worth a certain value, but each chest also has a weight.The crew's ship, the Black Pearl, can carry at most W units of weight before it sinks.Captain Jack must decide which chests to load onto the ship to maximize the total value of treasure carried.Each chest can be:Taken completely, or Left behind completely.A chest cannot be split.Find the maximum treasure value that can be transported.

**Input Format**

N W v1 v2 v3 ... vN w1 w2 w3 ... wN N = number of treasure chests W = maximum weight capacity of the Black Pearl vi = value of the i-th chest wi = weight of the i-th chest

**Constraints**

1 ≤ N ≤ 100 1 ≤ W ≤ 1000 1 ≤ vi ≤ 10^5 1 ≤ wi ≤ 1000

**Output Format**

Maximum Treasure Value

**Sample Input 0**

3 7
3 4 5
2 3 4

**Sample Output 0**

9

**Explanation 0**

Take:

Chest 2 → value 4, weight 3 Chest 3 → value 5, weight 4

Total weight = 7

Total value = 9

**Sample Input 1**

4 8
2 5 7 8
1 3 4 5

**Sample Output 1**

14

**Explanation 1**

Take:

Chest 2 (5) Chest 3 (7) Chest 1 (2)

Weight = 3 + 4 + 1 = 8

Value = 14


```java
import java.io.*;
import java.util.*;

public class Solution {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int w = sc.nextInt();
        
        int[] value = new int[n];
        int[] weight = new int[n];
    
        for(int i = 0; i < n; i++){
            value[i] = sc.nextInt();
        } 
        for(int i = 0; i < n; i++){
            weight[i] = sc.nextInt();
        }
        
        int[] dp = new int[w + 1];
        for(int i = 0; i < n; i++){
            for(int j = w; j >= weight[i]; j--){
                dp[j] = Math.max(dp[j], value[i] + dp[j - weight[i]]);
            }
        }
        System.out.println(dp[w]);
    }
}
```