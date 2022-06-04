# ListView

`ListView`はスクロール可能なウィジェットです。

## 最初の例

`ListView`に画面からはみ出る程度の内容を含んだサンプルです。

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
      body: ListView(
        children: [
          for (var i = 1; i <= 20; i++)
            Container(
              padding: const EdgeInsets.all(20.0),
              child: Text('List$i'),
            )
        ],
      ),
    );
  }
}
```

`ListView`の主なコンストラクタ引数は次のとおりです。

| 引数 | 型 | 説明 |
|-|-|-|
| `children` | `<Widget>[]` | リストに含めるウィジェット |

### コンテナの装飾

`Container`に対して、`decoration`を設定できます。例えば、下線を引くには次のようにします。

```dart
Container(
  padding: const EdgeInsets.all(20.0),
  decoration: const BoxDecoration(
    border: Border(
      bottom: BorderSide(
        width: 1.0,
        color: Colors.black,
      ),
    ),
  ),
  child: Text('List$i'),
)
```

### 表示件数が不明の場合

表示する件数が事前にw駆らないような場合には`ListView.builder`を使います。

```dart
body: ListView.builder(
  itemBuilder: (context, index) {
    return Container(
      decoration: const BoxDecoration(
        border: Border(
          bottom: BorderSide(
            width: 1.0,
            color: Colors.grey,
          ),
        ),
      ),
      child: Text('$index番目'),
    );
  },
),
```

この例では、無限にリストを作成しています。特にリストの項目が多い場合に、すべてのウィジェットを最初に作ってしまうと、メモリ等の資源を使ってしまいますし、時間もかかってしまいます。

`ListView.builder`では表示される部分のウィジェットを用意しますので、効率よくリストを扱うことができます。

