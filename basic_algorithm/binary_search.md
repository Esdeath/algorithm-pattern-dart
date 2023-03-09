# 二分搜索

## 二分搜索模板

给一个**有序数组**和目标值，找第一次/最后一次/任何一次出现的索引，如果没有出现返回-1

模板四点要素

- 1、初始化：start=0、end=len-1
- 2、循环退出条件：start + 1 < end
- 3、比较中点和目标值：A[mid] ==、 <、> target
- 4、判断最后两个元素是否符合：A[start]、A[end] ? target

时间复杂度 O(logn)，使用场景一般是有序数组的查找

典型示例

[binary-search](https://leetcode-cn.com/problems/binary-search/)

> 给定一个  n  个元素有序的（升序）整型数组  nums 和一个目标值  target  ，写一个函数搜索  nums  中的 target，如果目标值存在返回下标，否则返回 -1。

```dart
int binSearch(List<int> list, int target) {
  if (list.isEmpty) {
    return -1;
  }
  int start = 0;
  int end = list.length - 1;
  while (start + 1 < end) {
    int mid = start + (end - start) ~/ 2;
    if (list[mid] > target) {
      end = mid;
    } else if (list[mid] == target) {
      return mid;
    } else {
      start = mid;
    }
  }

  if (list[start] == target) {
    return start;
  }
  if (list[end] == target) {
    return end;
  }
  return -1;
}
```
大部分二分查找类的题目都可以用这个模板，然后做一点特殊逻辑即可


## 常见题目

### [search-for-range](https://www.lintcode.com/problem/search-for-a-range/description)

> 给定一个包含 n 个整数的排序数组，找出给定目标值 target 的起始和结束位置。
> 如果目标值不在数组中，则返回`[-1, -1]`

思路：核心点就是找第一个 target 的索引，和最后一个 target 的索引，所以用两次二分搜索分别找第一次和最后一次的位置

```dart
List<int> searchRange(List<int> a, int target) {
  List<int> res = [-1, -1];
  if (a.isEmpty) {
    return res;
  }

  //找到相等的数的初始位置
  int start = 0;
  int end = a.length - 1;

  while (start + 1 < end) {
    int mid = start + (end - start) ~/ 2;
    if (a[mid] == target) {
      end = mid;
    } else if (a[mid] > target) {
      end = mid;
    } else {
      start = mid;
    }
  }
  if (a[start] == target) {
    res = [start, start];
  } else if (a[end] == target) {
    res = [end, end];
  }

  //找到相等的数的结束位置
  start = 0;
  end = a.length - 1;

  while (start + 1 < end) {
    int mid = start + (end - start) ~/ 2;
    if (a[mid] == target) {
      start = mid;
    } else if (a[mid] > target) {
      end = mid;
    } else {
      start = mid;
    }
  }
  if (a[end] == target) {
    res[1] = end;
  } else if (a[start] == target) {
    res[1] = start;
  }

  return res;
}

```

### [search-insert-position](https://leetcode-cn.com/problems/search-insert-position/)

> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

```dart
int searchInsert(List<int> nums, int target) {
    if(nums.isEmpty){
        return 0;
    }
    int start = 0;
    int end = nums.length - 1;
    while(start + 1 < end){
        int mid = start + ((end - start)>>1);
        if(nums[mid] == target){
            start = mid; 
        } else if(nums[mid] < target){
            start = mid;
        } else {
            end = mid;
        }
    }


    if(nums[end] < target) {
        return end + 1;
    }
    if(nums[end] >= target && nums[start] < target){
        return end;
    }
    if(nums[start] >= target) {
        return start;
    }
    return 0;
}
```

### [search-a-2d-matrix](https://leetcode-cn.com/problems/search-a-2d-matrix/)

> 编写一个高效的算法来判断  m x n  矩阵中，是否存在一个目标值。该矩阵具有如下特性：
>
> - 每行中的整数从左到右按升序排列。
> - 每行的第一个整数大于前一行的最后一个整数。

```dart
bool searchMatrix(List<List<int>> matrix, int target) {
    if(matrix.isEmpty || matrix[0].isEmpty){
        return false;
    }
    int row = matrix.length;
    int col = matrix[0].length;
    int start = 0;
    int end = row*col -1;
    while(start + 1 < end) {
        int mid = start + ((end - start)>>1);
        if(target == matrix[mid~/col][mid%col]) {
            end = mid;
        } else if(target > matrix[mid~/col][mid%col]){
            start = mid;
        } else {
            end = mid;
        }
    }
    if(matrix[start~/col][start%col] == target || matrix[end~/col][end%col] == target){
        return true;
    }

    return false;
}
```

### [first-bad-version](https://leetcode-cn.com/problems/first-bad-version/)

> 假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。
> 你可以通过调用  bool isBadVersion(version)  接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

```dart
int firstBadVersion(int n) {
    if(n == 0) return 0;
    int start = 1;
    int end = n;
    while(start + 1 < end){
        int mid = start + ((end - start)>>1);
        if(isBadVersion(mid)) {
            end = mid;
        } else {
            start = mid;
        }
    }
    if(isBadVersion(start)) {
        return start;
    }
    if(isBadVersion(end)){
        return end;
    }
    return -1;
}
```

### [find-minimum-in-rotated-sorted-array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

> 假设按照升序排序的数组在预先未知的某个点上进行了旋转( 例如，数组  [0,1,2,4,5,6,7] 可能变为  [4,5,6,7,0,1,2] )。
> 请找出其中最小的元素。

```dart
int findMin(List<int> nums) {
    if(nums.isEmpty) return -1;
    int start = 0;
    int end = nums.length - 1;

    int target = nums.last;
    while(start + 1 < end){
        int mid = start + ((end - start)>>1);
        if(target > nums[mid]) {
            end = mid;
        } else if(target < nums[mid]){
            start = mid;
        }
    }
    return  min(nums[start],nums[end]);
}
```

### [find-minimum-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

> 假设按照升序排序的数组在预先未知的某个点上进行了旋转
> ( 例如，数组  [0,1,2,4,5,6,7] 可能变为  [4,5,6,7,0,1,2] )。
> 请找出其中最小的元素。(包含重复元素)

```dart
int findMin(List<int> nums) {
    if(nums.isEmpty) return -1;
    int start = 0;
    int end = nums.length - 1;

    int target = nums.last;
    while(start + 1 < end){
        int mid = start + ((end - start)>>1);
        //去除重复的元素
        while(start + 1< end && nums[end] == nums[end-1]) {
            end--;
        }
        while(start + 1 < end && nums[start] == nums[start+1]) {
            start++;
        } 
        if(target > nums[mid]) {
            end = mid;
        } else if(target < nums[mid]){
            start = mid;
        }
    }
    return  min(nums[start],nums[end]);
}
```

### [search-in-rotated-sorted-array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

> 假设按照升序排序的数组在预先未知的某个点上进行了旋转。
> ( 例如，数组  [0,1,2,4,5,6,7]  可能变为  [4,5,6,7,0,1,2] )。
> 搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回  -1 。
> 你可以假设数组中不存在重复的元素。

```dart

int search(List<int> nums, int target) {
    int start = 0;
    int end = nums.length - 1;
    while(start + 1 < end){
        int mid = start + ((end - start)>>1);
        if(nums[mid] == target) return mid;
        if(nums[mid] > nums[start]) {
            if(nums[mid] >= target && nums[start] <= target){
                end = mid;
            } else {
                start = mid;
            }
        } else if(nums[mid] < nums[end]) {
            if(nums[end] >= target && nums[mid] <= target){
                start = mid;
            } else {
                end = mid;
            }
        } 
    }
    if(nums[start] == target) return start;
    if(nums[end] == target) return end;
    return -1;
}
```

### [search-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

> 假设按照升序排序的数组在预先未知的某个点上进行了旋转。
> ( 例如，数组  [0,0,1,2,2,5,6]  可能变为  [2,5,6,0,0,1,2] )。
> 编写一个函数来判断给定的目标值是否存在于数组中。若存在返回  true，否则返回  false。(包含重复元素)

```dart
bool search(List<int> nums, int target) {
    if(search1(nums,target) == -1){
        return false;
    }
    return true;
}

int search1(List<int> nums, int target) {
    int start = 0;
    int end = nums.length - 1;
    while(start + 1 < end){
        while(start < end && nums[start] == nums[start + 1] ){
            start++;
        }
        while(start < end && nums[end] == nums[end - 1] ){
            end--;
        }
        int mid = start + ((end - start)>>1);

        if(nums[mid] == target) return mid;
        if(nums[mid] > nums[start]) {
            if(nums[mid] >= target && nums[start] <= target){
                end = mid;
            } else {
                start = mid;
            }
        } else if(nums[mid] < nums[end]) {
            if(nums[end] >= target && nums[mid] <= target){
                start = mid;
            } else {
                end = mid;
            }
        } 
    }
    if(nums[start] == target) return start;
    if(nums[end] == target) return end;
    return -1;
}
```

## 总结

二分搜索核心四点要素（必背&理解）

- 1、初始化：start=0、end=len-1
- 2、循环退出条件：start + 1 < end
- 3、比较中点和目标值：A[mid] ==、 <、> target
- 4、判断最后两个元素是否符合：A[start]、A[end] ? target

## 练习题

- [ ] [search-for-range](https://www.lintcode.com/problem/search-for-a-range/description)
- [ ] [search-insert-position](https://leetcode-cn.com/problems/search-insert-position/)
- [ ] [search-a-2d-matrix](https://leetcode-cn.com/problems/search-a-2d-matrix/)
- [ ] [first-bad-version](https://leetcode-cn.com/problems/first-bad-version/)
- [ ] [find-minimum-in-rotated-sorted-array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)
- [ ] [find-minimum-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)
- [ ] [search-in-rotated-sorted-array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
- [ ] [search-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)
