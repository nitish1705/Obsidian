
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must**take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**

**Input:** numCourses = 2, prerequisites = [[1,0]]
**Output:** true
**Explanation:** There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.

**Example 2:**

**Input:** numCourses = 2, prerequisites = [[1,0],[0,1]]
**Output:** false
**Explanation:** There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

**Constraints:**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- All the pairs prerequisites[i] are **unique**.


```java
class Solution {

    private boolean dfs(HashMap<Integer, List<Integer>> map, HashMap<Integer, Integer> state, int ind){
        if(state.get(ind) == null) return true;

        if(state.get(ind) != 0){
            return state.get(ind) == 2;
        }

        state.put(ind, 1);

        for(int num : map.get(ind)){
            if(!dfs(map, state, num)){
                return false;
            }
        }

        state.put(ind, 2);
        return true;

    }
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        HashMap<Integer, List<Integer>> map = new HashMap<>();
        HashMap<Integer, Integer> state = new HashMap<>();
        for (int i = 0; i < prerequisites.length; i++) {
            map.putIfAbsent(prerequisites[i][0], new ArrayList<>());
            map.get(prerequisites[i][0]).add(prerequisites[i][1]);
            state.putIfAbsent(prerequisites[i][0], 0);
        }
        
        int count = 0;
        for(int key : map.keySet()){
            if(!dfs(map, state, key)){
                return false;
            }
        }
        return true;
    }
}
```