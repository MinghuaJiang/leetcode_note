# Sorting

### Sorting Algorithm Comparison

| Sorting Algorithm | Time Complexity                                 | Space Complexity                  | Stableness           |
| ----------------- | ----------------------------------------------- | --------------------------------- | -------------------- |
| Bubble Sort       | <p>Best Case O(N)</p><p>Worst Case O(N^2)</p>   | O(1)                              | Stable               |
| Insertion Sort    | <p>Best Case O(N)</p><p>Worst Case O(N^2)</p>   | O(1)                              | Stable               |
| Selection Sort    | <p>Best Case O(N^2)</p><p>Worst Case O(N^2)</p> | O(1)                              | Instable (4 2 3 4 1) |
| Quick Sort        | Best Case O(NLogN) Worst Case O(N^2)            | Best Case O(logN) Worst Case O(N) | Instable             |
|                   |                                                 |                                   |                      |
|                   |                                                 |                                   |                      |
|                   |                                                 |                                   |                      |
|                   |                                                 |                                   |                      |
|                   |                                                 |                                   |                      |

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
        int tmp = array[i];
        array[i] = array[minIndex];
        array[minIndex] = tmp;
    }
}
```

### Quick Sort

* 核心思想是Divide and conquer, 用一个pivot把比piviot大和比pivot小的元素分割成左右两拨，其本质就是实现一个three way partition方法，然后继续以pivot为分界点递归细分，如果pivot取得好，logn层递归，所以最优复杂度是O(nlogn)。
* Pivot可以是固定某一端的点，比如第一个或者最后一个点，一个优化是取random的点和某一个端的点互换再和原来的算法一样。
* 值得注意的是，Quick Sort有两种不同的partition算法。

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

#### Two Partition Algorithm

* Lomuto Partition（单方向循环）
* Hoare's Partition（双方向循环）



#### Quick Select

### Non-linear time sorting相关Leetcode题目

* Merge Sort
* Quick Sort
* Heap Sort

### Linear time sorting相关Leetcode题目

* Counting Sort
* Bucket Sort
* Radix Sort

###





