# サンプルアプリケーションを読む

## 最小限のコード

自動生成されるコードのコメントを取り除いたコードです。

``` dart linenums="1"
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

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
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
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
              style: Theme.of(context).textTheme.headlineMedium,
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

``` dart linenums="1"
import 'package:flutter/material.dart';
```

Dartのプログラムですので、`main`関数がエントリーポイントであることは変わりありません。

``` dart linenums="1"
void main() {
  runApp(const MyApp());
}
```

`main`関数では、[runApp関数](https://api.flutter.dev/flutter/widgets/runApp.html)を呼び出します。`runApp`関数は、引数として`Widget`クラスのインスタンスが必要です。例では、`StatelessWidget`クラスを継承した`MyApp`クラスのインスタンスを渡しています。

`Widget`クラスを継承したクラスとしては、`StatelessWidget`や`StatefulWidget`があります。

- `StatelessWidget`は状態を持たない（変更がない）Widgetです
- `StatefulWidget`は状態を持つ（変更がある）Widgetです

次に`MyApp`クラスを見てみます。

``` dart linenums="1"
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
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

[ThemeData](https://api.flutter.dev/flutter/material/ThemeData-class.html)クラスのインスタンスを指定します。ここでは、紫系の色を指定しています。

`home`に指定したのは、`MyHomePage`クラスのインスタンスです。

`MyHomePage`クラスは次のようになっています。

``` dart linenums="1"
class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key, required this.title}) : super(key: key);

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}
```

画面の内容が変更される事がある（状態を持つ）場合、`StatefulWidget`クラスを使います。`StatefulWidget`クラスでは、`createState`メソッドをオーバーライドし、`State`クラスのインスタンスを作成します。

ここでは、`State`クラスを継承した`_MyHomePageState`クラスを作成しています。変更される状態自体はこちらのクラスで所持します。

``` dart linenums="1"
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
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
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
              style: Theme.of(context).textTheme.headlineMedium,
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

`_MyHomePageState`クラスは`State`クラスを継承しています。`State`クラスは少し定義が難しいですが、`State<T extends StatefulWidget>`という定義になっています。`<>`の中にかけるのは`StatefulWidget`クラスのサブクラスのみということになります。

`StatefulWidget`を実装したクラスでは、このクラスの中に状態（要は変数）を持ちます。今回は、単純に数字をカウントしていくので、`_count`変数を用意しています。

状態を変更させる場合には`setState`メソッドを呼び出して変更しないといけません。`setState`メソッドを経由することで変更があったことをフレームワークに通知します。`setState`メソッドは、引数が関数になっています。この関数は、引数も戻り値もありません。この関数の中で変数を変更します。

`_MyHomePageState`クラスでは、`build`メソッドをオーバーライドします。`StatelessWidget`クラスの`build`と同様のものになります。

`State`クラスのでは制御できるタイミングとしては、次の場合に呼び出されます。

- `StatefulWidget.initState`の後
- `StatefulWidget.didUpdateWidget`の後
- `setState`の呼び出しの後

`build`の引数としてはこの場合には`Scaffold`クラスを使っています。`Scaffold`の引数として、3つ指定されています。

`appBar`は上部のタイトルの部分になっています。

`body`はアプリの主となる部分です。ここでは`Center`クラスを使っています。`Center`クラスは引数`child`の内容を中央に表示します。今回`child`には`Column`クラスを使っています。

`Column`クラスは、引数の`children`の内容を縦に並べて表示します。また、縦に並べた内容を中央寄せにするために、引数`mainAxisAlignment`を`MainAxisAlignment.center`にしています。`children`では、`Text`クラスのインスタンスを使っています。`Text`クラスは文字を表示するためのクラスですが、`style`を使用して文字の大きさ等のスタイルを設定することができます。

最後に、`floatingActionButton`ですが、これは画面の右下に出ている![ボタン](image/flutter01.webp)の部分です。`child`で`+`のアイコンを設定し、このボタンを押したときの処理は`onPressed`で設定しています。`onPressed`の引数に、このクラスの`_incrementCounter()`メソッドを設定しています。`onPressed`の型は`VoidCallback`となっており、このボタンが押されたときに呼び出される関数を指定します。

#### 練習問題1-1

`_MyHomePageState`クラス内の`You have pushed the button this many times:`という文字列を`ボタンをたくさん押しましょう`に変えること。

#### 練習問題1-2

`_MyHomePageState`クラス内の、`Center`クラスのコンストラクタの引数`child`を`Column`から`Row`に変更し、どのように表示されるか確認すること。

#### 練習問題1-3

練習問題1-2で変更した`Row`クラスのコンストラクタの引数`children`について、その2番目の`Text`クラスのコンストラクタの引数`style`の`Theme.of(context).textTheme.headline4`の最後の数字`4`を`1`や`6`に変更してみること。

#### 練習問題1-4

`MyApp`クラス内の`MaterialApp`クラスのコンストラクタの引数`theme`内の`Colors.deepPurple`を`Colors.green`や、`Colors.red`に変えてみること。
