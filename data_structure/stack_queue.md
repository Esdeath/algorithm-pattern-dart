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

[number-of-islands](https://leetcode-cn.com/problems/number-of-islands/)

> 给定一个由  '1'（陆地）和 '0'（水）组成的的二维网格，计算岛屿的数量。一个岛被水包围，并且它是通过水平方向或垂直方向上相邻的陆地连接而成的。你可以假设网格的四个边均被水包围。

思路：通过深度搜索遍历可能性（注意标记已访问元素）

```go

func numIslands(grid [][]byte) int {
    var count int
    for i:=0;i<len(grid);i++{
        for j:=0;j<len(grid[i]);j++{
            if grid[i][j]=='1' && dfs(grid,i,j)>=1{
                count++
            }
        }
    }
    return count
}
func dfs(grid [][]byte,i,j int)int{
    if i<0||i>=len(grid)||j<0||j>=len(grid[0]){
        return 0
    }
    if grid[i][j]=='1'{
        // 标记已经访问过(每一个点只需要访问一次)
        grid[i][j]=0
        return dfs(grid,i-1,j)+dfs(grid,i,j-1)+dfs(grid,i+1,j)+dfs(grid,i,j+1)+1
    }
    return 0
}
```

[largest-rectangle-in-histogram](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

> 给定 _n_ 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
> 求在该柱状图中，能够勾勒出来的矩形的最大面积。

思路：求以当前柱子为高度的面积，即转化为寻找小于当前值的左右两边值

![image.png](https://img.fuiboom.com/img/stack_rain.png)

用栈保存小于当前值的左的元素

![image.png](https://img.fuiboom.com/img/stack_rain2.png)

```go
func largestRectangleArea(heights []int) int {
	if len(heights) == 0 {
		return 0
	}
	stack := make([]int, 0)
	max := 0
	for i := 0; i <= len(heights); i++ {
		var cur int
		if i == len(heights) {
			cur = 0
		} else {
			cur = heights[i]
		}
        // 当前高度小于栈，则将栈内元素都弹出计算面积
		for len(stack) != 0 && cur <= heights[stack[len(stack)-1]] {
			pop := stack[len(stack)-1]
			stack = stack[:len(stack)-1]
			h := heights[pop]
            // 计算宽度
			w := i
			if len(stack) != 0 {
				peek := stack[len(stack)-1]
				w = i - peek - 1
			}
			max = Max(max, h*w)
		}
        // 记录索引即可获取对应元素
		stack = append(stack, i)
	}
	return max
}
func Max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

## Queue 队列

常用于 BFS 宽度优先搜索

[implement-queue-using-stacks](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

> 使用栈实现队列

```go
type MyQueue struct {
    stack []int
    back  []int
}

/** Initialize your data structure here. */
func Constructor() MyQueue {
    return MyQueue{
        stack: make([]int, 0),
        back:  make([]int, 0),
    }
}

// 1
// 3
// 5

/** Push element x to the back of queue. */
func (this *MyQueue) Push(x int) {
    for len(this.back) != 0 {
        val := this.back[len(this.back)-1]
        this.back = this.back[:len(this.back)-1]
        this.stack = append(this.stack, val)
    }
    this.stack = append(this.stack, x)
}

/** Removes the element from in front of queue and returns that element. */
func (this *MyQueue) Pop() int {
    for len(this.stack) != 0 {
        val := this.stack[len(this.stack)-1]
        this.stack = this.stack[:len(this.stack)-1]
        this.back = append(this.back, val)
    }
    if len(this.back) == 0 {
        return 0
    }
    val := this.back[len(this.back)-1]
    this.back = this.back[:len(this.back)-1]
    return val
}

/** Get the front element. */
func (this *MyQueue) Peek() int {
    for len(this.stack) != 0 {
        val := this.stack[len(this.stack)-1]
        this.stack = this.stack[:len(this.stack)-1]
        this.back = append(this.back, val)
    }
    if len(this.back) == 0 {
        return 0
    }
    val := this.back[len(this.back)-1]
    return val
}

/** Returns whether the queue is empty. */
func (this *MyQueue) Empty() bool {
    return len(this.stack) == 0 && len(this.back) == 0
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * param_2 := obj.Pop();
 * param_3 := obj.Peek();
 * param_4 := obj.Empty();
 */
```

二叉树层次遍历

```go
func levelOrder(root *TreeNode) [][]int {
    // 通过上一层的长度确定下一层的元素
    result := make([][]int, 0)
    if root == nil {
        return result
    }
    queue := make([]*TreeNode, 0)
    queue = append(queue, root)
    for len(queue) > 0 {
        list := make([]int, 0)
        // 为什么要取length？
        // 记录当前层有多少元素（遍历当前层，再添加下一层）
        l := len(queue)
        for i := 0; i < l; i++ {
            // 出队列
            level := queue[0]
            queue = queue[1:]
            list = append(list, level.Val)
            if level.Left != nil {
                queue = append(queue, level.Left)
            }
            if level.Right != nil {
                queue = append(queue, level.Right)
            }
        }
        result = append(result, list)
    }
    return result
}
```

[01-matrix](https://leetcode-cn.com/problems/01-matrix/)

> 给定一个由 0 和 1 组成的矩阵，找出每个元素到最近的 0 的距离。
> 两个相邻元素间的距离为 1

```go
// BFS 从0进队列，弹出之后计算上下左右的结果，将上下左右重新进队列进行二层操作
// 0 0 0 0
// 0 x 0 0
// x x x 0
// 0 x 0 0

// 0 0 0 0
// 0 1 0 0
// 1 x 1 0
// 0 1 0 0

// 0 0 0 0
// 0 1 0 0
// 1 2 1 0
// 0 1 0 0
func updateMatrix(matrix [][]int) [][]int {
    q:=make([][]int,0)
    for i:=0;i<len(matrix);i++{
        for j:=0;j<len(matrix[0]);j++{
            if matrix[i][j]==0{
                // 进队列
                point:=[]int{i,j}
                q=append(q,point)
            }else{
                matrix[i][j]=-1
            }
        }
    }
    directions:=[][]int{{0,1},{0,-1},{-1,0},{1,0}}
    for len(q)!=0{
        // 出队列
        point:=q[0]
        q=q[1:]
        for _,v:=range directions{
            x:=point[0]+v[0]
            y:=point[1]+v[1]
            if x>=0&&x<len(matrix)&&y>=0&&y<len(matrix[0])&&matrix[x][y]==-1{
                matrix[x][y]=matrix[point[0]][point[1]]+1
                // 将当前的元素进队列，进行一次BFS
                q=append(q,[]int{x,y})
            }
        }
    }
    return matrix

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
