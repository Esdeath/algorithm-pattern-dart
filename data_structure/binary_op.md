# 二进制

## 常见二进制操作

### 基本操作

a=0^a=a^0

0=a^a

由上面两个推导出：a=a^b^b

### 交换两个数

a=a^b

b=a^b

a=a^b

### 移除最后一个 1

a=n&(n-1)

### 获取最后一个 1

diff=(n&(n-1))^n

diff = n&-n;

## 常见题目

[single-number](https://leetcode-cn.com/problems/single-number/)

> 给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

```dart
int singleNumber(List<int> nums) {
  //两个相同的数异或就变成0
  //任何数和0 异或 值不变
  int result = 0;
  for (var element in nums) {
    result = result ^ element;
  }
  return result;
}

```

[single-number-ii](https://leetcode-cn.com/problems/single-number-ii/)

> 给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现了三次。找出那个只出现了一次的元素。

```dart
//1.将数组中每个数，对应的位数进行叠加。如果出现一次的那个数，那个位是0 则为 3n，如果 是1则是3n+1 
//2.然后将每一位叠加和对3求余数（3n mod 3 或者 (3n+1)mod3），余数不是1就是0
//3.将余数归位到原来的数中，就是结果
int singleNumber(List<int> nums) {
  int result = 0;
  
  for (var i = 0; i < 64; i++) {
    int sum = 0;
    for (var element in nums) {
      sum += ((element >> i) & 1);
    }
    result = result | ((sum % 3) << i);
  }
  return result;
}
```

[single-number-iii](https://leetcode-cn.com/problems/single-number-iii/)

> 给定一个整数数组  `nums`，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。

```dart
//1.对列表所有元素xor,获取的结果为 两个只出现一次的元素异或结果
//2.取出最后结果的最后一位为1的数
//3.通过这个数进行&操作可以将原数组进行划分，分到了两个不同的单元
//4.对划分后的两个单元进行xor操作，就可以获取结果
List<int> singleNumber(List<int> nums) {
  int diff = 0;
  for (var element in nums) {
    diff ^= element;
  }
  diff = diff & -diff;
  List<int> res = [0, 0];
  for (var element in nums) {
    if ((diff & element) == 0) {
      res[0] ^= element;
    } else {
      res[1] ^= element;
    }
  }
  return res;
}

```

[number-of-1-bits](https://leetcode-cn.com/problems/number-of-1-bits/)

> 编写一个函数，输入是一个无符号整数，返回其二进制表达式中数字位数为 ‘1’  的个数（也被称为[汉明重量](https://baike.baidu.com/item/%E6%B1%89%E6%98%8E%E9%87%8D%E9%87%8F)）。

```dart
int hammingWeight(int n) {
  int res = 0;
  while (n > 0) {
    n = n & (n - 1);
    res++;
  }
  return res;
}

```

[counting-bits](https://leetcode-cn.com/problems/counting-bits/)

> 给定一个非负整数  **num**。对于  0 ≤ i ≤ num  范围中的每个数字  i ，计算其二进制数中的 1 的数目并将它们作为数组返回。

```dart
List<int> countBits(int n) {
  List<int> res = [];
  for (int i = 0; i <= n; i++) {
    int temp = 0;
    int iPos = i;
    while (iPos > 0) {
      iPos = iPos & (iPos - 1);
      temp++;
    }
    res.add(temp);
  }
  return res;
}
```

[reverse-bits](https://leetcode-cn.com/problems/reverse-bits/)

> 颠倒给定的 32 位无符号整数的二进制位。

思路：依次颠倒即可

```dar
  int reverseBits(int n) {
    int res = 0;
    for(int i = 0 ; i < 32 ;i++) {
        int temp = (n & 1);
        res =  (res<<1) + temp;
        n = (n >> 1); 
    }
    return res;
  }
```

[bitwise-and-of-numbers-range](https://leetcode-cn.com/problems/bitwise-and-of-numbers-range/)

> 给定范围 [m, n]，其中 0 <= m <= n <= 2147483647，返回此范围内所有数字的按位与（包含 m, n 两端点）。

```dart
int rangeBitwiseAnd(int m, int n) {
  // m 5 1 0 1
  //   6 1 1 0
  // n 7 1 1 1
  // 把可能包含0的全部右移变成
  // m 5 1 0 0
  //   6 1 0 0
  // n 7 1 0 0
  // 所以最后结果就是m<<count
  //一句话这道题的核心就是找公共前缀
  int count = 0;
  while (m != n) {
    m >>= 1;
    n >>= 1;
    count++;
  }
  int result = (m << count);
  return result;
}

```

## 练习

- [ ] [single-number](https://leetcode-cn.com/problems/single-number/)
- [ ] [single-number-ii](https://leetcode-cn.com/problems/single-number-ii/)
- [ ] [single-number-iii](https://leetcode-cn.com/problems/single-number-iii/)
- [ ] [number-of-1-bits](https://leetcode-cn.com/problems/number-of-1-bits/)
- [ ] [counting-bits](https://leetcode-cn.com/problems/counting-bits/)
- [ ] [reverse-bits](https://leetcode-cn.com/problems/reverse-bits/)
