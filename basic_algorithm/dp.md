# 动态规划

## 背景

先从一道题目开始~

如题  [triangle](https://leetcode-cn.com/problems/triangle/)

> 给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

例如，给定三角形：

```text
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

自顶向下的最小路径和为  11（即，2 + 3 + 5 + 1 = 11）。

使用 DFS（遍历 或者 分治法）

遍历

![image.png](https://img.fuiboom.com/img/dp_triangle.png)

分治法

![image.png](https://img.fuiboom.com/img/dp_dc.png)

优化 DFS，缓存已经被计算的值（称为：记忆化搜索 本质上：动态规划）

![image.png](https://img.fuiboom.com/img/dp_memory_search.png)

动态规划就是把大问题变成小问题，并解决了小问题重复计算的方法称为动态规划

动态规划和 DFS 区别

- 二叉树 子问题是没有交集，所以大部分二叉树都用递归或者分治法，即 DFS，就可以解决
- 像 triangle 这种是有重复走的情况，**子问题是有交集**，所以可以用动态规划来解决

动态规划

```dart
class Solution {
  int minimumTotal(List<List<int>> triangle) {
    int n = triangle.length;
    List<int> dp = List<int>.filled(n, 0);
    dp[0] = triangle[0][0];

    // 遍历三角形的每一行
    for (int i = 1; i < n; i++) {
      int temp1 = dp[0]; // 保存dp[0]的值，用于更新dp[1]
      dp[0] += triangle[i][0]; // 更新dp[0]
      for (int j = 1; j < i; j++) {
        int temp2 = dp[j]; // 保存dp[j]的值，用于更新dp[j+1]
        dp[j] = min(dp[j], temp1) + triangle[i][j]; // 更新dp[j]
        temp1 = temp2; // 更新temp1
      }
      dp[i] = temp1 + triangle[i][i]; // 更新dp[i]
    }

    // 找出dp数组中的最小值
    int minTotal = dp[0];
    for (int i = 1; i < n; i++) {
      minTotal = min(minTotal, dp[i]);
    }

    return minTotal;
  }
}

## 递归和动规关系

递归是一种程序的实现方式：函数的自我调用

```dart
Function(x) {
	...
	Funciton(x-1);
	...
}
```

动态规划：是一种解决问 题的思想，大规模问题的结果，是由小规模问 题的结果运算得来的。动态规划可用递归来实现(Memorization Search)

## 使用场景

满足两个条件

- 满足以下条件之一
  - 求最大/最小值（Maximum/Minimum ）
  - 求是否可行（Yes/No ）
  - 求可行个数（Count(\*) ）
- 满足不能排序或者交换（Can not sort / swap ）

如题：[longest-consecutive-sequence](https://leetcode-cn.com/problems/longest-consecutive-sequence/)  位置可以交换，所以不用动态规划

## 四点要素

1. **状态 State**
   - 灵感，创造力，存储小规模问题的结果
2. 方程 Function
   - 状态之间的联系，怎么通过小的状态，来算大的状态
3. 初始化 Intialization
   - 最极限的小状态是什么, 起点
4. 答案 Answer
   - 最大的那个状态是什么，终点

## 常见四种类型

1. Matrix DP (10%)
1. Sequence (40%)
1. Two Sequences DP (40%)
1. Backpack (10%)

> 注意点
>
> - 贪心算法大多题目靠背答案，所以如果能用动态规划就尽量用动规，不用贪心算法

## 1、矩阵类型（10%）

### [minimum-path-sum](https://leetcode-cn.com/problems/minimum-path-sum/)

> 给定一个包含非负整数的  *m* x *n*  网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

思路：动态规划
1、state: f[x][y]从起点走到 x,y 的最短路径
2、function: f[x][y] = min(f[x-1][y], f[x][y-1]) + A[x][y]
3、intialize: f[0][0] = A[0][0]、f[i][0] = sum(0,0 -> i,0)、 f[0][i] = sum(0,0 -> 0,i)
4、answer: f[n-1][m-1]

```dart
class Solution {
  int minPathSum(List<List<int>> grid) {
    int m = grid.length;
    int n = grid[0].length;

    // 自顶向下的动态规划
    // 计算从(0,0)到(i,j)的最小路径和
    for (int i = 1; i < m; i++) {
      grid[i][0] += grid[i - 1][0];
    }
    for (int j = 1; j < n; j++) {
      grid[0][j] += grid[0][j - 1];
    }
    for (int i = 1; i < m; i++) {
      for (int j = 1; j < n; j++) {
        grid[i][j] += grid[i - 1][j] < grid[i][j - 1] ? grid[i - 1][j] : grid[i][j - 1];
      }
    }

    // 返回从(0,0)到(m-1,n-1)的最小路径和
    return grid[m - 1][n - 1];
  }
}

```

### [unique-paths](https://leetcode-cn.com/problems/unique-paths/)

> 一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。
> 机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。
> 问总共有多少条不同的路径？

```dart
class Solution {
  int uniquePaths(int m, int n) {
    var dp = List<List<int>>.generate(m, (_) => List<int>.filled(n, 0));

    // 初始化第一行和第一列的格子为 1
    for (var i = 0; i < m; i++) {
      dp[i][0] = 1;
    }
    for (var j = 0; j < n; j++) {
      dp[0][j] = 1;
    }

    // 计算其他格子的路径数
    for (var i = 1; i < m; i++) {
      for (var j = 1; j < n; j++) {
        dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
      }
    }

    return dp[m - 1][n - 1];
  }
}

```

### [unique-paths-ii](https://leetcode-cn.com/problems/unique-paths-ii/)

> 一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。
> 机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。
> 问总共有多少条不同的路径？
> 现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

```go
class Solution {
  int uniquePathsWithObstacles(List<List<int>> obstacleGrid) {
    int m = obstacleGrid.length;
    int n = obstacleGrid[0].length;

    //初始化dp数组
    var dp = List.generate(m, (_) => List.filled(n, 0));

    //对于第一行和第一列，如果有障碍，则该点之后的所有点都不能到达，因此dp数组的初始值应为0
    for (int i = 0; i < m && obstacleGrid[i][0] == 0; i++) {
      dp[i][0] = 1;
    }
    for (int j = 0; j < n && obstacleGrid[0][j] == 0; j++) {
      dp[0][j] = 1;
    }

    //填充dp数组
    for (int i = 1; i < m; i++) {
      for (int j = 1; j < n; j++) {
        if (obstacleGrid[i][j] == 0) {  //如果当前格子没有障碍
          dp[i][j] = dp[i - 1][j] + dp[i][j - 1];  //从上面和左边的格子到达当前格子的路径数之和
        }
      }
    }

    return dp[m - 1][n - 1];  //返回从左上角到右下角的路径数
  }
}

```

## 2、序列类型（40%）

### [climbing-stairs](https://leetcode-cn.com/problems/climbing-stairs/)

> 假设你正在爬楼梯。需要  *n*  阶你才能到达楼顶。

```dart
class Solution {
  int climbStairs(int n) {
    // 如果楼梯只有一级或两级，只有一种方式
    if (n == 1 || n == 2) {
      return n;
    }

    // 定义一个数组用来存储每一级楼梯的不同方式数目
    var dp = List<int>.filled(n, 0);

    // 初始化dp数组前两个元素
    dp[0] = 1;
    dp[1] = 2;

    // 从第三个元素开始遍历dp数组，逐步计算出每一级楼梯的不同方式数目
    for (var i = 2; i < n; i++) {
      dp[i] = dp[i - 1] + dp[i - 2];
    }

    // 返回最后一个元素，即为n级楼梯的不同方式数目
    return dp[n - 1];
  }
}

```

### [jump-game](https://leetcode-cn.com/problems/jump-game/)

> 给定一个非负整数数组，你最初位于数组的第一个位置。
> 数组中的每个元素代表你在该位置可以跳跃的最大长度。
> 判断你是否能够到达最后一个位置。

```dart
class Solution {
  bool canJump(List<int> nums) {
    var n = nums.length;
    var maxJump = 0;
    
    // 遍历每个位置，更新当前能到达的最大距离
    for (var i = 0; i < n; i++) {
      // 如果当前位置超出当前能到达的最大距离，说明无法到达当前位置
      if (i > maxJump) {
        return false;
      }
      // 更新当前能到达的最大距离
      maxJump = max(maxJump, i + nums[i]);
    }
    
    return true;
  }
}

```

### [jump-game-ii](https://leetcode-cn.com/problems/jump-game-ii/)

> 给定一个非负整数数组，你最初位于数组的第一个位置。
> 数组中的每个元素代表你在该位置可以跳跃的最大长度。
> 你的目标是使用最少的跳跃次数到达数组的最后一个位置。

```dart
class Solution {
  int jump(List<int> nums) {
    int n = nums.length;
    int end = 0; // 当前能够到达的最远位置
    int farthest = 0; // 经过最少步数能够到达的最远位置
    int jumps = 0; // 跳跃次数

    for (int i = 0; i < n - 1; i++) {
      farthest = max(farthest, i + nums[i]); // 更新能够到达的最远位置

      if (end == i) { // 需要跳一步
        jumps++;
        end = farthest; // 更新当前能够到达的最远位置
      }
    }

    return jumps;
  }
}

```


### [palindrome-partitioning-ii](https://leetcode-cn.com/problems/palindrome-partitioning-ii/)

> 给定一个字符串 _s_，将 _s_ 分割成一些子串，使每个子串都是回文串。
> 返回符合要求的最少分割次数。

```dart
class Solution {
  int minCut(String s) {
    var n = s.length;

    // dp[i]表示字符串s[i..n-1]的最小分割次数
    var dp = List<int>.filled(n, 0);

    // isPalindrome[i][j]表示s[i..j]是否为回文字符串
    var isPalindrome = List<List<bool>>.generate(n, (_) => List<bool>.filled(n, false));

    // 初始化dp数组，最多需要分割n-1次
    for (var i = 0; i < n; i++) {
      dp[i] = n - i - 1;
    }

    // 从右向左遍历，计算dp和isPalindrome数组
    for (var i = n - 1; i >= 0; i--) {
      for (var j = i; j < n; j++) {
        if (s[i] == s[j] && (j - i <= 1 || isPalindrome[i + 1][j - 1])) {
          isPalindrome[i][j] = true;

          // 如果s[i..j]是回文字符串，计算dp[i]的值
          if (j == n - 1) {
            dp[i] = 0;
          } else {
            dp[i] = dp[i].compareTo(dp[j + 1] + 1) > 0 ? dp[j + 1] + 1 : dp[i];
          }
        }
      }
    }

    return dp[0];
  }
}

```

注意点

- 判断回文字符串时，可以提前用动态规划算好，减少时间复杂度

### [longest-increasing-subsequence](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

> 给定一个无序的整数数组，找到其中最长上升子序列的长度。

```go
class Solution {
  int lengthOfLIS(List<int> nums) {
    // 如果数组为空，则LIS长度为0
    if (nums.isEmpty) {
      return 0;
    }

    // 定义一个数组dp，dp[i]表示以第i个数字为结尾的LIS长度
    var dp = List<int>.filled(nums.length, 1);

    // 定义LIS的最大长度变量
    var maxLength = 1;

    // 遍历数组，计算dp[i]的值
    for (var i = 1; i < nums.length; i++) {
      // 遍历i之前的数字，找到所有小于nums[i]的数字，并计算dp[i]的值
      for (var j = 0; j < i; j++) {
        if (nums[j] < nums[i]) {
          dp[i] = dp[i].compareTo(dp[j] + 1) == 1 ? dp[i] : dp[j] + 1;
        }
      }

      // 更新LIS的最大长度
      maxLength = maxLength.compareTo(dp[i]) == 1 ? maxLength : dp[i];
    }

    return maxLength;
  }
}

```

### [word-break](https://leetcode-cn.com/problems/word-break/)

> 给定一个**非空**字符串  *s*  和一个包含**非空**单词列表的字典  *wordDict*，判定  *s*  是否可以被空格拆分为一个或多个在字典中出现的单词。

```dart
class Solution {
  bool wordBreak(String s, List<String> wordDict) {
    // 将单词列表转为 set 方便查找
    var dict = wordDict.toSet();

    // 定义一个数组用来记录 s 中前 i 个字符是否可以被单词拆分
    var dp = List.filled(s.length + 1, false);
    dp[0] = true; // 空字符串可以被任何单词拆分

    // 遍历 s 的所有子串
    for (var i = 1; i <= s.length; i++) {
      for (var j = 0; j < i; j++) {
        // 如果前 j 个字符可以被单词拆分，且 j 到 i 的子串在单词列表中出现过
        // 那么前 i 个字符也可以被单词拆分
        if (dp[j] && dict.contains(s.substring(j, i))) {
          dp[i] = true;
          break; // 如果已经可以被拆分了，就不用继续查找了
        }
      }
    }

    return dp[s.length];
  }
}

```

小结

常见处理方式是给 0 位置占位，这样处理问题时一视同仁，初始化则在原来基础上 length+1，返回结果 f[n]

- 状态可以为前 i 个
- 初始化 length+1
- 取值 index=i-1
- 返回值：f[n]或者 f[m][n]

## Two Sequences DP（40%）

### [longest-common-subsequence](https://leetcode-cn.com/problems/longest-common-subsequence/)

> 给定两个字符串  text1 和  text2，返回这两个字符串的最长公共子序列。
> 一个字符串的   子序列   是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
> 例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。

```dart
class Solution {
  int longestCommonSubsequence(String text1, String text2) {
    var m = text1.length, n = text2.length;
    var dp = List.generate( // 初始化一个二维列表
      m + 1, // 行数为 m + 1
      (i) => List.generate(n + 1, (j) => 0), // 列数为 n + 1，每个元素初始值为 0
    );

    for (var i = 1; i <= m; i++) {
      for (var j = 1; j <= n; j++) {
        if (text1[i - 1] == text2[j - 1]) { // 如果两个字符相同，则更新对角线元素的值
          dp[i][j] = dp[i - 1][j - 1] + 1;
        } else { // 否则更新上方或左方元素的值中的较大值
          dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
      }
    }

    return dp[m][n]; // 返回右下角元素的值，即最长公共子序列的长度
  }
}

```


### [edit-distance](https://leetcode-cn.com/problems/edit-distance/)

> 给你两个单词  word1 和  word2，请你计算出将  word1  转换成  word2 所使用的最少操作数  
> 你可以对一个单词进行如下三种操作：
> 插入一个字符
> 删除一个字符
> 替换一个字符

思路：和上题很类似，相等则不需要操作，否则取删除、插入、替换最小操作次数的值+1

```dart
class Solution {
  int minDistance(String word1, String word2) {
    int m = word1.length;
    int n = word2.length;

    // 创建一个二维数组 dp 用于存储子问题的解
    // dp[i][j] 表示将 word1[0..i-1] 转换为 word2[0..j-1] 的最少操作数
    var dp = List.generate(m + 1, (_) => List.filled(n + 1, 0));

    // 初始化边界条件
    for (var i = 1; i <= m; i++) {
      dp[i][0] = i;
    }
    for (var j = 1; j <= n; j++) {
      dp[0][j] = j;
    }

    // 自底向上计算子问题的解
    for (var i = 1; i <= m; i++) {
      for (var j = 1; j <= n; j++) {
        if (word1[i - 1] == word2[j - 1]) {
          // 如果 word1[i-1] 和 word2[j-1] 相同，不需要操作
          dp[i][j] = dp[i - 1][j - 1];
        } else {
          // 否则需要进行操作，取三种操作中的最小值
          dp[i][j] = 1 + min(dp[i - 1][j - 1], min(dp[i - 1][j], dp[i][j - 1]));
        }
      }
    }

    // 返回最终问题的解
    return dp[m][n];
  }
}

```

说明

> 另外一种做法：MAXLEN(a,b)-LCS(a,b)

## 零钱和背包（10%）

### [coin-change](https://leetcode-cn.com/problems/coin-change/)

> 给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回  -1。

思路：和其他 DP 不太一样，i 表示钱或者容量

```go
class Solution {
  int coinChange(List<int> coins, int amount) {
    var dp = List<int>.filled(amount + 1, amount + 1);
    dp[0] = 0;

    for (var i = 1; i <= amount; i++) {
      for (var coin in coins) {
        if (coin <= i) { // 如果硬币面值小于等于 i，说明可以使用该硬币
          dp[i] = dp[i].compareTo(dp[i - coin] + 1) > 0 // 取当前状态和使用该硬币状态中较小的那个
              ? dp[i - coin] + 1
              : dp[i];
        }
      }
    }

    return dp[amount] > amount ? -1 : dp[amount];
  }
}

```

注意

> dp[i-a[j]] 决策 a[j]是否参与

### [backpack](https://www.lintcode.com/problem/backpack/description)

> 在 n 个物品中挑选若干物品装入背包，最多能装多满？假设背包的大小为 m，每个物品的大小为 A[i]

```dart
int backPack(int m, List<int> A) {
  int n = A.length;
  
  // 初始化一个二维数组dp，用来记录当前物品放或不放时的背包容量和价值
  List<List<int>> dp = List.generate(n + 1, (_) => List.filled(m + 1, 0));
  
  // 遍历每个物品
  for (int i = 1; i <= n; i++) {
    // 遍历每个背包容量
    for (int j = 1; j <= m; j++) {
      // 如果当前物品的大小大于背包容量j，则当前物品不能放入背包，取上一个物品的最优解
      if (A[i - 1] > j) {
        dp[i][j] = dp[i - 1][j];
      }
      // 如果当前物品的大小小于等于背包容量j，则可以考虑将当前物品放入背包
      else {
        // 取放入当前物品和不放入当前物品两种情况下的最大价值
        dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - A[i - 1]] + A[i - 1]);
      }
    }
  }

  return dp[n][m];  // 返回n个物品放入容量为m的背包中所能得到的最大价值
}


```

### [backpack-ii](https://www.lintcode.com/problem/backpack-ii/description)

> 有 `n` 个物品和一个大小为 `m` 的背包. 给定数组 `A` 表示每个物品的大小和数组 `V` 表示每个物品的价值.
> 问最多能装入背包的总价值是多大?

思路：f[i][j] 前 i 个物品，装入 j 背包 最大价值

```dart
int backPackII(int m, List<int> A, List<int> V) {
  // 初始化二维数组dp
  List<List<int>> dp = List.generate(A.length + 1, (_) => List.filled(m + 1, 0));
  for (int i = 1; i <= A.length; i++) {
    for (int j = 1; j <= m; j++) {
      // 如果当前物品的体积大于背包容量，则不能放入
      if (A[i - 1] > j) {
        dp[i][j] = dp[i - 1][j];
      }
      // 否则，需要考虑放入或不放入该物品的情况
      else {
        dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - A[i - 1]] + V[i - 1]);
      }
    }
  }
  return dp[A.length][m];
}

```

## 练习

Matrix DP (10%)

- [ ] [triangle](https://leetcode-cn.com/problems/triangle/)
- [ ] [minimum-path-sum](https://leetcode-cn.com/problems/minimum-path-sum/)
- [ ] [unique-paths](https://leetcode-cn.com/problems/unique-paths/)
- [ ] [unique-paths-ii](https://leetcode-cn.com/problems/unique-paths-ii/)

Sequence (40%)

- [ ] [climbing-stairs](https://leetcode-cn.com/problems/climbing-stairs/)
- [ ] [jump-game](https://leetcode-cn.com/problems/jump-game/)
- [ ] [jump-game-ii](https://leetcode-cn.com/problems/jump-game-ii/)
- [ ] [palindrome-partitioning-ii](https://leetcode-cn.com/problems/palindrome-partitioning-ii/)
- [ ] [longest-increasing-subsequence](https://leetcode-cn.com/problems/longest-increasing-subsequence/)
- [ ] [word-break](https://leetcode-cn.com/problems/word-break/)

Two Sequences DP (40%)

- [ ] [longest-common-subsequence](https://leetcode-cn.com/problems/longest-common-subsequence/)
- [ ] [edit-distance](https://leetcode-cn.com/problems/edit-distance/)

Backpack & Coin Change (10%)

- [ ] [coin-change](https://leetcode-cn.com/problems/coin-change/)
- [ ] [backpack](https://www.lintcode.com/problem/backpack/description)
- [ ] [backpack-ii](https://www.lintcode.com/problem/backpack-ii/description)
