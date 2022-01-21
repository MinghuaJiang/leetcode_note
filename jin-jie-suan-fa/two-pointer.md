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
  * [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs)
  * [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group)
  * [61. Rotate List](https://leetcode.com/problems/rotate-list)
  * [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii)
  * [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii)
  * [143. Reorder List](https://leetcode.com/problems/reorder-list)
  * [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements)
  * [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list)
  * LinkedList会有不少题需要reverse list，这里有两种情况，一种是reverse整个list，一种是reverse部分。其实可以把两种情况都看成是reverse一部分直到快指针遇到了一个target node，核心思想是把快指针指向慢指针，然后快指针一上来是head, 慢指针一上来就是那个target node。

```java
private ListNode reverseList(ListNode head, ListNode target){
    ListNode prev = target;
    ListNode curr = head;
    while (curr != target){
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }

    return prev;
}
```

```java
public ListNode reverseKGroup(ListNode head, int k) {
    ListNode dummy = new ListNode(-1, head);
    ListNode prev = dummy;
    ListNode curr = head;
    int count = 1;

    while (curr != null){
        ListNode next = curr.next;
        if (count % k == 0){
            ListNode prevNext = prev.next;
            prev.next = this.reverseList(prev.next, next);
            prev = prevNext;
        }

        curr = next;
        count++;
    }

    return dummy.next;
}
```

```java
public ListNode reverseList(ListNode head) {
    return this.reverseList(head, null);
}
```

* Array
  * [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii)
* Floyd's Cycle Detection
  * [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle)
  * [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii)
  * [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number)
* Sliding Window

```java
```

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

