# データ削除処理の実装

一覧画面に削除ボタンを設置し，クリックしたら該当するデータを削除できるようにする．

## 一覧画面に削除ボタンを追加

`index.blade.php`を以下のように編集する．

```php
<x-app-layout>
  <x-slot name="header">
    <h2 class="font-semibold text-xl text-gray-800 leading-tight">
      {{ __('Todo Index') }}
    </h2>
  </x-slot>

  <div class="py-12">
    <div class="max-w-7xl mx-auto sm:w-10/12 md:w-8/10 lg:w-8/12">
      <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
        <div class="p-6 bg-white border-b border-gray-200">
          <table class="text-center w-full border-collapse">
            <thead>
              <tr>
                <th class="py-4 px-6 bg-grey-lightest font-bold uppercase text-lg text-grey-dark border-b border-grey-light">todo</th>
                <th class="py-4 px-6 bg-grey-lightest font-bold uppercase text-lg text-grey-dark border-b border-grey-light">deadline</th>
                <th class="py-4 px-6 bg-grey-lightest font-bold uppercase text-lg text-grey-dark border-b border-grey-light">actions</th>
              </tr>
            </thead>
            <tbody>
              @foreach ($todos as $todo)
              <tr class="hover:bg-grey-lighter">
                <td class="py-4 px-6 border-b border-grey-light">
                  <a href="{{ route('todo.show',$todo->id) }}">{{$todo->todo}}</a>
                </td>
                <td class="py-4 px-6 border-b border-grey-light">{{$todo->deadline}}</td>
                <td class="py-4 px-6 border-b border-grey-light flex justify-center">
                  <!-- 更新ボタン -->
                  <!-- 削除ボタン -->
                  <form action="{{ route('todo.destroy',$todo->id) }}" method="POST">
                    @method('delete')
                    @csrf
                    <button type="submit" class="mr-2 ml-2 text-sm bg-black hover:bg-gray-900 hover:shadow-none text-white py-1 px-2 focus:outline-none focus:shadow-outline">
                      <svg class="h-6 w-6 text-gray-500" fill="none" viewBox="0 0 24 24" stroke="white">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                      </svg>
                    </button>
                  </form>
                </td>
              </tr>
              @endforeach
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</x-app-layout>
```

> 【解説】
>
> - 削除ボタンクリック時には，コントローラの`destroy()`関数にリクエストが送られる．
> - 削除の処理を行うには`DELETE`メソッドでリクエストを送る必要があるが，formからはGETまたはPOSTでしか送れない．
> - `@method('delete')`を記述することで，`DELETE`メソッドで送信できる．

一覧画面を確認し，削除ボタンが表示されていればOK．

![削除ボタン追加](../img/20210104-index-delete-button.png)

## 指定したデータを削除する処理の作成

まずIDで指定したデータを1件抽出し，そのデータを削除する，という流れ．

`/laravel_todo/app/Http/Controllers/TodoController.php`の`destroy()`を内容を以下のように編集する．

```php
public function destroy($id)
{
  $result = Todo::find($id)->delete();
  return redirect()->route('todo.index');
}
```

動作させて確認する．削除ボタンをクリックし，該当するデータが削除されればOK．

