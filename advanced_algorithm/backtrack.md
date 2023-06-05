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
    List<List<int>> result = []; // 存储最终结果的列表
    List<int> temp = []; // 存储当前子集的临时列表
    backtrack(nums, 0, temp, result); // 调用回溯函数生成所有子集
    return result; // 返回最终结果
  }

  void backtrack(
    List<int> nums, // 原始数组
    int index, // 当前遍历的索引
    List<int> temp, // 当前子集的临时列表
    List<List<int>> result, // 存储最终结果的列表
  ) {
    result.add([...temp]); // 将当前子集的拷贝添加到最终结果列表中

    for (int i = index; i < nums.length; i++) {
      temp.add(nums[i]); // 将当前元素添加到临时列表中

      // 递归调用回溯函数，遍历剩余元素
      backtrack(nums, i + 1, temp, result);

      temp.removeLast(); // 移除最后一个元素，回溯到上一层状态
    }
  }
}

```

### [subsets-ii](https://leetcode-cn.com/problems/subsets-ii/)

> 给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。说明：解集不能包含重复的子集。

```dart
class Solution {
  List<List<int>> subsetsWithDup(List<int> nums) {
    List<List<int>> result = []; // 存储最终结果的列表
    List<int> temp = []; // 存储当前子集的临时列表
    nums.sort(); // 对原始数组进行排序，以便处理重复元素
    backtrack(nums, 0, temp, result); // 调用回溯函数生成所有子集
    return result; // 返回最终结果
  }

  void backtrack(
    List<int> nums, // 给定的集合
    int index, // 下次添加到集合中的元素位置索引
    List<int> temp, // 临时结果集合(每次需要复制保存)
    List<List<int>> result, // 最终结果
  ) {
    result.add([...temp]); // 将当前子集的拷贝添加到最终结果列表中

    // 选择时需要剪枝、处理、撤销选择
    for (int i = index; i < nums.length; i++) {
      // 剪枝条件：跳过重复元素，但保留第一次出现的元素
      if (i > 0 && nums[i] == nums[i - 1] && i != index) continue;

      temp.add(nums[i]); // 将当前元素添加到临时列表中

      // 递归调用回溯函数，遍历剩余元素
      backtrack(nums, i + 1, temp, result);

      temp.removeLast(); // 移除最后一个元素，回溯到上一层状态
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
    List<List<int>> res = []; // 存储所有排列结果
    List<int> temp = []; // 存储当前排列结果
    Set<int> used = Set(); // 存储已经使用过的数字

    // 递归函数，用来生成所有的排列结果
    void backtrack() {
      // 如果当前排列结果的长度等于nums的长度，则将其加入res中
      if (temp.length == nums.length) {
        res.add([...temp]); // 注意要将temp的拷贝加入res中，否则会影响后续的操作
        return;
      }

      // 遍历nums数组，尝试将每个数字加入排列中
      for (int i = 0; i < nums.length; i++) {
        if (!used.contains(nums[i])) {
          temp.add(nums[i]); // 将当前数字加入排列中
          used.add(nums[i]); // 将当前数字标记为已经使用过
          backtrack(); // 递归，继续生成下一个数字的排列结果
          temp.removeLast(); // 回溯，将当前数字从排列中移除
          used.remove(nums[i]); // 回溯，将当前数字标记为未使用过
        }
      }
    }

    // 调用递归函数，生成所有的排列结果
    backtrack();

    return res;
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
