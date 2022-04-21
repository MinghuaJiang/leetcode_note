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

### 常见Leetcode题目

* [207. Course Schedule](https://leetcode.com/problems/course-schedule)
* [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii)
* [269. Alien Dictionary](https://leetcode.com/problems/alien-dictionary)
* [310. Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees)
* [329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix)
* [631. Design Excel Sum Formula](https://leetcode.com/problems/design-excel-sum-formula)
