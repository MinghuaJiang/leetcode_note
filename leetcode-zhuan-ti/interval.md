# Interval

### 基本思路

* Sorting相关
  * 基本上基于interval的开始区间排序，如果开始会有重复的，根据情况决定。
* 扫描线相关
  * 扫描线是基于把所有和时间相关的排序在同一个时间轴上，然后垂直扫过去看每个时间点有多少同时发生的事情
* Binary Search
  * 如果已经sort过，要寻找插入position，那么这种题就是Binary Search，即使没有sort过，如果需要在数组中比较两个元素的，那么先sort再binary search也能从o(n^2) 降到O(nlogn)同时也做了search上的优化。

### Leetcode相关题

* Sorting相关
  * [56. Merge Intervals](https://leetcode.com/problems/merge-intervals)
  * [57. Insert Interval](https://leetcode.com/problems/insert-interval)
  * [252. Meeting Rooms](https://leetcode.com/problems/meeting-rooms)
  * [1272. Remove Interval](https://leetcode.com/problems/remove-interval)
  * [1288. Remove Covered Intervals](https://leetcode.com/problems/remove-covered-intervals)
* 扫描线相关
  * [253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii)
* Binary Search
  * [436. Find Right Interval](https://leetcode.com/problems/find-right-interval)
