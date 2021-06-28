# 【Docker編】todoアプリケーションの実装


基本的なCRUD処理を実装する．

Laravelでは，テーブル設計，データ操作，画面表示などアプリケーションの流れを一貫して実装することができ，開発を支援するライブラリやコマンドも豊富に用意されている．

todoアプリケーションの実装を通して，これらの機能を体験する．

## 多用するコマンドまとめ

### 仮想コンテナを立ち上げる

```bash
$ ./vendor/bin/sail up -d
```

### 仮想コンテナを終了させる

```bash
$ ./vendor/bin/sail down
```

### Laravelコンテナへログインする

```bash
$ docker-compose exec laravel.test bash
```

### MySQLコンテナへログインする

```bash
$ docker-compose exec mysql bash
```

### コンテナからログアウトする

```bash
$ exit
```

