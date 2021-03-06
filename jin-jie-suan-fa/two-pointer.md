# Two Pointer

### 基本思路

* 这个技巧常运用在数组，LinkedList中，主要分为单向快慢指针和反向面对面双指针, LinkedList考题一般是Singly Linked List，所以通常只能用快慢指针，数组题两个技巧都能用。还有一种同向双指针，这种通常是针对input有两个数组或者LinkedList的。
* 快慢指针有两个比较特别的应用在Leetcode里比较常见
  * Floyd's Cycle Detection
    * 思路是利用快慢指针龟兔赛跑，如果有环，快指针总能追上慢指针
    * 如果要定位cycle的交叉点，那么做法是找到快慢指针的相遇点，把其中一个指针放到起点，然后两个指针同时从相遇点和起点一起往前走，那么下一个相遇的点就是这个交叉点。
  * Sliding Window
    * 这一类题属于快慢双指针,  用来避免重复计算，达到O(N)的时间复杂度，有时会有和hash或者单调queue的结合，通常有四种类型
      * 求满足条件的最小窗口，一般是二分的情况，初始的窗口不满足条件，随着窗口的扩大会遇到满足条件的点，因为是算最小值，所以这时候接下去就开始缩小窗口，直到条件又不满足，所以每次缩小窗口的时候就更新下最小值。
      * ```
        public int slidngwindow(String s) {
            int left = 0;
            int right = 0;
            int minLength = Integer.MAX_VALUE;
            while (right < s.length()){
                // expand the window using right pointer
                if (some criteria meet){
                    // calculate the result based on current sliding window
                    minLength = Math.min(minLength, right - left + 1); 
                    // shrink the window
                    // update left so that it no longer meet the criteria
                }
                
                right++;
            }
            
            return minLength;
        }
        ```
      * ```
        public int slidngwindow(String s) {
            int left = 0;
            int right = 0;
            int minLength = Integer.MAX_VALUE;
            while (right < s.length()){
                // expand the window using right pointer
                while (some criteria meet){
                    // calculate the result based on current sliding window
                    minLength = Math.min(minLength, right - left + 1); 
                    left++;
                }
                
                right++;
            }
            
            return minLength;
        }
        ```
      * 求满足条件的最大窗口，这里的条件通常也是二分的，一般是考at most something，换言之就是初始窗口是满足条件的，扩大到了某个窗口开始不满足条件，然后需要缩小窗口，直到重新满足条件，因为是算最大值，所以这里每次窗口的扩大都更新下最大值
      * ```
        public int slidngwindow(String s) {
            int left = 0;
            int right = 0;
            int maxLength = 0;
            while (right < s.length()){
                // expand the window using right pointer
                // shrink the window as long as the criteria not meet
                if (some criteria not meet){
                    // update left so that it will meet the criteria
                    // this will usually take advantage of a hashmap 
                    // to quickly jump left pointer
                }
                
                // calculate max as long as the criteria meet
                maxLength = Math.max(maxLength, right - left + 1);
                right++;
            }
            
            return maxLength;
        }
        ```
      * ```
        public int slidngwindow(String s) {
            int left = 0;
            int right = 0;
            int maxLength = 0;
            while (right < s.length()){
                // expand the window using right pointer
                // shrink the window as long as the criteria not meet
                while (some criteria not meet){
                    left++;
                }
                
                // calculate max as long as the criteria meet
                maxLength = Math.max(maxLength, right - left + 1);
                right++;
            }
            
            return maxLength;
        }
        ```
      * 求满足条件的窗口个数
        * 可以考一上来满足条件，到了某个点就不满足条件了，这种题类似于上面最长窗口的解法，另一种是一上来不满足条件，到了某个窗口开始满足条件，窗口继续扩大还可以满足条件，然后继续扩大又不满足条件了，类似于三分的scenario，比如计算subarray的sum满足某个target，然后subarray可以包含0, 这里一种做法是把这题转换成二分的，计算满足小于等于target的窗口个数 - 满足小于等于target - 1的窗口个数， 然后做法就和计算最大窗口类似了。
        * ```
          public int slidingwinowThreeWayPartition(int[] nums, int k) {
              return this.slidingwinowTwoWayPartition(nums, k) - this.slidingwinowTwoWayPartition(nums, k - 1);
          }
          ```
        * ```
          public int slidingwinowTwoWayPartition(int[] nums, int k) {
              int left = 0;
              int right = 0;
              int count = 0;
              while (right < nums.length){
                  // 把当前值添加到窗口里来
                  while (left <= right && //不满足的条件){
                      // 结果去掉左边的值
                      left++;
                  }

                  // Every valid window add (right - left + 1) subarray count
                  count += (right - left + 1);

                  right++;
              }

              return count;
          }
          ```
        * 求满足条件的所有窗口
          * 一般会考一上来不满足条件，之后会满足条件的窗口题，而且通常会考固定大小窗口题，这样窗口大小到了固定窗口就可以开始缩小。类似于计算最小窗口题的写法。
          * ```
            public List<Integer> slidingwindow(String s, String p) {
                List<Integer> result = new ArrayList<>();
                int left = 0;
                int right = 0;

                while (right < s.length()){
                    // 如果窗口满足某个固定大小，开始缩小窗口
                    if (right - left + 1 == p.length()){
                        if (满足某个条件) {
                            result.add(left);
                        }

                        left++;
                    }

                    right++;
                }

                return result;
            }
            ```
      * 窗口题还有另外一种维度的划分就是分为固定大小窗口，和可变窗口大小题&#x20;

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
  * [31. Next Permutation](https://leetcode.com/problems/next-permutation)
  * [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii)
  * [189. Rotate Array](https://leetcode.com/problems/rotate-array)
  * [344. Reverse String](https://leetcode.com/problems/reverse-string)
  * [541. Reverse String II](https://leetcode.com/problems/reverse-string-ii)
* Floyd's Cycle Detection
  * [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle)
  * [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii)
  * [202. Happy Number](https://leetcode.com/problems/happy-number)
  * [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number)
* Sliding Window
  * 寻找最小窗口
    * [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring)
    * [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum)
    * [727. Minimum Window Subsequence](https://leetcode.com/problems/minimum-window-subsequence)
    * [862. Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k)
  * 寻找最大窗口
    * [3. Longest Substring Without Repeating Characte](https://leetcode.com/problems/longest-substring-without-repeating-characters)r
    * [159. Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters)
    * [340. Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters)
    * [395. Longest Substring with At Least K Repeating](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters)
    * [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement)
    * [487. Max Consecutive Ones II](https://leetcode.com/problems/max-consecutive-ones-ii)
    * [718. Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray)
    * [978. Longest Turbulent Subarray](https://leetcode.com/problems/longest-turbulent-subarray)
    * [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii)
    * [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](two-pointer.md#ji-ben-si-lu)
  * 寻找所有窗口
    * [30. Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words)
    * [187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences)
    * [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string)
  * 寻找窗口个数
    * [713. Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k)
    * [930. Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum)
    * [992. Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers)
    * [1248. Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays)
  * 固定大小窗口题
    * [30. Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words)
    * [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum)
    * [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string)
    * [480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median)

#### 反向面对面双指针

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



