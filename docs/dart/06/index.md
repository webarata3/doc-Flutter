# 非同期

時間がかかる処理を行うと、プログラムはそこで待たされます。

次の例は、`sleepFunction`関数内でプログラムをスリープ（待機）させているので、実行に時間がかかります。

```dart
import 'dart:io';

void main() {
  sample1();
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
```

次の例では、処理に時間がかかるのは同じですが、処理の終了を待ちません。

```dart
import 'dart:async';

void main() {
  sample2();
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
```

処理の終了を待たない場合には、戻り値として`Future`を返します。`Future`はジェネリクスになっており、処理終了時の戻り値の型を指定します。

処理の終了を待たないので、`noWaitFunction`を`print`すると`Future`そのままが表示されます。

```実行結果
# 1
Instance of 'Future<String>'
# 2
Instance of 'Future<String>'
# 3
Instance of 'Future<String>'
# 4
```

`Future`の戻り値の結果を受け取りたい場合には、`Future`の`then`メソッドを使います。

```dart
import 'dart:async';

void main() {
  sample3();
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

Future<String> noWaitFunction(String message, int time) {
  return Future.delayed(Duration(seconds: time), () {
    return '時間がかかる処理: $message';
  });
}
```

結果は次のようになります。

```実行結果
# 1
# 2
# 3
# 4
時間がかかる処理: 2
時間がかかる処理: 1
時間がかかる処理: 3
```

`Future`が戻り値の関数の戻り値を受け取る方法として、結果が出るまで待つ事ができます。

待つためには、`Future`が戻り値となる関数を利用する関数に`async`を付けます。そのうえで、`Future`の関数の前に`await`を付けることで、結果が返されるまで待機します。

```dart
import 'dart:async';

void main() {
  sample4();
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

Future<String> noWaitFunction(String message, int time) {
  return Future.delayed(Duration(seconds: time), () {
    return '時間がかかる処理: $message';
  });
}
```

`Future`の関数をいくつか実行し、すべての結果が終わるまで待機するということもできます。

```dart
import 'dart:async';

void main() {
  sample5();
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

Future<String> noWaitFunction(String message, int time) {
  return Future.delayed(Duration(seconds: time), () {
    return '時間がかかる処理: $message';
  });
}
```
