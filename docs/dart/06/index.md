# 非同期

```dart
import 'dart:io';
import 'dart:async';

void main() {
  sample6();
}

void sample1() {
  print('# 1');
  print(sleepFunction('1', 3));
  print('# 2');
  print(sleepFunction('2', 1));
  print('# 3');
  print(sleepFunction('3', 3));
  print('# 4');
}

String sleepFunction(String message, int time) {
  sleep(Duration(seconds: time));
  return '時間がかかる処理: $message';
}

void sample2() {
  print('# 1');
  print(noWaitFunction('1', 3));
  print('# 2');
  print(noWaitFunction('2', 1));
  print('# 3');
  print(noWaitFunction('3', 3));
  print('# 4');
}

Future<String> noWaitFunction(String message, int time) {
  return Future.delayed(Duration(seconds: time), () {
    return '時間がかかる処理: $message';
  });
}

void sample3() {
  print('# 1');
  noWaitFunction('1', 3).then((value) => print(value));
  print('# 2');
  noWaitFunction('2', 1).then((value) => print(value));
  print('# 3');
  noWaitFunction('3', 3).then((value) => print(value));
  print('# 4');
}

void sample4() async {
  print('# 1');
  print(await noWaitFunction('1', 3));
  print('# 2');
  print(await noWaitFunction('2', 1));
  print('# 3');
  print(await noWaitFunction('3', 3));
  print('# 4');
}

void sample5() async {
  print('# 1');
  Future.wait([
    noWaitFunction('1', 3),
    noWaitFunction('2', 1),
    noWaitFunction('3', 3),
  ]).then((list) {
    for (var value in list) {
      print(value);
    }
  });
  print('# 2');
}
```
