# レイアウト

## サンプルアプリケーションを変更する

サンプルアプリケーションの内容を削除（必要ならすぐにまた作れます）して、次のコードのみ残します。

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const LayoutTest(),
    );
  }
}

class LayoutTest extends StatelessWidget {
  const LayoutTest({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('レイアウトテスト'),
      ),
    );
  }
}
```

以下、`LaytouTest`クラスを変更していきます。

### ButtonNavigationBar

`Scaffold`のコンストラクタの引数に次のものを追加します。

```dart
bottomNavigationBar: BottomNavigationBar(
  items: const <BottomNavigationBarItem>[
    BottomNavigationBarItem(
      icon: Icon(Icons.home),
      label: 'ホーム',
    ),
    BottomNavigationBarItem(
      icon: Icon(Icons.settings),
      label: '設定',
    ),
    BottomNavigationBarItem(
      icon: Icon(Icons.search),
      label: '検索',
    ),
  ],
  currentIndex: 0,
),
```

`BottomNavigationBar`は、次のようにボタンを並べます。

![ナビゲーションバー](image/01.webp)

`BottomNavigationBar`はコンストラクタで`items`を指定します。`items`は`BottomNavigationBarItem`の`List`を指定します。`BottomNavigationBarItem`はそれぞれ1つのボタンに対応します。`icon`と`label`を指定します。また、どのボタンが選ばれているかを`currentIndex`で指定します。

#### 練習問題

上記の`BottomNavigationBar`のサンプルに`BottomNavitaionBarItem`を1つ追加する頃こと。

### FloatingActionButton

次に`FloatingActionButton`を追加します。`BottomNavigationBar`の次に`floatingActionButton`を追加します。

```dart
floatingActionButton: FloatingActionButton(
  onPressed: () {},
  child: const Icon(Icons.plus_one),
),
```

これで、画面の下部に次のように追加されます。

![floatingActionButton](image/02.webp)

ボタンを押したときに反応がないのは寂しいので、ボタンを押したときの反応を追加します。ボタンを押したときには、`onPressed`で指定した関数が呼び出されます。その関数の中で、`SnackBar`を使いメッセージを表示します。

```dart
onPressed: () {
  ScaffoldMessenger.of(context).showSnackBar(const SnackBar(
    content: Text('ボタンが押されました！'),
  ));
},
```

ボタンを押すと次のように表示され、しばらく待つと消えます。

![SnackBar](image/03.webp)

## いろいろな配置

### 横に並べる

部品を横に並べたいときには、`Row`クラスを使います。

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const LayoutTest(),
    );
  }
}

class LayoutTest extends StatelessWidget {
  const LayoutTest({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('レイアウトテスト'),
      ),
      body: Row(
        children: const [
          Text('テキスト1'),
          Text('テキスト2'),
          Text('テキスト3'),
          Text('テキスト4'),
        ],
      ),
    );
  }
}
```

