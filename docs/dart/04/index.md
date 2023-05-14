# 関数

## 関数の基本

関数は基本的には次のような形式で作成します。

```
戻り値の型 関数名(引数の型 変数名) {
  // 処理内容
}
```

関数の戻り値の型や、引数の型は必須ではありませんが、明示したほうが良いです。

また、関数の処理内容が1行だけの場合には`=>`で書けます（アロー構文と呼ばれます）。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  var value = 4;
  if (isEven(value)) {
    print(&#x27;偶数です&#x27;);
  } else {
    print(&#x27;奇数です&#x27;);
  }
}

bool isEven(int value) =&gt; value % 2 == 0;
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  printProfile(name: &#x27;山田&#x27;, age: 30);
  printProfile(name: &#x27;田中&#x27;);
  printProfile(age: 22);
}

void printProfile({String? name, int? age}) {
  print(&#x27;自己紹介&#x27;);
  if (name != null) {
    print(&#x27;名前は$name&#x27;);
  }
  if (age != null) {
    print(&#x27;年齢は$age&#x27;);
  }
}
</code>
</pre>

??? オフライン用

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

名前付きの引数は、そのままだと任意の引数になります。そのため、名前付きの引数は`null`になる可能性があるので、`?`を付ける必要があります。

通常の引数と名前付き引数を混在させることもできます。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  test(3, b: 4);
}

void test(int a, {int? b}) {
  print(&#x27;$a $b&#x27;);
}
</code>
</pre>

??? オフライン用

    ```dart
    void main() {
      test(3, b: 4);
    }
    
    void test(int a, {int? b}) {
      print('$a $b');
    }
    ```

## オプションパラメータ

引数を`[]`で囲むと、引数の指定が任意になります。任意の変数のため、型は`null`許容型にしないといけません。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  printProfile(&#x27;山田&#x27;, 30);
  printProfile(&#x27;田中&#x27;);
}

void printProfile(String name, [int? age]) {
  print(&#x27;自己紹介&#x27;);
  print(&#x27;名前は$name&#x27;);
  if (age != null) {
    print(&#x27;年齢は$age&#x27;);
  }
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  printProfile(&#x27;山田&#x27;, 30);
  printProfile(&#x27;田中&#x27;);
}

void printProfile(String name, [int age = 20]) {
  print(&#x27;自己紹介&#x27;);
  print(&#x27;名前は$name&#x27;);
  print(&#x27;年齢は$age&#x27;);
}
</code>
</pre>

??? オフライン用

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

名前付き引数もデフォルト値を設定できます。また。`required`をつけることで、その変数の指定が必須になります（必須のため、`?`は不要）。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main() {
  printProfile(name: &#x27;山田&#x27;, age: 30);
  printProfile(name: &#x27;田中&#x27;);
}

void printProfile({required String name, int age = 30}) {
  print(&#x27;自己紹介&#x27;);
  print(&#x27;名前は$name&#x27;);
  print(&#x27;年齢は$age&#x27;);
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main(List&lt;String&gt; arguments) {
  for (var value in arguments) {
    print(value);
  }
}
</code>
</pre>

??? オフライン用

    ```dart
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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main(List&lt;String&gt; arguments) {
  var p = printHello;

  p();
}

void printHello() {
  print(&#x27;hello&#x27;);
}
</code>
</pre>

??? オフライン用

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

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main(List&lt;String&gt; arguments) {
  var p = () =&gt; print(&#x27;Hello&#x27;);

  p();
}
</code>
</pre>

??? オフライン用

    ```dart
    void main(List<String> arguments) {
      var p = () => print('Hello');
    
      p();
    }
    ```

## 匿名関数

匿名関数は処理内容が1行であれば、次のように書けます。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main(List&lt;String&gt; arguments) {
  var p = () =&gt; print(&#x27;Hello&#x27;);

  p();
}
</code>
</pre>

??? オフライン用

    ```dart
    void main(List<String> arguments) {
      var p = () => print('Hello');
    
      p();
    }
    ```

処理がたくさんあれば、次のように通常の関数の定義から関数名を除いた形で書くことができます。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
void main(List&lt;String&gt; arguments) {
  [1, 2, 3].forEach((element) {
    print(element);
  });
}
</code>
</pre>

??? オフライン用

    ```dart
    void main(List<String> arguments) {
      [1, 2, 3].forEach((element) {
        print(element);
      });
    }
    ```

#### 練習問題4-1

次の関数を作成入力すること。

```dart
void printTest({String? name, int? age}) {
  print('名前は${name ?? "なし"}');
  print('年齢は${age ?? 0}');
}
```

`main`関数を作成し、次の4パターンで`printTest`関数を呼び出すこと。

- 引数なし
- 引数`name`に`山田`を渡す。
- 引数`age`に`20`を渡す。
- 引数`name`に`田中`、引数`age`に`19`を渡す。

??? 解答例

    ```dart
    void printTest({String? name, int? age}) {
      print('名前は${name ?? "なし"}');
      print('年齢は${age ?? 0}');
    }
    
    void main() {
      printTest(name: '山田');
      printTest(age: 20);
      printTest(name: '田中', age: 19);
    }
    ```

#### 練習問題4-2

次の仕様の関数を作成すること。

- 関数名: `panel`
- 名前付き引数
    - `String`型の`title`
    - `int`型の`version`。`version`は必須（`required`）とする
- 戻り値なし
- 処理内容は、`title`と`version`を`print`関数で表示する。`title`が`null`の場合には`無題`とすること。

動作確認のため、`main`関数を作成して、適当な引数で作った関数を呼び出すこと。

??? 解答例

    ```dart
    void main() {
      panel(title: 'テスト', version: 1);
      panel(version: 2);
    }
    ```

#### 練習問題4-3

次の関数を入力すること。

```dart
int calc(int x, int y, [bool? plus]) {
  if (plus ?? true) {
    return x + y;
  } else {
    return x * y;
  }
}
```

`main`関数を作成し、次の3パターンで`calc`関数を呼び出すこと。

最初の引数は`5`、2番目の引数は`3`とする

- 3番目の引数なし
- 3番目の引数`true`
- 3番目の引数`false`

??? 解答例

    ```dart
    int calc(int x, int y, [bool? plus]) {
      if (plus ?? true) {
        return x + y;
      } else {
        return x * y;
      }
    }
    
    void main() {
      print(calc(3, 5));
      print(calc(3, 5, true));
      print(calc(3, 5, false));
    }
    ```
