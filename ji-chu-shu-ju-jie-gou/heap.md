# Heap

* Heap是一棵root小于或者大于任何children的完整二叉树，通常基于array来实现，分为最大堆和最小堆。
* 插入一个元素和删除top都是o(logn), 初始化构建一个堆是O(n)， 对于每一个heap中的元素with index i，他的left child和right child分别对应2 \* i + 1. 2 \* i + 2， 反过来它的parent是 (i - 1) / 2;
* 下面是一个Heap的实现代码
  * 包括Heapify来初始化堆
  * 插入一个元素
  * 删除堆的顶部
* Heap通常有以下几个实用场景
  * 排序
    * Heap Sort
  * 计算top K
    * O(nlogk)
    * 把头k个元素放入堆里，然后维持堆里的元素是top k的，对于不满足的元素通过和堆顶比较就可以排除
  * median相关问题
    * 需要同时用到min-max heap，然后维持数量相同的大小heap，这样要么多一个元素的堆的顶是中位数，要么两个堆顶的元素相加除以2是中位数
* Heap相关的Leetcode
  * median相关问题
    * [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream)
    * [480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median)
  * 计算top K
    * [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists)
    * [373. Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums)
