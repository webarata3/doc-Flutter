# 関数

## 関数の基本

関数は基本的には次のような形式で作成します。

```
戻り値の型 関数名(引数の型 変数名) {
  // 処理内容
}
```

関数の戻り値の型や、引数の型は必須ではありませんが書いたほうがいいです。

また、関数の処理内容が1行だけの場合には`=>`で書けます。

```dart
void main() {
  var value = 4;
  if (isEven(value)) {
    print('偶数です');
  } else {
    print('奇数です');
  }
}

bool isEven(int value) => value % 2 == 0;
```

## 引数

関数は名前付きの引数も作成できます。

例えば、次のように作成して使用することができます。

```dart
void main() {
  printProfile(name: '山田', age: 30);
  printProfile(name: '田中');
  printProfile(age: 22);
}

void printProfile({String? name, int? age}) {
  print('自己紹介');
  if (name != null) {
    print('名前は$name');
  }
  if (age != null) {
    print('年齢は$age');
  }
}
```

名前付きの引数は、そのままだと任意の引数になります。そのため、名前付きの引数は`null`になる可能性があります。

## オプションパラメータ

引数を`[]`で囲むと、引数の指定が任意になります。任意の変数のため、型はnull許容型にしないといけません。

```dart
void main() {
  printProfile('山田', 30);
  printProfile('田中');
}

void printProfile(String name, [int? age]) {
  print('自己紹介');
  print('名前は$name');
  if (age != null) {
    print('年齢は$age');
  }
}
```

## デフォルト値

引数にはデフォルト値を設定できます。デフォルト値を設定できるためnull許容型にしなくても良くなります。

```dart
void main() {
  printProfile('山田', 30);
  printProfile('田中');
}

void printProfile(String name, [int age = 20]) {
  print('自己紹介');
  print('名前は$name');
  print('年齢は$age');
}
```

名前付き引数もデフォルト値を設定できます。

```dart
void main() {
  printProfile(name: '山田', age: 30);
  printProfile(name: '田中');
}

void printProfile({required String name, int age = 30}) {
  print('自己紹介');
  print('名前は$name');
  print('年齢は$age');
}
```

## main関数

`main`関数はエントリーポイントになっていてます。`main`関数は戻り値は`void`で、引数はなしか`List<String>`になります。

引数を`List<String>`にした場合には、コマンドライン引数を受け取ることができます。

```dart title="test.dart"
void main(List<String> arguments) {
  for (var value in arguments) {
    print(value);
  }
}
```

実行する際に、引数をつけるとそれが`main`関数に渡されます。

```
> dart test.dart 1 こんにちは
1
こんにちは
```

## 第1級オブジェクト

関数は第1級オブジェクトのため、変数に代入することができます。

```dart
void main(List<String> arguments) {
  var p = printHello;

  p();
}

void printHello() {
  print('hello');
}
```

匿名関数を代入することもできます。

```dart
void main(List<String> arguments) {
  var p = () => print('Hello');

  p();
}
```

## 匿名関数

匿名関数は処理内容が1行であれば、次のように書けます。

```dart
void main(List<String> arguments) {
  var p = () => print('Hello');

  p();
}
```

処理がたくさんあれば、次のように通常の関数の定義から関数名を除いた形で書くことができます。

```dart
void main(List<String> arguments) {
  [1, 2, 3].forEach((element) {
    print(element);
  });
}
```

## 戻り値

すべての関数は戻り値を返す。明示して`return`がない場合は`null`が返されます。





