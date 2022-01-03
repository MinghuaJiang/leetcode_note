# Sorting

### Sorting Algorithm Comparison

| Sorting Algorithm | Time Complexity                                    | Space Complexity                  | Stableness           |
| ----------------- | -------------------------------------------------- | --------------------------------- | -------------------- |
| Bubble Sort       | <p>Best Case O(N)</p><p>Worst Case O(N^2)</p>      | O(1)                              | Stable               |
| Insertion Sort    | <p>Best Case O(N)</p><p>Worst Case O(N^2)</p>      | O(1)                              | Stable               |
| Selection Sort    | <p>Best Case O(N^2)</p><p>Worst Case O(N^2)</p>    | O(1)                              | Instable (4 2 3 4 1) |
| Quick Sort        | <p>Best Case O(NLogN) </p><p>Worst Case O(N^2)</p> | Best Case O(logN) Worst Case O(N) | Instable             |
| Merge Sort        | <p>Best Case O(NLogN) </p><p>Worst Case O(N^2)</p> | O(N)                              | Stable               |
| Heap Sort         | <p>Best Case O(N)</p><p>Worst Case O(NLogN)</p>    |                                   | Instable             |
| Counting Sort     |                                                    |                                   |                      |
| Bucket Sort       |                                                    |                                   |                      |
| Radix Sort        |                                                    |                                   |                      |
|                   |                                                    |                                   |                      |

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

_Lomuto Partition （Three-way partition)_

* 这个partition思路是把数组分隔成<=pivot， pivot和>pivot的三波（等号在哪边都一样），partition method返回的是pivot的index。所以接下去左右递归分别是以pivot - 1和pivot + 1作为边界的。

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

* _另外Lomuto也可以做成左右双方向循环，没有单方向这么简单直白。_

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

_Hoare's Partition （Two way partition）_

* 这个partition思路是把四种分成了左右两拨，一波\<pivot, 一波>=pivot, partition method返回的是左侧分界点。所以接下去的左右递归，分界点是相邻的。

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

        swap(array, left, right);
        left++;
        right--;
    }

    return right;
}
```

#### Quick Select

这个算法是基于quicksort partition的思想，解决类似计算top k类问题的一种O(N)的解法。每次通过去某一边的partition而不是两边都去来做到O(N)的复杂度，感兴趣的读者可以自行证明。

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

```
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

### Non-linear time sorting相关Leetcode题目

* Merge Sort
* Quick Sort
* Heap Sort

### Linear time sorting相关Leetcode题目

* Counting Sort
* Bucket Sort
* Radix Sort

###





