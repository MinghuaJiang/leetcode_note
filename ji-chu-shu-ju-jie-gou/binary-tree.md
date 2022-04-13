# Binary Tree

* 主要考点
  * 二叉树的三种遍历 (DFS)
    * Pre-order traversal
      * DFS写法
        * ```
              public List<Integer> preorderTraversal(TreeNode root) {
                  List<Integer> result = new ArrayList<>();
                  preOrder(root, result);
                  return result;
              }

              private void preOrder(TreeNode root, List<Integer> list){
                  if (root == null){
                      return;
                  }

                  list.add(root.val);
                  preOrder(root.left, list);
                  preOrder(root.right, list);
              }
          ```
      * Iterative写法1
        * ```
             public List<Integer> preorderTraversalIterativeV1(TreeNode root) {
                  List<Integer> result = new ArrayList<>();
                  Stack<TreeNode> stack = new Stack<>();
                  if (root == null) {
                      return result;
                  }

                  stack.add(root);
                  while (!stack.isEmpty()) {
                      TreeNode node = stack.pop();
                      result.add(node.val);
                      if (node.right != null) {
                          stack.add(node.right);
                      }
                      if (node.left != null) {
                          stack.add(node.left);
                      }
                  }

                  return result;
              }
          ```
      * Iterative写法2
        * ```
              public List<Integer> preorderTraversalIterativeV2(TreeNode root) {
                  List<Integer> res = new ArrayList<>();
                  Stack<TreeNode> stack = new Stack<>();
                  TreeNode curr = root;
                  while (curr != null || !stack.isEmpty()) {
                      while (curr != null) {
                          res.add(curr.val);
                          stack.push(curr);
                          curr = curr.left;
                      }

                      curr = stack.pop();
                      curr = curr.right;
                  }

                  return res;
              }
          ```
    * In-order traversal
      * DFS写法
        * ```
              public List<Integer> inorderTraversal(TreeNode root) {
                  List<Integer> result = new ArrayList<>();
                  inOrder(root, result);
                  return result;
              }

              private void inOrder(TreeNode root, List<Integer> list){
                  if (root == null){
                      return;
                  }

                  inOrder(root.left, list);
                  list.add(root.val);
                  inOrder(root.right, list);
              }
          ```
      * Iterative写法
        * ```
              public List<Integer> inorderTraversalIterative(TreeNode root) {
                  List<Integer> res = new ArrayList<>();
                  Stack<TreeNode> stack = new Stack<>();
                  TreeNode curr = root;
                  while (curr != null || !stack.isEmpty()) {
                      while (curr != null) {
                          stack.push(curr);
                          curr = curr.left;
                      }

                      curr = stack.pop();
                      res.add(curr.val);
                      curr = curr.right;
                  }

                  return res;
              }
          ```
    * Post-order traversal
      * DFS写法
        * ```
              public List<Integer> postorderTraversal(TreeNode root) {
                  List<Integer> result = new ArrayList<>();
                  postOrder(root, result);
                  return result;
              }

              private void postOrder(TreeNode root, List<Integer> list){
                  if (root == null){
                      return;
                  }

                  postOrder(root.left, list);
                  postOrder(root.right, list);
                  list.add(root.val);
              }
          ```
      * Iterative写法1
        * ```
              // Think of post order as reverse way of root, right, left from result perspective
              public List<Integer> postOrderTraversalIterativeV1(TreeNode root) {
                  LinkedList<Integer> result = new LinkedList<>();
                  Stack<TreeNode> stack = new Stack<>();
                  if (root == null) {
                      return result;
                  }

                  stack.add(root);
                  while (!stack.isEmpty()) {
                      TreeNode node = stack.pop();
                      result.addFirst(node.val);
                      if (node.left != null) {
                          stack.add(node.left);
                      }
                      if (node.right != null) {
                          stack.add(node.right);
                      }
                  }

                  return result;
              } 
          ```
      * Iterative写法2
        * ```
              public List<Integer> postOrderTraversalIterativeV2(TreeNode root) {
                  List<Integer> result = new ArrayList<Integer>();
                  if(root == null)
                      return result;
                  TreeNode prev = null;
                  TreeNode curr = root;
                  Stack<TreeNode> stack = new Stack();
                  while(curr != null || !stack.empty()){
                      while (curr != null){
                          stack.push(curr);
                          curr = curr.left;
                      }

                      curr = stack.peek();
                      if(curr.right == null || curr.right == prev){
                          result.add(curr.val);
                          stack.pop();
                          prev = curr;
                          curr = null;
                      }
                      else {
                          curr = curr.right;
                      }
                  }

                  return result;
              }
          ```
  * 二叉树的Level Order Traversal (BFS)
    * ```
      public List<List<Integer>> levelOrderBottom(TreeNode root) {
              LinkedList<List<Integer>> result = new LinkedList<>();
              Queue<TreeNode> queue = new LinkedList<>();
              if (root != null) {
                  queue.offer(root);
              }

              while (!queue.isEmpty()){
                  int size = queue.size();
                  List<Integer> list = new ArrayList<>();
                  for (int i = 0; i < size; i++){
                      TreeNode curr = queue.poll();
                      list.add(curr.val);
                      if (curr.left != null){
                          queue.offer(curr.left);
                      }

                      if (curr.right != null){
                          queue.offer(curr.right);
                      }
                  }

                  result.addFirst(list);
              }

              return result;
          }
      ```
  * Recursion
    * 考点一般主要看每次递归需要返回什么内容，可能需要同时返回多个信息，然避免重复计算。
  * Serialize and Deserialize
  * Iterator
    * 考察Traverse的iterative写法
* Leetcode 考题
  * Recursion (基于三种order之一）
    * [100. Same Tree](https://leetcode.com/problems/same-tree)
    * [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree)
    * [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree)
    * [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree)
    * [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree)
    * [112. Path Sum](https://leetcode.com/problems/path-sum)
    * [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum)
    * [129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers)
    * [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree)
    * [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree)
    * [339. Nested List Weight Sum](https://leetcode.com/problems/nested-list-weight-sum)
    * [364. Nested List Weight Sum II](https://leetcode.com/problems/nested-list-weight-sum-ii)
    * [437. Path Sum III](https://leetcode.com/problems/path-sum-iii)
    * [1644. Lowest Common Ancestor of a Binary Tr](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-ii)ee II
    * [1650. Lowest Common Ancestor of a Binary Tree III](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iii)
  * Backtracking
    * [113. Path Sum II](https://leetcode.com/problems/path-sum-ii)
    * [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths)
  * Level Order Traversal
    * [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal)
    * [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal)
    * [107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii)
    * [116. Populating Next Right Pointers in Each Nod](https://leetcode.com/problems/populating-next-right-pointers-in-each-node)e
    * [117. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii) II
    * [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view)[ ](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii)
    * [314. Binary Tree Vertical Order Traversal](https://leetcode.com/problems/binary-tree-vertical-order-traversal)
    * [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree)
    * [545. Boundary of Binary Tree](https://leetcode.com/problems/boundary-of-binary-tree)
    * [958. Check Completeness of a Binary Tree](https://leetcode.com/problems/check-completeness-of-a-binary-tree)
    * [987. Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree)
    * [1522. Diameter of N-Ary Tree](https://leetcode.com/problems/diameter-of-n-ary-tree)
  * Traversal
    * [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal)
    * [105. Construct Binary Tree from Preorder and Inor](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)der Traversal
    * [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal)
    * [114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list)
    * [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal)
    * [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal)
    * [156. Binary Tree Upside Down](https://leetcode.com/problems/binary-tree-upside-down)
    * [889. Construct Binary Tree from Preorder and Posto](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal)rder Traversal
    * [1028. Recover a Tree From Preorder Traversal](https://leetcode.com/problems/recover-a-tree-from-preorder-traversal)
  * Serialize and Deserialize
    * [297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree)
    * [536. Construct Binary Tree from String](https://leetcode.com/problems/construct-binary-tree-from-string)
  * Iterator
    *
