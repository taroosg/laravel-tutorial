# 【AWS Cloud9編】実装したコードをGitHubへpush

まずはAWS Cloud9上で実装したコードをGitHub上にpushする．

## ssh-keyの準備

>AWS Cloud9での操作

プロジェクトのenvironmentを立ち上げておく．

以下のコマンドを順番に実行する．

```bash
$ cd ~/.ssh
$ ssh-keygen
```

実行結果

```bash
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:n7fnnBQCsfgRW+skKRqTGscuroFrnZvo2t3zh4uaQEQ ubuntu@ip-172-31-35-172
The key's randomart image is:
+---[RSA 2048]----+
|  w       w .    |
| .   . . . w .   |
|  . . * w w w    |
| .   = + w *     |
|  . w w w . w .  |
| w . .   . . . . |
|. + w    .w . .  |
| +.B.w... .. +.. |
|*www=.wwww  .w+  |
+----[SHA256]-----+
```

引き続き以下を実行し，ssh-keyを表示する．

```bash
$ cat ~/.ssh/id_rsa.pub
```

実行結果

```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCu8btWWrpYcfx3Wn02FMs1v0DnZw+vNJi4U9Jq4Jq/K1f2uv40fOrfQ26asux9ME6ai+aCyNnmz7oQnWv9gcqMEHtVLzi4x0CJ11oskIqNoAJe0do2wwwwwwwwwwwwwwwwg9WnbB0aqaJtntZOs4I8RM3SD5LPvGtktPhpjBWLK4lt6bV+KDxYuWPN5ciVX7fp7H9MI7jJu/ksvDXsU4OStutPyAucPHt6iBVC3c3IlL124yoevkZGOYZyXHX+jlRcRO8oyCa0L6cRHUzi8djBcuBAX7Uwpk1aS5kplzCsLLifALwePiiy+I8Calsq9ThX+uqo16VXiZkY/+JKJDH/ ubuntu@ip-172-31-35-172
```

## 作成したssh-keyをGitHubに登録

上記で表示したssh-keyを全てコピーする．

>ブラウザでの操作

GitHubのサイトにAWS Cloud9で作成したssh-keyを登録する．

プロジェクト用のリポジトリを新しく作成し，URLをコピーしておく．

## AWS Cloud9で作成したプロダクトをGitHubにpushする．

>AWS Cloud9での操作

【重要】まずプロジェクトのディレクトリに移動する．

以下のコマンドを実行する．

```bash
$ cd ~/environment/プロジェクト名
$ git init
$ git remote add origin YOUR_REPOSITORY_URL
$ git add .
$ git commit -m"first commit"
$ git push origin master
```

GitHubにプロダクトのソースコードがpushされていればOK！