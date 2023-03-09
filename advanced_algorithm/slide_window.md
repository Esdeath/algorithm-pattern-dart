# 滑动窗口

## 模板

```cpp
/* 滑动窗口算法框架 */
void slidingWindow(string s, string t) {
    unordered_map<char, int> need, window;
    for (char c : t) need[c]++;

    int left = 0, right = 0;
    int valid = 0;
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 右移窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        printf("window: [%d, %d)\n", left, right);
        /********************/

        // 判断左侧窗口是否要收缩
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 左移窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
    }
}
```

需要变化的地方

- 1、右指针右移之后窗口数据更新
- 2、判断窗口是否要收缩
- 3、左指针右移之后窗口数据更新
- 4、根据题意计算结果

## 示例

[minimum-window-substring](https://leetcode-cn.com/problems/minimum-window-substring/)

> 给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字母的最小子串

```dart
class Solution {
	String minWindow(String s, String t) {
		// 初始化计数数组freq，用于记录t中每个字符出现的次数
		var freq = List<int>.filled(128, 0);
		for (var c in t.codeUnits) {
		freq[c]++;
		}
		// 初始化左右指针、最小长度、起始位置和字符匹配计数器
		var left = 0, right = 0, minLen = s.length + 1, start = 0, count = t.length;

		// 移动右指针，扩大窗口
		while (right < s.length) {
			// 如果右指针指向的字符出现在t中，则计数器减1
			if (freq[s.codeUnitAt(right++)]-- > 0) {
				count--;
			}

			// 如果当前窗口包含了t中的所有字符，则移动左指针，缩小窗口
			while (count == 0) {
				// 更新最小覆盖子串的长度和起始位置
				if (right - left < minLen) {
				minLen = right - left;
				start = left;
				}

				// 如果左指针指向的字符出现在t中，则计数器加1
				if (freq[s.codeUnitAt(left++)]++ == 0) {
				count++;
				}
			}
		}
		// 如果最小覆盖子串的长度为s.length+1，则不存在符合要求的子串，返回空字符串
		// 否则，返回最小覆盖子串
		return minLen == s.length + 1 ? '' : s.substring(start, start + minLen);
	}
}
```

[permutation-in-string](https://leetcode-cn.com/problems/permutation-in-string/)

> 给定两个字符串  **s1**  和  **s2**，写一个函数来判断  **s2**  是否包含  **s1 **的排列。

```dart
class Solution {
  bool checkInclusion(String s1, String s2) {
    var freq = List<int>.filled(128, 0);

    // 统计 s1 中每个字符出现的次数
    for (var c in s1.codeUnits) {
      freq[c]++;
    }

    var left = 0, right = 0, count = s1.length;

    while (right < s2.length) {
      // 如果当前字符在 s1 中出现过，则减少其在 freq 数组中的出现次数
      if (freq[s2.codeUnitAt(right++)]-- > 0) {
        count--;
      }

      // 如果 s1 中所有字符都出现在了当前窗口中，则返回 true
      if (count == 0) {
        return true;
      }

      // 如果当前窗口大小已经等于 s1 的长度了，则需要缩小窗口
      if (right - left == s1.length) {
        // 如果左边界字符在 s1 中出现过，则需要增加其在 freq 数组中的出现次数
        if (freq[s2.codeUnitAt(left++)]++ >= 0) {
          count++;
        }
      }
    }

    // 如果遍历整个 s2 都没找到符合条件的子串，则返回 false
    return false;
  }
}

```

[find-all-anagrams-in-a-string](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

> 给定一个字符串  **s **和一个非空字符串  **p**，找到  **s **中所有是  **p **的字母异位词的子串，返回这些子串的起始索引。

```dart
class Solution {
  List<int> findAnagrams(String s, String p) {
    var result = <int>[];

    // 记录p中每个字符出现的次数
    var target = List<int>.filled(26, 0);
    for (var i = 0; i < p.length; i++) {
      target[p.codeUnitAt(i) - 97]++;
    }

    var left = 0, right = 0, count = p.length;

    // 移动右指针，扩大窗口
    while (right < s.length) {
      // 如果当前字符在p中出现过，则count减1
      if (target[s.codeUnitAt(right++) - 97]-- > 0) {
        count--;
      }

      // 如果count变为0，说明窗口中包含了p中所有字符的排列，将左指针加入结果数组
      if (count == 0) {
        result.add(left);
      }

      // 当窗口大小等于p的长度时，移动左指针，缩小窗口
      if (right - left == p.length) {
        // 如果当前左指针指向的字符在p中出现过，则count加1
        if (target[s.codeUnitAt(left++) - 97]++ >= 0) {
          count++;
        }
      }
    }

    return result;
  }
}

```

[longest-substring-without-repeating-characters](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

> 给定一个字符串，请你找出其中不含有重复字符的   最长子串   的长度。
> 示例  1:
>
> 输入: "abcabcbb"
> 输出: 3
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

```dart
class Solution {
  int lengthOfLongestSubstring(String s) {
    int n = s.length;
    Set<String> set = Set(); // 用 Set 记录当前窗口中出现的字符
    int ans = 0, i = 0, j = 0; // ans 记录最长子串长度，i 和 j 记录窗口左右边界

    while (i < n && j < n) { // 右指针 j 向右移动，直到字符串末尾
      if (!set.contains(s[j])) { // 如果当前字符不在窗口中，加入窗口
        set.add(s[j++]);
        ans = ans > j - i ? ans : j - i; // 更新最长子串长度
      } else { // 如果当前字符已经在窗口中，左指针 i 向右移动，缩小窗口
        set.remove(s[i++]);
      }
    }

    return ans;
  }
}
```

## 总结

- 和双指针题目类似，更像双指针的升级版，滑动窗口核心点是维护一个窗口集，根据窗口集来进行处理
- 核心步骤
  - right 右移
  - 收缩
  - left 右移
  - 求结果

## 练习

- [ ] [minimum-window-substring](https://leetcode-cn.com/problems/minimum-window-substring/)
- [ ] [permutation-in-string](https://leetcode-cn.com/problems/permutation-in-string/)
- [ ] [find-all-anagrams-in-a-string](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)
- [ ] [longest-substring-without-repeating-characters](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
