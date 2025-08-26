# 関数

## 関数の基本

関数は基本的には次のような形式で作成します。

```
戻り値の型 関数名(引数の型 変数名) {
  // 処理内容
}
```

関数の戻り値の型や、引数の型は必須ではありませんが、明示したほうがわかりやすい場合が多いです。

``` dart linenums="1" hl_lines="10-12"
void main() {
  var value = 4;
  if (isEven(value)) {
    print('偶数です');
  } else {
    print('奇数です');
  }
}

bool isEven(int value) {
  return value % 2 == 0;
}
```

また、関数の処理内容が1行だけの場合には`=>`で書けます（アロー構文と呼ばれます）。

``` dart linenums="1" hl_lines="10"
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

### やってみよう1

次の仕様の関数を作成し、引数`5`で呼び出して戻り値を`print`しましょう。

- 関数名: `twice`
- 引数: `int`型の`num`
- 戻り値: `int`
- 処理内容: 引数の`num`を2倍して返す

??? 解答例

    ``` dart linenums="1"
    void main() {
      print(twice(5));
    }

    int twice(int num) {
      return num * 2;
    }
    ```

### やってみよう2

やってみよう1の関数をアロー関数にしましょう。


??? 解答例

    ``` dart linenums="1"
    void main() {
      print(twice(5));
    }

    int twice(int num) {
      return num * 2;
    }
    ```

## 名前付き引数

引数に名前をつけることができます。名前付きの引数は、関数の引数を`{}`内に書きます。次の例の場合、変数`x`が名前付き引数となります。関数を呼び出すときには、引数として`x: 9`のように名前を指定して呼び出します。

``` dart linenums="1" hl_lines="5"
void main() {
  test(x: 9);
}

void test({int? x}) {
  print(x ?? -1);
}
```

名前付き引数はデフォルトで任意の引数となります。そのため、引数が渡されないことがあるため型に`?`を付け`null`許容型にしています。引数が渡されなかった場合、その変数の値は`null`となります。

次のように複数の名前付きの引数があっても構いません。また、関数を呼び出すときに名前付きの引数は好きな順番に書くことができます。

``` dart linenums="1" hl_lines="2-3 6"
void main() {
  test(x: 9); // xのみ
  test(y: 3, x: 2); // y、xの順番にしてもいい
}

void test({int? x, int? y}) {
  var ans = (x ?? -1) * (y ?? -1);
  print(ans);
}
```

通常の引数（位置引数と呼ばれます）と名前付き引数を混在させることもできます。位置引数は名前付き引数より前に書かないといけません。

``` dart linenums="1" hl_lines="2 5"
void main() {
  test(3, b: 4);
}

void test(int a, {int? b}) {
  print('$a $b');
}
```

呼び出すときには、名前付き引数の後ろでも呼び出せます。

``` dart linenums="1" hl_lines="2 5"
void main() {
  test(b: 4, 3); // 3がaに代入されます
}

void test(int a, {int? b}) {
  print('$a $b');
}
```

### やってみよう3

次の関数を作成しましょう。

``` dart linenums="1"
void view({int? a, int? b}) {
  print('$a $b');
}
```

作成後、`main`関数内で次の3パターンで呼び出してみましょう。

- `a`を3
- `b`を9
- `b`を4、`a`を1

??? 解答例

    ``` dart linenums="1"
    void main() {
      view(a: 3);
      view(b: 9);
      view(b: 4, a: 1);
    }

    void view({int? a, int? b}) {
      print('$a $b');
    }
    ```

## 必須パラメーター

``` dart linenums="1" hl_lines="6"
void main() {
  test(b: 3);
  // test(a: 1); // エラー
}

void test({int? a, required int b}) {
  print('$a $b');
}
```

### やってみよう3

次の関数を作成しましょう。

``` dart linenums="1"
void view({required int a, required int b}) {
  print('$a $b');
}
```

作成後、`main`関数内で次の引数で呼び出してみましょう。エラーになるものはエラーになることを確認してください。

- `a`を3
- `b`を9
- `b`を4、`a`を1

??? 解答例

    ``` dart linenums="1"
    void main() {
      // view(a: 3); // エラー
      // view(b: 9); // エラー
      view(b: 4, a: 1);
    }

    void view({required int? a, required int? b}) {
      print('$a $b');
    }
    ```

## オプションパラメーター

引数を`[]`で囲むと、引数の指定が任意になります。任意の変数のため、後述する`required`を付けない場合には、型は`null`許容型にしないといけません。オプションパラメーターは、位置引数となります。

次の例の場合には、`count`がオプションパラメーターとなっています。

``` dart linenums="1" hl_lines="6"
void main() {
  printName('たなか');
  printName('さとう', 4);
}

void printName(String name, [int? count]) {
  for (var i = 0; i < (count ?? 1); i++) {
    print(name);
  }
}
```

DartはJavaと違いメソッドのオーバーロード（名前が同じで、引数が異なる）ができません。関数の場合も同名の関数を複数作ることはできません。その代わりにオプションパラメーターを活用することができます。

## デフォルト値

引数にはデフォルト値を設定できます。デフォルト値を使うことでnull許容型を避けることができます。

オプションパラメーターの例のプログラムでデフォルト値を使うとコードをスッキリさせることができます。

デフォルト値は、`引数 = デフォルト値`とすることで設定できます。

``` dart linenums="1" hl_lines="6"
void main() {
  printName('たなか');
  printName('さとう', 4);
}

void printName(String name, [int count = 1]) {
  for (var i = 0; i < count; i++) {
    print(name);
  }
}
```

名前付き引数でもデフォルト値を利用できます。

``` dart linenums="1" hl_lines="6"
void main() {
  test();
  test(a: 9);
}

void test({int a = 2, int b = 4}) {
  print('$a $b');
}
```

## やってみよう4

次のプログラムを入力しましょう。

``` dart linenums="1"
void main() {
  test(1);
}

void test(int x, [int? y]) {
  print(x + (y ?? 0));
}
```

`test`関数の引数`y`の`?`を消すとエラーになることを確認しましょう。また、その状態から引数`y`に初期値0を設定してエラーが消えることを確認しましょう。

## やってみよう5

次のプログラムを入力しましょう。名前付き引数とデフォルト値をどのように活用しているかを考えてみましょう。

``` dart linenums="1"
void main() {
  print('1,000円の税込価格=${calcPrice(1000)}円');
  print('1,000円の20%割引の税込価格=${calcPrice(1000, discountRate: 20)}円');
  print(
    '1,000円の20%割引で消費税が15%の場合=${calcPrice(1000, taxRate: 15, discountRate: 20)}円',
  );
}

int calcPrice(int price, {int taxRate = 10, int discountRate = 0}) {
  final discountPrice = (price * (100 - discountRate)) ~/ 100;
  return (discountPrice * (100 + taxRate)) ~/ 100;
}
```

## main関数

`main`関数はエントリーポイントになっています。`main`関数は戻り値は`void`で、引数はなしか`List<String>`になります。

引数を`List<String>`にした場合には、コマンドライン引数を受け取ることができます。

``` dart linenums="1"
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

``` dart linenums="1" hl_lines="2"
void main(List<String> arguments) {
  var p = printHello; // 関数自体を変数pに代入

  p(); // 変数に代入された関数が実行される
}

void printHello() {
  print('hello');
}
```

## 匿名関数

名前がない（名前をつけない）関数のことを匿名関数といいます。

例えば、次のように作ることができます。引数がなく戻り値もない関数です。Dartの関数は戻り値や引数の型が明らかな場合には明示しなくてもいいです。

``` dart linenums="1" hl_lines="2-4"
void main() {
  var f = () {
    print('hello');
  };

  f();
}
```

匿名関数の場合には、戻り値がある場合に戻り値の型は書くことができません。次の場合、`return 1`としていることから戻り値は`int`型と判断されます。

``` dart linenums="1" hl_lines="2-4"
void main() {
  var f = () { // var f = int () { とは書けない
    return 1;
  };

  print(f());
}
```

匿名関数は、アロー関数にしても構いません。

``` dart linenums="1" hl_lines="2"
void main() {
  var f = () => print('hello');

  f();
}
```

???+ note "即時関数"

    JavaScriptの即時関数のようなことも可能です（Dartの場合は必要なケースがあるかは別問題です）。

    次の例の場合には、匿名関数が定義されて、それが即実行されます。

    ``` dart linenums="1"
    void main() {
      (() {
        print('hello');
      })();
    }
    ```

## 関数を引数にする

関数の引数を関数にすることができます。次の例では引数なし、戻り値なしの関数を引数としています。その場合には、引数の型は`void f()`となります。仮引数名は`f`となります。

``` dart linenums="1" hl_lines="2 5"
void main() {
  repeat(times: 2, () => print('hello'));
}

void repeat(void Function() f, {int times = 1}) {
  for (var i = 0; i < times; i++) {
    f();
  }
}
```

もう一つ例を示しますが、引数がある場合です。引数を匿名関数とする場合には、引数の型が明確なため、引数の型を省略することができます。

``` dart linenums="1" hl_lines="2 5"
void main() {
  repeat(times: 5, (x) => print(x * 2));
}

void repeat(void f(int i), {int times = 1}) {
  for (var i = 1; i <= times; i++) {
    f(i);
  }
}
```

## 関数を戻り値にする

関数を戻り値にする場合には、`Function`型を使います。下記の例の場合には、`test`関数の戻り値を単純に`Function`だけにしても動作はします。ただ、できるだけ詳細な戻り値の型にしたほうが良いため、戻り値を`int Function(int n)`としています。これは、戻り値が関数で、その関数は`int`型の引数を取り、戻り値が`int`型ということを表しています。

``` dart linenums="1" hl_lines="2 6"
void main() {
  var f = test();
  print(f(5));
}

int Function(int n) test() {
  return (int x) {
    return x * 2;
  };
}
```

この場合、`return`していて型が明確になるので戻り値を省略することができます。

``` dart linenums="1" hl_lines="6"
void main() {
  var f = test();
  print(f(5));
}

test() {
  return (int x) {
    return x * 2;
  };
}
```

Javaと違いクロージャーを作ることもできます。

``` dart linenums="1"
void main() {
  var counter1 = counter();
  var counter2 = counter();
  counter1();
  counter1();
  counter2();
  counter1();
  counter2();
}

void Function() counter() {
  var count = 0;
  return () {
    count++;
    print(count);
  };
}
```

???+ note "クロージャー"

    クロージャーは、関数が宣言されたときのスコープの変数を保持しているものです。

    上記の例だと、`counter1`と`counter2`はそれぞれ別の`count`を保持しています。そのため、`counter1`と`counter2`を呼び出すとそれぞれ別の値としてカウントアップしていきます。
