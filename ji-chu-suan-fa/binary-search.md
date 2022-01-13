# Binary Search

### 不同的Search类型

* 在数组中找寻满足条件的唯一一个结果
  * 比条件小left = mid + 1
  * 比条件大right = mid - 1
  * 满足条件就return mid
* 在数组中找寻第一个满足条件的结果
  * 比条件小left = mid + 1
  * 比条件大right = mid - 1
  * 满足条件right = mid （继续往左看有没有更早满足条件的）
* 在数组中找寻最后一个满足条件的结果
  * 比条件小left = mid + 1
  * 比条件大right = mid - 1
  * 满足条件left = mid （继续往右看有没有更晚满足条件的）&#x20;

### 不同的算法模板

* 双指针左右交错退出， left <= right， 这种情况要么循环里找到了结果，要么就是return -1或者return题目要求的结果
  * 比较适合在数组中找寻满足条件的唯一一个结果的情况

```java
public int search(int[] nums, int target) {
    int low = 0;
    int high = nums.length - 1;
    while (low <= high){
        int mid = low + (high - low) / 2;
        if (nums[mid] == target){
            return mid;
        }else if (nums[mid] < target){
            low = mid + 1;
        }else{
            high = mid - 1;
        }
    }

    return -1;
}
```

```java
public int searchInsertPosition(int[] nums, int target) {
    int low = 0;
    int high = nums.length - 1;
    while (low <= high){
        int mid = low + (high - low) / 2;
        if (nums[mid] == target){
            return mid;
        }else if (nums[mid] < target){
            low = mid + 1;
        }else{
            high = mid - 1;
        }
    }

    return low;
}
```

* 双指针相遇退出，left < right，这种情况循环里找到的是一个candidate，需要额外check这个candidate是否满足条件。
  * 比较适合在数组中找寻第一个满足条件的结果
  * 也可以handle在数组中找唯一一个结果的情况
  * 对于在数组中找最后一个满足条件的scenario，当left = 0, right = 1时会导致死循环

```java
private int searchFirst(int[] nums, int target){
    int low = 0;
    int high = nums.length - 1;
    while (low < high){
        int mid = low + (high - low) / 2;
        if (nums[mid] == target){
            high = mid;
        }else if (nums[mid] < target){
            low = mid + 1;
        }else{
            high = mid - 1;
        }
    }

    // 这里需要check low，因为low代表了最后那个candidate
    // 而low = high - 1的时候，high最后可能比low小1，high就有可能是-1。
    // 如果没有不存在target的情况，那么就不需要做这个check，直接返回low。
    if (nums[low] == target){
        return low;
    }

    return -1;
}

```

* 双指针左节点面对面就退出。left + 1 < right。这种做法比较通用，不会死循环， 缺点是退出循环后找到的是两个candidate，所以要分别check这两个candidate哪个更满足条件。

```java
private int searchLast(int[] nums, int target){
    int low = 0;
    int high = nums.length - 1;
    while (low + 1 < high){
        int mid = low + (high - low) / 2;
        if (nums[mid] == target){
            low = mid;
        }
        else if (nums[mid] < target){
            low = mid + 1;
        }else{
            high = mid - 1;
        }
    }

    if (nums[high] == target){
        return high;
    }else if (nums[low] == target){
        return low;
    }else{
        return -1;
    }
}
```

### Leetcode题型

* 在排序数组中找唯一target
  * [35. Search Insert Position](https://leetcode.com/problems/search-insert-position)
  * [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted)
  * [374. Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower)
* 在数组中找第一个
  * [34. Find First and Last Position of Element in Sorted](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array)
  * [162. Find Peak Element](https://leetcode.com/problems/find-peak-element)
* 在数组中找最后一个
  * [34. Find First and Last Position of Element in Sorted](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array)
  * [275. H-Index II](https://leetcode.com/problems/h-index-ii)
* 其它变形题
  * 旋转数组
    * [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array) (找唯一target)
    * [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii)&#x20;
    * [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array) （找第一个满足条件的）
    * [154. Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii)
  * 2D数组
    * [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix)
    * [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii)
  * 倍乘法
    * 需要通过倍乘的方式先找到二分搜索范围，再进行二分搜索
      * [69. Sqrt(x)](https://leetcode.com/problems/sqrtx)
      * [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays)
