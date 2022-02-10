# Rolling Hash

* 这个算法也被称为Rabin Karp算法。是用来快速做string search的算法
  * [https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp\_algorithm](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp\_algorithm)
* 基于Sliding Window的一种特殊技巧，本质上是在窗口到达要求大小时，把两个要比较的窗口序列先做hash，如果hash不匹配，就快速排除，更新窗口，只有当hash匹配，再去一个一个比较具体的值。思想有点类似Bloom Filter
  * [187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences)
  * [214. Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome)
  * [718. Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray)
  * [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring)
  * [1062. Longest Repeating Substring](https://leetcode.com/problems/longest-repeating-substring)
  * [1554. Strings Differ by One Character](https://leetcode.com/problems/strings-differ-by-one-character)
  * [1698. Number of Distinct Substrings in a Strin](https://leetcode.com/problems/number-of-distinct-substrings-in-a-string)g
