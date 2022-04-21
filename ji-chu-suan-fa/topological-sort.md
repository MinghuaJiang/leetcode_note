# Topological Sort

### 基本思路

```
L = Empty list that will contain the sorted elements
S = Set of all nodes with no incoming edge

while S is non-empty do
    remove a node n from S
    add n to tail of L
    for each node m with an edge e from n to m do
        remove edge e from the graph
        if m has no other incoming edges then
            insert m into S

if graph has edges then
    return error   (graph has at least one cycle)
else 
    return L   (a topologically sorted order)
```

* 拓扑排序可以从起点开始，也可以反过来从终点开始，如果是起点开始，那么就需要构建indree数组，找Indgree是0的作为起始点，然后构建outdegree的dependency graph。如果是从终点反过来看，那么就需要构建outdegree数组找outdegree是0的点作为起始点，然后构建indegree方向的dependency graph。

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
        if (prerequisites.length == 0){
            return true;
        }

        int[] indgree = new int[numCourses];
        Map<Integer, Set<Integer>> graph = new HashMap<>();

        for (int[] relation : prerequisites) {
            indgree[relation[0]]++;
            graph.putIfAbsent(relation[1], new HashSet<>());
            graph.get(relation[1]).add(relation[0]);
        }

        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < indgree.length; i++) {
            if (indgree[i] == 0) {
                queue.offer(i);
            }
        }

        int count = 0;
        while (!queue.isEmpty()){
            int currentCourse = queue.poll();
            count++;
            if (graph.containsKey(currentCourse)) {
                for (int nextCourse : graph.get(currentCourse)) {
                    indgree[nextCourse]--;
                    if (indgree[nextCourse] == 0) {
                        queue.offer(nextCourse);
                    }
                }
            }
        }

        return count == numCourses;
    }
```

### 常见Leetcode题目

* [207. Course Schedule](https://leetcode.com/problems/course-schedule)
* [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii)
* [269. Alien Dictionary](https://leetcode.com/problems/alien-dictionary)
* [310. Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees)
* [329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix)
* [631. Design Excel Sum Formula](https://leetcode.com/problems/design-excel-sum-formula)
