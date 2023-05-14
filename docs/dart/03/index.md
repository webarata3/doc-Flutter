# 制御フロー

## if

`if`は、単独で使ったり、`else`と組み合わせたりということができます。基本的な`if`文は次の通りです。
<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  var speed = 50;

  if (speed &gt; 40) {
    print(&#x27;スピード違反です&#x27;);
  }
}
</code>
</pre>

??? オフライン用

    ```dart
    void main() {
      var speed = 50;
    
      if (speed > 40) {
        print('スピード違反です');
      }
    }
    ```

`if`の後ろの`()`内が`true`の場合に、`{}`の中の命令が実行されます。`false`の場合には単に無視されます。また、`{}`の中の命令文が1つだけの場合には、`{}`は省略できます。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  var speed = 50;

  if (speed &gt; 40) print(&#x27;スピード違反です&#x27;);
}
</code>
</pre>

??? オフライン用

    ```dart
    void main() {
      var speed = 50;
    
      if (speed > 40) print('スピード違反です');
    }
    ```

`if`の条件を満たさないときにも命令を実行したい場合には`else`を使います。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  var speed = 50;

  if (speed &gt; 40) {
    print(&#x27;スピード違反です&#x27;);
  } else {
    print(&#x27;安全運転です&#x27;);
  }
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  var speed = 50;

  if (speed &lt;= 40) {
    print(&#x27;安全運転です&#x27;);
  } else if (speed &lt;= 60) {
    print(&#x27;スピード違反です&#x27;);
  } else {
    print(&#x27;大変危険です&#x27;);
  }
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  for (var i = 0; i &lt; 10; i++) {
    print(i);
  }
}
</code>
</pre>

??? オフライン用

    ```dart
    void main() {
      for (var i = 0; i < 10; i++) {
        print(i);
      }
    }
    ```

`List`は`for-in`で繰り返すことができます。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  var list = [1, 2, 3];
  for (var value in list) {
    print(value);
  }
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  int i = 0;
  while (i &lt; 10) {
    print(i);
    i++;
  }
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  int i = 10;
  do {
    print(i);
    i++;
  } while (i &lt; 10);
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  print(&#x27;break&#x27;);
  for (var i = 0; i &lt; 10; i++) {
    if (i == 3) break;
    print(i);
  }
  print(&#x27;continue&#x27;);
  for (var i = 0; i &lt; 10; i++) {
    if (i == 3) continue;
    print(i);
  }
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  var value = 2;

  switch (value) {
    case 0:
      print(&#x27;0です&#x27;);
      break;
    case 1:
      print(&#x27;1です&#x27;);
      break;
    case 2:
      print(&#x27;2です&#x27;);
      break;
    default:
      print(&#x27;0～2以外です&#x27;);
  }
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  var value = 0;

  switch (value) {
    case 0:
    case 1:
      print(&#x27;0か1です&#x27;);
      break;
    case 2:
      print(&#x27;2です&#x27;);
      break;
    default:
      print(&#x27;0～2以外です&#x27;);
  }
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  var value = 0;

  switch (value) {
    case 0:
      print(&#x27;0です&#x27;);
      continue label2;
    case 1:
      print(&#x27;1です&#x27;);
      break;
    label2:
    case 2:
      print(&#x27;2です&#x27;);
      break;
    default:
      print(&#x27;0～2以外です&#x27;);
  }
}
</code>
</pre>

??? オフライン用

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

Dart 3.0からswitch式が使えるようになりました。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
import &#x27;dart:math&#x27;;

void main() {
  var r = Random().nextInt(4);
  var uranai = switch (r) {
    0 =&gt; &#x27;大吉&#x27;,
    1 =&gt; &#x27;中吉&#x27;,
    2 =&gt; &#x27;吉&#x27;,
    _ =&gt; &#x27;凶&#x27;,
  };

  print(&#x27;今日の運勢は:$uranai&#x27;);
}
</code>
</pre>

??? オフライン用

    ```dart
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

`case`は書かず、`条件 => 返す値`となります。`_`は`default`と同じで、`default`と書いてもいいです。

単純な定数と一致する場合ではなく次のように範囲の指定もできます。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
import &#x27;dart:math&#x27;;

void main() {
  var r = Random().nextInt(10);
  var uranai = switch (r) {
    0 || 1 =&gt; &#x27;大吉&#x27;,
    &gt;= 2 &amp;&amp; &lt;= 6 =&gt; &#x27;中吉&#x27;,
    7 || 8 || 9 =&gt; &#x27;吉&#x27;,
    _ =&gt; &#x27;凶&#x27;,
  };

  print(&#x27;今日の運勢は:$uranai&#x27;);
}
</code>
</pre>

??? オフライン用

    ```dart
    import 'dart:math';
    
    void main() {
      var r = Random().nextInt(10);
      var uranai = switch (r) {
        0 || 1 => '大吉',
        >= 2 && <= 6 => '中吉',
        7 || 8 || 9 => '吉',
        _ => '凶',
      };
    
      print('今日の運勢は:$uranai');
    }
    ```
