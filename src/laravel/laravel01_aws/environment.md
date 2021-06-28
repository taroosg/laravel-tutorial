# Environment（仮想マシン）準備


## environmentの準備

- environmentとはアプリケーションを動作させる環境．AWS上に仮想マシンが立ち上がる．
- 1つのwebアプリケーションに対して，1つのenvironmentいう理解でOK．

まず，AWSのマネジメントコンソールから「cloud9」を探してアクセスする．

![awsコンソール画面](../img/20190611-aws_console.png)

下記の画面になるので「create environment」をクリック．

![cloud9初期画面](../img/20190611-create_environment.png)

名前を適当に入力．（Descriptionは任意）

![名前入力画面](../img/20190611-input_name.png)

【重要】「Platform」は「`ubuntu server 18.04 LTS`」を選択．「Instance type」は「`t2.micro`」を選択．その他はデフォルトで「create environment」をクリック．

![セットアップ画面01](../img/20190611-setup_01.png)

確認画面が出るので「next step」をクリック．

![セットアップ画面02](../img/20190611-setup_02.png)

しばらく待つと下記の画面が表示される．これで準備完了．

![設定完了後の画面](../img/20190611-cloud9_aws.png)
