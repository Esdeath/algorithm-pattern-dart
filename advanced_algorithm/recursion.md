# 递归

## 介绍

将大问题转化为小问题，通过递归依次解决各个小问题

## 示例

[reverse-string](https://leetcode-cn.com/problems/reverse-string/)

> 编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组  `char[]`  的形式给出。

```dart
void reverseString(List<String> s) {
  reverse(s, 0, s.length - 1);
}

void reverse(List<String> s, int start, int end) {
  if (start >= end) return;
  var temp = s[start];
  s[start] = s[end];
  s[end] = temp;
  reverse(s, ++start, --end);
}

```

[unique-binary-search-trees-ii](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)

> 给定一个整数 n，生成所有由 1 ... n 为节点所组成的二叉搜索树。

```dart
List<TreeNode?> generateTree(int start, int end) {
    if (start > end) {
        return [null];
    }
    List<TreeNode> allTrees = [];
    // 枚举可行根节点
    for (int i = start; i <= end; i++) {
        // 获得所有可行的左子树集合
        List<TreeNode?> leftTrees = generateTree(start, i - 1);

        // 获得所有可行的右子树集合
        List<TreeNode?> rightTrees = generateTree(i + 1, end);

        // 从左子树集合中选出一棵左子树，从右子树集合中选出一棵右子树，拼接到根节点上
        for (var left in leftTrees) {
            for (var right in  rightTrees) {
                TreeNode currTree =  TreeNode();
                currTree.val = i;
                currTree.left = left;
                currTree.right = right;
                allTrees.add(currTree);
            }
        }
    }
    return allTrees;
}

List<TreeNode?> generateTrees(int n) {
    if (n == 0) {
        return [];
    }
    return generateTree(1, n);
}
```

## 递归+备忘录

[fibonacci-number](https://leetcode-cn.com/problems/fibonacci-number/)

> 斐波那契数，通常用  F(n) 表示，形成的序列称为斐波那契数列。该数列由  0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：
> F(0) = 0,   F(1) = 1
> F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
> 给定  N，计算  F(N)。

```dart
  Map<int,int> mapOne = {} ;
  int fib(n){
    if(n < 2) return n;
    if(mapOne.containsKey(n)){
      return mapOne[n] ?? 0;
    } else {
      mapOne[n] = fib(n-1) + fib(n-2);
      return mapOne[n] ?? 0;
    }
  }
```

## 练习

- [ ] [reverse-string](https://leetcode-cn.com/problems/reverse-string/)
- [ ] [swap-nodes-in-pairs](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)
- [ ] [unique-binary-search-trees-ii](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)
- [ ] [fibonacci-number](https://leetcode-cn.com/problems/fibonacci-number/)
