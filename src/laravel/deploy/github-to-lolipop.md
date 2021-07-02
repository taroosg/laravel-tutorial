# GitHub上のコードをマネージドクラウドにデプロイ

ここまでで下記のように必要な通信ができるようになった．

- ローカルPCとGitHub
- ローカルPCとマネージドクラウドの仮想マシン
- マネージドクラウドの仮想マシンとGitHub

通信の環境が整ったので，マネージドクラウドの仮想マシン上にGitHubのコードをデプロイする．

## 仮想マシンの準備

>🔗 Lolipopでの作業

まず，デプロイに関する準備（以下）をする．

- デプロイ用ディレクトリの作成
- `composer`のダウンロード
- 必要なパスを通す
- Laravelの準備

### デプロイ用ディレクトリの作成

以下を順に実行する．

```bash
$ cd /var/www
$ mkdir bin
$ cd bin
```

### composerのダウンロード

```bash
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
$ php composer-setup.php
```

実行結果

```bash
All settings correct for using Composer
Downloading...

Composer (version 1.10.8) successfully installed to: /var/www/bin/composer.phar
Use it: php composer.phar
```

composerのファイルを移動する．

```bash
$ mv composer.phar composer
```

### パスを通す

下記のコマンドを実行する．

```bash
$ export PATH=$PATH:/var/www/bin
```

`.bash_profile`を開き，設定ファイルを編集する．

```bash
$ vi ~/.bash_profile
```

`.bash_profile`に以下を追記する．

```bash
export PATH=$PATH:/var/www/bin
```

### Laravelの準備

ディレクトリを移動し，Laravelのインストーラを準備する．

```bash
$ cd /var/www
$ composer global require 'laravel/installer'
```

実行結果

```bash
...
symfony/console suggests installing psr/log (For using the console logger)
guzzlehttp/guzzle suggests installing psr/log (Required for using the Log middleware)
Writing lock file
Generating autoload files
11 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
```


## GitHub上のコードをデプロイ

GitHubから自分が作成したプロジェクトのファイルをダウンロードして`project`ディレクトリにコピーし，アプリケーションが動作するように設定を行う．

```bash
$ git clone YOUR_REPOSITORY_URL project
```

実行結果

```bash
Cloning into 'project'...
Warning: Permanently added the RSA host key for IP address '11.11.111.11' to the list of known hosts.
remote: Enumerating objects: 140, done.
remote: Counting objects: 100% (140/140), done.
remote: Compressing objects: 100% (104/104), done.
remote: Total 140 (delta 17), reused 140 (delta 17), pack-reused 0
Receiving objects: 100% (140/140), 195.58 KiB | 0 bytes/s, done.
Resolving deltas: 100% (17/17), done.
Checking connectivity... done.
```

### 💡 Key Point

>初回は`git clone`コマンドでソースコードをダウンロードするが，2回め以降（機能追加した場合など）は`git pull`コマンドで差分管理する．

デプロイしたディレクトリに移動し，必要なライブラリをインストールする．

```bash
$ cd /var/www/project/
$ composer install
```

実行結果（長い）

```bash
...
Discovered Package: beyondcode/laravel-dump-server
Discovered Package: fideloper/proxy
Discovered Package: laravel/tinker
Discovered Package: nesbot/carbon
Discovered Package: nunomaduro/collision
Package manifest generated successfully.
43 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
```

## DB関連の準備

設定ファイルを準備し，DB関連の設定を記述する．

```bash
$ cp .env.example .env
$ vi .env
```

以下のように編集する．

```bash
DB_CONNECTION=mysql
DB_HOST=マネクラ管理画面にあるDBのホスト名
DB_PORT=3306
DB_DATABASE=マネクラ管理画面にあるDB名
DB_USERNAME=マネクラ管理画面にあるユーザー名
DB_PASSWORD=プロジェクト作成時に決めたパスワード
```

マイグレーションを実行してテーブルを準備する．

```bash
$ php artisan migrate
Migration table created successfully.
Migrating: 2014_10_12_000000_create_users_table
Migrated:  2014_10_12_000000_create_users_table (0.01 seconds)
Migrating: 2014_10_12_100000_create_password_resets_table
Migrated:  2014_10_12_100000_create_password_resets_table (0.01 seconds)
Migrating: 2019_08_19_000000_create_failed_jobs_table
Migrated:  2019_08_19_000000_create_failed_jobs_table (0 seconds)
Migrating: 2020_06_23_024905_create_tasks_table
Migrated:  2020_06_23_024905_create_tasks_table (0 seconds)
```

アプリケーションキーを作成する．

```bash
$ php artisan key:generate
```

実行結果

```bash
Application key set successfully.
```

## シンボリックリンクの作成

ルートディレクトリを設定できないので，`/var/www/html`をシンボリックリンクにして`/var/www/project/public/`を参照する．

```bash
$ rm -rf /var/www/html/
$ ln -s /var/www/project/public/ /var/www/html
```

ブラウザでプロジェクトのURLにアクセスして表示されればOK！

以上である( `･ω･)b
