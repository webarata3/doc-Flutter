# クラス

## インスタンス変数

クラスは`class`の後にクラス名を付けます。

```dart
class Human {}
```

インスタンス変数は`{}`の中に書きます。他の言語のようにアクセス修飾子（`public`、`private`）は付けられません。

```dart
class Human {
  String? name;
  int? age;
  List<Human> family = [];
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
