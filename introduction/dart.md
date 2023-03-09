# Dart 快速入门

## 基本语法

[Dart语言官网](https://dart.dev/guides/language/language-tour)
由于leetcode现在还是有些题不支持dart语言(以后肯定会支持的),大家可以在[dartpad](https://dartpad.dev/)上调试程序.

### 1.数组 List
数组在算法中使用场景最多的数据结构,所以基本的遍历,和一些常用的函数，变量尽量多掌握一些
``` dart

  List<int> emptyList = [];
  final list = ['one', 'two', 'three'];
  for (var i = 0; i < list.length; i++) {
    print('${list[i]}');
  }
  for (var item in list) {
    print('$item');
  }
  list.forEach((element) {
    print('$element');
  });
  list.add('four');
  list.insert(1, '1');
  list.removeAt(1);
  list.removeLast();
  
  int length = list.length;
  var last = list.last;
  var first = list.first;
  bool isContains = list.contains('one');
  bool isEmpty = list.isEmpty;
  bool isNotEmpty = list.isNotEmpty;

```

### 2.字典
字典在算法中使用的比数组少一些，一般用来存储数据。字典一般掌握初始化，遍历，遍历key,判断是否包含key
``` Dart
  Map map = {};
  map['0'] = 'zero';
  map['1'] = 'one';
  map.forEach((key, value) {
    print('key = $key : value = $value');
  });
  bool isEmpty = map.isEmpty;
  List keys = map.keys.toList();
  List value = map.values.toList();
  bool isContainsKey = map.containsKey('1');
  map.remove('1');

```

### 3.Set
set在算法中用到最少。一般用于去重,掌握初始化和判断包含元素既可。
``` dart 
  Set set = {'one'};
  set.add('two');
  set.remove('one');
  bool isContains = set.contains('one');
```



### 4.常用的一些算法
``` dart
  //字符串转浮点数整数
  double oneDouble = double.parse('1.1');
  int twoInt = int.parse('2');

  //浮点数整数转字符串
  String oneStr =   oneDouble.toString();
  String twoStr =   twoInt.toString();

  //获取最大值 最小值
  int maxNum = max(1, 2);
  int minNum = min(1, 2);
  //获取数组的最大值
  int max = [1,2,3,4,5].reduce(max);
  
  //求绝对值
  int num = (-1).abs();

  //取余数
  print(oneDouble % twoInt);
  //除法
  print(oneDouble / twoInt);
  //整除
  print(oneDouble ~/ twoInt);
}
```
