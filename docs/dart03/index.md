# 制御フロー

## if

`if`は、単独で使ったり、`else`と組み合わせたりということができます。基本的な`if`文は次の通り。

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

## while

`while`ループも同様に使用できます。

```dart
void main() {
  var list = [1, 2, 3];
  for (var value in list) {
    print(value);
  }
}
```

最低でも1度実行したい場合には`do-while`も使用できます。

```dart
void main() {
  var list = [1, 2, 3];
  for (var value in list) {
    print(value);
  }
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

## switch

`if`の代わりに`switch`を使うことも可能です。

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
