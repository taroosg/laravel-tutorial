# データ作成処理の実装

必要な画面を揃えたので，作成ページから実際に DB にデータを保存できるようにする．

## Modelの編集

Modelの役割は「DBとのデータをやり取り」である．ModelにはDBからどのようにデータを取得するか，などの処理を記述する．

ただし，Modelには予め使用頻度の高い処理（基本的なCRUD）が用意されているため，今回記述するコードはそれほど多くない．

まず，テーブル対して，Laravel側からデータの作成や編集が行えないカラムを設定する．

これはクライアントから送信されてくるデータがIDやユーザ権限などの情報を書き換えてしまうリスクを低減させるためである．

`laravel_todo/app/Models/Todo.php`を以下のように編集する．

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Todo extends Model
{
  use HasFactory;

  // アプリケーション側でcreateなどできない値を記述
  // ↓以下の処理を記述
  protected $guarded = [
    'id',
    'created_at',
    'updated_at',
  ];
}

```

> 【解説】
>
> - `$guarded`はアプリケーション側から変更できないカラムを指定する（ブラックリスト）．
> - 対して，`$fillable`はアプリケーション側から変更できるカラムを指定する（ホワイトリスト）．
> - どちらを使用しても良いが，どちらかを使用する必要がある．

## Controllerの編集

続いて，コントローラに「Modelに対しての命令」を記述する．DBとやり取りする関数はすでに定義されているのでモデル側に新たな記述は必要ない．

`/laravel_todo/app/Http/Controllers/TodoController.php`の`store()`を内容を以下のように編集する．

```php
public function store(Request $request)
{
  // バリデーション
  $validator = Validator::make($request->all(), [
    'todo' => 'required | max:191',
    'deadline' => 'required',
  ]);
  // バリデーション:エラー
  if ($validator->fails()) {
    return redirect()
      ->route('todo.create')
      ->withInput()
      ->withErrors($validator);
  }
  // create()は最初から用意されている関数
  // 戻り値は挿入されたレコードの情報
  $result = Todo::create($request->all());
  // ルーティング「todo.index」にリクエスト送信（一覧ページに移動）
  return redirect()->route('todo.index');
}

```

> 【解説】
>
> - `create.blade.php`から送信されていたデータは`$request`に入っている．`$request->all()`とすることで全部取得することができる．
> - `redirect()->route('todo.index')`は指定したコントローラの関数を動かす．今回は`TodoController`の`index()`関数．

