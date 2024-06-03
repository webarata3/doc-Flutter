# クラス

## インスタンス変数

クラスは`class`の後にクラス名を付けます。

``` dart linenums="1"
class Human {}
```

インスタンス変数は`{}`の中に書きます。Javaのようにアクセス修飾子（`public`、`private`）は付けられません。インスタンス変数は定義と同時に初期化していない場合には、`null`になる場合があるため、`?`が必要です。

``` dart linenums="1"
class Human {
  String? name;
  int? age;
  List<Human> family = [];
}
```

インスタンス変数は（例えば、コンストラクタ等で）必ず初期化される場合には`late`をつけることで初期化を遅らせることができます。

``` dart linenums="1"
void main() {
  var s = Sample('名前');
  print(s.name);
}

class Sample {
  late String name;

  Sample(this.name);
}
```

## コンストラクタ

クラスと同名の関数を作るとコンストラクタになります。

``` dart linenums="1"
class Human {
  String? name;
  int? age;
  List<Human> family = [];

  Human(String name, int age) {
    this.name = name;
    this.age = age;
  }
}
```

`this`は上記のように使わないと区別がつかない場合のみ記述します。

上記の例は、次のように省略して書くこともできます。

``` dart linenums="1"
class Human {
  String? name;
  int? age;
  List<Human> family = [];

  Human(this.name, this.age);
}
```

コンストラクタを明示して作らなかった場合、自動的に引数なしの何もしないコンストラクタが作られます。

また、この場合、`name`と`age`は必ず初期化されるので、`?`は不要です。また、`final`にすることもできます。

``` dart linenums="1"
void main() {
  var h = Human('Dartくん', 3);
}

class Human {
  final String name;
  final int age;
  List<Human> family = [];

  Human(this.name, this.age);
}
```

## 名前付きコンストラクタ

複数のコンストラクタを作る場合には、名前付きコンストラクタとして作成します。

``` dart linenums="1"
class Human {
  String? name;
  int? age;
  List<Human> family = [];

  Human(this.name, this.age);

  Human.origin()
      : name = 'Dummy',
        age = 30;
}
```

上記のようにコンストラクタの後ろに`:`をつけることでコンストラクタの初期化リストをつけることができます。初期化リストではフィールドの初期化をすることができます。

``` dart linenums="1"
void main() {
  var h = Human('Dartくん', 3);
}

class Human {
  final String name;
  final int age;
  List<Human> family = [];

  Human(this.name, this.age);

  Human.init()
      : name = '初期化くん',
        age = 10;
}
```

## スーパークラスのコンストラクタ

コンストラクタの呼び出しがあると、親クラスの引数なしのコンストラクタが呼び出されてから、コンストラクタが実行されます。

``` dart linenums="1"
class Animal {
  Animal() {
    print('Animalのコンストラクタ');
  }
}

class Cat extends Animal {
  Cat() {
    print('Catのコンストラクタ');
  }
}

void main() {
  var animal = Cat();
}
```

## 他のコンストラクタの呼び出し

`this`を使って他のコンストラクタを呼び出すこともできます。

``` dart linenums="1"
class Animal {
  Animal() {
    print('Animalのコンストラクタ');
  }

  Animal.other() : this();
}

void main() {
  var animal = Animal.other();
}
```

## 抽象クラス

インスタンスを作成できない、抽象クラスを作成することもできます。

``` dart linenums="1"
abstract class Device {
  void printInfo();
}

class Computer extends Device {
  void printInfo() {
    print('computer');
  }
}
```

## 暗黙のインタフェース

すべてのクラスは、暗黙的にインタフェースが作られます。インタフェースの実装は`implements`で行います。

``` dart linenums="1"
abstract class Human {
  String _name;

  Human(this._name);

  void printInfo();
}

class Man implements Human {
  String? name2;
  String get _name => '';
  void set _name(value) {
    name2 = value;
  }

  void printInfo() {
    print(name2);
  }
}
```

インタフェースはJavaと同じように、複数`implements`することができます。

インタフェースは実装を持たないため、次のように書くと`Human`インターフェースの`printInfo`は実装を持たないため、`Man`クラスでオーバーライドしないとエラーになります。

``` dart linenums="1"
class Human {
  void printInfo() {
    print('人間です');
  }
}

class Man implements Human {
  // エラー。printInfoをオーバーライドしないといけない
}
```

## クラスの継承

`exnteds`でクラスの継承が行なえます。

``` dart linenums="1"
class Book {
  String name;
  int price;

  Book(this.name, this.price);
}

class DartBook extends Book {
  DartBook(name, price) : super(name, price);

  void printInfo() {
    print('$name $price円');
  }
}

void main() {
  var b = DartBook('Dart', 2980);
  b.printInfo();
}
```

### メソッドのオーバーライド

`@override`アノテーションを付けると、正しくオーバーライドされているかのチェックが行われます。

``` dart linenums="1"
class Book {
  String name;
  int price;

  Book(this.name, this.price);

  void printInfo() {
    print('$name ${price}円');
  }
}

class DartBook extends Book {
  DartBook(name, price) : super(name, price);

  @override
  void printInfo() {
    print('この本は$name ${price}円');
  }
}
```

### 列挙型

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

## mixin

mixinはクラスのようなものですが、コンストラクタを含むことはできません。

``` dart linenums="1"
mixin Book {
  String name = '本';
  int price = 0;
}

class DartBook with Book {}

void main() {
  var d = DartBook();
  print(d.name);
}
```

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

## プロパティとprivate

インスタンス変数は、変数名の先頭に`_`をつけることで、privateにすることができます。privateにしたものは、別のファイルからは読み書きすることはできません。

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

別のファイルでもアクセスする必要がある場合には、getterとsetterを用意します。それぞれ次のように書きます。

``` dart linenums="1"
class Human {
  String _name;
  int _age;

  String get name => _name;

  set name(String value) {
    _name = value;
  }

  int get age => _age;

  set age(int value) {
    _age = value;
  }

  Human(this._name, this._age);
}

void main() {
  var h = Human('Dartくん', 22);
  // setterを使うときには = で代入するように使える
  h.name = 'プロパティさん';
  h.age = 22;
  // getterはインスタンス変数と同様に使える
  print('${h.name} ${h._age}');
}
```
