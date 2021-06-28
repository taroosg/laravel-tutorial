# データベース準備

## MySQLの準備

今回のtodoリストアプリケーションでは，DBとしてMySQLを使用する．

すでにMySQL自体は動作する状態になっているが，Laravelから扱うにはいくつかの設定が必要となる．下記コマンドを実行し，MySQLにログインする．

```bash
$ sudo mysql
```

実行結果．今回は管理者でログインした状態．

```bash
...
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

下記コマンドを実行し，ユーザ名「`root`」パスワード「`root`」で入れるように設定する．

> 【解説】
>
> Laravelから扱うには，何らかのユーザ名とパスワードでMySQLにログインできるよう設定しておく必要がある．デプロイする際のレンタルサーバなどでは，予めユーザとパスワードが設定されている場合がほとんどなので本項の実施は必要ない場合が多い．

```bash
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
```

実行結果

```bash
Query OK, 0 rows affected (0.02 sec)
```

設定できたらログアウトする．

```bash
mysql> exit;
Bye
```

ユーザ名`root`パスワード`root`でログインできることを確認する．下記コマンドを実行．

```bash
$ mysql -u root -p
```

パスワードを求められるので「root」を入力して Enter（パスワードは画面上に表示されないので注意）．

実行結果

```bash
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

今回のアプリケーションで使用するデータベースを作成する．データベース名は `laravel_todo`とする．

下記コマンドを実行でデータベースを作成．

```bash
mysql> create database laravel_todo;
```

実行結果

```bash
Query OK, 1 row affected (0.00 sec)
```

下記コマンドを実行してデータベースの一覧を表示する．

```bash
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| laravel_todo       |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.02 sec)
```

確認できたらログアウトする．

```bash
mysql> exit;
Bye
```

これで MySQL 側の準備は完了．

## LaravelからDBに接続するための準備

続いて，LaravelからMySQLにアクセスするための設定を行う．前項で設定したユーザ名，パスワードなどを設定ファイルに記述する．

Cloud9画面左側のツリーから`.env`ファイルを開く．`.env`ファイルは`laravel_todo`ディレクトリの直下に配置されている．

なお，`.env`ファイルは隠しファイルなので表示されない場合がある．その場合は，ツリー画面右上の歯車マークをクリックして「Show Hidden Files」にチェックを入れると表示される．

`.env`ファイルをダブルクリックして開き，10-15行目を以下のように編集する（コメントは削除しよう）．

```env
DB_CONNECTION=mysql
DB_HOST=localhost         // 編集
DB_PORT=3306
DB_DATABASE=laravel_todo  // 編集
DB_USERNAME=root
DB_PASSWORD=root          // 編集
```

`.env`ファイルを更新したらキャッシュをクリアする．

> 【解説】
>
> 設定ファイルはサーバ起動時にキャッシュに保存されるため，変更した場合は「キャッシュをクリアする」「サーバを立ち上げ直す」のどちらかが必要になる．

```bash
$ php artisan config:cache
```

実行結果

```bash
Configuration cache cleared!
Configuration cached successfully!
```

