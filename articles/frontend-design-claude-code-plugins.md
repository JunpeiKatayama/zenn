---
title: frontend-design@claude-code-plugin 試してみたメモ
emoji: "🎨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["claudecode", "claude", "ai", "frontend", "design"]
published: true
---

## はじめに

私は職務経歴書を github-pages で公開しています。なんとなくかっこいい気がして。
しかし、単に md を html にパースしているだけなので非常にしょぼい見た目をしています・・・。

![しょぼい職務経歴書](/images/スクリーンショット%202025-12-02%20231615.png)

そこで、以下 X でちょっとバズっていた拡張を使って、私のあまりにもしょうもなさすぎる職務履歴サイトを改善してみようと思います。
https://x.com/9m/status/1995132941824131300?s=20

## claude-code-plugin とは

そもそも、Claude Code に Skills という概念ができたことは知っていましたが、利用したことはありませんでした。
「プラグイン」という概念は、skills やそれに必要なコマンド、MCP などをまとめたパッケージとして扱う、というもののようです。
なのでまずはこのプラグインを利用可能にしたいと思います。

### SKILL を特定ディレクトリに配置するだけで使える

今回の場合 `SKILL.md` のみでよいので、使いたいプロジェクトの `./claude/skills` 配下に配置します。

```bash
curl -o .claude/skills/frontend-design/SKILL.md https://raw.githubusercontent.com/anthropics/claude-code/main/plugins/frontend-design/skills/frontend-design/SKILL.md
```

これだけでセットアップは完了だそうです。
この状態で、フロントエンドのデザインをお願いするような指示をすれば、このスキルを参照して作業してくれるとのことです。

### claude コマンドからインストールする方法もある

こっちが正式な導入方法っぽいです。
https://code.claude.com/docs/ja/plugin-marketplaces

1. claude code をターミナルで起動し `/plugin` コマンドを実行
   VS Code の拡張機能のネイティブ UI からだとできなかったです。(ターミナルで開きますか？と聞いてくれる)
   ![plugin](/images/スクリーンショット%202025-12-02%20225920.png)

2. Add MarketPlace でマーケットプレイスを登録
   例にも記載されていますが、今回は GitHub リポジトリの名前を記載。
   `anthropics/claude-code` を入力。
   ![AddMarketPlace](/images/スクリーンショット%202025-12-02%20225929.png)

3. Browse and install plugins でプラグインをインストール
   ![browse to install](/images/スクリーンショット%202025-12-02%20230004.png)
   今回の場合 `frontend-design` を選択。
   ![select plugin](/images/スクリーンショット%202025-12-02%20230017.png)
   Install now を選択。
   ![install now](/images/スクリーンショット%202025-12-02%20230024.png)
   インストールされていることが確認できた。
   ![確認](/images/スクリーンショット%202025-12-02%20230044.png)
   この場合、`~/.claude/plugins/marketplaces/claude-code-plugins/plugins/frontend-design/skills/frontend-design/` にインストールされていました。
   なので、グローバルで有効になっているはず。
   特定のリポジトリのみで機能させたいのであれば、作業ディレクトリの `.claude` 配下にあるとよさそう。

## 実際にやってみた

成果物は以下です！
![新職務経歴書](/images/スクリーンショット%202025-12-02%20231607.png)
https://junpeikatayama.github.io/about-me/index.html

おお・・・なんか良さそうな気がします。
多分トンマナは以前のバージョンを参考にしてくれたのかな、といった感じ。

スクロールするとアニメーションとともに経歴が流れてくるようになっています。
また、左側にメニューがあって好きなセクションに飛べるのもいい感じですね。

スマホ表示でもまあまあ見やすいのではないでしょうか。
![スマホ版](/images/スクリーンショット%202025-12-02%20232252.png)

ただ SKILL.md を落としてくるだけでこれなのでかなりいいのではないでしょうか。

## 感想

眉唾だと思っていたけどけっこうよさそうでした。
とはいえ職務経歴って簡素なもので充分だと思っているので、新しいものをトップページに据えつつも、旧版も「シンプル版」として残すことにしました。
2 重管理になってめんどくさくていつか消す姿が見える。
