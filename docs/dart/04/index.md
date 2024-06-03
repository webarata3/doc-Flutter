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

``` dart linenums="1"
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

### 練習問題 関数-1

次の要件の関数を作成し、引数`5`で呼び出すこと。

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

### 練習問題 関数-2

1の問題の関数をアロー関数にすること。

??? 解答例

    ``` dart linenums="1"
    void main() {
      print(twice(5));
    }

    int twice(int num) {
      return num * 2;
    }
    ```

## 引数

関数は名前付きの引数も作成できます。

例えば、次のように作成して使用することができます。

``` dart linenums="1"
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

``` dart linenums="1"
void main() {
  test(3, b: 4);
}

void test(int a, {int? b}) {
  print('$a $b');
}
```

### 練習問題 名前付き引数-1

次の関数を作成すること。

- 関数名: `printMessage`
- 引数
    - 1つ目: 通常の位置引数 `String`型の`message`
    - 2つ目: 名前付き引数 `int`型の`count` 任意
- 戻り値: なし
- 処理内容: 引数の`message`を表示する。`count`が`null`でない場合には`count`の回数分`message`を表示する

また、次の引数で実行すること

- 1回目: ('こんにちは')
- 2回目: ('おはよう', 3)

??? 解答例

    ``` dart linenums="1"
    void main() {
      printMessage('こんにちは');
      printMessage('おはよう', count: 3);
    }

    void printMessage(String message, {int? count}) {
      for (var i = 0; i < (count ?? 1); i++) {
        print(message);
      }
    }
    ```

## オプションパラメータ

引数を`[]`で囲むと、引数の指定が任意になります。任意の変数のため、型は`null`許容型にしないといけません。

``` dart linenums="1"
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

``` dart linenums="1"
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

``` dart linenums="1"
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

``` dart linenums="1"
void main(List<String> arguments) {
  var p = printHello;

  p();
}

void printHello() {
  print('hello');
}
```

匿名関数を代入することもできます。

``` dart linenums="1"
void main(List<String> arguments) {
  var p = () => print('Hello');

  p();
}
```

## 匿名関数

匿名関数は処理内容が1行であれば、次のように書けます。

``` dart linenums="1"
void main(List<String> arguments) {
  var p = () => print('Hello');

  p();
}
```

処理がたくさんあれば、次のように通常の関数の定義から関数名を除いた形で書くことができます。

``` dart linenums="1"
void main(List<String> arguments) {
  [1, 2, 3].forEach((element) {
    print(element);
  });
}
```

### 練習問題 匿名関数-1

次の要件の関数を作成すること。

- 関数名: `add`
- 引数
    - 1つ目: `int`型の`x`
    - 2つ目: `int`型の`y`
- 戻り値: `int`
- 処理内容: 引数`x`と`y`の合計を返す

また、次の関数を入力すること。

``` dart linenums="1"
int calc(int x, int y, int f(int a, int b)) {
  return f(x, y);
}
```

最後に、`main`関数で`calc(3, 5, add)`として`calc`関数を呼び出し、結果を表示すること

??? 解答例

    ``` dart linenums="1"
    void main() {
      print(calc(3, 5, add));
    }

    int add(int x, int y) {
      return x + y;
    }

    int calc(int x, int y, int f(int a, int b)) {
      return f(x, y);
    }
    ```

### 練習問題 匿名関数-2

1の問題の`add`関数をアロー関数に書き換えること

??? 解答例

    ``` dart linenums="1"
    void main() {
      print(calc(3, 5, add));
    }

    int add(int x, int y) => x + y;

    int calc(int x, int y, int f(int a, int b)) {
      return f(x, y);
    }
    ```

### 練習問題 匿名関数-3

2の問題の`add`関数を削除し、`calc`関数の呼び出しの際に`add`の代わりに匿名関数を用いること。

??? 解答例

    ``` dart linenums="1"
    void main() {
      print(calc(3, 5, (x, y) => x + y));
    }

    int calc(int x, int y, int f(int a, int b)) {
      return f(x, y);
    }
    ```
