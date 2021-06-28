# Laravelインストール準備

## Node.js の更新

Laravel（7以降）では，JSやCSSをビルドする必要があり，その際にNode.jsが使用される（実際にNode.jsを書くことはない）．

しかし，AWS Cloud9ではNode.jsのバージョンが古く，ビルド時に失敗する．そのため，Node.jsを最新バージョンに更新する．

Node.jsのバージョンを確認するため，Cloud9のターミナルで下記コマンドを実行する．

```bash
$ node -v
v10.23.0
```

アップデートするため，下記コマンドを実行する．LTS（安定バージョン）の最新版をインストールする．

`nvm`は Node.jsの管理ツールであり，AWS Cloud9にはじめからインストールされている．

```bash
$ nvm install --lts
```

実行結果

```bash
Installing latest LTS version.
Downloading https://nodejs.org/dist/v14.15.3/node-v14.15.3-linux-x64.tar.xz...
################################################################################# 100.0%
Now using node v14.15.3 (npm v6.14.9)
nvm_ensure_default_set: a version is required
```

最新版がインストールされる（今回は`14.15.3`）．

このままだとデフォルトのバージョンが古いままなので，下記コマンドを実行してインストールした最新版をデフォルトにする（`v14.15.3`の部分は上でインストールした番号を入れること）．

```bash
$ nvm alias default v14.15.3
default -> v14.15.3
```

アップデート後のバージョンを確認する．

```bash
$ node -v
v14.15.3
```

バージョンが上がっていればOK．実行する時期により細かい数字は異なるが，`14.*.*`のバージョンであれば問題ない．

## PHPの更新

Laravelの最新版では，PHP7.2.25以上が必要となるが，AWS Cloud9上でインストールされているバージョンは7.2.24であるため，アップデートが必要になる．

今回は最新であるPHP8にアップデートする．

以下はバージョンの確認コマンド．

```bash
$ php -v
PHP 7.2.24-0ubuntu0.18.04.7 (cli) (built: Oct  7 2020 15:24:25) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.24-0ubuntu0.18.04.7, Copyright (c) 1999-2018, by Zend Technologies
    with Xdebug v2.6.0, Copyright (c) 2002-2018, by Derick Rethans
```

アップデートの準備をする．下記コマンドを実行する．今回のOS「Ubuntu」では，`apt`と呼ばれるパッケージ管理ツールを使用する．まずは`apt`に PHPのリポジトリを追加する（追加することで`apt`がPHPを扱えるようになる）．

```bash
$ sudo add-apt-repository ppa:ondrej/php
```

なにか訊かれたらEnterで続行．

```bash
...
# LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php
 More info: https://launchpad.net/~ondrej/+archive/ubuntu/php
Press [ENTER] to continue or Ctrl-c to cancel adding it.
```

実行結果

```bash
...
Get:7 http://ppa.launchpad.net/ondrej/php/ubuntu bionic/main amd64 Packages [81.2 kB]
Get:8 http://ppa.launchpad.net/ondrej/php/ubuntu bionic/main Translation-en [31.7 kB]
Fetched 222 kB in 1s (154 kB/s)
Reading package lists... Done
```

下記コマンドを実行し，`apt`が管理しているパッケージの最新バージョンをチェックする（たまにエラーになる場合があるが，少し待ってから再度実行するとうまくいく）．

```bash
$ sudo apt update
```

実行結果

```bash
...
Hit:5 http://ppa.launchpad.net/ondrej/php/ubuntu bionic InRelease
Get:6 http://security.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]
Fetched 88.7 kB in 1s (124 kB/s)
Reading package lists... Done
```

下記コマンドを実行し，PHP8をインストールする．

```bash
$ sudo apt install libapache2-mod-php8.0
```

なにか訊かれたら`y`を入力してEnter

```bash
...
1 upgraded, 7 newly installed, 0 to remove and 24 not upgraded.
Need to get 4528 kB of archives.
After this operation, 20.2 MB of additional disk space will be used.
Do you want to continue? [Y/n]
```

実行結果

```bash
...
Creating config file /etc/php/8.0/apache2/php.ini with new version
libapache2-mod-php8.0: php7.2 module already enabled, not enabling PHP 8.0
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
Processing triggers for libc-bin (2.27-3ubuntu1.4) ...
Processing triggers for php8.0-cli (8.0.0-1+ubuntu18.04.1+deb.sury.org+1) ...
Processing triggers for libapache2-mod-php8.0 (8.0.0-1+ubuntu18.04.1+deb.sury.org+1) ...
```

下記コマンドを実行し，現在のPHP7.2を無効化する．

```bash
$ sudo a2dismod php7.2
```

実行結果

```bash
Module php7.2 disabled.
To activate the new configuration, you need to run:
  systemctl restart apache2
```

下記コマンドを実行し，インストールしたPHP8を有効にする．

```bash
$ sudo a2enmod php8.0
```

実行結果

```bash
Considering dependency mpm_prefork for php8.0:
Considering conflict mpm_event for mpm_prefork:
Considering conflict mpm_worker for mpm_prefork:
Module mpm_prefork already enabled
Considering conflict php5 for php8.0:
Enabling module php8.0.
To activate the new configuration, you need to run:
  systemctl restart apache2
```

下記コマンドを実行し，周辺のモジュールをインストールする．

```bash
$ sudo apt install php8.0-dom php8.0-mbstring php8.0-zip php8.0-mysql
```

なにか訊かれたら`y`を入力して Enter．

```bash
After this operation, 2685 kB of additional disk space will be used.
Do you want to continue? [Y/n]
```

実行結果

```bash
...
Creating config file /etc/php/8.0/mods-available/zip.ini with new version
Processing triggers for libapache2-mod-php8.0 (8.0.0-1+ubuntu18.04.1+deb.sury.org+1) ...
Processing triggers for libc-bin (2.27-3ubuntu1.4) ...
Processing triggers for php8.0-cli (8.0.0-1+ubuntu18.04.1+deb.sury.org+1) ...
```

PHPのバージョンを確認し，`8.0.x`になっていればOK．

```bash
$ php -v
PHP 8.0.0 (cli) (built: Nov 27 2020 12:26:05) ( NTS )
Copyright (c) The PHP Group
Zend Engine v4.0.0-dev, Copyright (c) Zend Technologies
    with Zend OPcache v8.0.0, Copyright (c), by Zend Technologies
```

## composer のインストール

`Compoesr`はPHPの依存性管理ツールである．依存性管理ツールとは，あるライブラリに必要な別のライブラリなどを一括で管理していくれるものである．

Laravelではプロジェクト作成やライブラリのインストールなど，多岐にわたって使用するため，インストールが必要になる．

Cloud9のターミナルで下記コマンドを実行し，必要なファイルをダウンロードする．

```bash
$ curl -sS https://getcomposer.org/installer | php
```

実行結果

```bash
All settings correct for using Composer
Downloading...

Composer (version 2.0.8) successfully installed to: /home/ubuntu/environment/composer.phar
Use it: php composer.phar
```

下記コマンドを実行する．Composerは決まったディレクトリにファイルを置かないと動作しないため，ダウンロードしたファイルを移動している．

```bash
$ sudo mv composer.phar /usr/bin/composer
```

`composer`と入力することでバージョンなどが確認できる．

```bash
$ composer
```

実行結果

```bash
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 2.0.8 2020-12-03 17:20:38
...
```

上記のようなバージョン情報が表示されればOK（細かいバージョンは異なっていて問題なし）．

ここまでで，Laravelでアプリケーションを作成する土台が完成した．

