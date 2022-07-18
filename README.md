# 「Python Flask によるWebアプリ開発入門」のローカルでの再現
※1※2

## ローカルでの実行
想定するフォルダ構成
```
~/flaskbook/flaskbook/apps/~
                     /.env.local
                     /requirements.txt
                     ~
```

クローン
```
$ git clone https://github.com/RyuKurihara/flaskbook.git
```

### Mac/Linuxの場合：
本書のコード※3を参考にしてください。

### Windows（Powershell）の場合：
①スクリプト実行の前に実行ポリシーを変更
```
> Set-ExecutionPolicy RemoteSigned CurrentUser
```

②cdで一つ目のflaskbookにディレクトリを移動

③venvで仮想環境を作成
```
PS C:\~\flaskbook> py -m venv venv
PS C:\~\flaskbook> venv\Scripts\Activate.ps1
```

④環境変数ファイル設置
```
(venv) PS C:\~\flaskbook> cp .env.local .env
```

⑤パッケージインストール（今後動かす.pyファイルに必要）
```
(venv) PS C:\~\flaskbook> pip install -r requirements.txt
```

⑥DBのマイグレート（画像やログイン情報の保持に必要）※初回のみ必要
```
(venv) PS C:\~\flaskbook> flask db init
(venv) PS C:\~\flaskbook> flask db migrate
(venv) PS C:\~\flaskbook> flask db upgrade
```


⑦物体検知モデルのモデル作成（Githubに落とすにはファイルが大きすぎるため別途作成）
```
(venv) PS C:\~\flaskbook> cd .\flaskbook\apps\detector\
(venv) PS C:\~\flaskbook> python
(中略)
>>> import torch
>>> import torchvision
>>> model = torchvision.models.detection.maskrcnn_resnet50_fpn(pretrained=True)
>>> torch.save(model, "model.pt")
>>> quit()
(venv) PS C:\~\flaskbook> cd ..\..
```

⑧アプリケーションの起動
```
(venv) PS C:\~\flaskbook\flaskbook> flask run
```

⑨ http://127.0.0.1:5000/ にアクセス

## 実行イメージ

⑩に従います。
![image](https://user-images.githubusercontent.com/60368967/179615953-bcfd2d86-68e8-470e-8e1b-2c839103d35a.png)


するとこのような画面になります。
![image](https://user-images.githubusercontent.com/60368967/179616379-d8e44a03-52e7-43e3-be14-11a0e034e1b8.png)


右上の「新規登録」を押し、任意のユーザー名、メールアドレス、パスワードを打ち込みます(次回ログイン時も使用するのでメモしてください)。
![image](https://user-images.githubusercontent.com/60368967/179616775-20e7b5d6-9026-4af1-ab06-e812a7129b6a.png)


「画像の新規登録」を押します。
![image](https://user-images.githubusercontent.com/60368967/179617242-abea06bb-18b7-4eae-a323-14022b3cd322.png)


「ファイルを選択」から、アップする画像を選びます。
![image](https://user-images.githubusercontent.com/60368967/179618341-fb4e639b-0503-49e6-a918-c3fdc7b7dd91.png)


このように画像がアップされます。「検知」を押すと、画像に対し多クラスの物体検知が行われます。
![image](https://user-images.githubusercontent.com/60368967/179618933-254c0330-8f61-4134-b632-bf0ce5aec12a.png)


物体検知の結果、矩形とカテゴリ名が記載された画像が得られました。
この画像はログインしているユーザーのみ削除可能です。
また、①先にアップした画像②物体検知後の画像の二枚はapps/images/内に格納されます。
![image](https://user-images.githubusercontent.com/60368967/179619259-528a56cb-11af-4138-b1dd-66fe9adceb51.png)


※1. 上記書籍のうち、物体検知Webアプリのローカルでの再現(主に第2, 3章)を目標としています。
※2. 上記書籍のソースコードを参考に、まずは写経から！という気持ちで取り組みました。
※3. https://github.com/ml-flaskbook/flaskbook
