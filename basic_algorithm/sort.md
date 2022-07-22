# 排序

## 常考排序

### 快速排序

```dart

void qs(List<int> nums, int left, int right) {
  if (left >= right) {
    return;
  }

  int start = left;
  int end = right;
  int pivot = nums[end];

  while (start <= end) {
    while (start <= end && nums[start] < pivot) {
      start++;
    }
    while (start <= end && nums[end] > pivot) {
      end--;
    }
    if (start <= end) {
      int temp = nums[start];
      nums[start] = nums[end];
      nums[end] = temp;
      start++;
      end--;
    }
  }
  qs(nums, left, end);
  qs(nums, start, right);
}

```

### 归并排序

```dart
void mergeSort(List<int> list, int start, int end, List<int> temp) {
  if (start >= end) {
    return;
  }
  int mid = start + (end - start) ~/ 2;
  mergeSort(list, start, mid, temp);
  mergeSort(list, mid + 1, end, temp);
  merge(list, start, end, temp);
}

void merge(List<int> list, int start, int end, List<int> temp) {
  int mid = start + (end - start) ~/ 2;

  int index = start;
  int leftIndex = start;
  int rightIndex = mid + 1;

  while (leftIndex <= mid && rightIndex <= end) {
    if (list[leftIndex] <= list[rightIndex]) {
      temp[index++] = list[leftIndex++];
    } else {
      temp[index++] = list[rightIndex++];
    }
  }

  while (leftIndex <= mid) {
    temp[index++] = list[leftIndex++];
  }

  while (rightIndex <= end) {
    temp[index++] = list[rightIndex++];
  }

  for (var i = start; i <= end; i++) {
    list[i] = temp[i];
  }
}

```

### 堆排序

用数组表示的完美二叉树 complete binary tree

> 完美二叉树 VS 其他二叉树

![image.png](https://img.fuiboom.com/img/tree_type.png)

[动画展示](https://www.bilibili.com/video/av18980178/)

![image.png](https://img.fuiboom.com/img/heap.png)


## 参考

[十大经典排序](https://www.cnblogs.com/onepixel/p/7674659.html)

[二叉堆](https://labuladong.gitbook.io/algo/shu-ju-jie-gou-xi-lie/er-cha-dui-xiang-jie-shi-xian-you-xian-ji-dui-lie)

## 练习

- [ ] 手写快排、归并
