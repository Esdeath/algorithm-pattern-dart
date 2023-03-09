# 栈和队列

## 简介

栈的特点是后入先出

![image.png](https://img.fuiboom.com/img/stack.png)

根据这个特点可以临时保存一些数据，之后用到依次再弹出来，常用于 DFS 深度搜索

队列一般常用于 BFS 广度搜索，类似一层一层的搜索

## Stack 栈

[min-stack](https://leetcode-cn.com/problems/min-stack/)

> 设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

思路：用两个栈实现，一个最小栈始终保证最小值在顶部

```dart
class MinStack {
  List<int> stack = [];
  List<int> minstack = [];

  void push(int val) {
    stack.add(val);
    if (minstack.isEmpty) {
      minstack.add(val);
    } else {
      minstack.add(min(minstack.last, val));
    }
  }

  void pop() {
    stack.removeLast();
    minstack.removeLast();
  }

  int top() {
    return stack.last;
  }

  int getMin() {
    return minstack.last;
  }
}
```

[evaluate-reverse-polish-notation](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)

> **波兰表达式计算** > **输入:** `["2", "1", "+", "3", "*"]` > **输出:** 9
>
> **解释:** `((2 + 1) * 3) = 9`

思路：通过栈保存原来的元素，遇到表达式弹出运算，再推入结果，重复这个过程

```dart
int evalRPN(List<String> tokens) {
  List<String> stack = [];
  
  for(int i = 0; i < tokens.length;i++){
    if(tokens[i] == '+' || tokens[i] == '-' || tokens[i] == '*' || tokens[i] == '/'){
      
      int num1 = int.parse(stack.removeLast());
      
      int num2 = int.parse(stack.removeLast());
      if(tokens[i] == '+' ){
        stack.add((num1 + num2).toString());
      }
       if(tokens[i] == '-' ){
        stack.add((num2 - num1).toString());
      }
      if(tokens[i] == '*' ){
          stack.add((num2 * num1).toString());
      }
      if(tokens[i] == '/' ){
         stack.add((num2 ~/ num1).toString());
      }
      
    } else {
      stack.add(tokens[i]);
    }
  }
  return int.parse(stack.last);
}

```

[decode-string](https://leetcode-cn.com/problems/decode-string/)

> 给定一个经过编码的字符串，返回它解码后的字符串。
> s = "3[a]2[bc]", 返回 "aaabcbc".
> s = "3[a2[c]]", 返回 "accaccacc".
> s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".

思路：通过栈辅助进行操作

```dart
String decodeString(String str) {
  int length = str.length;
  List<String> stackStr = [];
  for(int i = 0 ; i < length;i++){
    String temp = str[i];
    if(temp == "]"){
      //1.弹出[]里面所有字母 保存在s中
      String strCode = '';
      while(stackStr.isNotEmpty && stackStr.last != "[") {
          strCode = stackStr.removeLast() + strCode;
      }
      //2.弹出'['
      if(stackStr.isNotEmpty && stackStr.last == "[")stackStr.removeLast();
      //3.弹出所有的数字 
      String strNum = '';
      while(stackStr.isNotEmpty) {
        String temp = stackStr.last;
        if (int.tryParse(temp) != null){
          strNum = temp + strNum ;
          stackStr.removeLast();
        } else {
          break;
        }
      }
      //4.将数字 * 字符串 展开，然后压入栈中
      String strAll = '';
      int result = int.parse(strNum);
      for(int i = 0;i < result;i++){
        strAll = strAll + strCode;
      }
      stackStr.add(strAll);
    } else {
      stackStr.add(temp);
    }
  }
  
  return stackStr.join();
}
```

[binary-tree-inorder-traversal](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

> 给定一个二叉树，返回它的*中序*遍历。

```dart
//递归的方式
List<int> res = [];
List<int> inorderTraversal(TreeNode? root) {
  dfs(root);
  return res;
}
void dfs(TreeNode? root) {
  if (root == null) return;
  dfs(root.left);
  res.add(root.val!);
  dfs(root.right);
}

//非递归的方式
//思考一下递归的方式，首先会先调用root.left 不停的访问下去。
//直到节点为null，这时候开始访问右边节点.访问右边节点的时候，
//又会递归，首先访问root.left，不停的访问下去。
List<int> inorderTraversal(TreeNode? root) {
  List<int> res = [];
  List<TreeNode> stackNode = [];
  while(stackNode.isNotEmpty || root != null){
    while(root != null){
      stackNode.add(root);
      root= root.left;
    }
    TreeNode node = stackNode.removeLast();
    res.add(node.val!);
    root = node.right;
  }
  return res;
}
```

[number-of-islands](https://leetcode-cn.com/problems/number-of-islands/)

> 给定一个由  '1'（陆地）和 '0'（水）组成的的二维网格，计算岛屿的数量。一个岛被水包围，并且它是通过水平方向或垂直方向上相邻的陆地连接而成的。你可以假设网格的四个边均被水包围。
思路：通过深度搜索遍历可能性（注意标记已访问元素）

```dart
class Solution {
  int numIslands(List<List<String>> grid) {
    int m = grid.length;  // 网格的行数
    int n = grid[0].length;  // 网格的列数
    int count = 0;  // 岛屿数量的计数器

    // 四个方向的偏移量，用于遍历网格的相邻单元格
    List<List<int>> directions = [      [1, 0],   // 向下
      [-1, 0],  // 向上
      [0, 1],   // 向右
      [0, -1],  // 向左
    ];

    // 将指定的单元格标记为已访问，并将其添加到队列中
    void markVisited(int row, int col, List<List<int>> queue) {
      grid[row][col] = '0';  // 将单元格标记为已访问
      queue.add([row, col]);  // 将单元格添加到队列中
    }

    // 遍历指定单元格的相邻单元格，并将未访问的单元格添加到队列中
    void traverseAdjacentCells(int row, int col, List<List<int>> queue) {
      for (List<int> direction in directions) {
        int r = row + direction[0];  // 计算相邻单元格的行坐标
        int c = col + direction[1];  // 计算相邻单元格的列坐标

        // 如果相邻单元格在网格范围内且未被访问，则标记为已访问并添加到队列中
        if (r >= 0 && r < m && c >= 0 && c < n && grid[r][c] == '1') {
          markVisited(r, c, queue);
        }
      }
    }

    // 遍历整个网格，对每个未被访问的岛屿进行 BFS 遍历，并增加岛屿数量计数器
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (grid[i][j] == '1') {  // 如果单元格是陆地而未被访问
          count++;  // 岛屿数量加1
          List<List<int>> queue = [];  // 创建一个队列用于 BFS 遍历
          markVisited(i, j, queue);  // 标记当前单元格为已访问，并添加到队列中

          while (queue.isNotEmpty) {  // 当队列不为空时进行 BFS 遍历
            List<int> curr = queue.removeAt(0);  // 从队列中取出一个单元格
            int row = curr[0];  // 取出该单元格的行坐标
            int col = curr[1];  // 取出该单元格的列坐标
            traverseAdjacentCells(row, col, queue);  // 遍历
          }
        }
      }
    }

    return count;  // 返回岛屿数量
  }
}


```

[largest-rectangle-in-histogram](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

> 给定 _n_ 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
> 求在该柱状图中，能够勾勒出来的矩形的最大面积。

思路：求以当前柱子为高度的面积，即转化为寻找小于当前值的左右两边值

![image.png](https://img.fuiboom.com/img/stack_rain.png)

用栈保存小于当前值的左的元素

![image.png](https://img.fuiboom.com/img/stack_rain2.png)

```dart
class Solution {
  int largestRectangleArea(List<int> heights) {
    // 使用单调栈来维护每个柱子的左右边界
    List<int> stack = [];
    int maxArea = 0;
    int i = 0;
    while (i < heights.length) {
      // 如果栈为空或者当前柱子高度大于栈顶柱子的高度，则将当前柱子的下标压入栈中
      if (stack.isEmpty || heights[i] >= heights[stack.last]) {
        stack.add(i);
        i++;
      } else {
        // 如果当前柱子高度小于栈顶柱子的高度，则弹出栈顶柱子
        int topIndex = stack.removeLast();
        // 计算弹出的栈顶柱子对应的最大面积，并更新maxArea
        int area = heights[topIndex] *
            (stack.isEmpty ? i : i - stack.last - 1);
        maxArea = max(maxArea, area);
      }
    }
    // 处理栈中剩余的柱子
    while (!stack.isEmpty) {
      int topIndex = stack.removeLast();
      int area = heights[topIndex] *
          (stack.isEmpty ? i : i - stack.last - 1);
      maxArea = max(maxArea, area);
    }
    return maxArea;
  }
}

```

## Queue 队列

常用于 BFS 宽度优先搜索

[implement-queue-using-stacks](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

> 使用栈实现队列

```dart

class MyQueue {
  List<int> inStack = [];
  List<int> outStack = [];

  void push(int x) {
    inStack.add(x);
  }

  int pop() {
    if (outStack.isNotEmpty) {
      return outStack.removeLast();
    } else {
      while (inStack.isNotEmpty) {
        outStack.add(inStack.removeLast());
      }
      return outStack.removeLast();
    }
  }

  int peek() {
    if (outStack.isNotEmpty) {
      return outStack.last;
    } else {
      while (inStack.isNotEmpty) {
        outStack.add(inStack.removeLast());
      }
      return outStack.last;
    }
  }

  bool empty() {
    return inStack.isEmpty && outStack.isEmpty;
  }
}
```

二叉树层次遍历

```dart

class TreeNode {
  int? val;
  TreeNode? left;
  TreeNode? right;
}

List<List<int>> levelOrder(TreeNode? root) {
  List<List<int>> result = [[]];
  if (root == null) {
    return result;
  }
  List<TreeNode> queue = [];
  queue.add(root);
  while (queue.isNotEmpty) {
    int count = queue.length;
    List<int> temp = [];
    //一层一层的移除元素
    for (int i = 0; i < count; i++) {
      TreeNode node = queue.removeAt(0);
      temp.add(node.val!);
      if (node.left != null) {
        queue.add(node.left!);
      }
      if (node.right != null) {
        queue.add(node.right!);
      }
    }
    result.add(temp);
  }
  return result;
}
```

[01-matrix](https://leetcode-cn.com/problems/01-matrix/)

> 给定一个由 0 和 1 组成的矩阵，找出每个元素到最近的 0 的距离。
> 两个相邻元素间的距离为 1

```dart
//多源BFS问题：
//对于二叉树来说只有一个起点，所以二叉树在进行BFS的时候，只需要把根节点加入到queue里
//对于图来说 有多个点可以做为起点，所以要把所有起点加入queue，并且要记录已经访问的节点
//防止重复访问
List<List<int>> updateMatrix(List<List<int>> mat) {
  int row = mat.length;
  int col = mat[0].length;

  List<List<int>> queue = [];
  for (int i = 0; i < row; i++) {
    for (int j = 0; j < col; j++) {
      if (mat[i][j] == 0) {
        queue.add([i, j]);
      } else {
        mat[i][j] = -1;
      }
    }
  }
  List<int> xD = [1, 0, -1, 0];
  List<int> yD = [0, 1, 0, -1];
  while (queue.isNotEmpty) {
    List<int> pos = queue.removeAt(0);
    for (int i = 0; i < 4; i++) {
      int x = pos[0] + xD[i];
      int y = pos[1] + yD[i];
      if (x >= 0 && x < row && y >= 0 && y < col && mat[x][y] == -1) {
        mat[x][y] = mat[pos[0]][pos[1]] + 1;
        queue.add([x, y]);
      }
    }
  }
  return mat;
}
```

## 总结

- 熟悉栈的使用场景
  - 后入先出，保存临时值
  - 利用栈 DFS 深度搜索
- 熟悉队列的使用场景
  - 利用队列 BFS 广度搜索

## 练习

- [ ] [min-stack](https://leetcode-cn.com/problems/min-stack/)
- [ ] [evaluate-reverse-polish-notation](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)
- [ ] [decode-string](https://leetcode-cn.com/problems/decode-string/)
- [ ] [binary-tree-inorder-traversal](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
- [ ] [clone-graph](https://leetcode-cn.com/problems/clone-graph/)
- [ ] [number-of-islands](https://leetcode-cn.com/problems/number-of-islands/)
- [ ] [largest-rectangle-in-histogram](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)
- [ ] [implement-queue-using-stacks](https://leetcode-cn.com/problems/implement-queue-using-stacks/)
- [ ] [01-matrix](https://leetcode-cn.com/problems/01-matrix/)
