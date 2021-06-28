# ライブラリ準備

### ライブラリ準備

作成するアプリケーションでライブラリを使用する場合はコマンドでインストールを行う．今回は認証を実装するための`Laravel Breeze`をインストールする．

下記コマンドを実行し，必要なファイルをダウンロードする．

**（必ず`laravel_todo`ディレクトリで実行すること）**

```bash
$ composer require laravel/breeze --dev
```

実行結果

```bash
...
Discovered Package: nesbot/carbon
Discovered Package: nunomaduro/collision
Package manifest generated successfully.
73 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
```

下記コマンドでインストールする．

```bash
$ php artisan breeze:install
```

実行結果

```bash
Breeze scaffolding installed successfully.
Please execute the "npm install && npm run dev" command to build your assets.
```

下記コマンドを実行し，その他必要なパッケージをインストールしてビルドする（ここでNode.jsが動く）．

```bash
$ npm install && npm run dev
```

実行結果（やや時間がかかる）

```bash
 DONE  Compiled successfully in 35024ms                                       3:03:55 AM

       Asset      Size   Chunks             Chunk Names
/css/app.css  3.75 MiB  /js/app  [emitted]  /js/app
  /js/app.js   669 KiB  /js/app  [emitted]  /js/app
```

### https と http 混在状態で動作するように設定

AWS cloud9で動かす場合，`https`と `http`が混在する問題があり，下記設定を行わないとトップページ以外動作しない．

`laravel_todo/app/Http/Middleware/TrustProxies.php`を以下のように編集する．

```php
<?php

namespace App\Http\Middleware;

use Fideloper\Proxy\TrustProxies as Middleware;
use Illuminate\Http\Request;

class TrustProxies extends Middleware
{
  /**
   * The trusted proxies for this application.
   *
   * @var array|string|null
   */
  // ↓この1行を編集
  protected $proxies = '*';

  /**
   * The headers that should be used to detect proxies.
   *
   * @var int
   */
  protected $headers = Request::HEADER_X_FORWARDED_ALL;
}
```

記述したらサーバを立ち上げ，動作を確認する．

```bash
$ php artisan serve --port=8080
```

プレビューして以下の画面が表示されればOK（右上に`Login`と`register`が表示される）．

![breezeインストール確認](../img/20210104-breeze-installed.png)

右上の`register`をクリックするとユーザ登録画面に移動するが，まだこの段階ではユーザ登録機能は動作しない．ユーザ登録画面が表示されればOK．

![register画面](../img/20210104-register.png)

もし register画面が表示されない場合は下記の手順を実行する．

1. 一旦サーバを停止する．
2. EC2 設定画面からインスタンスを再起動．
3. 再度サーバ立ち上げて動作確認．

ここまででライブラリの準備は完了．

