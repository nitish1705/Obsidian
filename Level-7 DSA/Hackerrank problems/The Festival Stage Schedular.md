
The kingdom of Arithmia is hosting its grand annual festival. Throughout the day, performers from different villages arrive to showcase their talents on the royal stage.Each performance requires exclusive use of the stage from its start time to its end time. Since there is only one stage, two performances cannot overlap.As the royal event coordinator, your task is to select the maximum number of performances that can be scheduled on the stage.Your goal is to determine the largest possible number of non-overlapping performances.

**Input Format**

The first line contains an integer N, representing the number of performances. The next N lines each contain two integers: Sᵢ — start time of the performance. Fᵢ — finish time of the performance.

**Constraints**

1 ≤ N ≤ 100000

0 ≤ Sᵢ < Fᵢ ≤ 10^9

**Output Format**

Print a single integer representing the maximum number of performances that can be scheduled without overlap.

**Sample Input 0**

6
1 4
3 5
0 6
5 7
8 9
5 9

**Sample Output 0**

3

**Explanation 0**

One optimal selection is:

(1,4) (5,7) (8,9)

Total performances = 3.

**Sample Input 1**

5
1 2
2 3
3 4
4 5
5 6

**Sample Output 1**

5

**Explanation 1**

All performances can be scheduled because each one starts exactly when the previous one ends.






CODE : 
`import java.io.*;`

`import java.util.*;`

  

`public class Solution {`

  

`public static void main(String[] args) {`

`Scanner sc = new Scanner(System.in);`

`int n = sc.nextInt();`

`int[][] arr = new int[n][2];`

`for(int i = 0; i < n; i++){`

`int n1 = sc.nextInt();`

`int n2 = sc.nextInt();`

`arr[i][0] = n1;`

`arr[i][1] = n2;`

`}`

`Arrays.sort(arr, (a,b) -> a[1] - b[1]);`

`int start = 1, end = arr[0][1], count = 1;`

`while(start < n){`

`if(arr[start][0] >= end){`

`count++;`

`end = arr[start][1];`

`}`

`start++;`

`}`

`System.out.println(count);`

`}`

`}`