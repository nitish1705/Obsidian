

[802. Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

There is a directed graph of `n` nodes with each node labeled from `0` to `n - 1`. The graph is represented by a **0-indexed** 2D integer array `graph` where `graph[i]` is an integer array of nodes adjacent to node `i`, meaning there is an edge from node `i` to each node in `graph[i]`.

A node is a **terminal node** if there are no outgoing edges. A node is a **safe node** if every possible path starting from that node leads to a **terminal node** (or another safe node).

Return _an array containing all the **safe nodes** of the graph_. The answer should be sorted in **ascending** order.

**Example 1:**

![Illustration of graph](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)

**Input:** graph = [[1,2],[2,3],[5],[0],[5],[],[]]
**Output:** [2,4,5,6]
**Explanation:** The given graph is shown above.
Nodes 5 and 6 are terminal nodes as there are no outgoing edges from either of them.
Every path starting at nodes 2, 4, 5, and 6 all lead to either node 5 or 6.

**Example 2:**

**Input:** graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
**Output:** [4]
**Explanation:**
Only node 4 is a terminal node, and every path starting at node 4 leads to node 4.

**Constraints:**

- `n == graph.length`
- `1 <= n <= 104`
- `0 <= graph[i].length <= n`
- `0 <= graph[i][j] <= n - 1`
- `graph[i]` is sorted in a strictly increasing order.
- The graph may contain self-loops.
- The number of edges in the graph will be in the range `[1, 4 * 104]`.

```java
import java.util.*;

public class Main{
    private static boolean dfs(int ind, int[] state, int[][] graph){
        if(state[ind] == 1) {
            return false;
        }
        
        if(state[ind] == 2) return true;
        state[ind] = 1;

        for(int next : graph[ind]){
            if(!dfs(next, state, graph)){
                return false;
            }
        }

        state[ind] = 2;
        return true;

    }
    public static List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        int[] state = new int[n];
        for(int i = 0; i < n; i++){
            if(graph[i].length == 0){
                state[i] = 2;
            }
        }
        List<Integer> res = new ArrayList<>();
        for(int i = 0; i < n; i++){
            if(state[i] == 2){
                res.add(i);
            }
            else{
                if(dfs(i, state, graph)){
                    res.add(i);
                }
            }
        }
        return res;
    }
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int[][] graph = {{1,2}, {2,3}, {5}, {0}, {5}, {}, {}};
        List<Integer> temp = eventualSafeNodes(graph);
        System.out.println(temp);
        sc.close();
    }
}
```




```java
class Solution {

    private boolean dfs(int ind, int[] state, List<List<Integer>> adj){
        if(state[ind] != 0){
            return state[ind] == 2;
        }

        state[ind] = 1;

        for(int next : adj.get(ind)){
            if(!dfs(next, state, adj)){
                return false;
            }
        }

        state[ind] = 2;
        return true;
    }
    public boolean isCyclic(int N, List<List<Integer>> adj) {
        int[] state = new int[N];

        for(int i = 0; i < N; i++){
            if(!dfs(i, state, adj)){
                return true;
            }
        }
        
        return false;
    }
}

```