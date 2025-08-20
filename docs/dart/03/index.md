# 制御フロー

## if

`if`文はJavaと変わりません。

`if`はJavaと同じで単独で使ったり、`else`と組み合わせたりということができます。基本的な`if`文は次の通りです。

``` dart linenums="1"
void main() {
  var speed = 50;

  if (speed > 40) {
    print('スピード違反です');
  }
}
```

`if`の後ろの`()`内が`true`の場合に、`{}`の中の命令が実行されます。`false`の場合には単に無視されます。また、`{}`の中の命令文が1つだけの場合には、`{}`は省略できます。

``` dart linenums="1"
void main() {
  var speed = 50;

  if (speed > 40) print('スピード違反です');
}
```

`if`の条件を満たさないときにも命令を実行したい場合には`else`を使います。

``` dart linenums="1"
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

``` dart linenums="1"
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

## switch文

`switch`文はJavaに近いですが、`break`は必要ありません。次の場合、`1です`とだけ表示されます。

``` dart linenums="1"
void main() {
  var value = 1;

  switch (value) {
    case 0:
      print('0です');
    case 1:
      print('1です');
    case 2:
      print('2です');
    default:
      print('0～2以外です');
  }
}
```

次のように0と1の場合が同じ時は、`case`を続けます。

``` dart linenums="1"
void main() {
  var value = 0;

  switch (value) {
    case 0:
    case 1:
      print('0か1です');
    case 2:
      print('2です');
    default:
      print('0～2以外です');
  }
}
```

また、`continue`を使うこともできます。

``` dart linenums="1"
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

## switch式

Javaと同様に、`switch`式もあります。ただし、Javaの構文とはかなり違っています。式のため、値が返されます。

次のプログラムは、ランダムに0～3までの数字を作成し、その値によって分岐しています。`case`は書かず、`条件 => 返す値`となります。`_`は`default`と同じで、`default`と書いてもいいです。

``` dart linenums="1"
import 'dart:math';

void main() {
  var r = Random().nextInt(4);
  var uranai = switch (r) {
    0 => '大吉',
    1 => '中吉',
    2 => '吉',
    _ => '凶',
  };

  print('今日の運勢は:$uranai');
}
```

単純な定数と一致する場合ではなく次のように範囲の指定もできます。

``` dart linenums="1"
import 'dart:math';

void main() {
  var r = Random().nextInt(10);
  var uranai = switch (r) {
    0 || 1 => '大吉',       // 0か1
    >= 2 && <= 6 => '中吉', // 2～6
    7 || 8 || 9 => '吉',    // 7か8か9
    _ => '凶',              // それ以外
  };

  print('今日の運勢は:$uranai');
}
```
## for

回数を指定するような繰り返しは`for`ループを使用します。

``` dart linenums="1"
void main() {
  for (var i = 0; i < 10; i++) {
    print(i);
  }
}
```

Javaの拡張`for`文と同様のものは、`for-in`で行います。Dartには配列がないので、主に`List`が対象になります。

``` dart linenums="1"
void main() {
  var list = [1, 2, 3];
  for (var value in list) {
    print(value);
  }
}
```

#### やってみよう1

`for`文を使って1～10までの数字を`print`してみましょう。

??? 解答例

    ``` dart linenums="1"
    void main() {
      for (var i = 1; i <= 10; i++) {
        print(i);
      }
    }
    ```

#### やってみよう2

10、20、99の3つの数字を入れた`List`を作成しましょう。その`List`の内容を`for-in`を使ってすべて表示しましょう。

??? 解答例

    ``` dart linenums="1"
    void main() {
      var list = [10, 20, 99];
      for (var value in list) {
        print(value);
      }
    }
    ```

## while

`while`ループもJavaと変わりません。

``` dart linenums="1"
void main() {
  int i = 0;
  while (i < 10) {
    print(i);
    i++;
  }
}
```

最低でも1度実行したい場合には`do-while`も使用できます。`do-while`もJavaと変わりません。

``` dart linenums="1"
void main() {
  int i = 10;
  do {
    print(i);
    i++;
  } while (i < 10);
}
```

## break、continue

ループの中断や継続には`break`と`continue`が使えます。これもJavaと変わりません。

``` dart linenums="1"
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

#### やってみよう3

繰り返しの命令を使って1～10の数を作成し、それを表示しましょう。ただし、`contine`を使って3の倍数の数は表示しないようにしましょう。

??? 解答例

    ``` dart linenums="1"
    void main() {
      for (var i = 1; i <= 10; i++) {
        if (i % 3 == 0) continue;
        print(i);
      }
    }
    ```
