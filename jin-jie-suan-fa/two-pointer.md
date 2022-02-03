# Two Pointer

### 基本思路

* 这个技巧常运用在数组，LinkedList中，主要分为单向快慢指针和双方向双指针, LinkedList考题一般是Singly Linked List，所以通常只能用快慢指针，数组题两个技巧都能用。还有一种同向双指针，这种通常是针对input有两个数组或者LinkedList的。
* 快慢指针有两个比较特别的应用在Leetcode里比较常见
  * Floyd's Cycle Detection
    * 思路是利用快慢指针龟兔赛跑，如果有环，快指针总能追上慢指针
    * 如果要定位cycle的交叉点，那么做法是找到快慢指针的相遇点，把其中一个指针放到起点，然后两个指针同时从相遇点和起点一起往前走，那么下一个相遇的点就是这个交叉点。
  * Sliding Window
    * 这一类题属于双指针和hash的结合

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
  * [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array)
  * [27. Remove Element](https://leetcode.com/problems/remove-element)
  * [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii)
  * [189. Rotate Array](https://leetcode.com/problems/rotate-array)
  * [344. Reverse String](https://leetcode.com/problems/reverse-string)
  * [541. Reverse String II](https://leetcode.com/problems/reverse-string-ii)
* Floyd's Cycle Detection
  * [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle)
  * [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii)
  * [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number)
* Sliding Window
  * [3. Longest Substring Without Repeating Characte](https://leetcode.com/problems/longest-substring-without-repeating-characters)r
  * [30. Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words)
  * [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring)
  * [159. Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters)
  * [187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences)
  * [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum)
  * [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum)
  * [340. Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters)
  * [395. Longest Substring with At Least K Repeating](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters)
  * [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement)
  * [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string)
  * [480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median)
  * [487. Max Consecutive Ones II](https://leetcode.com/problems/max-consecutive-ones-ii)
  * [718. Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray)
  * [727. Minimum Window Subsequence](https://leetcode.com/problems/minimum-window-subsequence)
  * [862. Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k)
  * [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii)
  * [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring)

#### 双方向双指针

* [15. 3Sum](https://leetcode.com/problems/3sum)
* [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest)
* [18. 4Sum](https://leetcode.com/problems/4sum)
* [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted)
* [259. 3Sum Smaller](https://leetcode.com/problems/3sum-smaller)
* [1099. Two Sum Less Than K](https://leetcode.com/problems/two-sum-less-than-k)

#### 同向双指针

* [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers)
* [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists)
* [86. Partition List](https://leetcode.com/problems/partition-list)
* [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array)
* [160. Intersection of Two Linked List](https://leetcode.com/problems/intersection-of-two-linked-lists)
* [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list)
* [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays)
* [350. Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii)
* [445. Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii)



