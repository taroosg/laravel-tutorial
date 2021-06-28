# Laravelプロジェクト作成

ここから，Laravelのアプリケーションを作成していく．まずは Laravelフレームワークの基本的なファイルを準備する必要がある（準備はコマンドが用意されている）．

## インストーラの準備

まずはファイルのダウンロードに使用するインストーラを用意する．下記コマンドを実行．

```bash
$ sudo composer global require "laravel/installer"
```

なにか訊かれたら`yes`を入力してEnter．

```bash
Continue as root/super user [yes]?
```

実行結果

```bash
...
6 package suggestions were added by new dependencies, use `composer suggest` to see details.
Generating autoload files
10 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
```

続いて，Laravelのプロジェクトを作成する．Laravelでは「プロジェクト」という単位でアプリケーションを管理する．一つのアプリケーションが一つのプロジェクト，という理解で OK．

下記コマンドを実行する．`laravel_todo`が今回のアプリケーション名．

```bash
$ composer create-project laravel/laravel laravel_todo
```

実行結果（しばらく時間がかかる）

```bash
Package manifest generated successfully.
73 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
> @php artisan key:generate --ansi
Application key set successfully.
```

プロジェクトのディレクトリ（`laravel_todo`）に移動する．以降，アプリケーションに関する操作はすべて`laravel_todo`内で実行する．

```bash
$ cd laravel_todo
```

下記コマンドでインストールされているLaravelのバージョンを確認できる．

```bash
$ php artisan -V
Laravel Framework 8.20.1
```

## Laravelのインストール確認

Laravelインストールの確認にはwebブラウザで動作させて確認する．Cloud9にはwebサーバが標準で搭載されているため，以下の手順で確認を行う．

コマンド実行中は他のコマンドを打てないため，別タブで新しくターミナルを立ち上げておくと便利．

下記コマンドを実行し，webサーバを立ち上げる．

```bash
$ php artisan serve --port=8080
```

実行結果

```bash
Starting Laravel development server: http://127.0.0.1:8080
[Mon Jan  4 13:23:03 2021] PHP 8.0.0 Development Server (http://127.0.0.1:8080) started
```

- 【重要】サーバーが動いた状態となるが，停止する場合は「`ctrl + c`」で停止できる．

- 実際の動作は下記手順で確認できる．

  1. 画面上部の「preview」をクリック．
  2. 「prewiew running application」をクリック．
  3. 右下にプレビュー画面が表示される．
  4. プレビュー画面右上の「Browserの右側のボタン」をクリック．
  5. 新しいタブで下記画面が表示されればOK．

![トップ画面](../img/20210104-laravel-firstview.png)

動作を確認できたらCloud9のターミナル上で`ctrl + c`を入力し，一旦サーバを停止させておこう．

