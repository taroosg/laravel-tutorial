# Laravelプロジェクト作成

下記コマンドで一撃．`laravel_todo`部分がプロジェクト名となる．

```bash
$ curl -s https://laravel.build/laravel_todo | bash
```

実行結果

```bash
 _                               _
| |                             | |
| |     __ _ _ __ __ ___   _____| |
| |    / _` | '__/ _` \ \ / / _ \ |
| |___| (_| | | | (_| |\ V /  __/ |
|______\__,_|_|  \__,_| \_/ \___|_|

Warning: TTY mode requires /dev/tty to be read/writable.
    Creating a "laravel/laravel" project at "./laravel_todo"
    Installing laravel/laravel (v8.5.20)
      - Downloading laravel/laravel (v8.5.20)
      - Installing laravel/laravel (v8.5.20): Extracting archive
...
```

途中，Laravel側でいろいろ実行するためPCのログインパスワードを求められるので入力する．

```bash
...
Please provide your password so we can make some final adjustments to your application's permissions.

[sudo] password for taroosg:

Thank you! We hope you build something incredible. Dive in with: cd laravel_todo && ./vendor/bin/sail up

```


## 権限の変更

この方法でプロジェクトを作成するとエディタで操作する場合に権限で引っかかる場合があるので権限を変更する．

```bash
$ sudo chmod -R 777 laravel_todo
```


## 起動

早速仮想コンテナを立ち上げる．

```bash
$ cd laravel_todo
$ ./vendor/bin/sail up -d
```

実行結果

```bash
Creating network "laravel_todo_sail" with driver "bridge"
Creating volume "laravel_todo_sailmysql" with local driver
Creating volume "laravel_todo_sailredis" with local driver
Creating volume "laravel_todo_sailmeilisearch" with local driver
Creating laravel_todo_mailhog_1     ... done
Creating laravel_todo_meilisearch_1 ... done
Creating laravel_todo_redis_1       ... done
Creating laravel_todo_selenium_1    ... done
Creating laravel_todo_mysql_1       ... done
Creating laravel_todo_laravel.test_1 ... done
```


## 動作確認

ブラウザでlocalhostにアクセスし，下記画面が表示されればOK．


![トップ画面](../img/20210104-laravel-firstview.png)


仮想マシン（コンテナ）を終了させる場合は以下のコマンドを実行する．

```bash
$ ./vendor/bin/sail down
```
