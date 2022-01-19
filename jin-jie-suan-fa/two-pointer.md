# Two Pointer

### 基本思路

* 这个技巧常运用在数组，LinkedList中，主要分为单向快慢指针和双方向双指针, LinkedList考题一般是Singly Linked List，所以通常只能用快慢指针，数组题两个技巧都能用。还有一种同向双指针，这种通常是针对input有两个数组或者LinkedList的。
* 快慢指针有两个比较特别的应用在Leetcode里比较常见
  * Floyd's Cycle Detection
    * 思路是利用快慢指针龟兔赛跑，如果有环，快指针总能追上慢指针
    * 如果要定位cycle的交叉点，那么做法是找到快慢指针的相遇点，把其中一个指针放到起点，然后两个指针同时从相遇点和起点一起往前走，那么下一个相遇的点就是这个交叉点。
  * Sliding Window

### 常见Leetcode题型

#### 快慢指针

* LinkedList
  * [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list)
  * [61. Rotate List](https://leetcode.com/problems/rotate-list)
  * [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii)
  * [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii)
  * [143. Reorder List](https://leetcode.com/problems/reorder-list)
  * [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements)
  * [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list)

{% code title="RotateList.java" %}
```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null){
        return null;
    }

    ListNode fast = head;
    int length = 0;
    while (fast != null) {
        fast = fast.next;
        length++;
    }

    k = k % length;

    fast = head;
    while (fast != null && k > 0){
        fast = fast.next;
        k--;
    }

    ListNode slow = head;
    while (fast != null && fast.next != null){
        fast = fast.next;
        slow = slow.next;
    }

    fast.next = head;
    ListNode newHead = slow.next;
    slow.next = null;

    return newHead;
}
```
{% endcode %}

* Array
  * [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii)
* Floyd's Cycle Detection
  * [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle)
  * [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii)
  * [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number)

#### 双方向双指针

* [15. 3Sum](https://leetcode.com/problems/3sum)
* [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest)
* [18. 4Sum](https://leetcode.com/problems/4sum)
* [259. 3Sum Smaller](https://leetcode.com/problems/3sum-smaller)

#### 同向双指针

* [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers)
* [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists)
* [86. Partition List](https://leetcode.com/problems/partition-list)
* [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array)
* [160. Intersection of Two Linked List](https://leetcode.com/problems/intersection-of-two-linked-lists)
* [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list)
* [445. Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii)

