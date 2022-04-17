# Binary Search Tree

### 基本思路

* 任何子树左节点小于parent，右节点大于parent。
* 平衡的BST，可以用来做binary search或quickselect。
* 找median的题，要么minmax heap，要么binary search, 要么bst
* BST的插入

```java
public TreeNode insertNode(TreeNode root, int val){
    if (root == null){
        return new TreeNode(val);
    }

    if (val > root.val){
        root.right = insertNode(root.right, val);
    }else{
        root.left = insertNode(root.left, val);
    }

    return root;
}

public TreeNode insertNodeV2(TreeNode root, int val){
    TreeNode curr = root;
    TreeNode prev = null;
    while (curr != null){
        prev = curr;
        if (val > curr.val){
            curr = curr.right;
            if (curr == null){
                prev.right = new TreeNode(val);
            }
        }else{
            curr = curr.left;
            if (curr == null){
                prev.left = new TreeNode(val);
            }
        }
    }

    return root;
}
```

* BST的删除
  * 先找到要删除的点
  * 然后分三种情况
    * 没有左右children，直接删掉就好
    * 有一个child, 把child node赋值给要被删的Node就好了
    * 左右children都在，那么可以和successor或者predessor做交换，然后递归删除那个successor或predessor

```java
public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null){
            return null;
        }

        if (key > root.val){
            root.right = this.deleteNode(root.right, key);
        }else if (key < root.val){
            root.left = this.deleteNode(root.left, key);
        }else{
            if (root.left == null && root.right == null){
                root = null;
            }else if (root.left != null && root.right != null){
                TreeNode curr = root.right;
                while (curr.left != null){
                    curr = curr.left;
                }

                root.val = curr.val;
                root.right = this.deleteNode(root.right, root.val);
            }else{
                if (root.left != null){
                    root = root.left;
                }else if (root.right != null){
                    root = root.right;
                }
            }
        }

        return root;
    }
```

### Leetcode题目

* Binary Search Tree特性基础
  * [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree)
  * [99. Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree)
  * [173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator)
  * [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree)
  * [333. Largest BST Subtree](https://leetcode.com/problems/largest-bst-subtree)
  * [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst)
  * [653. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst)
  * [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree)
  * [938. Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst)
  * [1382. Balance a Binary Search Tree](https://leetcode.com/problems/balance-a-binary-search-tree)
* 找k个Number
  * [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst)
  * [501. Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree)
* Inorder Traversal
  * [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree)
  * [109. Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree)
  * [285. Inorder Successor in BST](https://leetcode.com/problems/inorder-successor-in-bst)
  * [426. Convert Binary Search Tree to Sorted Doubly Linked List](https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list)
  * [510. Inorder Successor in BST II](https://leetcode.com/problems/inorder-successor-in-bst-ii)
  * [1008. Construct Binary Search Tree from Preorder ](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal)
* Search in a BST
  * [270. Closest Binary Search Tree Value](https://leetcode.com/problems/closest-binary-search-tree-value)
  * [272. Closest Binary Search Tree Value II](https://leetcode.com/problems/closest-binary-search-tree-value-ii)
  * [653. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst)
  * [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree)
  * [1214. Two Sum BSTs](https://leetcode.com/problems/two-sum-bsts)
* Serialize and Deserialize
  * [449. Serialize and Deserialize BST](https://leetcode.com/problems/serialize-and-deserialize-bst)
