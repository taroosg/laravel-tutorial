# ローカルからロリポップマネージドクラウドへのログイン

GitHub上のソースコードをサーバ上にデプロイするため，ローカルPCのターミナルでマネージドクラウドの仮想マシンを操作する．

まずはローカルPCとマネージドクラウドの間で通信ができるようにするためのssh-keyを準備する．

（本手順は初めてマネージドクラウドと通信する場合のみ必要であり，プロジェクトが2つ目以降の場合は操作不要．）

>ブラウザでの操作

Lolipopのページの右上から「SSH公開鍵の追加」をクリックする．

自分のPCに保存されているssh-keyを適当に登録する．

>**ない場合は，ローカルPCのターミナルで**以下の流れで作成&表示し，マネクラに登録する．
>
>```bash
>$ cd ~/.ssh
>$ ssh-keygen
>$ cat ~/.ssh/id_rsa.pub
>```

ブラウザでプロジェクトのページを開いていない場合は再度開いておく．

>PCのターミナルでの操作

マネクラのプロジェクトページに表示されている「SSHコマンド」を入力してサーバにアクセスする．

**下記は一例なので必ず「自分のプロジェクト管理画面に表示されているSSHコマンド」を実行すること．**

```bash
$ ssh -p 38216 reliable-amami-8204@ssh-1.mc.lolipop.jp
```

実行結果．（何か訊かれたら`yes`で進む）

```bash
The authenticity of host '[ssh-1.mc.lolipop.jp]:38216 ([157.7.190.236]:38216)' can't be established.
ECDSA key fingerprint is SHA256:fqQ1YxW9OFrAwKVtnt92YIB2Bv6MQfJVGRo73gktLmk.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[ssh-1.mc.lolipop.jp]:38216,[157.7.190.236]:38216' (ECDSA) to the list of known hosts.
Last login: Thu Jun 20 02:06:09 2019 from 10.1.36.1
  __  __  ____   _          _ _
 |  \/  |/ ___| | |    ___ | (_)_ __   ___  _ __
 | |\/| | |     | |   / _ \| | | '_ \ / _ \| '_ \
 | |  | | |___ _| |__| (_) | | | |_) | (_) | |_) |
 |_|  |_|\____(_)_____\___/|_|_| .__/ \___/| .__/
export PATH=$PATH:/var/www/bin
                               |_|         |_|

******* Welcome to Lolipop! Managed Cloud *******

reliable-amami-8204@ssh-aws-laravel01:~$
```

この状態で`reliable-hoge-6666@ssh-aws-laravel01:~$ `（← 状況によって文字列は異なる）となっていればうまくログインできている状態．

ある程度時間が経過すると自動的にログアウトされるので，その場合は再度SSHコマンドを実行するとログインできる．

### 💡 Key Point

>**※以下の「Lolipopでの操作」は上記の状態（ターミナルが`reliable-hoge-6666@ssh-aws-laravel01:~$ `）になっていることを確認すること．**
