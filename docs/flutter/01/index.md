# Flutterの基本

## 最小限のコード

自動生成されるコードのコメントを取り除いたコードです。

```dart title="main.dart"
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
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key, required this.title}) : super(key: key);

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headline4,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

Flutterを使うためには1行目の`import`が必要です。

```dart
import 'package:flutter/material.dart';
```

Dartのプログラムですので、`main`関数がエントリーポイントであることは変わりありません。

```dart
void main() {
  runApp(const MyApp());
}
```

`main`関数では、[runApp関数](https://api.flutter.dev/flutter/widgets/runApp.html)を呼び出します。`runApp`関数は、引数として`Widget`クラスのインスタンスが必要です。例では、`StatelessWidget`クラスを継承した`MyApp`クラスのインスタンスを渡しています。

`Widget`クラスを継承したクラスとしては、`StatelessWidget`や`StatefulWidget`があります。

- `StatelessWidget`は状態を持たない（変更がない）Widgetです
- `StatefulWidget`は状態を持つ（変更がある）Widgetです

次に`MyApp`クラスを見てみます。

```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

`StatelessWidget`クラスの`build`メソッドは[ドキュメントより](https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html)次の3つの場合に呼び出されます。

- ウィジェットが初めてツリーに挿入されるとき
- ウィジェットの親が設定を変更するとき
- 依存するInheritedWidgetが変更されるとき

`build`メソッドでは`Widget`のインスタンスを返します。ここでは、`MaterialApp`クラスのインスタンスを作成し、それを返しています。

`MaterialApp`クラスはマテリアルデザインのアプリを作るためのクラスです。`MaterialApp`クラスのコンストラクタは[クラス定義](https://api.flutter.dev/flutter/material/MaterialApp/MaterialApp.html)を見ても分かる通り、たくさんの名前付き引数が設定されています。すべて任意もしくは、初期値が設定されているため、必要なもののみ指定します。ここでは、実際に指定されているものだけを確認します。

- `title`: どこに反映されているか未確認
- `theme`: デフォルトのテーマ。`ThemeData`クラスのインスタンスを指定します
- `home`: 最初に表示される`Widget`

[ThemeData](https://api.flutter.dev/flutter/material/ThemeData-class.html)クラスのインスタンスを指定します。ここでは、青系の色を指定しています。

`home`に指定したのは、`MyHomePage`クラスのインスタンスです。
