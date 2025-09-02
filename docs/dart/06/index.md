# 非同期

## Future

### Futureの基本

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
void main() {
  print('call # 1');
  print(noWaitFunction('# 1', 3));
  print('call # 2');
  print(noWaitFunction('# 2', 1));
  print('call # 3');
  print(noWaitFunction('# 3', 3));
  print('end');
}

Future<String> noWaitFunction(String message, int time) {
  return Future.delayed(Duration(seconds: time), () {
    return 'return $message';
  });
}
```

処理の終了を待たない場合には、戻り値として`Future`を返します。`Future`クラスはジェネリクスになっており、処理終了時の戻り値の型を指定します。

処理の終了を待たないので、`noWaitFunction`を`print`すると`Future`そのままが表示されます。

```
call # 1
Instance of 'Future<String>'
call # 2
Instance of 'Future<String>'
call # 3
Instance of 'Future<String>'
end
```

`Future`の戻り値の結果を受け取りたい場合には、`Future`の`then`メソッドを使います。

```dart
void main() {
  print('call # 1');
  noWaitFunction('# 1', 3).then((value) => print(value));
  print('call # 2');
  noWaitFunction('# 2', 1).then((value) => print(value));
  print('call # 3');
  noWaitFunction('# 3', 3).then((value) => print(value));
  print('end');
}

Future<String> noWaitFunction(String message, int time) {
  return Future.delayed(Duration(seconds: time), () {
    return 'return $message';
  });
}
```

結果は次のようになります。

```実行結果
call # 1
call # 2
call # 3
end
return # 2
return # 1
return # 3
```

`Future`が戻り値の場合には、関数の実行を待ちません。ただ、場合によっては関数の実行が終わってから、処理を行いたい場合があります。

その場合には、`Future`が戻り値とな関数を呼び出す関数に`async`を付けます。その上で、`Future`の関数の前に`await`を付けることで、結果が返されるまで待機します。

```dart
void main() async {
  print('call # 1');
  print(await noWaitFunction('# 1', 3));
  print('call # 2');
  print(await noWaitFunction('# 2', 1));
  print('call # 3');
  print(await noWaitFunction('# 3', 3));
  print('end');
}

Future<String> noWaitFunction(String message, int time) {
  return Future.delayed(Duration(seconds: time), () {
    return 'return $message';
  });
}
```

### Futureのエラー処理

`Future`がついた関数でエラー（例外）が発生した場合には、`catchError`メソッドで処理をすることができます。

``` dart linenums="1"
void main() {
  fetchUser('https://~~~')
      .then((value) => print(value))
      .catchError((e) => print('エラー: ${e.toString()}'));
  fetchUser('')
      .then((value) => print(value))
      .catchError((e) => print('エラー: ${e.toString()}'));
}

Future<String> fetchUser(String url) {
  if (url.isEmpty) return Future.error(Exception('Url is not empty'));
  return Future.value('$urlから取得したデータ');
}
```

これは実行すると次のようになります。

```
https://~~~から取得したデータ
エラー: Exception: Url is not empty
```

また、`then`の名前付き引数として`onError`を次のように用意することで、エラー発生したときの代わりの値を用意することができます。

``` dart linenums="1"
void main() async {
  final result1 = await fetchUser(
    'https://~~~',
  ).then((v) => v, onError: (e, st) => 'error');
  print(result1);

  final result2 = await fetchUser(
    '',
  ).then((v) => v, onError: (e, st) => 'error');
  print(result2);
}

Future<String> fetchUser(String url) {
  if (url.isEmpty) return Future.error(Exception('Url is not empty'));
  return Future.value('$urlから取得したデータ');
}
```

この実行結果は次のようになります。

```
https://~~~から取得したデータ
error
```

### 戻り値Future

`Future`を戻り値にする場合、`Future.xxx`というメソッドをたくさん書く必要がありプログラムが次のようにやや煩雑になります。

``` dart linenums="1"
Future<String> fetchUser(String url) {
  if (url.isEmpty) return Future.error(Exception('Url is not empty'));
  return Future.value('$urlから取得したデータ');
}
```

`Future`を戻り値にする関数に`async`をつけることで、自動的に｀Future`でラップされます。

``` dart linenums="1"
Future<String> fetchUser(String url) async {
  if (url.isEmpty) throw Exception('Url is not empty');
  return '$urlから取得したデータ';
}
```
