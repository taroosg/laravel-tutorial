# 【Docker編】実装したコードをGitHubへpush

いつもどおりGitHubにコードをpushする．


## リポジトリの作成

適当につくっておく．


## ソースコードのpush

>ローカルでの操作．「コンテナにログインしていない状態」で実行すること．

以下のコマンドを実行する．

必ずプロジェクトのフォルダに`cd`してから実行すること！！

```bash
$ git init
$ git remote add origin YOUR_REPOSITORY_URL
$ git add .
$ git commit -m"first commit"
$ git push origin main
```

GitHubにプロダクトのソースコードがpushされていればOK！

