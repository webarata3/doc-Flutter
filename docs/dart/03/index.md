# 制御フロー

## if

`if`は、単独で使ったり、`else`と組み合わせたりということができます。基本的な`if`文は次の通りです。

```dart
void main() {
  var speed = 50;

  if (speed > 40) {
    print('スピード違反です');
  }
}
```

`if`の後ろの`()`内が`true`の場合に、`{}`の中の命令が実行されます。`false`の場合には単に無視されます。また、`{}`の中の命令文が1つだけの場合には、`{}`は省略できます。

```dart
void main() {
  var speed = 50;

  if (speed > 40) print('スピード違反です');
}
```

`if`の条件を満たさないときにも命令を実行したい場合には`else`を使います。

```dart
void main() {
  var speed = 50;

  if (speed > 40) {
    print('スピード違反です');
  } else {
    print('安全運転です');
  }
}
```

`else`も`if`と同様に命令が1つの場合には`{}`は省略できます。

`if`と`else`の組み合わせは1つだけではなく`else`に`if`を続けて書くこともできます。

```dart
void main() {
  var speed = 50;

  if (speed <= 40) {
    print('安全運転です');
  } else if (speed <= 60) {
    print('スピード違反です');
  } else {
    print('大変危険です');
  }
}
```

`else if`とすると、最初の`if`の条件を満たさない場合のうち、さらに`if`でチェックをします。上記の場合、`speed`が40以下の場合は`安全運転です`、40より大きい場合のうち60以下の場合`スピード違反です`、それ以外、つまり60より大きい場合`大変危険です`となります。

## for

回数を指定するような繰り返しは`for`ループを使用します。

```dart
void main() {
  for (var i = 0; i < 10; i++) {
    print(i);
  }
}
```

`List`は`for-in`で繰り返すことができます。

```dart
void main() {
  var list = [1, 2, 3];
  for (var value in list) {
    print(value);
  }
}
```

#### 練習問題3-1

10、20、99の3つの数字を入れた`List`を作成すること。その`List`の内容を`for`を使ってすべて表示すること。

??? 解答例

    ```dart
    void main() {
      var list = [10, 20, 99];
      for (var value in list) {
        print(value);
      }
    }
    ```

## while

`while`ループも同様に使用できます。

```dart
void main() {
  int i = 0;
  while (i < 10) {
    print(i);
    i++;
  }
}
```

最低でも1度実行したい場合には`do-while`も使用できます。

```dart
void main() {
  int i = 10;
  do {
    print(i);
    i++;
  } while (i < 10);
}
```

## break、continue

`break`を使うと、ループを途中で終了します。。`continue`は次のループに戻ります。

```dart
void main() {
  print('break');
  for (var i = 0; i < 10; i++) {
    if (i == 3) break;
    print(i);
  }
  print('continue');
  for (var i = 0; i < 10; i++) {
    if (i == 3) continue;
    print(i);
  }
}
```

#### 練習問題3-2

繰り返しの命令を使って1～10の数を作成し、それを表示すること。ただし、`contine`を使って3の倍数の数は表示しないようにすること。

??? 解答例

    ```dart
    void main() {
      for (var i = 1; i <= 10; i++) {
        if (i % 3 == 0) continue;
        print(i);
      }
    }
    ```

## switch

`if`の代わりに`switch`を使うことも可能です。

`case`ごとに`break`を書かないとエラーになります。

```dart
void main() {
  var value = 2;

  switch (value) {
    case 0:
      print('0です');
      break;
    case 1:
      print('1です');
      break;
    case 2:
      print('2です');
      break;
    default:
      print('0～2以外です');
  }
}
```

次のように`case`の処理が空の場合には`break`がなくても大丈夫です。

```dart
void main() {
  var value = 0;

  switch (value) {
    case 0:
    case 1:
      print('0か1です');
      break;
    case 2:
      print('2です');
      break;
    default:
      print('0～2以外です');
  }
}
```

また、`continue`を使うこともできます。

```dart
void main() {
  var value = 0;

  switch (value) {
    case 0:
      print('0です');
      continue label2;
    case 1:
      print('1です');
      break;
    label2:
    case 2:
      print('2です');
      break;
    default:
      print('0～2以外です');
  }
}
```
