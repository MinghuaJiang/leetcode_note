# Trie

### 基本思路

可以用O(L)的时间在一堆单词里找到某个prefix的单词，主要是用来handle dictionary不能用来做prefix search的scenario，很多时候用在dfs的剪枝上。

```java
public class Trie {
    private TrieNode root;
    public Trie() {
        this.root = new TrieNode();
    }

    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode curr = this.root;
        for (char c: word.toCharArray()){
            if (curr.children[c - 'a'] == null){
                curr.children[c - 'a'] = new TrieNode();
            }

            curr = curr.children[c - 'a'];
        }

        curr.isWord = true;
    }

    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode curr = this.root;
        for (char c: word.toCharArray()){
            if (curr.children[c - 'a'] == null){
                return false;
            }

            curr = curr.children[c - 'a'];
        }

        return curr.isWord;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode curr = this.root;
        for (char c: prefix.toCharArray()){
            if (curr.children[c - 'a'] == null){
                return false;
            }

            curr = curr.children[c - 'a'];
        }

        return true;
    }

    class TrieNode{
        TrieNode[] children;
        boolean isWord;
        public TrieNode(){
            this.children = new TrieNode[26];
        }
    }
}
```

### Leetcode题

* [140. Word Break II](https://leetcode.com/problems/word-break-ii)
* [208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree)
* [211. Design Add and Search Words Data Structur](https://leetcode.com/problems/design-add-and-search-words-data-structure)e
* [212. Word Search II](https://leetcode.com/problems/word-search-ii)
* [336. Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs)
* [386. Lexicographical Numbers](https://leetcode.com/problems/lexicographical-numbers)
* [421. Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array)
* [472. Concatenated Words](https://leetcode.com/problems/concatenated-words)
* [588. Design In-Memory File System](https://leetcode.com/problems/design-in-memory-file-system)
* [1166. Design File System](https://leetcode.com/problems/design-file-system)
* [1268. Search Suggestions System](https://leetcode.com/problems/search-suggestions-system)
* [1948. Delete Duplicate Folders in System](https://leetcode.com/problems/delete-duplicate-folders-in-system)
