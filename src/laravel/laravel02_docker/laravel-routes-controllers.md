# ルーティングとコントローラ

Laravelでは，クライアントからリクエストされたURLに対して，どの関数を実行するかを決める．この対応を決定するのが「ルーティング」であり，実際に実行される関数を記述するのが「コントローラ」である．

## ルーティングとコントローラの作成

>📦 **Laravelコンテナ内の操作**
>
>```bash
>$ docker-compose exec laravel.test bash
>root@8544d96d2334:/var/www/html#
>```

コントローラとルーティングを作成する．今回のコントローラ名は`TodoController`とする．

`--resource`をつけることで，よく使用する処理（代表的なCRUD処理）を一括して作成することができる．

下記コマンドを実行する．

```bash
$ php artisan make:controller TodoController --resource
```

実行結果

```bash
Controller created successfully.
```

これでコントローラファイルが作成された．続いてルーティングを作成する．

**ルーティングファイルは`laravel_todo/routes`以下に配置される．**

`/laravel_todo/routes/web.php`を以下のように編集する．

```php
<?php

use Illuminate\Support\Facades\Route;
// 下記1行を追記
use App\Http\Controllers\TodoController;

Route::get('/', function () {
  return view('welcome');
});

// 下記1行を追記
Route::resource('todo', TodoController::class);

Route::get('/dashboard', function () {
  return view('dashboard');
})->middleware(['auth'])->name('dashboard');

require __DIR__ . '/auth.php';

```

ルーティングの一覧は下記で確認できる．

```bash
$ php artisan route:list
```

実行結果（ユーザ操作などのルーティングは省略）

> 【解説】
>
> **この表は非常に重要．**
>
> - 例えば，`/todo/`に`GET`でリクエストが来た場合，`TodoController`の`index`関数が動作することを示している．
> - このように，URLと動作する関数の対応を決めているのがルーティングである．

```bash
+--------+-----------+---------------------------------+---------------------+------------------------------------------------+--------------+
| Domain | Method    | URI                             | Name                | Action                                         | Middleware   |
+--------+-----------+---------------------------------+---------------------+------------------------------------------------+--------------+
|        | GET|HEAD  | todo                            | todo.index          | App\Http\Controllers\TodoController@index      | web          |
|        | POST      | todo                            | todo.store          | App\Http\Controllers\TodoController@store      | web          |
|        | GET|HEAD  | todo/create                     | todo.create         | App\Http\Controllers\TodoController@create     | web          |
|        | GET|HEAD  | todo/{todo}                     | todo.show           | App\Http\Controllers\TodoController@show       | web          |
|        | PUT|PATCH | todo/{todo}                     | todo.update         | App\Http\Controllers\TodoController@update     | web          |
|        | DELETE    | todo/{todo}                     | todo.destroy        | App\Http\Controllers\TodoController@destroy    | web          |
|        | GET|HEAD  | todo/{todo}/edit                | todo.edit           | App\Http\Controllers\TodoController@edit       | web          |
+--------+-----------+---------------------------------+---------------------+------------------------------------------------+--------------+
```

これで，ルーティング部分は完了である．

## コントローラの実装（一部）

URLにリクエストが来た場合に実行される関数はコントローラに記述される．まずはリクエストが来た場合に特定の画面を表示するよう処理を記述する．

**コントローラは`laravel_todo/app/Http/Controllers`以下に配置される．**

`/laravel_todo/app/Http/Controllers/TodoController.php`の内容を以下のように編集する．

`view()`関数は指定したビューファイル（画面）を表示する．`todo.index`は`todo`フォルダの`index.blade.php`の意味．ビューファイルは次項で作成する．

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

// 下記2行を追加
use Validator;
use App\Models\Todo;

class TodoController extends Controller
{
  /**
   * Display a listing of the resource.
   *
   * @return \Illuminate\Http\Response
   */
  public function index()
  {
    // 追加
    return view('todo.index');
  }

  /**
   * Show the form for creating a new resource.
   *
   * @return \Illuminate\Http\Response
   */
  public function create()
  {
    // 追記
    return view('todo.create');
  }

  // 以降は変更なし
}

```

