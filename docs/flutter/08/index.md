# ファイルを分けてアプリを作成する（TODO）

## 初期画面部分を別ファイルにする

初期表示される画面を`view`フォルダ内に、`todo_page.dart`ファイルとして作成します。画面の定義だけで、ボタン等は動きません。最初に作ったものと違い、TODOの追加はFloatingActionButtonを使います。

```dart title="view/todo_page.dart"
import 'package:flutter/material.dart';

class TodoPage extends StatefulWidget {
  const TodoPage({Key? key}) : super(key: key);

  @override
  State<TodoPage> createState() => _TodoPageState();
}

class _TodoPageState extends State<TodoPage> {
  final _todos = [];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('TODO'),
      ),
      body: Column(
        children: [
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
                          child: Text(_todos[index]),
                        ),
                        TextButton(
                          onPressed: () {},
                          child: const Text('変更'),
                        ),
                        TextButton(
                          onPressed: () {},
                          child: const Text('削除'),
                        ),
                      ],
                    ),
                  ),
                );
              },
              itemCount: _todos.length,
            ),
          )
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

初期画面を表示するために、`main.dart`を次のようにします。

```dart title="main.dart"
import 'package:flutter/material.dart';
import 'view/todo_page.dart';

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
      home: const TodoPage(),
    );
  }
}
```

ファイルを分割した場合、そのファイルで定義されたクラス等を使いたい場合には、`import`を書きます。`import`は取り込みたいファイルの相対パスを記載します。

## 追加できるようにする

最初にTODOを追加できるようにします。まず、`TodoState`クラスに次のメソッドを追加します。

```dart title="view/todo_page.dart"
void showAddDialog() async {
  final textController = TextEditingController();
  var result = await showDialog(
    context: context,
    builder: (_) {
      return AlertDialog(
        title: const Text('TODOの追加'),
        content: Padding(
          padding: const EdgeInsets.only(
            left: 10.0,
            right: 10.0,
          ),
          child: TextField(
            controller: textController,
            decoration: const InputDecoration(labelText: 'やること'),
          ),
        ),
        actions: [
          TextButton(
            child: const Text('キャンセル'),
            onPressed: () => Navigator.pop(
              context,
              null,
            ),
          ),
          TextButton(
            child: const Text('登録'),
            onPressed: () => Navigator.pop(
              context,
              textController.text,
            ),
          ),
        ],
      );
    },
  );
  if (result != null && result.length != 0) {
    setState(() {
      _todos.insert(0, result);
    });
  }
}
```

また、`FloatingActionButton`を押したときに、このメソッドが呼び出される夜にします。

```dart
onPressed: showAddDialog,
```

## 削除できるようにする

`deleteTodo`メソッドを追加します。

```dart
void deleteTodo(int index) async {
  var result = await showDialog(
    context: context,
    builder: (_) {
      return AlertDialog(
        title: const Text('TODOの削除'),
        content: Text('「${_todos[index]}」を削除してよろしいですか'),
        actions: [
          TextButton(
            child: const Text('いいえ'),
            onPressed: () => Navigator.pop(
              context,
              false,
            ),
          ),
          TextButton(
            child: const Text('はい'),
            onPressed: () => Navigator.pop(
              context,
              true,
            ),
          ),
        ],
      );
    },
  );
  if (result) {
    setState(() {
      _todos.removeAt(index);
    });
  }
}
```

また、削除ボタンで、`deleteTodo`メソッドが呼び出されるようにします。

```dart
onPressed: () => deleteTodo(index),
```

## 更新できるようにする

更新画面は別の画面に表示するので、`view`フォルダ内に、`todo_update_page.dart`ファイルを作成します。

```dart
import 'package:flutter/material.dart';

class TodoUpdatePage extends StatelessWidget {
  TodoUpdatePage({Key? key, required String todo}) : super(key: key) {
    _textController.text = todo;
  }

  final TextEditingController _textController = TextEditingController();

  void updateTodo(BuildContext context) {
    Navigator.pop(context, _textController.text);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('todoの更新'),
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.only(
              left: 10.0,
              right: 10.0,
            ),
            child: TextField(
              controller: _textController,
              decoration: const InputDecoration(labelText: 'やること'),
            ),
          ),
          ElevatedButton(
            onPressed: () => updateTodo(context),
            child: const Text('更新'),
          ),
        ],
      ),
    );
  }
}
```

`todo_page.dart`に次の`import`を追加します。

```dart
import 'todo_update_page.dart';
```

`updateTodo`メソッドを追加します。

```dart
void updateTodo(BuildContext context, int index) async {
  var result = await Navigator.push(
    context,
    MaterialPageRoute(
      builder: (context) => TodoUpdatePage(
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

変更ボタンにメソッドを設定します。

```dart
onPressed: () => updateTodo(context, index),
```
