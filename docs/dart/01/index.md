# 組み込み型と変数

## エントリーポイント

最初にプログラムの実行方法について解説します。

プログラムを実行したときのスタート地点のことを**エントリーポイント**といいます。Javaは`public static void main(String[] args)`メソッドがエントリーポイントです。Dartは`main`関数がエントリーポイントになります。関数については、後述しますが、`main`の関数を次のように書くとエントリーポイントになります。プログラムを実行するとこの関数から動作します。

???+ note "Javaのエントリーポイント"

    記載の通り、Javaは`public static void main(String[] args)`がエントリーポイントでした。当然、メソッドのため何らかのクラスに属している形でした。

    2025年9月にリリースのJava 25からは次のように`void main()`をエントリーポイントとすることができるようになりました。また、クラスの記述も必須ではなくなりました。そのため、エントリーポイントだけ見ると、Dartとほぼ同じように記述することができます。

    ``` java linenums="1"
    void main() {
        System.out.println("Hello, World!");
    }
    ```


例えば、次のプログラムを`test.dart`というファイル名で作成してください。Dartは拡張子を`.dart`とします。

``` dart linenums="1"
void main() {
  print('こんにちは');
}
```

記述したら、ターミナルを開きます。ターミナルで、`dart test.dart`とすると実行することができます。実行すると画面に`こんにちは`と出てくるはずです。

VSCodeの場合、次の画像のように`main`関数の前に`Run`と出ているので、それをクリックしても実行できます。

![](image/01.webp)

`main`は関数です。関数については別途解説します。Dartは`main`の後ろの`{}`の間の命令を上から順番に実行します。そのため、以降プログラムを動作させて確認したい場合には、{=={}の間に命令を記載してください==}。

## 型と変数

### 型の基本

変数は、Javaと同じように型を明示して作成します。例えば、整数を格納する変数`a`を作りたいときには

``` dart
int a;
```

とします。

また、Javaと同様に次のように変数宣言と同時に初期化することもできます。

``` dart
int a = 100;
```

さらに、宣言と同時に初期化する場合には、Javaと同様に型を`var`としても構いません。

``` dart
var a = 100;
```

???+ note "型の確認"

    変数の型を確認したい場合には`変数.runtimeType`で確認できます。

    ``` dart linenums="1"
    void main() {
      var a = 100;
      print(a.runtimeType); // int
    }
    ```

### nullの扱い

Javaの場合には`int`や`boolean`等のプリミティブ型（基本型）には`null`を代入することができません。しかし、Dartの場合には`int`や`bool`等はクラス（例えば、[intクラス](https://api.dart.dev/stable/2.19.5/dart-core/int-class.html){target=_blank}）のため、`null`を代入することができます。ただし、`null`を代入する場合には型に`?`を付けないといけません。`?`を付けないと`null`を代入することはできません。

例えば、次のように型に`?`を付けずに`null`を代入するとエラーになります。

``` dart
int a = null;
```

変数に`null`を代入したい場合には、型の後ろに`null`を代入するかもしれないという意味で`?`を付けます。

``` dart
int? a = null;
```

`null`が入る可能性がある場合には、必ず`?`を付けなければいけません。逆に、`null`が絶対に入らないのであれば、`?`をつけるべきではありません。

`null`を代入することがある場合には、型が明確にならないため`var`を使うことはできません。

宣言時に変数を初期化できない場合でも、次のような場合、必ず変数に値が入るため`?`を付ける必要はありません。

``` dart linenums="1"
void main() {
  int a;
  int b = 3;

  if (b == 3) { // 分岐しているが、elseがあり、必ずaに代入される
    a = 1;
  } else {
    a = 2;
  }

  print(a);
}
```

???+ note "ノート"

    Javaの場合には、参照型の場合には`Optional<T>`、プリミティブ型の場合には`OptionalInt`や`OptionalDouble`等の型を使うことで（少し煩雑ですが）同様のことができます。

#### やってみよう1

変数`x`を`int`型で作成しましょう（`?`は付けない）。変数の作成と同時に、`x`を`null`で初期化（代入）して、エラーになることを確認しましょう。

??? 解答例

    ``` dart linenums="1"
    void main() {
      int x = null; // ここがエラーになります
    }
    ```

#### やってみよう2

やってみよう1のプログラムでエラーが起きないように変数の型を変更してみましょう。

??? 解答例

    2行目の`int`の後ろに`?`を入れます。

    ``` dart linenums="1"
    void main() {
      int? x = null;
    }
    ```

## 組み込み型

Dartは、いくつかの型に対して特別扱いをしています。Javaよりもそれは広範にわたっており、Javaのコレクションクラスにあたる、`List`、`Map`、`Set`等もこれらに含まれています。それらの特別な型について、以下、一つ一つ具体的に確認していきます。

### 数値

数値は、整数のために`int`型、小数のために`double`型が用意されています。Javaと違い数値はこの2つだけです。

`int`型は符号付き64bit整数のため、-2^63^～2^63^-1（約±900京、1京は1兆の1万倍）までの数を表すことができます。ただし、Webで扱う場合には、JavaScriptの数値となるため、-2^53^～2^53^-1（約±9007兆）までとなります。

`double`型は、[IEEE 754](https://ja.wikipedia.org/wiki/IEEE_754)の浮動小数点として表されます。浮動小数点数のため、次の例のように誤差が出る場合があります。

``` dart linenums="1"
void main() {
  print(0.1 + 0.2);
}
```

実行すると、次のようになります。このように`double`型の計算では誤差が出ることがありますので、誤差が出ても問題ない場合や、誤差を吸収できるような仕組みを考える必要があります。

```
> dart test.dart
0.30000000000000004
```

???+ note "小数の厳密な計算"

    Javaの場合には、`BigDecimal`クラスを使うことで任意精度の小数を厳密に扱うことができます。Dartの場合には標準として用意されているものはないようです。[decimal](https://pub.dev/packages/decimal){target=_blank}といったライブラリーを活用することで実現できるようです。

整数のリテラルは`0x`をつけると16進数として認識されます。

``` dart linenums="1"
void main() {
  print(0x1a);
}
```

また、Javaと同様に数値の途中に`_`を入れることができます。先頭や最後は入れられません。

``` dart linenums="1"
void main() {
  var a = 1_000_000; // 100万
  var b = 1__0__0__0; // 1000
}
```

数値に関してはJavaと比較すると次のようになります。

| Java | Dart |
|-|-|
| `byte`: 1byte | `int`: 8byte|
| `short`: 2byte | 同上 |
| `int`: 4byte | 同上 |
| `long`: 8byte | 同上 |
| `float`: IEEE754 単精度 | `double`: IEEE754 倍精度 |
| `double`: IEEE754 倍精度 | 同上 |

### 文字列

文字列は`String`型を使います。文字列リテラルは`'`（シングルクォート）か`"`（ダブルクォート）で囲みます。どちらでも構いませんが、Dartのドキュメントではシングルクォートを使っている例が多くあります。そのため、この資料では基本的にシングルクォートで文字列は表します。

文字列は、Javaと同じように見えて違う部分が多くあります。

相違点の1つは文字列の比較です。文字列の比較はJavaと異なり`==`で行うことができます。

Dartの`==`演算子は、Javaと同じ等値です。等値ですが`==`で比較できるのは、`==`演算子をオーバーライドして定義して同じ文字列であれば`true`としているためです（`==`演算子のオーバーライドについて詳しいことはクラスのところで解説します）。

???+ note "Javaの等価と等値"

    Javaでは`==`は等値（equality）で同じインスタンスかどうかを確認する（プリミティブ型は同じ値かどうかです）演算子です。そのため、2つ`String`のインスタンスを`new`して作成した場合には、同じ文字列を持っている場合でも等値にはなりません。

    `new String("こんにちは")`と2つインスタンスを作り、その文字列が同じかどうかを確認するためには、等価（equivalent）かどうかを確認する必要があります。Javaの場合には等価は`equals`メソッドを使って行います。

文字列リテラルは、`'''`もしくは`"""`で囲むことで、改行を含めることができます。

``` dart linenums="1"
void main() {
  var s = '''
  こんにちは
  Dart
  ''';

  print('>$s<');
}
```

実行すると次のようになります。

```
>  こんにちは
  Dart
  <
```

プログラムの7行目で変数`s`の内容を表示していますが、文字列のリテラルの中に`$変数名`もしくは`${変数名}`とすることで、変数の値を埋め込むことができます。

プログラムの実行した結果を見ると、最初の`'''`の行の改行は文字列に含まれませんが、最後の`'''`の前の改行は文字列に含まれています。また、それぞれの行の行頭のスペースも文字列に含まれています。

文字列の中でエスケープシーケンスを無視した、生の文字列を扱いたい場合には、文字列リテラルの先頭に`r`をつけます。

``` dart linenums="1"
void main() {
  print(r'Hello \n Dart');
}
```

文字列は`+`で結合することもできますが、文字列同士を隣接させると自動的に結合されます。

```dart linenums="1"
void main() {
  const s = 'あいうえお' 'かきくけこ' 'さしすせそ';
  print(s);
}
```

#### やってみよう3

`String`型の変数`html`を作成して、次の文字列を格納しましょう。見た目通り、改行も含めて変数に入れてください。。また、最後に`print(html);`として画面にその内容を表示してみましょう。

```
<section>
<p>こんにちは</p>
</section>
```

??? 解答例

    ``` dart linenums="1"
    void main() {
      var s = '''
    <section>
    <p>こんにちは</p>
    </section>''';
      print(s);
    }
    ```

#### やってみよう4

変数`name`に`山本`、変数`age`に`19`を入れましょう。その変数を使って`私は山本です。19歳です`という文字列を`+`を使わずに作成しましょう。作成した変数を`print`を使って表示しましょう。

??? 解答例

    ``` dart linenums="1"
    void main() {
      var name = '山本';
      var age = 19;
      var s = '私は${name}です。${age}歳です';
      print(s);
    }
    ```

### 文字列の応用

文字の中には、🐸や🗾と言った絵文字があります。また、𠮷（上が士ではなく、土）のような少し特殊な文字もあります。こういった文字は、例えば文字数をカウントするとおかしなことになります。

``` dart linenums="1"
void main() {
  var s1 = '𠮷';
  var s2 = '🐸';

  print('${s1}=${s1.length}');
  print('${s2}=${s2.length}');
}
```

実行すると次のようになります。

```
𠮷=2
🐸=2
```

これらの文字は、いわゆるサロゲートペアと呼ばれる文字で`String.length`では正確な文字数がわかりません。

こういった事があるため、`Runes`というクラスが用意されています。これを使うことで、絵文字等が入った文字列の操作が通常通り行うことができます。

``` dart linenums="1"
void main() {
  var s1 = '𠮷';
  var s2 = '🐸';

  print('${s1}=${s1.runes.length}');
  print('${s2}=${s2.runes.length}');
}
```

また、次のように`substring`を使って文字列を抜き出したい場合にも正しく動作しません。

``` dart linenums="1"
void main() {
  var s1 = '吉あいうえお';
  var s2 = '𠮷あいうえお';

  print('${s1.substring(1, 3)}');
  print('${s2.substring(1, 3)}');
}
```

実行結果は次のようになります。

```
あい
�あ
```

これは、`Runes`を使っても簡単に行うことはできません。これらを行うためには、`characters`パッケージを使う方法があります。ここでは、その方法は割愛します。

### 真偽値

`true`、`false`の2値を取る真偽値型は`bool`型となります。Javaの`boolean`と変わりません。

### 定数

Dartでも値を変更することができる変数と、値を変更することができない定数があります。定数には`final`と`const`の2つがありますが、ここまで出てきている`int`、`double`、`bool`、`String`型ではそれらの違いはわかりません。

定数は、次のように`final`や`const`をつけることで宣言できます。違いは現段階ではないので、`const`を使っておけばいいです。また、すぐに初期化する場合には、型を明示しなくても構いません（`int`や`bool`を付けなくても良い）。

``` dart linenums="1"
void main() {
  const i = 3; // intは不要
  const b = true; // boolは不要
  // i = 5; 変更しようとするとエラー
}
```

## 標準入力と型の変換

### 標準入力

キーボードからの入力を受け取りたい場合には、`stdin`を使います。使う場合には`import`が必要です。

``` dart
import 'dart:io';
```

その上で、次のようにすることで入力を受け付けることができます。

``` dart
var input = stdin.readLineSync();
```

また、`io`ライブラリを使うことで、次の命令も使用できます。

``` dart
stdout.write('改行されない');
```

`print`と似ていますが、`write`を使うと引数の文字列を表示のあとに改行されません。

#### やってみよう5

キーボードからの入力を受け付ける`stdin.readLineSync`メソッドを使い文字列を取得して、変数`line`に入れましょう。また、変数`line`の内容を表示しましょう。

??? 解答例

    ``` dart linenums="1"
    import 'dart:io';

    void main() {
      var line = stdin.readLineSync();
      print(line);
    }
    ```

### 型の変換

文字列から`int`型に変換するには、`int`の`parse`メソッドを使います。

``` dart
int num = int.parse('123');
```

次のように数値にできない場合には例外が発生します。例外については後述しますがJavaの例外とほぼ同じ考え方です。

``` dart
int num = int.parse('abc');
```

`parse`メソッドではなく、`tryParse`メソッドを使うと数値に変換できない場合には`null`になります。

``` dart linenums="1"
void main() {
  print(int.tryParse('111')); // 111
  print(int.tryParse('abc')); // null
}
```

Javaは`double`型の変数に`int`型の値を代入することはできますが、Dartは相互に代入することはできません。

``` dart linenums="1" hl_lines="3 5"
void main() {
  double d = 1.1;
  int i = d; // エラー
  int i2 = 1;
  double d2 = i2; // エラー
}
```

`double`型を`int`型にするには、`toInt()`メソッドを使います。もちろん、`int`型にするので整数部分だけ残り、小数部は切り捨てられます。

``` dart linenums="1" hl_lines="3"
void main() {
  var d = 10.1;
  var i = d.toInt();

  print(i); // 10
}
```

## リスト、セット、マップ

Javaのコレクションクラスと同様のものがDartにもあります。Javaと違い組み込み型として準備されているので、使い勝手はかなり良くなっています。

### リスト

Dartには配列はなく、順序があるリストが組み込み型として用意されています。型は`List`で、リストの中に入れるデータの型を`<>`の中に書きます。例えば、整数のリストの場合には`List<int>`とします。

``` dart linenums="1"
void main() {
  List<int> list = [];
  list.add(3);
  list.add(7);
  print(list); // [3, 7]
}
```

Javaと違い、`[]`で空のリストを表現できます。また、次のように、`[]`の中に`,`区切りでリストの初期値を設定することができます。

``` dart linenums="1"
void main() {
  var list1 = [1, 2, 3];
  var list2 = [
    1,
    2,
    3,
  ];
}
```

上記のようにリストの最後の要素の後に`,`を入れるとフォーマッタが要素ごとに改行を入れます。

`List`も宣言と同時に初期化をする場合には、型の明示は不要です。次のように変数にカーソルを合わせると型を正しく認識しています。

![](image/02.webp)

個別のデータを取ってきたり、データを変更する場合には、`[]`内にインデックス番号を書きます。インデックスはJavaと同じで先頭が0です。

``` dart linenums="1"
void main() {
  var list = [1, 2, 3];
  print(list);
  print(list[0]);
  list[0] = 99;
  print(list[0]);
}
```

Javaと同じで、インデックスの範囲外のアクセスはエラーになります。

``` dart linenums="1"
void main() {
  var list = [1, 2, 3];
  list[3] = 10;
}
```

実行結果。

```
Unhandled exception:
RangeError (index): Invalid value: Not in inclusive range 0..2: 3
#0      List._setIndexed (dart:core-patch/growable_array.dart:290:49)
#1      List.[]= (dart:core-patch/growable_array.dart:285:5)
```

`List`で定数の`final`と`const`の違いが確認できます。まず、`final`から確認します。`final`で`List`を作成すると、次のように変数そのものに他のものを代入するとエラーになります。

``` dart linenums="1"
void main() {
  final list = [1, 2, 3];
  // list = [4, 5, 6];  //エラー
  var list2 = [4, 5, 6];
  // list = list2; // エラー
}
```

ただし、`final`の場合には`List`の内容の変更は行うことができます。

``` dart linenums="1"
void main() {
  final list = [1, 2, 3];
  list[2] = 5;
  list.add(9);
  print(list); // [1, 2, 5, 9]
}
```

`const`は`final`と違い内容の変更もできません。

``` dart linenums="1" hl_lines="3"
void main() {
  var list = const [1, 2, 3];
  list[0] = 3; // 実行時にエラー
}
```

リストはいろいろな方法で、値の追加ができます。

``` dart linenums="1" hl_lines="3 11"
void main() {
  var list1 = [1, 2, 3];
  var list2 = [0, ...list1]; // 他のリストの最後に追加

  print('list2=${list2}');

  var flag = false;
  var list3 = [
    0,
    1,
    if (flag) 3, // 条件を満たす場合追加
  ];
  print('list3=${list3}');
}
```

この例の場合、`list2`の最後に`list1`の内容を追加しています。

`list3`は最後に`if`の条件を書き、`true`の場合には値を追加するということをしています。

また、次のように`for`ループの結果をリストに含むということもできます。

``` dart linenums="1"
void main() {
  var list = [for (var i = 1; i <= 10; i++) i];
  print('list=${list}');
}
```

`List`の[API](https://api.dart.dev/stable/2.16.2/dart-core/List-class.html)からいくつか使えそうなものを紹介します。

`isEmpty`、`isNotEmpty`では要素が0件か、そうでないかのチェックができます。

``` dart linenums="1"
void main() {
  var list = [1, 2, 3];
  print(list.isEmpty);
  print(list.isNotEmpty);
}
```

`length`で`List`に入っている件数を確認できます。

``` dart linenums="1"
void main() {
  var list = [1, 2, 3];
  print(list.length);
}
```

データの追加は、主に2つです。

- `add`メソッド: 最後に要素を追加
- `insert`メソッド: 指定した位置に、要素を挿入

``` dart linenums="1"
void main() {
  var list = [1, 2, 3];
  list.add(4);
  list.insert(1, 999);
  print(list);
}
```

???+ warning "Listの実装"

    Dartの`List`はJavaの`ArrayList`と同様です。最後に追加する場合は特に問題ありませんが、大量に挿入（`insert`）する場合にはパフォーマンスに問題が出る場合があります。

データの削除は、要素を指定するか、インデックスを指定するかどちらかになります。要素の指定の場合には、複数ある場合には最初の要素になります。

``` dart linenums="1"
void main() {
  var list = [1, 2, 3, 4, 5, 1, 2];
  list.remove(1);
  print(list);
  list.removeAt(2);
  print(list);
}
```

データの検索は次のものを使います。

- `contains`: 要素が含まれているかどうか
- `indexOf`、`lastIndexOf`: 要素がどこに入っているか

``` dart linenums="1"
void main() {
  var list = [1, 2, 3, 4, 5, 1, 2];
  print(list.contains(3));
  print(list.indexOf(2));
  print(list.lastIndexOf(2));
  print(list.lastIndexOf(7));
}
```

#### やってみよう6

次のプログラムを書きましょう。

``` dart linenums="1"
void main() {
  var list = [1, 2, 3];
  print(list);
}
```

続いて、`list`の最後の要素の`3`の直後に`,`を入れてファイルを保存して、要素ごとに改行されることを確認しましょう。

#### やってみよう7

変数`list`に次の要素を含むリストを作成しましょう。

- 1
- 2
- 3

変数作成後、`list`の最後に`4`を追加しましょう。

最後に、その要素の内容を表示しましょう（`print(list)`でいいです）。

??? 解答例

    ``` dart linenums="1"
    void main() {
      var list = [1, 2, 3];
      list.add(4);
      print(list);
    }
    ```

#### やってみよう8

変数`list`に次の要素を含むリストを作成しましょう。

- 1
- 2
- 3

変数作成後、`list`の（先頭が0番目として）1番目に`99`を挿入しましょう。

最後に、その要素の内容を表示しましょう。（`print(list)`でいいです）。

??? 解答例

    ``` dart linenums="1"
    void main() {
      var list = [1, 2, 3];
      list.insert(1, 99);
      print(list);
    }
    ```

### セット

重複せずに、順番が保証されないセットが用意されています。セットは`{}`で値を囲んで作成します。

``` dart linenums="1"
void main() {
  var set = {'東京都', '北海道', '沖縄県', '東京都', '東京都'};

  print('set=${set}'); // set={東京都, 北海道, 沖縄県}
}
```

上記のように、作成すると`東京都`は重複しているため1つだけセットされます（上記のようにリテラルでセットを作り、重複しているとエディタで警告が表示されます）。

空の`Set`のリテラルを書く場合は、次のように型を明示します。

``` dart linenums="1"
void main() {
  var set = <String>{};

  print('set=${set}');
}
```

基本的なメソッドは次のとおりです。

``` dart linenums="1"
void main() {
  final set = {'石川県', '富山県', '福井県', '東京都'};
  print('1: $set');
  set.add('北海道');
  print('2: $set');
  set.add('石川県');
  print('3: $set');
  print('石川県? ${set.contains("石川県")}');
}
```

### マップ

キーと値をセットにしたマップも組み込み型として用意されています。マップはセットと同様にリテラルは`{}`で定義します。`{}`の中では`:`でキーと値を区切ります。

``` dart linenums="1"
void main() {
  var map = {
    '長野県': '長野市',
    '北海道': '札幌市',
    '富山県': '富山市',
  };

  print('富山県の県庁所在地は${map["富山県"]}');
}
```

マップの値の取得は`[]`の中にキーを書くことで行えます。
