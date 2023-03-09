# 二叉树

## 知识点

### 二叉树遍历

**前序遍历**：**先访问根节点**，再前序遍历左子树，再前序遍历右子树

**中序遍历**：先中序遍历左子树，**再访问根节点**，再中序遍历右子树

**后序遍历**：先后序遍历左子树，再后序遍历右子树，**再访问根节点**

注意点

- 以根访问顺序决定是什么遍历
- 左子树都是优先右子树


```dart
//二叉树的数据结构
class TreeNode {
  int? val;
  TreeNode? left;
  TreeNode? right;
}
```

#### 递归

```dart
void preorderDFS(TreeNode? root) {
  if (root == null) {
    return;
  }
  //前序遍历---输出放在前面 
  //print('${root.val}');
  preorderDFS(root.left);
  //中序遍历---输出放在中间 
  //print('${root.val}');
  preorderDFS(root.right);
  //后续遍历---输出放在后面 
  //print('${root.val}');
}
```

#### 前序非递归

```dart
List<TreeNode?> preordeerTravesal(TreeNode? root) {
  if (root == null) {
    return [];
  }
  List<TreeNode?> results = [];
  List<TreeNode?> stack = [];

  TreeNode? node = root;

  while (stack.isNotEmpty || node != null) {
    //把最左边的一排数字加入 栈中
    while (node != null) {
      stack.add(node);
      results.add(node);
      node = node.left;
    }
    //移除栈顶元素
    node = stack.removeLast();
    //访问栈顶元素的右子树
    node = node?.right;
  }

  return results;
}
```

#### 中序非递归

```go

List<TreeNode?> inordeerTravesal(TreeNode? root) {
  if (root == null) {
    return [];
  }
  List<TreeNode?> results = [];
  List<TreeNode?> stack = [];

  TreeNode? node = root;

  while (stack.isNotEmpty || node != null) {
    while (node != null) {
      stack.add(node);
      node = node.left;
    }

    node = stack.removeLast();
    results.add(node);//这个放置的地方和前序遍历进行比较
    node = node?.right;
  }

  return results;
}

```

#### 后序非递归
后序遍历其实就是将前序遍历反转就行了。
```dart
List<TreeNode?> lastorderTravesal(TreeNode? root) {
  return preordeerTravesal(root).reversed.toList();
}
```

### 二叉树的层次遍历(BFS)
```dart
void bfs(TreeNode? root){
  if(root == null) {
    return;
  }
  
  List<TreeNode> queue = [];
  queue.add(root);
  
  while(queue.isNotEmpty) {
    var length = queue.length;
    //一层一层的移除元素
    for(var i = 0;i < length; i++) {
      var node = queue.removeAt(0);
      if(node.left != null) {
        queue.add(node.left!);
      }
      if(node.right != null) {
        queue.add(node.right!);
      }
    }
  }
}

```

常见题目示例

#### maximum-depth-of-binary-tree

[maximum-depth-of-binary-tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

> 给定一个二叉树，找出其最大深度。

思路：典型的层次遍历的题型

```dart 
int maxDepth(TreeNode? root){
  if(root == null) {
    return;
  }
  
  List<TreeNode> queue = [];
  queue.add(root);
  var result = 0;
  while(queue.isNotEmpty) {
    var length = queue.length;
    for(var i = 0;i < length; i++) {
      var node = queue.removeLast();
      if(node.left != null) {
        queue.add(node.left!);
      }
      if(node.right != null) {
        queue.add(node.right!);
      }
    }
    result += 1
  }
  return result;
}

```

#### balanced-binary-tree

[balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)

> 给定一个二叉树，判断它是否是高度平衡的二叉树。

思路：分治法，左边平衡 && 右边平衡 && 左右两边高度 <= 1，
因为需要返回是否平衡及高度，要么返回两个数据，要么合并两个数据，
所以用-1 表示不平衡，>0 表示树高度（二义性：一个变量有两种含义）。

```dart
bool isBalanced(TreeNode? root) {
  return getHeight(root) != -1;
}

int getHeight(TreeNode? root) {
  if (root == null) {
    return 0;
  }

  int leftHeight = getHeight(root.left);
  if (leftHeight == -1) { // 左子树不平衡，直接返回 -1
    return -1;
  }

  int rightHeight = getHeight(root.right);
  if (rightHeight == -1) { // 右子树不平衡，直接返回 -1
    return -1;
  }

  if ((leftHeight - rightHeight).abs() > 1) { // 左右子树高度差超过 1，不平衡
    return -1;
  }

  return (leftHeight > rightHeight ? leftHeight : rightHeight) + 1; // 返回节点高度
}

```

#### binary-tree-maximum-path-sum

[binary-tree-maximum-path-sum](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

> 给定一个**非空**二叉树，返回其最大路径和。

思路：分治法，分为三种情况：左子树最大路径和最大，右子树最大路径和最大，左右子树最大加根节点最大，需要保存两个变量：一个保存子树最大路径和，一个保存左右加根节点和，然后比较这个两个变量选择最大值即可

```dart
class Solution {
  int ret = -1000000;
  int maxPathSum(TreeNode? root) {
    helper(root);
    return ret;
  }

  int helper(TreeNode? node) {
    if (node == null) {
      return 0;
    }
    int left = max(0, helper(node.left));
    int right = max(0, helper(node.right));
    ret = max(ret, node.val! + left + right);
    return max(left, right) + node.val!;
  }
}
```

#### lowest-common-ancestor-of-a-binary-tree

[lowest-common-ancestor-of-a-binary-tree](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

> 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

思路：分治法，有左子树的公共祖先或者有右子树的公共祖先，就返回子树的祖先，否则返回根节点

```dart
  TreeNode? lowestCommonAncestor(TreeNode? root, TreeNode? p, TreeNode? q) {
    if (root == null) {
      return root;
    }
    if (root == p || root == q) {
      return root;
    }

    TreeNode? left = lowestCommonAncestor(root.left, p, q);
    TreeNode? right = lowestCommonAncestor(root.right, p, q);
    if (left != null && right != null) {
      return root;
    }
    if (left != null) {
      return left;
    }
    if (right != null) {
      return right;
    }
    return null;
  }
```

### BFS 层次应用

#### binary-tree-level-order-traversal

[binary-tree-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

> 给你一个二叉树，请你返回其按  **层序遍历**  得到的节点值。 （即逐层地，从左到右访问所有节点）

思路：用一个队列记录一层的元素，然后扫描这一层元素添加下一层元素到队列（一个数进去出来一次，所以复杂度 O(logN)）

```dart
  List<List<int>> levelOrder(TreeNode? root) {
    List<List<int>> results = [];
    if (root == null) {
      return results;
    }
    List<TreeNode> queue = [];
    queue.add(root);
    while (queue.isNotEmpty) {
      int length = queue.length;
      List<int> temp = [];
      for (var i = 0; i < length; i++) {
        TreeNode node = queue.removeAt(0);
        temp.add(node.val!);
        if (node.left != null) {
          queue.add(node.left!);
        }
        if (node.right != null) {
          queue.add(node.right!);
        }
      }
      results.add(temp);
    }
    return results;
  }
```

#### binary-tree-level-order-traversal-ii

[binary-tree-level-order-traversal-ii](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

> 给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

思路：在层级遍历的基础上，翻转一下结果即可

```dart
  List<List<int>> levelOrderBottom(TreeNode? root) {
    List<List<int>> results = [];
    if (root == null) {
      return results;
    }
    List<TreeNode> queue = [];
    queue.add(root);
    while (queue.isNotEmpty) {
      int length = queue.length;
      List<int> temp = [];
      for (var i = 0; i < length; i++) {
        TreeNode node = queue.removeAt(0);
        temp.add(node.val!);
        if (node.left != null) {
          queue.add(node.left!);
        }
        if (node.right != null) {
          queue.add(node.right!);
        }
      }
      results.add(temp);
    }
    return results.reversed.toList();
  }
```

#### binary-tree-zigzag-level-order-traversal

[binary-tree-zigzag-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

> 给定一个二叉树，返回其节点值的锯齿形层次遍历。Z 字形遍历

```dart
  List<List<int>> levelOrder(TreeNode? root) {
    List<List<int>> results = [];
    if (root == null) {
      return results;
    }
    List<TreeNode> queue = [];
    queue.add(root);
    while (queue.isNotEmpty) {
      int length = queue.length;
      List<int> temp = [];
      for (var i = 0; i < length; i++) {
        TreeNode node = queue.removeAt(0);
        temp.add(node.val!);
        if (node.left != null) {
          queue.add(node.left!);
        }
        if (node.right != null) {
          queue.add(node.right!);
        }
      }
      results.add(temp);
    }
    List<List<int>> temp = [];
    for (var i = 0; i < results.length; i++) {
      if (i % 2 == 0) {
        temp.add(results[i]);
      } else {
        temp.add(results[i].reversed.toList());
      }
    }
    return temp;
  }
```

### 二叉搜索树应用

#### validate-binary-search-tree

[validate-binary-search-tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)

> 给定一个二叉树，判断其是否是一个有效的二叉搜索树。

中序遍历，检查结果列表是否已经有序

```dart
class Solution {
  int ret = -100000000000;//取一个最小整数
  bool result = true;

  bool isValidBST(TreeNode? root) {
    dfs(root);
    return result;
  }

  void dfs(TreeNode? root) {
    if (root == null) {
      return;
    }
    dfs(root.left);
    if (root.val! <= ret) {
      result = false;
      return;
    }
    ret = root.val!;
    dfs(root.right);
  }
}

```


#### insert-into-a-binary-search-tree

[insert-into-a-binary-search-tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

> 给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。

思路：找到最后一个叶子节点满足插入条件即可

```dart
// DFS查找插入位置
  TreeNode? insertIntoBST(TreeNode? root, int val) {
    if (root == null) {
      TreeNode? node = TreeNode()..val = val;
      return node;
    }
    if (root.val! > val) {
      root.left = insertIntoBST(root.left, val);
    } else {
      root.right = insertIntoBST(root.right, val);
    }
    return root;
  }
```

## 总结

- 掌握二叉树递归与非递归遍历
- 理解 DFS 前序遍历与分治法
- 理解 BFS 层次遍历

## 练习

- [ ] [maximum-depth-of-binary-tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
- [ ] [balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)
- [ ] [binary-tree-maximum-path-sum](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
- [ ] [lowest-common-ancestor-of-a-binary-tree](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
- [ ] [binary-tree-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
- [ ] [binary-tree-level-order-traversal-ii](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)
- [ ] [binary-tree-zigzag-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
- [ ] [validate-binary-search-tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)
- [ ] [insert-into-a-binary-search-tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)
