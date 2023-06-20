# 複雑なレイアウト

## ListView

`ListView`は同じ種類のデータを並べて、スクロール可能なウィジェットです。

### 最初の例

`ListView`に画面からはみ出る程度の内容を含んだサンプルです。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
import &#x27;package:flutter/material.dart&#x27;;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: &#x27;Flutter Demo&#x27;,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
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
        title: const Text(&#x27;レイアウトテスト&#x27;),
      ),
      body: ListView(
        children: [
          for (var i = 1; i &lt;= 20; i++)
            Container(
              padding: const EdgeInsets.all(20.0),
              child: Text(&#x27;List$i&#x27;),
            )
        ],
      ),
    );
  }
}

</code>
</pre>

??? オフライン用

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
            colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
            useMaterial3: true,
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

このパターンは、事前に表示件数がわかっている場合にのみ使えます。また、件数がわかっていても、多すぎる場合には次の、表示件数が不明の場合の方を使ったほうがいいです。

このパターンの場合、`children`に指定されているウィジェットがすべて作成されます。そのため、ウィジェットの数が多い場合にメモリをたくさん使用するおそれがあります。

次の`ListView.builder`コンストラクタを使用すると、画面に表示する必要があるウィジェットだけ作成されるので、効率的です。また、表示範囲外になったウィジェットは破棄されます。

### 表示件数が不明の場合

表示する件数が事前にわからないような場合には`ListView.builder`を使います。次の例は無限にリストを作成しています。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
import &#x27;package:flutter/material.dart&#x27;;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: &#x27;Flutter Demo&#x27;,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
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
        title: const Text(&#x27;レイアウトテスト&#x27;),
      ),
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
            child: Text(&#x27;$index番目&#x27;),
          );
        },
      ),
    );
  }
}
</code>
</pre>

??? オフライン用

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
            colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
            useMaterial3: true,
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
        );
      }
    }
    ```


この例では、無限にリストを作成しています。特にリストの項目が多い場合に、すべてのウィジェットを最初に作ってしまうと、メモリ等の資源を使ってしまいますし、時間もかかってしまいます。

`ListView.builder`では表示される部分のウィジェットを用意しますので、効率よくリストを扱うことができます。

## Table

`Table`は`HTML`の`<table>`タグと同じような構造になっています。

| HTML | Flutter |
|-|-|
| `<table>` | `Table` |
| `<tr>` | `TableRow` |
| `<td>` | `TableCell` |

最初に、3行3列の例を確認します。わかりやすいように、各セルに背景色をつけています。

<pre>
<code class="language-run-dartpad:theme-light:mode-flutter:ga_id-example1">
import &#x27;package:flutter/material.dart&#x27;;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: &#x27;Flutter Demo&#x27;,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
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
        title: const Text(&#x27;レイアウトテスト&#x27;),
      ),
      body: Table(
        children: [
          TableRow(
            children: [
              TableCell(
                child: Container(
                  decoration: BoxDecoration(
                    color: Colors.blue[100],
                  ),
                  child: Text(
                    &#x27;1&#x27;,
                    style: Theme.of(context).textTheme.headlineMedium,
                  ),
                ),
              ),
              TableCell(
                child: Container(
                  decoration: BoxDecoration(
                    color: Colors.blue[200],
                  ),
                  child: Text(
                    &#x27;2&#x27;,
                    style: Theme.of(context).textTheme.headlineMedium,
                  ),
                ),
              ),
              TableCell(
                child: Container(
                  decoration: BoxDecoration(
                    color: Colors.blue[300],
                  ),
                  child: Text(
                    &#x27;3&#x27;,
                    style: Theme.of(context).textTheme.headlineMedium,
                  ),
                ),
              ),
            ],
          ),
          TableRow(
            children: [
              TableCell(
                child: Container(
                  decoration: BoxDecoration(
                    color: Colors.red[100],
                  ),
                  child: Text(
                    &#x27;1&#x27;,
                    style: Theme.of(context).textTheme.headlineMedium,
                  ),
                ),
              ),
              TableCell(
                child: Container(
                  decoration: BoxDecoration(
                    color: Colors.red[200],
                  ),
                  child: Text(
                    &#x27;2&#x27;,
                    style: Theme.of(context).textTheme.headlineMedium,
                  ),
                ),
              ),
              TableCell(
                child: Container(
                  decoration: BoxDecoration(
                    color: Colors.red[300],
                  ),
                  child: Text(
                    &#x27;3&#x27;,
                    style: Theme.of(context).textTheme.headlineMedium,
                  ),
                ),
              ),
            ],
          ),
          TableRow(
            children: [
              TableCell(
                child: Container(
                  decoration: BoxDecoration(
                    color: Colors.green[100],
                  ),
                  child: Text(
                    &#x27;1&#x27;,
                    style: Theme.of(context).textTheme.headlineMedium,
                  ),
                ),
              ),
              TableCell(
                child: Container(
                  decoration: BoxDecoration(
                    color: Colors.green[200],
                  ),
                  child: Text(
                    &#x27;2&#x27;,
                    style: Theme.of(context).textTheme.headlineMedium,
                  ),
                ),
              ),
              TableCell(
                child: Container(
                  decoration: BoxDecoration(
                    color: Colors.green[300],
                  ),
                  child: Text(
                    &#x27;3&#x27;,
                    style: Theme.of(context).textTheme.headlineMedium,
                  ),
                ),
              ),
            ],
          ),
        ],
      ),
    );
  }
}
</code>
</pre>

??? オフライン用

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
            colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
            useMaterial3: true,
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
          body: Table(
            children: [
              TableRow(
                children: [
                  TableCell(
                    child: Container(
                      decoration: BoxDecoration(
                        color: Colors.blue[100],
                      ),
                      child: Text(
                        '1',
                        style: Theme.of(context).textTheme.headlineMedium,
                      ),
                    ),
                  ),
                  TableCell(
                    child: Container(
                      decoration: BoxDecoration(
                        color: Colors.blue[200],
                      ),
                      child: Text(
                        '2',
                        style: Theme.of(context).textTheme.headlineMedium,
                      ),
                    ),
                  ),
                  TableCell(
                    child: Container(
                      decoration: BoxDecoration(
                        color: Colors.blue[300],
                      ),
                      child: Text(
                        '3',
                        style: Theme.of(context).textTheme.headlineMedium,
                      ),
                    ),
                  ),
                ],
              ),
              TableRow(
                children: [
                  TableCell(
                    child: Container(
                      decoration: BoxDecoration(
                        color: Colors.red[100],
                      ),
                      child: Text(
                        '1',
                        style: Theme.of(context).textTheme.headlineMedium,
                      ),
                    ),
                  ),
                  TableCell(
                    child: Container(
                      decoration: BoxDecoration(
                        color: Colors.red[200],
                      ),
                      child: Text(
                        '2',
                        style: Theme.of(context).textTheme.headlineMedium,
                      ),
                    ),
                  ),
                  TableCell(
                    child: Container(
                      decoration: BoxDecoration(
                        color: Colors.red[300],
                      ),
                      child: Text(
                        '3',
                        style: Theme.of(context).textTheme.headlineMedium,
                      ),
                    ),
                  ),
                ],
              ),
              TableRow(
                children: [
                  TableCell(
                    child: Container(
                      decoration: BoxDecoration(
                        color: Colors.green[100],
                      ),
                      child: Text(
                        '1',
                        style: Theme.of(context).textTheme.headlineMedium,
                      ),
                    ),
                  ),
                  TableCell(
                    child: Container(
                      decoration: BoxDecoration(
                        color: Colors.green[200],
                      ),
                      child: Text(
                        '2',
                        style: Theme.of(context).textTheme.headlineMedium,
                      ),
                    ),
                  ),
                  TableCell(
                    child: Container(
                      decoration: BoxDecoration(
                        color: Colors.green[300],
                      ),
                      child: Text(
                        '3',
                        style: Theme.of(context).textTheme.headlineMedium,
                      ),
                    ),
                  ),
                ],
              ),
            ],
          ),
        );
      }
    }
    ```

実行すると次のようになります。

![Table](image/table01.webp)

実行結果の通り、`TableCell`を1行に3つ置くと、3等分されて配置されます。
