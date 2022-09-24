# 回溯法

## 背景

回溯法（backtrack）常用于遍历列表所有子集，是 DFS 深度搜索一种，一般用于全排列，穷尽所有可能，遍历的过程实际上是一个决策树的遍历过程。时间复杂度一般 O(N!)，它不像动态规划存在重叠子问题可以优化，回溯算法就是纯暴力穷举，复杂度一般都很高。

## 模板

```dart
result = []
backtrack(选择列表,路径):
	if 满足结束条件:
        result.add(路径)
        return
    for 选择 in 选择列表:
        做选择
        backtrack(选择列表,路径)
        撤销选择
```

核心就是从选择列表里做一个选择，然后一直递归往下搜索答案，如果遇到路径不通，就返回来撤销这次选择。

## 示例

### [subsets](https://leetcode-cn.com/problems/subsets/)

> 给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

遍历过程

![image.png](https://img.fuiboom.com/img/backtrack.png)

```dart
class Solution {
  List<List<int>> subsets(List<int> nums) {
      List<List<int>> result = [];
      List<int> temp = [];
      backtrack(nums,0,temp,result);
      return result;
  }

  void backtrack(List<int> nums,int index,List<int> temp,List<List<int>> result){
      result.add([...temp]);// result里面存储temp的拷贝
      for(int i = index;i < nums.length;i++){
          temp.add(nums[i]);
          backtrack(nums,i+1,temp,result);
          temp.removeLast();
      }
  }
}
```

### [subsets-ii](https://leetcode-cn.com/problems/subsets-ii/)

> 给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。说明：解集不能包含重复的子集。

```dart
class Solution {
  List<List<int>> subsetsWithDup(List<int> nums) {
      List<List<int>> result = [];
      List<int> temp = [];
      nums.sort();
      backtrack(nums,0,temp,result);
      return result;
  }
	// nums 给定的集合
	// pos 下次添加到集合中的元素位置索引
	// list 临时结果集合(每次需要复制保存)
	// result 最终结果
  void backtrack(List<int> nums,int index,List<int> temp,List<List<int>> result){
      result.add([...temp]);// result里面存储temp的拷贝
	  	// 选择时需要剪枝、处理、撤销选择
      for(int i = index;i < nums.length;i++){
          if(i > 0 &&nums[i] == nums[i -1] && i != index) continue;
          temp.add(nums[i]);
          backtrack(nums,i+1,temp,result);
          temp.removeLast();
      }
  }
}
```

### [permutations](https://leetcode-cn.com/problems/permutations/)

> 给定一个   没有重复   数字的序列，返回其所有可能的全排列。

思路：需要记录已经选择过的元素，满足条件的结果才进行返回

```dart
class Solution {
  List<List<int>> permute(List<int> nums) {
      List<bool> isVisited = List<bool>.filled(nums.length, false);
      List<List<int>> result = [];
      List<int> temp = [];

      backtrack(nums,isVisited,temp,result);
      return result;
  }
	// nums 输入集合
	// visited 当前递归标记过的元素
	// list 临时结果集(路径)
	// result 最终结果
  void backtrack(List<int> nums,List<bool> isVisited,List<int> temp,List<List<int>> result){
	  // 返回条件：临时结果和输入集合长度一致 才是全排列
      if(temp.length == nums.length){
          result.add([...temp]);
          return;
      }
      for(int i = 0;i < nums.length;i++){
		// 已经添加过的元素，直接跳过
          if(i > 0 &&nums[i] == nums[i -1] && !isVisited[i]) continue;
          temp.add(nums[i]);
          isVisited[i] = true;
          backtrack(nums,isVisited,temp,result);
          isVisited[i] = false;
          temp.removeLast();
      }
  }
}
```

### [permutations-ii](https://leetcode-cn.com/problems/permutations-ii/)

> 给定一个可包含重复数字的序列，返回所有不重复的全排列。

```dart
class Solution {
  List<List<int>> permuteUnique(List<int> nums) {
    List<bool> isVisited = List<bool>.filled(nums.length, false);
      List<List<int>> result = [];
      List<int> temp = [];
      nums.sort();
      backtrack(nums,isVisited,temp,result);
      return result;
  }
	// nums 输入集合
	// visited 当前递归标记过的元素
	// list 临时结果集(路径)
	// result 最终结果
  void backtrack(List<int> nums,List<bool> isVisited,List<int> temp,List<List<int>> result){
	  // 返回条件：临时结果和输入集合长度一致 才是全排列
      if(temp.length == nums.length){
          result.add([...temp]);
          return;
      }
      for(int i = 0;i < nums.length;i++){
		// 已经添加过的元素，直接跳过
          if(isVisited[i]) continue;
		  if(i > 0 && nums[i-1] ==nums[i] && !isVisited[i-1]) continue;
          temp.add(nums[i]);
          isVisited[i] = true;
          backtrack(nums,isVisited,temp,result);
          isVisited[i] = false;
          temp.removeLast();
      }
  }
}
```

## 练习

- [ ] [subsets](https://leetcode-cn.com/problems/subsets/)
- [ ] [subsets-ii](https://leetcode-cn.com/problems/subsets-ii/)
- [ ] [permutations](https://leetcode-cn.com/problems/permutations/)
- [ ] [permutations-ii](https://leetcode-cn.com/problems/permutations-ii/)

挑战题目

- [ ] [combination-sum](https://leetcode-cn.com/problems/combination-sum/)
- [ ] [letter-combinations-of-a-phone-number](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)
- [ ] [palindrome-partitioning](https://leetcode-cn.com/problems/palindrome-partitioning/)
- [ ] [restore-ip-addresses](https://leetcode-cn.com/problems/restore-ip-addresses/)
- [ ] [permutations](https://leetcode-cn.com/problems/permutations/)
