# クラス

## クラスの基本とインスタンス変数

クラスは`class`の後にクラス名を付けます。

``` dart linenums="1"
class Human {}
```

インスタンス変数は`{}`の中に書きます。Javaのようにアクセス修飾子（`public`、`private`）は付けられません。アクセスの制限は違った方法で行います。インスタンス変数は定義と同時に初期化しない場合には、`null`になる場合があるため`?`が必要です。

``` dart linenums="1"
class Human {
  String? name;
  int? age;
  List<Human> family = [];
}
```

インスタンス変数は、例えばコンストラクター等で必ず初期化される場合には`late`をつけることで初期化を遅らせることができます。その場合には`null`許容型にしなくても大丈夫です。

また、クラスのインスタンスの作成はコンストラクターを使いますが、Javaと違い`new`演算子は必須ではありません（通常は書きません）。

``` dart linenums="1"
void main() {
  var sample = Sample();
  print(sample.value);
}

class Sample {
  late int value;

  Sample() {
    value = 5;
  }
}
```

## コンストラクター

### コンストラクターの基本

Javaと同様にクラスと同名のメソッドのようなものを作るとコンストラクターになります。

``` dart linenums="1"
class Human {
  late String name;
  late int age;

  Human(String name, int age) {
    this.name = name;
    this.age = age;
  }
}
```

`this`はこの例のように使わないと区別がつかない（`name`がインスタンス変数か、コンストラクターの引数か）場合のみ記述します。

またこの例は、次のように省略して書くこともできます。この場合には、コンストラクターの引数でインスタンス変数の初期化をすることが明確になるため、`late`も必要なくなります。

``` dart linenums="1"
class Human {
  String name;
  int age;

  Human(this.name, this.age);
}
```

さらに、値の変更をしない場合には、`final`をつけることもできます。

``` dart linenums="1"
class Human {
  final String name;
  final int age;

  Human(this.name, this.age);
}
```

コンストラクターを明示して作らなかった場合、自動的に引数なしの特別なことをしないコンストラクターが作られます。

### 名前付きコンストラクター

関数のオーバーロードのようなことができないため、Javaと同様のコンストラクターのオーバーロードはできません。そのため、複数のコンストラクターが必要な場合には、`クラス名.名前()`として、名前付きのコンストラクターを作成します。

``` dart linenums="1"
class Human {
  final String name;
  final int age;

  Human(this.name, this.age);

  Human.origin() : name = 'Dummy', age = 30;
}
```

上記のようにコンストラクターの後ろに`:`をつけることでコンストラクターの初期化リストをつけることができます。初期化リストではフィールドの初期化をすることができます。

名前付きのコンストラクターは、使用する場合には名前まで記載します。

``` dart linenums="1"
class Human {
  final String name;
  final int age;

  Human(this.name, this.age);

  Human.origin() : name = 'Dummy', age = 30;
}

void main() {
  var humnan = Human.origin();
}
```

???+ note "初期化リスト"

    `final`のインスタンス変数を初期化するのは、初期化リストで行わないといけません。コンストラクター内で初期化しようとしてもエラーとなります。

    ``` dart linenums="1"
    class Human {
      final String name;
      final int age;

      Human(this.name, this.age);

      Human.origin() { // エラー
        name = 'Dummy';
        age = 30;
      }
    }
    ```

### 継承とスーパークラスのコンストラクター

Javaと同様に`extends`を使うと他のクラスを継承できます。Javaと同様に多重継承は行えず、親となるクラスは1つのみです。

``` dart linenums="1"
class Animal {}

class Cat extends Animal {}
```

このクラスにそれぞれ引数なしのコンストラクターを作成します。この状態で`Cat`クラスのインスタンスを作成します。

``` dart linenums="1"
class Animal {
  Animal() {
    print('Animalのコンストラクター');
  }
}

class Cat extends Animal {
  Cat() {
    print('Catのコンストラクター');
  }
}

void main() {
  var animal = Cat();
}
```

実行すると次のようになります。

```
Animalのコンストラクター
Catのコンストラクター
```

サブクラスの`Cat`から明示して、スーパークラスのコンストラクターを呼び出していませんが、実行結果を見ると呼び出されているのがわかります。これはつまり、Javaと同様にサブクラスでは明示してスーパークラスのコンストラクターを呼び出していない場合には、スーパークラスの引数なしのコンストラクターが呼び出されているということです。そのため、次のようにスーパークラスから引数なしのコンストラクターをなくしてしまうとエラーになります。

``` dart linenums="1"
class Animal {
  Animal(int n) {
    print('Animalのコンストラクター');
  }
}

class Cat extends Animal {
  Cat() {
    print('Catのコンストラクター');
  }
}
```

エラーを解決しようと次のようにすることはできません。

``` dart linenums="1" hl_lines="9"
class Animal {
  Animal(int n) {
    print('Animalのコンストラクター');
  }
}

class Cat extends Animal {
  Cat() {
    super(3); // エラー
    print('Catのコンストラクター');
  }
}
```

この解決法は、このあと説明します。

### 他のコンストラクターの呼び出し（コンストラクターのリダイレクト）

`this`を使って他のコンストラクターを呼び出すには、`this`を使います。この場合には、コンストラクターの本体は書くことができません。

``` dart linenums="1"
class Animal {
  Animal() {
    print('Animalのコンストラクター');
  }

  Animal.other() : this();
}

void main() {
  var animal = Animal.other();
}
```

同様にスーパークラスのコンストラクターを呼び出す事もできます。この場合は、コンストラクターの本体を記載できます。

``` dart linenums="1"
class Animal {
  Animal(int n) {
    print('Animalのコンストラクター');
  }
}

class Cat extends Animal {
  Cat() : super(3) {
    print('Catのコンストラクター');
  }
}
```

### コンストラクターでの名前付き引数

コンストラクターでも名前付きの引数を利用できます。

``` dart linenums="1"
class Human {
  late final String name;
  late final int age;

  Human({required String name, required int age}) {
    this.name = name;
    this.age = age;
  }
}

void main() {
  var human = Human(name: 'テスト', age: 30);
}
```

このような場合、次のように省略した書き方で同様のことができます。

``` dart linenums="1"
class Human {
  final String name; // lateは不要
  final int age;

  Human({required this.name, required this.age});
}

void main() {
  var human = Human(name: 'テスト', age: 30);
}
```

## メソッド

### 演算子

演算子を定義して、インスタンス同士の演算を行うことができます。

``` dart linenums="1"
class Complex {
  int real;
  int imaginary;

  Complex({required this.real, required this.imaginary});

  Complex operator +(Complex v) =>
      Complex(real: real + v.real, imaginary: imaginary + v.imaginary);

  Complex operator -(Complex v) =>
      Complex(real: real - v.real, imaginary: imaginary - v.imaginary);

  @override
  bool operator ==(Object other) =>
      other is Complex && real == other.real && imaginary == other.imaginary;

  @override
  String toString() {
    return '$real${imaginary < 0 ? "" : "+"}${imaginary}i';
  }
}

void main() {
  final v1 = Complex(real: 2, imaginary: 1);
  final v2 = Complex(real: 4, imaginary: 4);

  print(v1 + v2);
  print(v1 - v2);
}
```

## 抽象クラス

抽象クラスを作成することもできます。Javaと同じです。抽象メソッドには`abstract`は不要です。単純にメソッドの実態を記載しません。メソッドのオーバーライドの際`@override`アノテーションは必須ではありませんが、つけることでオーバーライドできていない場合（スーパークラスにメソッドがない場合等）に警告が表示されます。

``` dart linenums="1"
abstract class Device {
  void printInfo();
}

class Computer extends Device {
  @override
  void printInfo() {
    print('computer');
  }
}
```

## 暗黙のインタフェース

すべてのクラスは、暗黙的にインタフェースが作られます。インタフェースの実装は`implements`で行います。

``` dart linenums="1"
abstract class Animal {
  void move() {
    print('Animal is moving');
  }
}

class Cat implements Animal {
  @override
  void move() {
    print('Cat is moving!');
  }
}

void main() {
  var cat = Cat();
  cat.move();
}
```

この場合、`Animal`クラスで定義されている`move`メソッドのみが定義されたインターフェースになるため、`Cat`クラスでオーバーライドしないといけません。

## ライブラリープライバシー

インスタンス変数やクラス等の名前の先頭を`_`とすることで、ライブラリー内（ファイル内）でのみアクセスが可能になります。Javaのような`private`といった概念はなく、ファイル単位ので管理になります。

``` dart linenums="1"
class Human {
  String _name;
  int _age;

  Human(this._name, this._age);
}

void main() {
  var h = Human('Dartくん', 22);
  // 同じファイルの場合は読み書きできる
  print('${h._name} ${h._age}');
}
```

ここで、`main`関数と`Human`クラスを別のファイルとして定義し、次のようにすることでエラーになることを確認できます。

``` dart linenums="1" title="human.dart"
import 'human.dart';

void main() {
  var h = Human('Dartくん', 22);
  // 同じファイルの場合は読み書きできる
  print('${h._name} ${h._age}');
}
```

``` dart linenums="1"
import 'human.dart';

void main() {
  var h = Human('Dartくん', 22);
  // 同じファイルの場合は読み書きできる
  print('${h._name} ${h._age}');
}
```

## プロパティ

Javaでいうクラスのフィールドは、Dartの場合は単純なフィールドではありません。例えば次のようなクラスで見ていきます。

``` dart linenums="1"
class Human {
  int age;

  Human(this.age);
}

void main() {
  final human = Human(20);
  print('年齢=${human.age}');
  human.age = 30;
  print('年齢=${human.age}');
}
```

`Human`クラス内で`public`なインスタンス変数（変数名が`_`で始まらない）を作成した場合には自動的にプロパティになり、`getter`、`setter`が作成されます。

`main`関数内で、`human.age`と参照した場合には`getter`が呼び出され、`human.age = 30`のように代入した場合には`setter`が呼び出されます。

また、次のように自分で明示してプロパティを作成することができます。

``` dart linenums="1"
class Human {
  int _age;

  int get age => _age;

  set age(int value) => _age = value;

  Human(this._age);
}

void main() {
  final human = Human(20);
  print('年齢=${human.age}');
  human.age = 30;
  print('年齢=${human.age}');
}
```

`getter`は次のように定義します。

```
型 get プロパティ名 => 値;
```

もしくは、関数の形で書く場合。

```
型 get プロパティ名 {
  // 処理を書いてもいい
  return 値;
}
```

`setter`は同様に次のように定義します。

```
set プロパティ名(型 変数名) => 値をセットする処理;
```

もしくは、関数の形で書く場合。

```
set プロパティ名(型 変数名) {
  // 値をセットする処理
}
```

メソッドとプロパティはできることが一部重複しています。使い分けの基準は次のようにするのが良いです。

- プロパティは、内部状態のセットや確認に用いる
- メソッドはアクション（動詞）

例えば、次のようなクラスを参考にしてください。

## mixin

mixinはクラスのようなものですが、コンストラクターを含むことはできません。また、インスタンスを生成することはできません。抽象クラスに似ていて、他のクラスと合成をして利用します。

例えば、`Log`を出力するためのmixinを次のように作成して利用します。

``` dart linenums="1"
mixin Log {
  void log(String message) {
    print('${DateTime.now()} $message');
  }
}

class Human with Log {}

void main() {
  var human = Human();
  human.log('begin');
  human.log('end');
}
```

上記の`Log`クラスのように多数のクラスで共通の機能があった際に、mixinの意味が出てきます。`Log`をクラスや抽象クラスで作成することもできますが、多重継承ができないため`Log`を継承すると他のクラスを継承できないという制約が生まれます。インターフェースにすると、継承の製薬は生まれませんが、実装を記載することができないため目的の機能自体が実装できません。

また、mixinを使うことができる型を`on`を使って制限することができます。

``` dart linenums="1"
class Dart {}

mixin Book on Dart {
  String name = '本';
  int price = 0;
}

class DartBook extends Dart with Book {}

void main() {
  var d = DartBook();
  print(d.name);
}
```

## staticメソッド

staticメソッド（クラスメソッド）もJavaとほぼ同じように作ることができます。

``` dart linenums="1"
class Math {
  static var pi = 3.1415;

  static double circleArea(double r) {
    return r * r * pi;
  }
}

void main() {
  print('円周率は${Math.pi}');
  print('半径5の円の面積は${Math.circleArea(5)}');
}
```

## 拡張メソッド

ここは[ドキュメントのサンプル](https://dart.dev/language/extension-methods){targer=_blank}を例にします。

文字列を`int`型に変換するには、`int.parse`メソッドを使います。

``` dart
int.parse('42');
```

これが、もっと直感的に次のようにできるといいのではないかというところから来ています。

``` dart
'42'.parseInt();
```

ただ、`String`クラスは組み込みのクラスで継承ができないため簡単に行きません。そういったとき拡張メソッドが活用できます。

今回は`String`クラスを拡張してメソッドを追加したいので次のように定義します。

``` dart linenums="1"
extension NumberParsing on String {
  int parseInt() {
    return int.parse(this);
  }
}
```

そうすることで、次のように動作させることができます。

``` dart linenums="1"
void main() {
  var n = '42'.parseInt();
  print(n * 10);
}
```

## 呼び出し可能なオブジェクト

クラスのインスンタンスを関数のように呼び出せる機能があり、呼び出し可能なオブジェクトと言います。呼び出し可能なオブジェクトは、`call`という名前のメソッドを定義することで実装できます。`call`メソッドの引数と戻り値は自由です。具体的に見ていきます。

``` dart linenums="1"
class Counter {
  var count = 0;
  int call() {
    count++;
    return count;
  }
}

void main() {
  var counter = Counter();
  print(counter());
  print(counter());
  print(counter());
  print(counter());
}
```

これは、関数として利用したいが、内部状態を保持したいという場合に使うことができます。

## 拡張型

拡張型を使うことで、`int`や`double`に意味的な単位を持たせて、型安全性を担保することができます。例えば、長さの単位のマイルとメートルで考えます。

``` dart linenums="1"
extension type const Meter(double value) {}
extension type const Mile(double value) {}

Meter mileToMeter(Mile mile) {
  return Meter(mile.value * 1.609);
}

Mile meterToMile(Meter meter) {
  return Mile(meter.value / 1.609);
}

void main() {
  var speed = Mile(100);
  var meter = mileToMeter(speed);
  print('100マイルは${meter}m');
}
```

このようなプログラムを書くと、次の3行はすべてエラーになります。

``` dart linenums="1"
var n = speed + meter;
var m = speed + 1;
var s = meter + 1;
```

## 列挙型

`enum`で列挙型を作成できます。

``` dart linenums="1"
enum Color { red, green, blue }

void main() {
  print('赤 ${Color.red.index}');
  print('緑 ${Color.green.index}');
  print('青 ${Color.blue.index}');

  print(Color.values);

  var c = Color.red;

  switch (c) {
    case Color.red:
      print('赤です');
      break;
    case Color.green:
      print('緑です');
      break;
    case Color.blue:
      print('青です');
      break;
  }
}
```

