---
title: AtCoderの環境
tags:
  - AtCoder
  - 環境構築
  - 競プロ
private: false
updated_at: '2025-07-26T12:43:37+09:00'
id: 6fdcc9b7299948e0dc5f
organization_url_name: null
slide: false
ignorePublish: false
---
# 競プロとは
日本語で普通の変換をかけると「今日プロ」となる、厄介なやつです。

与えられた問題に対して、自分がプログラムを書いて、実用的な実行時間で正しい答えを出せるプログラムを作成します。

# 環境構築
[こんな感じ](https://github.com/rotarymars/atcoder-competitive_programming)で出来上がります。

使っているツールは
- oj
- atcoder-cli

という感じです。
## [oj](https://github.com/online-judge-tools/oj)
ojは、AtCoderの問題をダウンロードしたり、提出したりするためのツールです。

これによっていちいちサンプルを自分でコピーする手間もなくなり、また提出先を間違えてしまったりすることを防ぐことができます。

次のコマンドでインストールできます。
```bash
pip install git+https://github.com/online-judge-tools/oj
```

最近(2025/07/25)はAtCoderがReCaptchaを導入したので、簡単にはログインできなくなりましたが、少し調べるとoj用にログインできるようにしてくれるツールも存在します(あまり公にして良いものなのかわからないので紹介しません)。


### もともとのログイン方法
ojで自分のアカウントにログインします。

```bash
oj login https://atcoder.jp
```
普通であればブラウザが立ち上がり、そこでログインします。

一応引数でユーザー名とパスワードを渡すこともできます。

```bash
oj login https://atcoder.jp --username <username> --password <password>
```

これで、ログインができました。

コンテスト中で全体公開されていないものでも、これでサンプルの取得・提出ができるようになります。

## [atcoder-cli](https://github.com/Tatamo/atcoder-cli)
ojのラッパーみたいなやつです。

自分テンプレでファイルを構成でき、それをojで提出することができます。

nodejsのパッケージとしてインストールします。
```bash
npm install -g atcoder-cli
```

テンプレートを自分で書きます。
```bash
cd $(acc config-dir)
```
テンプレートの作成の詳細はatcoder-cliのドキュメントをご覧ください。

これで設定できました。

# 実際に動かす
自分のAtCoderの作業環境に移動してください。
次のコマンドでテンプレを作成します。

```bash
acc new --template kyopuro <contest_name>
```
contest_nameとは、urlの次の部分です。
```
https://atcoder.jp/contests/<contest_name>
```
これで、どの問題を解くか入力します。

矢印で上下移動、スペースで選択・解除、エンターで決定です。

これでディレクトリが作成されます。

それぞれのディレクトリに移動して回答を書きます。

書けたら、サブミットします。
```bash
acc submit [filename]
```
基本的には提出言語も拡張子から自動推測してくれます。

# おわりに
とても雑な記事にはなってしまいましたが、AtCoderを始めてみようというきっかけになればと思います。

ありがとうございました。
