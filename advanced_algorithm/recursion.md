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

```go
func generateTrees(n int) []*TreeNode {
    if n==0{
        return nil
    }
    return generate(1,n)

}
func generate(start,end int)[]*TreeNode{
    if start>end{
        return []*TreeNode{nil}
    }
    ans:=make([]*TreeNode,0)
    for i:=start;i<=end;i++{
        // 递归生成所有左右子树
        lefts:=generate(start,i-1)
        rights:=generate(i+1,end)
        // 拼接左右子树后返回
        for j:=0;j<len(lefts);j++{
            for k:=0;k<len(rights);k++{
                root:=&TreeNode{Val:i}
                root.Left=lefts[j]
                root.Right=rights[k]
                ans=append(ans,root)
            }
        }
    }
    return ans
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
