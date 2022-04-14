# クラス

## インスタンス変数

クラスは`class`の後にクラス名を付けます。

```dart
class Human {}
```

インスタンス変数は`{}`の中に書きます。他の言語のようにアクセス修飾子（`public`、`private`）は付けられません。初期化していない場合には、`null`になる場合があるため、`?`が必要です。

```dart
class Human {
  String? name;
  int? age;
  List<Human> family = [];
}
```

`late`をつけることで初期化を遅らせることができます。

```dart
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

```dart
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

```dart
class Human {
  String? name;
  int? age;
  List<Human> family = [];

  Human(this.name, this.age);
}
```

コンストラクタを明示して作らなかった場合、自動的に引数なしの何もしないコンストラクタが作られます。

## 名前付きコンストラクタ

複数のコンストラクタを作る場合には、名前付きコンストラクタとして作成します。

```dart
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

## スーパークラスのコンストラクタ

コンストラクタの呼び出しがあると、親クラスの引数なしのコンストラクタが呼び出されてから、コンストラクタが実行されます。

```dart
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

```dart
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

```dart
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

```dart
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

## クラスの継承

`exnteds`でクラスの継承が行なえます。

```dart
class Book {
  String name;
  int price;

  Book(this.name, this.price);
}

class DartBook extends Book {
  DartBook(name, price) : super(name, price);

  void printInfo() {
    print('$name ${price}円');
  }
}

void main() {
  var b = DartBook('Dart', 2980);
  b.printInfo();
}
```

### メソッドのオーバーライド

`@override`アノテーションを付けると、正しくオーバーライドされているかのチェックが行われます。

```dart
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

`enum`で列挙型を作成できます。列挙型はJavaとは違い、メソッド等を持つことはできません。

```dart
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

```dart
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

```dart
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

```dart
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
