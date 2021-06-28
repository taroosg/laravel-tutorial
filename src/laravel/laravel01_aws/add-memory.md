# ⚠️ swapメモリ作成

⚠️ 本項は2021/05/29時点で不要なので飛ばして問題ない

無料で作成できるインスタンスは実行メモリが 512MB しかなく，途中でメモリ不足になる．

そのため，ストレージから 1GB の容量を借りてきてメモリに割り当てる（これを swap 領域と呼ぶ）．

Cloud9 のターミナルで下記コマンドを実行する．swap 領域作成のためのファイルを作成している．

```bash
$ sudo dd if=/dev/zero of=/swapfile bs=1M count=1024
```

実行結果

```bash
1024+0 records in
1024+0 records out
1073741824 bytes (1.1 GB, 1.0 GiB) copied, 13.9046 s, 77.2 MB/s
```

続いて，下記コマンド実行する．作成した swap ファイルの権限を変更し，swap 領域を準備する．

```bash
$ sudo chmod 600 /swapfile
$ sudo mkswap /swapfile
```

実行結果

```bash
Setting up swapspace version 1, size = 1024 MiB (1073737728 bytes)
no label, UUID=a0e6a190-14b0-4766-95d5-89bd37782a24
```

下記コマンドを実行する．実際に swap 領域を作成し，メモリの状況を確認する．

```bash
$ sudo swapon /swapfile
$ free --mega
```

実行結果

```bash
              total        used        free      shared  buff/cache   available
Mem:           1002         475          75           2         451         376
Swap:          1548          13        1535
```

`Swap:`の`total`が 1500 程度になっていれば OK．
