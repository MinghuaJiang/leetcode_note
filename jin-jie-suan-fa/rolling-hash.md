# Rolling Hash

### 基本思路

* 这个算法也被称为Rabin Karp算法。是用来快速做string search的算法
  * [https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp\_algorithm](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp\_algorithm)
* 基于Sliding Window的一种特殊技巧，用于优化sliding window里，需要每次做sequence equal才能判断是否满足条件的题, 原本的复杂度是O(N \* L),这里本质上是在窗口到达要求大小时，把两个要比较的窗口序列先做hash，一种情况是，如果hash不匹配，就快速排除，更新窗口，只有当hash匹配，再去读取或者比较具体的窗口值，思想有点类似Bloom Filter。所以average的复杂度可以降到O(N)， 也可以用在Binary Search上
* 核心思路是
* ```
  hash = hash * PRIME + B[j];
  hash = hash - B[i] * (long) Math.pow(PRIME, window size);
  ```
* 这里有几个问题，hash一上来是0，所以如果高位每一位都是0的话，那么就会产生歧义，除非窗口大小是固定的，比如lc 187，或者就把每一位 + 1让它不可能是0。&#x20;
* ```
  hash = hash * PRIME + text.charAt(j);
  hash = hash * PRIME + text.charAt(j) - 'a' + 1;
  ```
* 第二个问题是，hash的计算会导致overflow
* 第三个问题是，hash可能会变成负数

### Leetcode常见题

* [28. Implement strStr()](https://leetcode.com/problems/implement-strstr/)
* [187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences)
* [718. Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray)
* [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring)
* [1062. Longest Repeating Substrin](https://leetcode.com/problems/longest-repeating-substring)
* [1316. Distinct Echo Substrings](https://leetcode.com/problems/distinct-echo-substrings/discuss/1181698/Java-clean-O\(n2\)-Rolling-Hash-Solution-oror-with-comments)
* [1554. Strings Differ by One Character](https://leetcode.com/problems/strings-differ-by-one-character)
* [1698. Number of Distinct Substrings in a Strin](https://leetcode.com/problems/number-of-distinct-substrings-in-a-string)g
