---
title: Googleの新エディタ『Antigravity』試してみたメモ
emoji: "🌍️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["antigravity", "google", "ai", "ide", "vscode", "エージェント開発"]
published: false
---

## はじめに

Antigravity、話題なので試してみました。
ADHD 向けであるということも書いてあったりしたのでとても気になりました。
私も会議２分前を告げるスマホのアラームを止めた瞬間にそのことを忘れて作業して、10 分後に会議のことを思い出すというレベルのｱｰﾃﾞｰﾊｰﾃﾞｰのため！

## Antigravity とは

https://antigravity.google/

Google が発表した AI ビルトインの IDE で、agentic development platform を謳っています。
つまり、エージェント駆動開発用の統合開発環境とのことっぽいですね。
エージェント駆動とのことで、人間は指示とレビューだけする感じなのかなと察せられます。

VSCode を fork して作られているようで、設定を引き継ぐこともできます。
（すべてそのまま使えるというわけではないと思うが）

## 環境

- OS: Windows11
- ターミナル: GitBash

筆者の環境では WSL を使っていないため、多くの人の Windows の開発環境とは異なるかもしれない。
(WSL 有効だと使えないアプリがあったりして、その流れで入れてない)

## 使ってみた

簡単なアプリを作る指示を出してみました。

```
poo.ps という、うんち関連のチャットを投稿する機能のみをもった WEB アプリを作りたいです。
投稿は４５分で消えるようにしたい。
```

![最初の指示](/images/スクリーンショット%202025-12-06%20152801.png)

以下のように解釈してくれたようです。

```
Developing the core concept

I'm solidifying the idea for poo.ps, a disappearing-message chat app. The core mechanic, messages vanishing after 45 minutes, is locked in. Now I need to build out the technical details, focusing on data storage and user interaction. This is still early conceptualization.

Mapping Out Implementation Steps

I'm now zeroing in on the practical steps. It's a real-time chat with message TTL, so I need a backend for storage and expiry. Given my environment, client-side persistence or a lightweight Next.js app with in-memory storage seems best. There's a workspace c:\Users\jupei\proj\poops; I should see if there's anything useful there. Considering the requested tech, I'm leaning toward a React/Next.js client with SQLite or local storage, aiming for a robust yet feasible demo.

Proposing a Technical Roadmap

現在、技術的なロードマップを検討しています。 Next.jsをフレームワークとして採用し、CSS Modulesを使用する案です。永続的な45分のTTLを実現するために、最初はインメモリデータストアを使用し、後でSQLiteに移行することを検討しています。プロジェクトの初期化と実装計画の作成から始めます。

Outlining Implementation Strategy

I'm zeroing in on the practical steps for poo.ps. First, I'll list the directory contents to understand my starting point. If it's empty, initializing a Next.js app is the logical first step, perfect for managing the frontend and backend logic. Since the prompt specifies "Vanilla CSS", I'll use standard CSS. Design-wise, I'm aiming for a premium, dynamic feel, perhaps with a brown/gold palette that is humorously poop-related.
```

タスクリストを作って、それを順に実行していくタイプのようです。
かなり kiro に近いですね。
![task駆動](/images/スクリーンショット%202025-12-06%20153943.png)

タスクが終わると、Walkthrough というドキュメントを作成してくれます。
タスクの説明と、テスト用の観点などが記載されていて、これを参考に動作確認を行ってね、とのことみたいです。
![walkthrough](/images/スクリーンショット%202025-12-06%20154317.png)

### ここからがすごい

さて、ここからが Antigravity の本領発揮です！
というのも、Antigravity には Chrome との強力な連携機能があり、ブラウザを直接操作して動作確認をしてくれるらしいのです。

上部のメニューから、Chrome のアイコンを押します。
![クロームアイコン](/images/スクリーンショット%202025-12-06%20161824.png)

するとブラウザが開き、Chrome 拡張のインストールを促す表記が出ます。
![拡張](/images/スクリーンショット%202025-12-06%20162121.png)

インストールすると以下のように、なにができるようになったか教えてくれる画面が出ます。
![インストール後](/images/スクリーンショット%202025-12-06%20154611.png)

Antigravity にテストを依頼すると、ブラウザの周辺が青くなり、Antigravity がテストを開始します。
![テストを指示](/images/スクリーンショット%202025-12-06%20155910.png)

テストが終わると、また Walkthrough が出力されます。
![walkthrough-2](/images/スクリーンショット%202025-12-06%20155956.png)
Walkthrough にはテスト時の画像と動画が添付されています。
![gif](/images/testing.gif)

また、Walkthrough にはレビューを追加することができ、それをもとに修正を行ってくれるようです。
![review](/images/スクリーンショット%202025-12-06%20163454.png)
文章や画像にマウスオーバーすると Review ボタンが出てきて、そこからレビューコメントを入れられます。

このブラウザを直接操作してくれるという部分が Antigravity を選ぶキモになってくるかな、と思います。

## 困ったこと

とくにこれといった問題は発生しませんでしたが、途中 `npm install`で延々と待機中となってしまうことがありました。
windows + gitbash という環境なので、そのせいだと思います。
Antigracvity のエージェントは停止ボタンを押さない限り Task を進め続けるみたいで、自分でターミナルから `npm install` してあげたうえで、エージェントの実行しているコマンドをキャンセルしてあげることで、自分で状況確認したうえでタスク消化を再開してくれました。

エージェントにやってもらっているときにコマンドの実行に失敗することはあるあるなので、Antigravity が悪いということではないと思います。

## 最後に

最終的に成果物は以下のようになりました。
![成果物](/images/スクリーンショット%202025-12-06%20160035.png)

生成されたコードは以下リポジトリにあります。
https://github.com/JunpeiKatayama/poo-ps

ここで取り上げた以外にも、たくさんの機能があるようです。
個人的には Agent Manager という、複数エージェントを使う場合に、それぞれがどういったタスクを行っているかを確認できる画面があって、便利だなと感じました。(Agent が今なにで困ってるのとかわからない問題がけっこうあったので)

モデルに関しても gemini だけでなく sonnet や opus も選択可能となっているので、WEB アプリを作りたい場合に今後強力な選択肢に入ってくるのではないかな、と思いました。

また、今回作ったものは以下ポストの影響を受けたものであり、決して私が下品な思想にまみれているわけではないので、よろしくお願いいたします！
https://x.com/zmukkuri/status/1995143439558856725?s=20
https://oshic.co/
