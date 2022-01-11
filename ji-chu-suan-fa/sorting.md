# Sorting

Sorting Algorithm Comparison

| Sorting Algorithm | Time Complexity                                                                | Space Complexity                  | Stableness           |
| ----------------- | ------------------------------------------------------------------------------ | --------------------------------- | -------------------- |
| Bubble Sort       | <p>Best Case O(N)</p><p>Average Case O(N^2)</p><p>Worst Case O(N^2)</p>        | O(1)                              | Stable               |
| Insertion Sort    | <p>Best Case O(N)</p><p>Average Case O(N^2)</p><p>Worst Case O(N^2)</p>        | O(1)                              | Stable               |
| Selection Sort    | <p>Best Case O(N^2)</p><p>Average Case O(N^2)</p><p>Worst Case O(N^2)</p>      | O(1)                              | Instable (4 2 3 4 1) |
| Quick Sort        | <p>Best Case O(NLogN) </p><p>Average Case O(NLogN)</p><p>Worst Case O(N^2)</p> | Best Case O(logN) Worst Case O(N) | Instable             |
| Merge Sort        | <p>Best Case O(NLogN) </p><p>Average Case O(NLogN)</p><p>Worst Case O(N^2)</p> | O(N)                              | Stable               |
| Heap Sort         | <p>Best Case O(N)</p><p>Average Case O(NLogN)</p><p>Worst Case O(NLogN)</p>    | O(1)                              | Instable             |
| Counting Sort     | O(N + M)                                                                       | O(N + M)                          | Stable               |
| Bucket Sort       | <p>Best Case O(N)</p><p>Average Case O(N)</p><p>Worst Case O(NLOGN)</p>        | O(N)                              | Stable               |
| Radix Sort        | O(k(N + M))                                                                    | O(N+M)                            | Stable               |

### Bubble Sort

* No Optimization

```java
public static void sort(int[] array){
    for (int i = 0; i < array.length - 1; i++){
        for (int j = 0; j < array.length - i - 1; j++){
            if (array[j] > array[j + 1]) {
                int tmp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = tmp;
            }
        }
    }
}
```

* Optimization 1 for best case scenario early termination，已经有序了外循环就不继续下去了。

```java
public static void sort(int[] array){
    for (int i = 0; i < array.length - 1; i++){
        boolean isSorted = true;
        for (int j = 0; j < array.length - i - 1; j++){
            if (array[j] > array[j + 1]) {
                int tmp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = tmp;
                isSorted = false;
            }
        }

        if (isSorted){
            break;
        }
    }
}
```

* Optimization 2 on top of optimization 1，考虑实际有序区大于外层循环轮数。比如3,4,2,1,5,6,7,8。第一轮循环有序区的大小已经是5而不是1了。

```java
public static void sort3(int[] array){
    int sortBorder = array.length - 1;
    int lastChangeIndex = 0;
    for (int i = 0; i < array.length - 1; i++){
        boolean isSorted = true;
        for (int j = 0; j < sortBorder; j++){
            if (array[j] > array[j + 1]) {
                int tmp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = tmp;
                isSorted = false;
                lastChangeIndex = j;
            }
        }

        sortBorder = lastChangeIndex;

        if (isSorted){
            break;
        }
    }
}
```

### Insertion Sort

```java
public static void sort(int[] array){
    for (int i = 1; i < array.length; i++){
        int tmp = array[i];
        int currentIndex = i;
        // 从已排序区往右shift找到当前未排序元素的插入位置
        while (currentIndex > 0 && tmp < array[currentIndex - 1]){
            array[currentIndex] = array[currentIndex - 1];
            currentIndex--;
        }

        array[currentIndex] = tmp;
    }
}
```

### Selection Sort

```java
public static void sort(int[] array){
    for (int i = 0; i < array.length - 1; i++){
        int minIndex = i;
        // 找到未排序区最小的
        for (int j = i + 1; j < array.length; j++){
            if (array[j] < array[minIndex]){
                minIndex = j;
            }
        }

        // 和已排序区域外第一个元素互换
        if (i != minIndex){
            int tmp = array[i];
            array[i] = array[minIndex];
            array[minIndex] = tmp;
        }
    }
}
```

### Quick Sort

* 核心思想是Divide and conquer, 用一个pivot把比piviot大和比pivot小的元素分割成左右两拨，然后继续以pivot为分界点递归细分，如果pivot取得好，logn层递归，所以最优复杂度是O(nlogn)。
* Pivot可以是固定某一端的点，比如第一个或者最后一个点，一个优化是取random的点和某一个端的点互换再和原来的算法一样。
* 值得注意的是，Quick Sort有两种不同的partition算法。

#### Two Partition Algorithm

_Lomuto Partition （单方向Three-way partition)_

* 这个partition思路是单向快慢指针，通常把数组分隔成<=pivot， pivot和>pivot的三波（等号在哪边都一样）
* 做法是先把快指针遇到的属于左边partition的交换给慢指针，然后前进慢指针。这样就分出了以pivot为分界（慢指针位置）的左右两块，最后只要把pivi就把ot和这个慢指针交换就把数组分成了三波，此处partition method返回的是pivot的index。所以接下去左右递归分别是以pivot - 1和pivot + 1作为边界的。

```java
public static void sort(int[] array){
    quickSort(array, 0, array.length - 1);
}

private static void quickSort(int[] array, int startIndex, int endIndex){
    if (startIndex < endIndex) {
        int pivot = partition(array, startIndex, endIndex);
        quickSort(array, startIndex, pivot - 1);
        quickSort(array, pivot + 1, endIndex);
    }
}
```

* 取startIndex为pivot

```java
private static int partition(int[] array, int startIndex, int endIndex){
    int pivot = array[startIndex];
    int currentIndex = startIndex;
    // 扫描区间 (start + 1) -> end
    for (int i = startIndex + 1; i <= endIndex; i++){
        if (array[i] <= pivot){
            currentIndex++;
            swap(array, currentIndex, i);
        }
    }

    // 这里需要的是和较小区域的右边界点交换
    swap(array, startIndex, currentIndex);
    return currentIndex;
}
```

* 取endIndex为pivot

```java
private static int partition(int[] array, int startIndex, int endIndex){
    int pivot = array[endIndex];
    int currentIndex = startIndex - 1;
    // 扫描区间 start -> (end - 1)
    for (int i = startIndex; i <= endIndex - 1; i++){
        if (array[i] <= pivot){
            currentIndex++;
            swap(array, currentIndex, i);
        }
    }

    // 这里需要和较大区的左边界点交换
    swap(array, endIndex, currentIndex + 1);
    return currentIndex + 1;
}
```

```java
```

_Hoare's Partition （双方向Two way partition）_

* 这个partition思路用双方向双指针，通常把数组分成了左右两拨，一波\<pivot, 一波>=pivot，pivot不一定在分界点上。&#x20;
* 做法是左右两个指针始终保持在属于左右两拨数据的分界点，然后互相交换数据，partition method返回的是左侧分界点。因为是two-way partition。接下去的左右递归，分界点是相邻的，具体怎么分取决于pivot在哪边。

```java
private static void quickSort(int[] array, int startIndex, int endIndex){
    if (startIndex < endIndex) {
        int pivot = partition(array, startIndex, endIndex);
        //这里取决于partition返回左边界还是右边界以及pivot在左边还是右边。
        //返回左边界那么就是pivot和pivot + 1, 返回右边界，
        //就是pivot - 1和pivot
        quickSort(array, startIndex, pivot);
        quickSort(array, pivot + 1, endIndex);
    }
}

public static int partition(int[] array, int startIndex, int endIndex)
{
    int pivot = array[startIndex];
    int left = startIndex;
    int right = endIndex;

    while (left < right) {
        while (array[left] < pivot) {
            left++;
        }

        while (array[right] > pivot) {
            right--;
        }

        if (left < right) {
            swap(array, left, right);
            left++;
            right--;
        }
    }

    // right是左分区边界，left是右分区边界
    // 这里如果返回right，接下去的递归是pivot和pivot + 1
    // 如果返回left，接下去的递归是pivot - 1和pivot
    return right;
}
```

* Hoare's partition的two-way partition版本不可以用endIndex作为pivot。
* Hoare's partition其实也可以做成three-way。和two-way的区别就是一上来保证pivot不会被swap,最后再把pivot swap到分界点上。返回分界点位置。

```java
private static int partition(int[] array, int startIndex, int endIndex){
    int pivot = array[startIndex];
    int left = startIndex;
    int right = endIndex;
    while (left < right){
        while (left < right && array[right] > pivot){
            right--;
        }

        while (left < right && array[left] <= pivot){
            left++;
        }

        if (left < right){
            swap(array, left, right);
        }

        array[startIndex] = array[left];
        array[left] = pivot;

        return left;
    }
}
```

#### Hoare's Partition vs Lomuto Partition

* 他们的时间复杂度是一样的，最大的区别在于Hoare's partition需要更少的swap operation。因为Hoare's是把左边换到最右边的指针位置，而Lomuto只是换到当前快指针位置，所以会做很多不必要的交换，极端的情况，如果左右本来就是以pivot大小分的，Hoare's partition一次交换都不需要，而Lomuto partition还是需要自己和自己交换。
* 对于等于pivot的值，Lomuto partition是要么放在pivot的左边要么放在pivot的右边。而对于Hoare's two-way partition来说，两边是都可以有的。

#### Quick Select

* 这个算法是基于quicksort partition的思想，解决类似计算top k类问题的一种O(N)的解法。每次通过去某一边的partition而不是两边都去来做到O(N)的复杂度，感兴趣的读者可以自行证明。
* The kth largest element in an array

```java
public int findKthLargest(int[] nums, int k) {
    return this.quickSelect(nums, 0, nums.length - 1,  nums.length + 1 - k);
}

private int quickSelect(int[] array, int startIndex, int endIndex, int k){
    if (startIndex == endIndex){
        return array[startIndex];
    }

    int pivotIndex = this.partition(array, startIndex, endIndex);

    if (pivotIndex - startIndex + 1 == k){
        return array[pivotIndex];
    }

    if (pivotIndex - startIndex + 1 < k){
        return quickSelect(array, pivotIndex + 1, endIndex, k - 1 + startIndex - pivotIndex);
    }else{
        return quickSelect(array, startIndex, pivotIndex - 1, k);
    }
}

private int partition(int[] array, int startIndex, int endIndex){
    int pivot = array[startIndex];
    int currentIndex = startIndex;
    for (int i = startIndex + 1; i <= endIndex; i++){
        if (array[i] <= pivot){
            currentIndex++;
            swap(array, currentIndex, i);
        }
    }

    swap(array, startIndex, currentIndex);
    return currentIndex;
}


private void swap(int[] array, int i, int j){
    int tmp = array[i];
    array[i] = array[j];
    array[j] = tmp;
}
```

### Merge Sort

* 也是一种基于divide and conquer的排序算法，区别在于quick sort类似于pre-order traverse的递归。而merge sort类似于post-order traverse的递归

```java
public static void sort(int[] array){
    mergeSort(array, 0, array.length - 1);
}

private static void mergeSort(int[] array, int start, int end){
    if (start < end){
        int mid = (start + end) / 2;
        mergeSort(array, start, mid);
        mergeSort(array, mid + 1, end);
        merge(array, start, mid, end);
    }
}

private static void merge(int[] array, int start, int mid, int end){
    int[] tempArray = new int[end - start + 1];
    int p1 = start;
    int p2 = mid + 1;
    int currentIndex = 0;
    while (p1 <= mid && p2 <= end){
        if (array[p1] < array[p2]){
            tempArray[currentIndex++] = array[p1++];
        }else{
            tempArray[currentIndex++] = array[p2++];
        }
    }

    while (p1 <= mid){
        tempArray[currentIndex++] = array[p1++];
    }

    while (p2 <= end){
        tempArray[currentIndex++] = array[p2++];
    }

    for (int i = 0; i < tempArray.length; i++){
        array[i + start] = tempArray[i];
    }
}
```

### Heap Sort

* 这个算法的思路是利用Heap的特性，通过循环"删除"heap的顶，并把被删除的元素留在当前heap有效大小的末尾，来达到排序效果。如果是从小到大排序就构建一个maxHeap，反之则需要minHeap。
* 利用maxHeap做从小到大排序的Code：

```java
public static void sort(int[] array){
    heapify(array);
    for (int i = array.length - 1; i > 0; i--){
        removeTop(array, i);
    }
}

private static void heapify(int[] array){
    for (int i = (array.length - 2) / 2; i >= 0; i--){
        siftDown(array, i, array.length);
    }
}

private static void removeTop(int[] array, int lastElementIndex){
    swap(array, 0, lastElementIndex);
    siftDown(array, 0, lastElementIndex);
}

private static void swap(int[] array, int i, int j){
    int tmp = array[i];
    array[i] = array[j];
    array[j] = tmp;
}

private static void siftDown(int[] array, int currentIndex, int heapSize){
    int temp = array[currentIndex];
    int childIndex = 2 * currentIndex + 1;
    while (childIndex < heapSize){
        //选左右子节点大的那个
        if ((childIndex + 1) < heapSize && array[childIndex + 1] > array[childIndex]){
            childIndex++;
        }

        if (temp >= array[childIndex]){
            break;
        }
        //类似insertion sort那样做shift
        array[currentIndex] = array[childIndex];
        currentIndex = childIndex;
        childIndex = 2 * childIndex + 1;
    }

    array[currentIndex] = temp;
}
```

### Counting Sort

* 这个算法的常见基础非stable实现是基于用一堆数字的最大值和最小值的区间构建一个bucket array, 然后根据每个值和最小值的offset来决定丢到哪个bucket里面，最后再循环bucket来排序原数组

```java
public static void sort(int[] array){
    int min = array[0];
    int max = array[0];
    // 获得最大最小值
    for (int i = 1; i < array.length;i++){
        min = Math.min(min, array[i]);
        max = Math.max(max, array[i]);
    }

    // 把每个数字放进对应的bucket, count加1
    int[] buckets = new int[max - min + 1];
    for (int i = 0; i < array.length;i++){
        buckets[array[i] - min]++;
    }

    // 从头遍历bucket array填充结果
    int index = 0;
    for(int i = 0;i < buckets.length;i++){
        for(int j = 0; j < buckets[i]; j++){
            array[index++] = min + i;
        }
    }
}
```

* 上面这版本的时间复杂度是O(N+M), 空间复杂度是O(M)
* Counting Sort也可以做成stable的版本，stable的版本可以看成是Bucket Sort每个bucket区间是固定值的特例。一种做法是先用bucket数组变形。变形的思路是算出从头到当前bucket位置的range sum。然后从右往左遍历原始数组的copy，bucket上的数字对应原始数组的index。如果让了一个值就在bucket上减一，这样就保证排在前那个相同的数字从bucket数字look up到的index会小一，从而保证了相同的先后顺序。

```java
public static void sort(int[] array){
    int min = array[0];
    int max = array[0];
    // 获得最大最小值
    for (int i = 1; i < array.length;i++){
        min = Math.min(min, array[i]);
        max = Math.max(max, array[i]);
    }

    // 把每个数字放进对应的bucket, count加1
    int[] buckets = new int[max - min + 1];
    for (int i = 0; i < array.length;i++){
        buckets[array[i] - min]++;
    }

    // 变形为prefix sum array
    for (int i = 1; i < buckets.length; i++){
        buckets[i] += buckets[i - 1];
    }

    int[] copiedArray = new int[array.length];
    for (int i = 0; i < array.length;i++){
        copiedArray[i] = array[i];
    }

    // 倒序遍历数组，输出结果
    for (int i = array.length - 1; i >= 0; i--){
        array[buckets[array[i] - min] - 1] = copiedArray[i];
        buckets[array[i] - min]--;
    }
}
```

* 上面这个做法的时间和空间复杂度是O(N + M)
* 另外一个做法是可以在每个bucket分配一个LinkedList或者Queue。然后最后还是按照bucket从左到右输出，这样时间和空间复杂度也是O(N + M)， Leetcode里把这种做法归为Bucket Sort。
* Counting Sort的主要限制是不支持非整数数组，第二是如果最大值和最小值差大多，空间和时间复杂度也会很大

### Bucket Sort

* Bucket Sort可以有不同策略来决定桶的数量，然后每个桶是一个区间，桶的区间大小 = （最大值 - 最小值）/ (桶的数量 - 1）
* 对于桶的数量，一种简单的方法是等于原来数组的大小

```java
public static void sort(double[] array){
    // 算出max和min值
    double min = array[0];
    double max = array[0];

    for (int i = 1; i < array.length;i++){
        min = Math.min(min, array[i]);
        max = Math.max(max, array[i]);
    }

    // 初始化桶
    int bucketNum = array.length;
    // 这一部分需要double，用int的话就不能先确定bucketNum
    double bucketSize = (double)(max - min) / (bucketNum - 1);
    List<List<Double>> buckets = new ArrayList<List<Double>>(bucketNum);
    for (int i = 0;i < bucketNum;i++){
        buckets.add(new LinkedList<Double>());
    }

    // 把元素放入bucket
    for (int i = 0; i < array.length; i++){
        int currentIndex = (int)((array[i] - min) / bucketSize);
        buckets.get(currentIndex).add(array[i]);
    }

    // 输出所有桶的结果
    int index = 0;
    for (List<Double> bucket : buckets){
        //每个桶内部排序
        Collections.sort(bucket);
        // 输出结果
        for(double element : bucket){
            array[index++] = element;
        }
    }
}
```

* 对于桶排序来说，如果每个桶的元素分配比较均匀，那么就可以达到O(N),不然如果有个桶元素特别多的话就会退化到O(NLogN)

### Radix Sort

* 这个算法可以针对排序手机号或者英语单词这样的scenario。
* 分多轮排序，每一轮按照某一位进行stable的counting Sort
  * 高位优先排序（MSD）
  * 低位优先排序（LSD）
* 如果长度不一样，以最长的字符串为准，不足的地方补0。
* LSD的代码例子如下

```java
public static void radixSort(String[] array){
    int maxLength = 0;
    for(int i = 0; i < array.length;i++){
        maxLength = Math.max(maxLength, array[i].length());
    }

    String[] sortedArray = new String[array.length];
    for (int k = maxLength - 1; k >= 0; k--){
        //从最低位开始做counting sort
        int[] count = new int[128];
        for (int i = 0; i < array.length;i++){
            // 长度不一致的按0看
            int index = array[i].length() < (k + 1) ? 0 : array[i].charAt(k);
            count[index]++;
        }

        for (int i = 1; i < count.length; i++){
            count[i] = count[i] + count[i - 1];
        }

        // 这里上一轮的顺序会影响到这一轮相同bucket的先后顺序
        for (int i = array.length - 1; i >= 0; i--){
            int index = array[i].length() < (k + 1) ? 0 : array[i].charAt(k);
            int sortedIndex = count[index] - 1;
            sortedArray[sortedIndex] = array[i];
            count[index]--;
        }

        // 更新Output，作为下一轮相同bucket情况下先后顺序的参照
        for (int i = 0; i < array.length; i++){
            array[i] = sortedArray[i];
        }
    }
}
```

### Comparison based sorting相关Leetcode题目

* Merge Sort
  * [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists)
  * [23. Merge K Sorted List](https://leetcode.com/problems/merge-k-sorted-lists/)
  * [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array)
  * [148. Sort List](https://leetcode.com/problems/sort-list)
* Quick Sort
  * [912. Sort an Array](https://leetcode.com/problems/sort-an-array)
* Insertion Sort
  * [147. Insertion Sort List](https://leetcode.com/problems/insertion-sort-list)
    * 这道题比较有意思, 因为insertion sort基于array是从右往左查看insertion position的，但是singly linked list是单向的，所以每次需要从左往右寻找插入点，然后创建一个dummy node in case这个点要插在head前面。
    * 这里通过快慢两个指针来分别做循环和寻找insert position

```java
public ListNode insertionSortList(ListNode head) {
    ListNode dummy = new ListNode(-1);
    ListNode curr = head;

    while (curr != null){
        ListNode prev = dummy;
        // 寻找插入点
        while (prev.next != null && prev.next.val < curr.val){
            prev = prev.next;
        }

        // 插入元素
        ListNode next = curr.next;
        curr.next = prev.next;
        prev.next = curr;
        curr = next;     
    }
    
    return dummy.next;
}
```

Non-comparison based sorting相关Leetcode题目

* Counting Sort
  * [274. H-Index](https://leetcode.com/problems/h-index)
  * [1051. Height Checker](https://leetcode.com/problems/height-checker)
  * [1122. Relative Sort Array](https://leetcode.com/problems/relative-sort-array)
* Bucket Sort
  * [164. Maximum Gap](https://leetcode.com/problems/maximum-gap)
  * [220. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-iii)I
  * [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency)
  * [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words)
* Radix Sort
  * [164. Maximum Gap](https://leetcode.com/problems/maximum-gap)

### Partition相关Leetcode题目

* [75. Sort Colors](https://leetcode.com/problems/sort-colors)
* [283. Move Zeroes](https://leetcode.com/problems/move-zeroes)

### QuickSelect相关Leetcode题目

* [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array)
* [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements)
* [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin)

###





