# 二叉搜索树

## 定义

- 每个节点中的值必须大于（或等于）存储在其左侧子树中的任何值。
- 每个节点中的值必须小于（或等于）存储在其右子树中的任何值。

## 应用

[validate-binary-search-tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)

> 验证二叉搜索树

```dart  
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *   int val;
 *   TreeNode? left;
 *   TreeNode? right;
 *   TreeNode([this.val = 0, this.left, this.right]);
 * }
 */
class Solution {
  bool isValidBST(TreeNode? root) {
    return validateBST(root, -1 << 63, 1 << 63 - 1);
  }

  bool validateBST(TreeNode? node, int minVal, int maxVal) {
    if (node == null) {
      return true; // 空节点满足二叉搜索树定义，返回 true
    }

    if (node.val <= minVal || node.val >= maxVal) {
      return false; // 当前节点值小于等于最小值或大于等于最大值，不满足二叉搜索树定义，返回 false
    }

    // 递归验证左子树和右子树
    return validateBST(node.left, minVal, node.val) &&
        validateBST(node.right, node.val, maxVal);
  }
}

```

[insert-into-a-binary-search-tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

> 给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 保证原始二叉搜索树中不存在新值。

> 如果该子树不为空，则问题转化成了将 val 插入到对应子树上。
> 否则，在此处新建一个以 val 为值的节点，并链接到其父节点 root 上。

```dart
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *   int val;
 *   TreeNode? left;
 *   TreeNode? right;
 *   TreeNode([this.val = 0, this.left, this.right]);
 * }
 */
class Solution {
  TreeNode? insertIntoBST(TreeNode? root, int val) {
      if(root == null){
          return TreeNode(val);
      }
      if(root.val < val){
          root.right = insertIntoBST(root.right,val);
      } else {
          root.left = insertIntoBST(root.left,val);
      }
      return root;

  }
}
```

[delete-node-in-a-bst](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

> 给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的  key  对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

``` dart
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *   int val;
 *   TreeNode? left;
 *   TreeNode? right;
 *   TreeNode([this.val = 0, this.left, this.right]);
 * }
 */
class Solution {
    // 删除节点分为三种情况：
    // 1、只有左节点 替换为右
    // 2、只有右节点 替换为左
    // 3、有左右子节点 左子节点连接到右边最左节点即可
  TreeNode? deleteNode(TreeNode? root, int key) {
      if(root == null) {
          return root;
      }
      if(root.val > key){
        root.left =  deleteNode(root.left,key);
      } else if(root.val < key){
          root.right = deleteNode(root.right,key);
      } else  if(root.val == key){
          if(root.right == null){
              return root.left;
          }else if(root.left == null){
              return root.right;
          } else {
              TreeNode? cur = root.right;
              while(cur?.left != null){
                  cur = cur?.left;
              }
              cur?.left = root.left;
              return root.right;
          }
      }
      return root;
  }
}
```

[balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)

> 给定一个二叉树，判断它是否是高度平衡的二叉树。

```dart
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *   int val;
 *   TreeNode? left;
 *   TreeNode? right;
 *   TreeNode([this.val = 0, this.left, this.right]);
 * }
 */
class Solution {
  bool isBalanced(TreeNode? root) {
      if(root == null){
          return true;
      } else {
         return (height(root.left) - height(root.right)).abs() <= 1 && isBalanced(root.left) && isBalanced(root.right);
      }
  }

  int height(TreeNode? root) {
      if(root == null){
          return 0;
      } else {
          return max(height(root.left),height(root.right)) +1;
      }
  }
}
```

## 练习

- [ ] [validate-binary-search-tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)
- [ ] [insert-into-a-binary-search-tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)
- [ ] [delete-node-in-a-bst](https://leetcode-cn.com/problems/delete-node-in-a-bst/)
- [ ] [balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)
