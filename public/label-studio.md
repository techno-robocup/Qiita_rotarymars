---
title: Label Studioを使ってYOLOのためのアノテーションをする。
tags:
  - 'LabelStudio'
private: false
updated_at: ''
id: null
organization_url_name: techno_robocup
slide: false
ignorePublish: false
---

# ことのはじまり

フィールドにある銀色・黒色のボールを認識したいと思った時に、「どうにかして楽にできないか」と考えたことが、Label StudioとYOLOへの出会いの始まりでした。

最初はCV2などの輪郭線抽出などを駆使して検出することも考えましたが、「物体検知と呼ばれるものをすれば楽では？」と思って、使ってみることにしました。

# 今回使うもの

## YOLO(You Only Look Once)

ワシントン大学のJoseph RedmonとAli Farhadiによって開発された物体検知のライブラリです。

Pythonライブラリとして提供されており、比較的少ない画像枚数であっても精度が高い物体検知をすることができるのに特化しています。

## Label Studio

Label Studioは、画像のアノテーションをするためのWebアプリケーションで、無料で使えるようになっています。

様々なライブラリ向けにアノテーションのデータをエクスポートできるようになっており、今回YOLOで学習させるためのデータのアノテーションもします。

# やり方

とりあえず画像のラベリングをするためのプロジェクトを作成しましょう。

```bash
mkdir yolo-demo
cd $_
```

まず、Label Studioを起動する場所を作ります。

```bash
mkdir label-studio
cd $_
```

Label Studioは様々な形態で提供されていますが、今回はDockerで動かすことにします。

次のような`docker-compose.yml`を作成します。

```yml:docker-compose.yml
version: "3.9"

services:
  label_studio:
    container_name: label_studio
    image: heartexlabs/label-studio:latest
    volumes:
      - label-studio-data:/label-studio/data
    ports:
      - 8080:8080
    environment:
      - LOCAL_FILES_SERVING_ENABLED=true

volumes:
  label-studio-data:

networks:
  default:
    name: label_link
```

書き込んだら、次のコマンドで起動します。

```bash
docker compose up --build -d
```

なお、落とすときは

```bash
docker compose down
```

で終了できます。

また、二回目以降の起動は

```bash
docker compose up -d
```

で十分です。

さて、しばらく待つと、Label Studioが`localhost:8080`に立ち上がります。

## ログイン

Label Studioにアクセスすると、ログイン画面が出てきます。

![Label Studio Login screen](https://techno-robocup.github.io/assets/images/20251112_login.png)

ローカルですが、アカウントが必要です。

さて、アカウントが作成されると、以下のような画面が出ます。

![Label Studio Home screen](https://techno-robocup.github.io/assets/images/20251112_home.png)

## プロジェクトの作成

`Create project`を選択して、新しいプロジェクトを開始します。

![Label Studio Create project](https://techno-robocup.github.io/assets/images/20251112_project_start.png)

プロジェクトやDescriptionを設定したのち、`Labeling Setup`を選択します。

今回は、YOLOで物体検知をするため、`Object Detection with Bounding Boxes`を選択します。

![Label Studio Labeling Setup](https://techno-robocup.github.io/assets/images/20251112_labeling.png)

デフォルトで`Airplane`と`Car`のラベルがあるので、削除しましょう。

そのあと、自分が使用したいラベルを追加します。

今回は、`silver_ball`と`black_ball`という名前で作成します。

そして右上にある`Save`を押してプロジェクト構成を保存します。

## プロジェクトの学習に使用する画像をインポートする

現在プロジェクトを開いているので、そこから画像をインポートします。

実は画像をアップロードするという手法もありますが、今回はローカルのファイルを参照するという方法で画像をアップロードしたいと思います。

まずは、今立ち上げているLabel Studioを終了します。

```bash
docker compose down
```

さて、一つ上のディレクトリに戻って、画像を入れるフォルダを作成します。

```bash
cd .. # cwd: yolo-demo
mkdir images
```

そして、ラベリングをしたい画像をそのフォルダに入れます。

さて、Label StudioのDockerコンテナからもアクセスできるように設定を書き足します。

```diff_yaml:docker-compose.yml
version: "3.9"

services:
  label_studio:
    container_name: label_studio
    image: heartexlabs/label-studio:latest
    volumes:
      - label-studio-data:/label-studio/data
+     - ../images/:/tmp/test_images
    ports:
      - 8080:8080
    environment:
      - LOCAL_FILES_SERVING_ENABLED=true

volumes:
  label-studio-data:

networks:
  default:
    name: label_link

```
