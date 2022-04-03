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
