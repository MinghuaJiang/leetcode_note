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

* 双指针左右交错退出， left <= right， 这种情况要么循环里找到了结果，要么就是return -1
  * 比较适合在数组中找寻满足条件的唯一一个结果的情况
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
    if (nums[low] == target){
        return low;
    }

    return -1;
}

```

* 双指针左节点面对面就退出。left < right - 1。这种做法比较通用，不会死循环， 缺点是退出循环后找到的是两个candidate，所以要分别check这两个candidate哪个更满足条件。

```java
private int searchLast(int[] nums, int target){
    int low = 0;
    int high = nums.length - 1;
    while (low < high - 1){
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
