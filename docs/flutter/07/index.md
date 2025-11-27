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
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const TodoPage(title: 'Todo'),
    );
  }
}

class TodoPage extends StatefulWidget {
  const TodoPage({super.key, required this.title});

  final String title;

  @override
  State<TodoPage> createState() => _TodoPageState();
}

class _TodoPageState extends State<TodoPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(widget.title)),
      body: Column(
        children: [
          Padding(
            padding: .only(left: 10.0, right: 10.0),
            child: TextField(decoration: InputDecoration(labelText: 'やること')),
          ),
          ElevatedButton(onPressed: () {}, child: Text('追加')),
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
final _todos = <String>[];
```

次に、++"追加"++ボタンのあとに、`ListView`を追加します。

```dart
Expanded(
  flex: 1,
  child: ListView.builder(
    itemBuilder: (context, index) {
      return Container(
        decoration: BoxDecoration(
          border: Border(
            bottom: .new(width: 1.0, color: Colors.grey),
          ),
        ),
        child: Container(
          padding: .all(10.0),
          child: Row(
            children: [Expanded(flex: 1, child: Text(_todos[index]))],
          ),
        ),
      );
    },
    itemCount: _todos.length,
  ),
),
```

## 追加ボタンで追加できるようにする

++"追加"++ボタンを押したときにTODOを追加できるようにします。

![画像](image/todo03.webp)

最初に、`TextField`の値を取得するために、`ElevatedButton`の引数の`controller`を設定します。合わせて、フィールドにコントローラを追加します。

```dart title="フィールドに追加"
final _textController = TextEditingController();
```

```dart title="TextFieldに追加"
controller: _textController,
```

```dart
void _addTodo() {
  var text = _textController.text;
  if (text.isEmpty) {
    return;
  }
  setState(() {
    _textController.clear();
    _todos.insert(0, text);
  });
}
```

```dart title="ElevatedButtonのonPressedに追加"
onPressed: _addTodo,
```

## 削除できるようにする

追加はできたので、追加したものが削除できるようにします。行ごとに++"削除"++ボタンを作ります。

![Todo](image/todo04.webp)

TODOを削除するためのメソッド`deleteTodo`を最初に作ります。

```dart
void _deleteTodo(int index) {
  setState(() {
    _todos.removeAt(index);
  });
}
```

次に、++"削除"++ボタンを作成します。

```dart
TextButton(
  onPressed: () => _deleteTodo(index),
  child: const Text('削除'),
),
```

## 変更画面への切り替え

変更は、++"変更"++ボタンを押したら画面が切り替わり、そこで変更するようにします。

まずは、次のような変更画面を作成します。

![画面](image/todo05.webp)

```dart
class _TodoUpdatePage extends StatelessWidget {
  _TodoUpdatePage();

  void _updateTodo(BuildContext context) {
    Navigator.pop(context);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('todoの更新')),
      body: Column(
        children: [
          Padding(
            padding: .only(left: 10.0, right: 10.0),
            child: TextField(decoration: InputDecoration(labelText: 'やること')),
          ),
          ElevatedButton(
            onPressed: () => _updateTodo(context),
            child: Text('更新'),
          ),
        ],
      ),
    );
  }
}
```

次に画面の++"変更"++ボタンを押したときに画面が切り替わるようにします。

まずは、画面の切り替え用のメソッドを追加します。

```dart
void _updateTodo(BuildContext context) {
  Navigator.push(
    context,
    MaterialPageRoute(
      builder: (context) => _TodoUpdatePage(),
    ),
  );
}
```

次に、++"削除"++ボタン前に++"変更"++ボタンを付けます。

```dart
TextButton(
  onPressed: () => _updateTodo(context),
  child: Text('変更'),
),
```

ここまでで画面が切り替わるようになります。

## 変更画面に選んだTODOを表示する

画面の切り替えができたので、変更画面を表示する際に、選択されたTODOを表示されるようにします。

まず、`_TodoUpdatePage`のフィールドに追加します。

```dart
final _textController = TextEditingController();
```

次に`_TodoUpdatePage`のコンストラクタを次のものにします。

```dart
_TodoUpdatePage({required String todo}) {
  _textController.text = todo;
}
```

最後に、`TextField`に`controller`を追加します。

```dart
controller: _textController,
```

`_TodoPageState`の`updataTodo`を次のようにします。

```dart
void updateTodo(BuildContext context, int index) {
  Navigator.push(
    context,
    MaterialPageRoute(
      builder: (context) => _TodoUpdatePage(
        todo: _todos[index],
      ),
    ),
  );
}
```

また、++"変更"++ボタンの`onPressed`を次のようにします。

```dart
onPressed: () => _updateTodo(context, index),
```

## 変更できるようにする

```dart
Future<void> _updateTodo(BuildContext context, int index) async {
  var result = await Navigator.push(
    context,
    MaterialPageRoute(
      builder: (context) => _TodoUpdatePage(
        todo: _todos[index],
      ),
    ),
  );
  if (result == null) return;
  setState(() {
    _todos[index] = result;
  });
}
```

```dart
void _updateTodo(BuildContext context) {
  Navigator.pop(context, _textController.text);
}
```

完成版。

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
      home: const TodoPage(title: 'Todo'),
    );
  }
}

class TodoPage extends StatefulWidget {
  const TodoPage({super.key, required this.title});

  final String title;

  @override
  State<TodoPage> createState() => _TodoPageState();
}

class _TodoPageState extends State<TodoPage> {
  final _todos = <String>[];
  final _textController = TextEditingController();

  void _addTodo() {
    var text = _textController.text;
    if (text.isEmpty) {
      return;
    }
    setState(() {
      _textController.clear();
      _todos.insert(0, text);
    });
  }

  void _deleteTodo(int index) {
    setState(() {
      _todos.removeAt(index);
    });
  }

  Future<void> _updateTodo(BuildContext context, int index) async {
    var result = await Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => _TodoUpdatePage(todo: _todos[index]),
      ),
    );
    if (result == null) return;
    setState(() {
      _todos[index] = result;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(widget.title)),
      body: Column(
        children: [
          Padding(
            padding: .only(left: 10.0, right: 10.0),
            child: TextField(
              controller: _textController,
              decoration: InputDecoration(labelText: 'やること'),
            ),
          ),
          ElevatedButton(onPressed: _addTodo, child: Text('追加')),
          Expanded(
            flex: 1,
            child: ListView.builder(
              itemBuilder: (context, index) {
                return Container(
                  decoration: BoxDecoration(
                    border: Border(
                      bottom: .new(width: 1.0, color: Colors.grey),
                    ),
                  ),
                  child: Container(
                    padding: .all(10.0),
                    child: Row(
                      children: [
                        Expanded(flex: 1, child: Text(_todos[index])),
                        TextButton(
                          onPressed: () => _updateTodo(context, index),
                          child: const Text('変更'),
                        ),
                        TextButton(
                          onPressed: () => _deleteTodo(index),
                          child: const Text('削除'),
                        ),
                      ],
                    ),
                  ),
                );
              },
              itemCount: _todos.length,
            ),
          ),
        ],
      ),
    );
  }
}

class _TodoUpdatePage extends StatelessWidget {
  _TodoUpdatePage({required String todo}) {
    _textController.text = todo;
  }

  final _textController = TextEditingController();

  void _updateTodo(BuildContext context) {
    Navigator.pop(context, _textController.text);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('todoの更新')),
      body: Column(
        children: [
          Padding(
            padding: .only(left: 10.0, right: 10.0),
            child: TextField(
              controller: _textController,
              decoration: InputDecoration(labelText: 'やること'),
            ),
          ),
          ElevatedButton(
            onPressed: () => _updateTodo(context),
            child: const Text('更新'),
          ),
        ],
      ),
    );
  }
}
```
