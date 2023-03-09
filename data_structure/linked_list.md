# 链表

## 基本技能

链表相关的核心点

- null/nil 异常处理
- dummy node 
- 快慢指针
- 插入一个节点到排序链表
- 从一个链表中移除一个节点
- 翻转链表
- 合并两个链表
- 找到链表的中间节点

## 常见题型

### [remove-duplicates-from-sorted-list](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

> 给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

```dart
ListNode? deleteDuplicates(ListNode? head) {
  var p = head;
  while (p != null && p.next != null) {
    if(p.val == p.next?.val) {
      p.next = p.next?.next;
    } else {
      p = p.next;
    }
  }
  return head;
}
```

### [remove-duplicates-from-sorted-list-ii](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

> 给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中   没有重复出现的数字。

思路：链表头结点可能被删除，所以用 dummy node 辅助删除

```dart
ListNode? deleteDuplicates(ListNode? head) {
  ListNode? dummy = ListNode();
  dummy.val = -1;
  dummy.next = head;

  ListNode? p = dummy;
  while (p?.next != null && p?.next?.next != null) {
    if (p?.next?.val == p?.next?.next?.val) {
      var val = p?.next?.val;
      while (p?.next != null && val == p?.next?.val) {
        p?.next = p.next?.next;
      }
    } else {
      p = p?.next;
    }
  }
  return dummy.next;
}
```

注意点
• A->B->C 删除 B，A.next = C
• 删除用一个 Dummy Node 节点辅助（允许头节点可变）
• 访问 X.next 、X.value 一定要保证 X != nil

### [reverse-linked-list](https://leetcode-cn.com/problems/reverse-linked-list/)

> 反转一个单链表。

思路：采用递归的方法进行翻转链表

```dart
  ListNode? reverseList(ListNode? head) {
    ListNode? prev;
    ListNode? post;
    ListNode? curr = head;
    while (curr != null) {
      post = curr.next;
      curr.next = prev;
      prev = curr;
      curr = post;
    }

    return prev;
  }
```

### [reverse-linked-list-ii](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

> 反转从位置  *m*  到  *n*  的链表。请使用一趟扫描完成反转。

思路：先遍历到 m 处，翻转，再拼接后续，注意指针处理

```dart
ListNode? reverseBetween(ListNode? head, int m, int n) {
  if (head == null) {
    return head;
  }
  ListNode? dummy = ListNode();
  dummy.val = 0;
  dummy.next = head;
  head = dummy;

  int i = 0;
  ListNode? pre = head;
  while (i < m) {
    pre = head;
    head = head?.next;
    i++;
  }

  pre?.next = null;

  ListNode? tail = head;

  ListNode? newHead;
  ListNode? next;
  while (i <= n) {
    next = head?.next;
    head?.next = newHead;
    newHead = head;
    head = next;
    i++;
  }

  pre?.next = newHead;
  tail?.next = head;
  return dummy.next;
}
```

### [merge-two-sorted-lists](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

> 将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

思路：通过 dummy node 链表，连接各个元素

```dart
ListNode? mergeTwoLists(ListNode? l1, ListNode? l2) {

  ListNode? dummy = ListNode();
  ListNode? p = dummy;
  while (l1 != null && l2 != null) {
    if (l1.val! < l2.val!) {
      p?.next = l1;
      l1 = l1.next;
    } else {
      p?.next = l2;
      l2 = l2.next;
    }
    p = p?.next;
  }
  if(l1 != null){
    p?.next = l1;
  }
  if (l2 != null) {
    p?.next = l2;
  }
  return dummy.next;
}
```

### [partition-list](https://leetcode-cn.com/problems/partition-list/)

> 给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于  *x*  的节点都在大于或等于  *x*  的节点之前。

思路：将大于 x 的节点，放到另外一个链表，最后连接这两个链表

```go

ListNode? partition(ListNode? head,int x) {
  //当前链表的dummy节点
  ListNode? dummy = ListNode();
  dummy.val = x-1;
  dummy.next = head;
  head = dummy;

  //新链表的dummy节点
  ListNode? newDummy = ListNode();
  ListNode? tail = newDummy;

  while(head?.next != null){
    if ((head?.next?.val)! < x) {
      head = head?.next;
    } else {
      ListNode? temp = head?.next;
      head?.next = temp?.next;
      tail?.next = temp;
      tail = tail?.next;
    }
  }

  tail?.next = null;
  head?.next = newDummy.next;
  return dummy.next;
}
```

哑巴节点使用场景

> 当头节点不确定的时候，使用哑巴节点

### [sort-list](https://leetcode-cn.com/problems/sort-list/)

> 在  *O*(*n* log *n*) 时间复杂度和常数级空间复杂度下，对链表进行排序。

思路：归并排序，找中点和合并操作

```dart
ListNode? sortList(ListNode? head) {
  if (head == null || head.next == null) {
    return head;
  }
  ListNode? start = head;
  //找到中间点 断开
  ListNode? mid = findMiddle(head);
  ListNode? tail = mid?.next;
  mid?.next = null;

  ListNode? left = sortList(start);
  ListNode? right = sortList(tail);
  ListNode? result = mergeLists(left, right);
  return result;
}

ListNode? findMiddle(ListNode? head) {
  ListNode? slow = head;
  ListNode? fast = head?.next;
  while (fast != null && fast.next != null) {
    slow = slow?.next;
    fast = fast.next?.next;
  }
  return slow;
}

ListNode? mergeLists(ListNode? left, ListNode? right) {
  ListNode? dummy = ListNode();
  ListNode? p = dummy;
  while (left != null && right != null) {
    if (left.val! < right.val!) {
      p?.next = left;
      left = left.next;
    } else {
      p?.next = right;
      right = right.next;
    }
    p = p?.next;
  }
  if (left != null) {
    p?.next = left;
  }
  if (right != null) {
    p?.next = right;
  }
  return dummy.next;
}
```

注意点

- 快慢指针 判断 fast 及 fast.Next 是否为 nil 值
- 递归 mergeSort 需要断开中间节点
- 递归返回条件为 head 为 nil 或者 head.Next 为 nil

### [reorder-list](https://leetcode-cn.com/problems/reorder-list/)

> 给定一个单链表  *L*：*L*→*L*→…→*L\_\_n*→*L*
> 将其重新排列后变为： *L*→*L\_\_n*→*L*→*L\_\_n*→*L*→*L\_\_n*→…

思路：找到中点断开，翻转后面部分，然后合并前后两个链表

```dart
void reorderList(ListNode? head) {
  if (head == null || head.next == null) {
    return ;
  }
  ListNode? start = head;

  ListNode? mid = findMiddle(head);
  ListNode? tail = mid?.next;
  mid?.next = null;
  tail = reverseList(tail);

  while (start != null && tail != null) {
    ListNode? startTemp = start.next;
    start.next = tail;
    ListNode? tailTemp = tail.next;
    tail.next = startTemp;
    start = startTemp;
    tail = tailTemp;
  }
}

ListNode? findMiddle(ListNode? head) {
  ListNode? slow = head;
  ListNode? fast = head?.next;
  while (fast != null && fast.next != null) {
    slow = slow?.next;
    fast = fast.next?.next;
  }
  return slow;
}

  ListNode? reverseList(ListNode? head) {
    ListNode? prev;
    ListNode? post;
    ListNode? curr = head;
    while (curr != null) {
      post = curr.next;
      curr.next = prev;
      prev = curr;
      curr = post;
    }

    return prev;
  }

```

### [linked-list-cycle](https://leetcode-cn.com/problems/linked-list-cycle/)

> 给定一个链表，判断链表中是否有环。

思路：快慢指针，快慢指针相同则有环，证明：如果有环每走一步快慢指针距离会减 1
![fast_slow_linked_list](https://img.fuiboom.com/img/fast_slow_linked_list.png)

```dart
bool hasCycle(ListNode? head) {
  bool hasCycle = false;
  if (head == null || head.next == null) {
    return hasCycle;
  }
  ListNode? slow = head;
  ListNode? fast = head.next;
  while (fast != null && fast.next != null) {
    if (slow == fast) {
      return true;
    }
    slow = slow?.next;
    fast = fast.next?.next;
  }
  return false;
}
```

### [linked-list-cycle-ii](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

> 给定一个链表，返回链表开始入环的第一个节点。  如果链表无环，则返回  `null`。

思路：快慢指针，快慢相遇之后，慢指针回到头，快慢指针步调一致一起移动，相遇点即为入环点
![cycled_linked_list](https://img.fuiboom.com/img/cycled_linked_list.png)

```go

ListNode? detectCycle(ListNode? head) {
    // 思路：快慢指针，快慢相遇之后，慢指针回到头，快慢指针步调一致一起移动，相遇点即为入环点
  if (head == null || head.next == null) {
    return null;
  }
  ListNode? slow = head;
  ListNode? fast = head.next;

  while (fast != null && fast.next != null) {
    if (slow == fast) {
    // 慢指针重新从头开始移动，快指针从第一次相交点下一个节点开始移动
      slow = head;
      fast = fast.next;
      while (fast != slow) {
        slow = slow?.next;
        fast = fast?.next;
      }
      return fast;
    }
    slow = slow?.next;
    fast = fast.next;
  }
  return null;
}
```

坑点

- 指针比较时直接比较对象，不要用值比较，链表中有可能存在重复值情况
- 第一次相交后，快指针需要从下一个节点开始和头指针一起匀速移动

### [palindrome-linked-list](https://leetcode-cn.com/problems/palindrome-linked-list/)

> 请判断一个链表是否为回文链表。

```dart
class Solution {
  bool isPalindrome(ListNode? head) {
      var ans=[];
      while(head!=null){
        ans.add(head.val);
        head=head.next;
      }
      var n=ans.length;
      for(var i=0;i<=n/2;i++){
          if(ans[i]!=ans[n-i-1])return false;
      }
      return true;
  }
}
```

### [copy-list-with-random-pointer](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

> 给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。
> 要求返回这个链表的 深拷贝。

思路：1、hash 表存储指针，2、复制节点跟在原节点后面

```dart

class Node {
  int? val;
  Node? next;
  Node? random;
  Node(val, next);
}

Node? copyRandomList(Node? head) {
  if (head == null) {
    return head;
  }
  Node? p = head;
  while (p != null) {
    Node? newNode = Node(p.val, p.next);
    p.next = newNode;
    p = newNode.next;
  }

  p = head;
  while (p != null && p.next != null) {
    p.next?.random = p.random?.next;
    p = p.next?.next;
  }

  p = head;
  Node? dummy = Node(0, null);
  Node? pDummy = dummy;
  while (p != null) {
    Node? temp = p.next;
    p.next = temp?.next;
    pDummy?.next = temp;
    temp?.next = null;
    pDummy = temp;
    p = p.next;
  }
  return dummy.next;
}

```

[swap-nodes-in-pairs](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

> 给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
> **你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

```go
ListNode? swapPairs(ListNode? head) {
  ListNode dummy = ListNode();
  dummy.next = head;
  head = dummy;

  while (head?.next != null && head?.next?.next != null) {
    ListNode? p = head?.next;
    ListNode? q = p?.next;

    p?.next = q?.next;
    q?.next = p;
    head?.next = q;
    head = p;
  }

  return dummy.next;
}
```

## 总结

链表必须要掌握的一些点，通过下面练习题，基本大部分的链表类的题目都是手到擒来~

- null/nil 异常处理
- dummy node 哑巴节点
- 快慢指针
- 插入一个节点到排序链表
- 从一个链表中移除一个节点
- 翻转链表
- 合并两个链表
- 找到链表的中间节点

## 练习

- [ ] [remove-duplicates-from-sorted-list](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
- [ ] [remove-duplicates-from-sorted-list-ii](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)
- [ ] [reverse-linked-list](https://leetcode-cn.com/problems/reverse-linked-list/)
- [ ] [reverse-linked-list-ii](https://leetcode-cn.com/problems/reverse-linked-list-ii/)
- [ ] [merge-two-sorted-lists](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
- [ ] [partition-list](https://leetcode-cn.com/problems/partition-list/)
- [ ] [sort-list](https://leetcode-cn.com/problems/sort-list/)
- [ ] [reorder-list](https://leetcode-cn.com/problems/reorder-list/)
- [ ] [linked-list-cycle](https://leetcode-cn.com/problems/linked-list-cycle/)
- [ ] [linked-list-cycle-ii](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
- [ ] [palindrome-linked-list](https://leetcode-cn.com/problems/palindrome-linked-list/)
- [ ] [copy-list-with-random-pointer](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)
