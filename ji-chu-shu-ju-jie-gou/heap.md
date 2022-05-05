# Heap

* Heap是一棵root小于或者大于任何children的完整二叉树，通常基于array来实现，分为最大堆和最小堆。
* 插入一个元素和删除top都是o(logn), 初始化构建一个堆是O(n)， 对于每一个heap中的元素with index i，他的left child和right child分别对应2 \* i + 1. 2 \* i + 2， 反过来它的parent是 (i - 1) / 2;
* 下面是一个Heap的实现代码
  * Heapify来初始化堆
  * 插入一个元素
  * 删除堆的顶部

```java
public class Heap {
    private int[] array;
    private int size;
    public Heap(int[] input){
        this.array = new int[input.length * 2];
        for (int i = 0;i < input.length; i++){
            this.array[i] = input[i];
        }

        this.size = input.length;
        this.heapify();
    }

    // 从最后一个node的parent开始做siftDown
    private void heapify(){
        for (int i = (size - 2) / 2; i >= 0; i--){
            this.siftDown(i);
        }
    }

    // 每次插入一个元素，需要做siftUp
    public void insert(int val){
        this.array[size] = val;
        this.siftUp(size);
        this.size++;
    }

    // 每次删除一个元素，把顶部元素和最后一个元素做交换，删除最后一个元素，然后把顶部元素做
    // siftDown
    public int removeMin(){
        int result = this.array[0];
        this.array[0] = this.array[size - 1];
        this.array[size - 1] = 0;
        this.size--;
        this.siftDown(0);
        return result;
    }

    public int getSize(){
        return this.size;
    }

    // 这里不需要一个一个交换，而是可以shift
    private void siftUp(int index){
        int temp = this.array[index];
        int parent = (index - 1) / 2;
        while (parent >= 0){
            if (array[parent] <= array[index]){
                break;
            }

            array[index] = array[parent];
            index = parent;
            parent = (index - 1) / 2;
        }

        array[index] = temp;
    }

    // 这里不需要一个一个交换，而是可以shift
    private void siftDown(int index){
        int temp = this.array[index];
        int childIndex = 2 * index + 1;
        while (childIndex < this.size){
            if (childIndex + 1 < this.size && array[childIndex + 1] < array[childIndex]){
                childIndex++;
            }

            if (temp <= array[childIndex]){
                break;
            }

            this.array[index] = array[childIndex];
            index = childIndex;
            childIndex = 2 * index + 1;
        }

        this.array[index] = temp;
    }
}
```

* Java里的Heap实现是PriorityQueue, 如果需要customize comparator的场景，那么就不能用到heapify，而需要一个一个添加元素。
* Heap通常有以下几个实用场景
  * 排序
    * Heap Sort
      * Sort相关的题一般倒是不需要去特意去用Heap, 因为直接用built-in的sort就好了。
  * 计算top K
    * O(nlogk)
    * 可以每个元素都往堆里丢，然后一旦堆大小大于k,就remove top，这样每一个元素都要至少add一次
    * ```
      for (int n: ...) {
         pq.offer(n);
         if (pq.size() > k){
             pq.poll();
         }
      }
      ```
    * 也可以把头k个元素放入堆里，然后维持堆里的元素是top k的，对于不满足的元素通过和堆顶比较就可以排除
    * ```
      for (int n: ...) {
          if (pq.size() < k) {
              pq.offer(n);
          }else if (n > pq.peek()) {
              pq.offer(n);
              pq.poll();
          }
      }
      ```
  * median相关问题
    * 需要同时用到min-max heap，然后维持数量相同的大小heap，这样要么多一个元素的堆的顶是中位数，要么两个堆顶的元素相加除以2是中位数
  * Task Scheduling
* Heap相关的Leetcode
  * median相关问题
    * [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream)
    * [480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median)
  * 计算top K
    * [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists)
    * [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements)
    * [358. Rearrange String k Distance Apart](https://leetcode.com/problems/rearrange-string-k-distance-apart)
    * [373. Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums)
    * [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix)
    * [658. Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements)
    * [719. Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance)
    * [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin)
  * Task Scheduling
    * [743. Network Delay Time](https://leetcode.com/problems/network-delay-time)
    * [759. Employee Free Time](https://leetcode.com/problems/employee-free-time)
    * [882. Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers)
    * [1834. Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu)
    *
