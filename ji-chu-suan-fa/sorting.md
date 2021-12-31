# Sorting

### Sorting Algorithm Comparison

| Sorting Algorithm | Time Complexity                               | Stableness | Comment |
| ----------------- | --------------------------------------------- | ---------- | ------- |
| Bubble Sort       | <p>Best Case O(N)</p><p>Worst Case O(N^2)</p> | Stable     |         |
|                   |                                               |            |         |
|                   |                                               |            |         |
|                   |                                               |            |         |
|                   |                                               |            |         |

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

* Optimization 1 for best case scenario early termination

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

* Optimization 2 on top of optimization 1

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

###

### Quick Sort

#### Two Partition Algorithm

* Hoare's Partition
* Lomuto Partition

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





