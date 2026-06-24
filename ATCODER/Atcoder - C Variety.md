
C - Variety ![](https://img.atcoder.jp/assets/top/img/flag-lang/ja.png) / ![](https://img.atcoder.jp/assets/top/img/flag-lang/en.png) 

---

Time Limit: 2 sec / Memory Limit: 1024 MiB

Score : 300 points

### Problem Statement

There are N gems. The color (represented as an integer) of the i-th gem is Ci​ and its value is Vi​.

Choose K gems from these N gems. Here, the chosen gems must have at least M distinct colors.

Find the maximum possible total value of the chosen gems. (Such a choice is always possible in the given input.)

### Constraints

- 1≤M≤K≤N≤2×105
- 1≤Ci​≤N
- 1≤Vi​≤109
- There exist gems of at least M distinct colors.
- All input values are integers.

---

### Input

The input is given from Standard Input in the following format:

N K M
C1​ V1​
C2​ V2​
⋮
CN​ VN​

### Output

Output the maximum possible total value of the chosen gems as an integer.

---

### Sample Input 1

Copy

5 3 2
1 30
1 40
1 50
2 10
3 20

### Sample Output 1

Copy

110

In this sample, choose three gems from five gems. The chosen gems must have at least two distinct colors.

Choosing gems 2,3,5 gives colors 1,1,3, which are two distinct colors. Their total value is 40+50+20=110, and this is the maximum possible value.

---

### Sample Input 2

Copy

5 3 3
1 30
1 40
1 50
2 10
3 20

### Sample Output 2

Copy

80

The gems and the number to choose are the same as in Sample Input 1, but the chosen gems must have at least three distinct colors.

Choosing gems 3,4,5 gives colors 1,2,3, which are three distinct colors. Their total value is 50+10+20=80, and this is the maximum possible value.

---

### Sample Input 3

Copy

5 5 1
4 1000000000
5 1000000000
4 1000000000
5 1000000000
4 1000000000

### Sample Output 3

Copy

5000000000

Beware of overflow.

```java
import java.util.*;

public class Main {
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int k = sc.nextInt();
        int m = sc.nextInt();
        List<Long[]> list = new ArrayList<>();
        for(int i = 0; i < n; i++){
            Long[] arr = new Long[2];
            arr[0] = sc.nextLong();
            arr[1] = sc.nextLong();
            list.add(arr);
        }
        Collections.sort(list, (a, b) -> Long.compare(b[1], a[1]));
        
        HashSet<Long> set = new HashSet<>();
        List<Long> remainingPool = new ArrayList<>();
        Long sum = 0L;
        for(Long[] arr : list){
            if(!set.contains(arr[0]) && set.size() < m){
                set.add(arr[0]);
                sum += arr[1];
            } else {
                remainingPool.add(arr[1]);
            }
        }
        int remainingNeeded = k - m;
        for(int i = 0; i < remainingNeeded; i++){
            sum += remainingPool.get(i);
        }
        System.out.println(sum);
    }
}
```