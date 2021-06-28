# ⚠️ 仮想マシンの容量追加（EC2操作）

⚠️ 本項は2021/05/29時点で不要なので飛ばして問題ない

environment を作成したことで，AWS 上に仮想マシンが割り当てられた状態となる．しかし，初期状態では容量不足になってしまう．そのため，設定を変更して容量を増やす．

1. AWS の画面の左上「Services」->「EC2」にアクセスする．

2. 画面左側のメニューから「Instances」をクリックする．

3. インスタンス（仮想マシン）の一覧が表示されるので，該当するもの（Running になっているもの）にチェックを入れる．

4. 画面下に表示されるタブの「Storage」をクリックし，表示された「Volume ID」のリンクをクリックする．

5. 画面上部の「Actions」をクリックし，「Modify Volume」を選択．「Size」を「10」から「20」に変更し，「Modify」をクリック．なにか訊かれたら「Yes」．

6. 画面左側のメニューから「Instances」をクリック．該当するインスタンスにチェックを入れて「Instance State」->「Reboot Instance」．なにか訊かれたら「Reboot」で進む．

ここまでで仮想マシンの設定は終了．

設定確認のため，Cloud9 画面のターミナルで下記をコマンド実行する．

```bash
$ df -h
```

出力結果

```bash
Filesystem      Size  Used Avail Use% Mounted on
udev            476M     0  476M   0% /dev
tmpfs            98M  788K   98M   1% /run
/dev/xvda1       20G  8.7G   11G  45% /
tmpfs           490M     0  490M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           490M     0  490M   0% /sys/fs/cgroup
/dev/loop0       13M   13M     0 100% /snap/amazon-ssm-agent/495
/dev/loop1       88M   88M     0 100% /snap/core/5328
/dev/loop2       33M   33M     0 100% /snap/amazon-ssm-agent/2996
/dev/loop3       56M   56M     0 100% /snap/core18/1944
/dev/loop4       98M   98M     0 100% /snap/core/10577
tmpfs            98M     0   98M   0% /run/user/1000
```

`/dev/xvda1`の Size が 20G になっていれば OK．

