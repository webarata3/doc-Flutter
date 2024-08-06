# 画面の切り替え

## 単純な画面切り替え

2画面しかないような場合の画面の切り替えは、`Navigator`クラスの`push`と`pop`を使用します。

まず具体的な例で確認します。

``` dart linenums="1"
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
      home: const FirstPage(),
    );
  }
}

class FirstPage extends StatelessWidget {
  const FirstPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("最初のページ"),
      ),
      body: Center(
        child: TextButton(
          child: const Text("次のページ"),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => const SecondPage(),
              ),
            );
          },
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  const SecondPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('2つ目のページ'),
      ),
      body: Center(
        child: TextButton(
          child: const Text('戻る'),
          onPressed: () {
            Navigator.pop(context);
          },
        ),
      ),
    );
  }
}
```

`push`を用いると任意の画面へ遷移することができます。

``` dart linenums="1"
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => const SecondPage(),
  ),
);
```

`pop`を使って、前の画面に戻ります。

``` dart linenums="1"
Navigator.pop(context);
```
