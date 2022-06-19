# TODOアプリ

次のようなTODOアプリを作成します。

![アプリ](image/todo01.webp)

## 入力欄を作成

最初に、TODOを入力するテキストフィールドまで作成します。`const`関連の警告が出ますが、後でまとめて対応するためスルーします。

![アプリ](image/todo02.webp)

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
      home: const TodoPage(title: 'Todo'),
    );
  }
}

class TodoPage extends StatefulWidget {
  const TodoPage({Key? key, required this.title}) : super(key: key);

  final String title;

  @override
  State<TodoPage> createState() => _TodoPageState();
}

class _TodoPageState extends State<TodoPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.only(
              left: 10.0,
              right: 10.0,
            ),
            child: TextField(
              decoration: const InputDecoration(labelText: 'やること'),
            ),
          ),
          ElevatedButton(
            onPressed: () {},
            child: const Text('追加'),
          ),
        ],
      ),
    );
  }
}
```

## TODOを表示するListView

次に、TODOを表示するための`ListView`を用意します。また、`ListView`に表示するための`List`として`todos`を用意します。

画面は変わりません。

まず、フィールドとして`_TodoPageState`クラスに次のものを追加します。

```dart
var todos = [];
```

次に、++"追加"++ボタンのあとに、`ListView`を追加します。

```dart
Expanded(
  flex: 1,
  child: ListView.builder(
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
          child: Container(
            padding: const EdgeInsets.all(10.0),
            child: Row(
              children: [
                Expanded(
                  flex: 1,
                  child: Text(todos[index]),
                ),
              ],
            ),
          ));
    },
    itemCount: todos.length,
  ),
),
```

## 追加ボタンで追加できるようにする

++"追加"++ボタンを押したときにTODOを追加できるようにします。

![画像](image/todo03.webp)

最初に、`TextField`の値を取得するために、`ElevatedButton`の引数の`controller`を設定します。合わせて、フィールドにコントローラを追加します。

```dart title="フィールドに追加"
var textController = TextEditingController();
```

```dart title="ElevatedButtonに追加"
controller: textController,
```

```dart
void addTodo() {
  var text = textController.text;
  if (text.isEmpty) {
    return;
  }
  setState(() {
    textController.clear();
    todos.insert(0, text);
  });
}
```

```dart title="ElevatedButtonのonPressedに追加"
onPressed: addTodo,
```

## 削除できるようにする

追加はできたので、追加したものが削除できるようにします。行ごとに++"削除"++ボタンを作ります。

![Todo](image/todo04.webp)

TODOを削除するためのメソッド`deleteTodo`を最初に作ります。

```dart
void deleteTodo(int index) {
  setState(() {
    todos.removeAt(index);
  });
}
```

次に、++"削除"++ボタンを作成します。

```dart
TextButton(
  onPressed: () => deleteTodo(index),
  child: const Text('削除'),
),
```
