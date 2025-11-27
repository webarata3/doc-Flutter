# TODOのデータベース対応

## 必要なパッケージの設定

`pubspec.yaml`ファイルに次の内容を追加します。追加するのは、`sqflite`と`path`の2つです。インデントに意味があるので、追加する部分の先頭には必ずスペース2つを入れてください。

``` yaml linenums="1" hl_lines="5-6"
dependencies:
  flutter:
    sdk: flutter

  sqflite: ^2.4.2
  path: ^1.9.1
```

## モデルの作成

TODOのデータを扱うために、モデルのクラスを作成します。TODOの1件に該当するクラスを次のように作成します。

``` dart linenums="1" title="model/todo.dart"
class Todo {
  final int? todoId;
  final String title;

  const Todo({
    this.todoId,
    required this.title,
  });

  static Todo fromJson(Map<String, Object?> map) => Todo(
        todoId: map['todo_id'] as int,
        title: map['title'] as String,
      );

  Map<String, Object?> toJson() => {
        'todo_id': todoId,
        'title': title,
      };
}
```

`todoId`はテーブルの主キーに対応する部分です。データの更新や削除の際に必要になります。実際にデータベースとデータをやり取りする場合には、データを`Map`に入れて操作します。そのため、`Map`から変換する`fromJson`と、`Map`へ変換する`toJson`の2つのメソッドがあります。

実際のデータベースを操作するクラスを次のように作成します。

``` dart linenums="1" title="helper/db_helper.dart"
import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';
import '../model/todo.dart';

class DbHelper {
  static final DbHelper instance = DbHelper._createInstance();
  static Database? _database;

  DbHelper._createInstance();

  Future<Database> get database async {
    return _database ??= await _initDatabase();
  }

  Future<Database> _initDatabase() async {
    final path = join(await getDatabasesPath(), 'todo.db');
    return openDatabase(
      path,
      version: 1,
      onCreate: _onCreate,
    );
  }

  Future _onCreate(Database database, int version) async {
    await database.execute('''
      CREATE TABLE todo(
        todo_id INTEGER PRIMARY KEY AUTOINCREMENT,
        title TEXT NOT NULL
      )
    ''');
  }

  Future create(Todo todo) async {
    final database = await instance.database;
    await database.insert('todo', todo.toJson());
  }

  Future<List<Todo>> selectAll() async {
    final database = await instance.database;
    final todoMapList = await database.query(
      'todo',
      orderBy: 'todo_id DESC',
    );

    return todoMapList.map((map) => Todo.fromJson(map)).toList();
  }

  Future<int> update(Todo todo) async {
    final database = await instance.database;

    return database.update(
      'todo',
      todo.toJson(),
      where: 'todo_id = ?',
      whereArgs: [todo.todoId],
    );
  }

  Future<int> delete(int todoId) async {
    final database = await instance.database;

    return await database.delete(
      'todo',
      where: 'todo_id = ?',
      whereArgs: [todoId],
    );
  }

  Future closeDatabase() async {
    final database = await instance.database;

    database.close();
  }
}
```

## データベースに登録できるようにする

最初に、TODOをデータベースに追加できるようにします。

まず、`import`を追加します。

``` dart linenums="1" title="view/todo_page.dart"
import '../helper/db_helper.dart';
import '../model/todo.dart';
```

`_TodoPageState`に次のものを追加します。

``` dart linenums="1"
var _todos = <Todo>[];

@override
void initState() {
  super.initState();
  getTodos();
}

@override
void dispose() {
  DbHelper.instance.closeDatabase();
  super.dispose();
}

void getTodos() async {
  _todos = await DbHelper.instance.selectAll();
  setState(() {});
}
```

`ListView`のTODOを表示する`Text`の内容を変更します。

``` dart linenums="1"
Expanded(
  flex: 1,
  child: Text(_todos[index].title),
),
```

`showAddDialog`の最後の`if`文を次のように変更します。

``` dart linenums="1"
if (result != null && result.length != 0) {
  await DbHelper.instance.create(Todo(title: result));
  getTodos();
}
```

`updateTodo`でエラーになるので、一時的にコメントにしておきます。

```dart
void updateTodo(BuildContext context, int index) async {
  // var result = await Navigator.push(
  //   context,
  //   MaterialPageRoute(
  //     builder: (context) => TodoUpdatePage(
  //       todo: _todos[index],
  //     ),
  //   ),
  // );
  // if (result == null) return;
  // setState(() {
  //   _todos[index] = result;
  // });
}
```

## 削除できるようにする

`deleteTodo`メソッドの`AlertDialog`コンストラクタの引数`content`を次のように変更します。

```dart
content: Text('「${_todos[index].title}」を削除してよろしいですか'),
```

また、`deleteTodo`の最後の`if`文を次のように変更します。

```dart
if (result != null && result) {
  var todoId = _todos[index].todoId;
  if (todoId == null) return;
  await DbHelper.instance.delete(todoId);
  getTodos();
}
```

## 更新できるようにする

`updateTodo`メソッドの`TodoUpdatePage`コンストラクタの引数`todo`を次のように変更します。

```dart
todo: _todos[index].title,
```

最後の`if`文以降を次のようにします。

```dart
if (result == null) return;
await DbHelper.instance
    .update(Todo(todoId: _todos[index].todoId, title: result));
getTodos();
```
