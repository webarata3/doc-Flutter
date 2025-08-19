# Firebase

## Firebase体験

最初にFirebaseのプロジェクトを作成します。作成するには、Webから行うのが簡単です。

[Firebase](https://firebase.google.com/?hl=ja){target=_blank}

Firebaseを使うためにはFirebaseを扱うためのツールが必要になります。ツールはNode.jsで動作しているためNode.jsをまずインストールします。

Node.jsインストール後、Firebaseのツールをインストールします。VSCodeで、++"ターミナル"++メニューから++"新しいターミナル"++を選択し、ターミナルを開きます。

開いたターミナルで次のコマンドを入力します。

```
npm install -g firebase-tools
```

管理者権限でPowershellを起動して、次のコマンドを入力します。

```
Set-ExecutionPolicy RemoteSigned
```

インストール後、Googleログインします。ターミナルで次のコマンドを入力するとブラウザが起動しますので、ブラウザからGoogleアカウントにログインします。

```
firebase login
```

次にプロジェクト用のフォルダを作成します。

```
md first
```

`md`はフォルダを作成するコマンドで、`first`という名前のフォルダを作成します。その後、そのフォルダに移動します。

```
cd first
```

ログイン後、次のコマンドでFirebaseを始めます。

```
firebase init
```

`firebase init`を使うと、実行したフォルダにFirebaseの設定が行われます。

あくまでも現在のフォルダに対しての設定になります。

Hostingの設定をします。Hostingの設定をすると`public`フォルダが作られます。その中に`index.html`ファイルがあるので、`body`の内容を次のように変更します。

```html
 <body>
  <h1>最初のFirebase</h1>
 </body>
```

ファイルを保存して、ターミナルから次のコマンドを入力します。

```
firebase deploy
```

そうすると、ローカルのファイルがサーバーにアップロードされ、公開されます。

コマンドの最後にHosging URLというのが表示されているので、コピーしてブラウザで表示してみてください。

```
+  Deploy complete!

Project Console: https://console.firebase.google.com/project/first-project-xxxxx/overview
Hosting URL: https://first-project-xxxxx.web.app ← ここの部分をコピーしてください
```

URLはプロジェクトIDがついていますが、このURLでインターネット上に公開されていて、誰でも見ることができるようになっています。

!!! warning

    インターネットに繋がれば誰でも見られるようになっているため、公序良俗に反するようなものや、他人の著作物をアップロードしないように十分注意してください。
