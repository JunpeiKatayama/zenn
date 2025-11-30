---
title: mizchi さんの blog コマンドを Windows + Git Bash で試したメモ
emoji: "🚀"
type: "tech"
topics: ["zenn", "deno", "cli", "windows", "gitbash"]
published: false
---

Windows 環境で [mizchi さんの Deno 製 /blog コマンド](https://gist.github.com/mizchi/b717fe720a5c37742d925fa518fe255b)を試したときのメモです。Git Bash からも動かせるように PATH を通して、記事作成から Git の push までを試しました。

https://x.com/mizchi/status/1945122763653394931?s=20

## 環境

- OS: Windows
- シェル: Git Bash

## まとめ

※このコマンドは自動的に `git add .`するため、非公開記事のあるユーザは注意して利用してください。

1. Deno をインストール  
   PowerShell で実行

   ```powershell
   iwr https://deno.land/install.ps1 -UseBasicParsing | iex
   ```

   Git Bash を使う場合パスを通す

   ```bash
   echo 'export PATH="$HOME/.deno/bin:$PATH"' >> ~/.bashrc
   source ~/.bashrc
   ```

2. Zenn リポジトリを用意  
    GitHub に記事用のリポジトリ(public)を作成したら、以下ページから Zenn と連携。
   https://zenn.dev/dashboard/deploys?tab=repo_settings

   以降、コマンドは Git Bash を使うものとして記載。
   リポジトリをクローンし、記事・本・添付画像用フォルダを作成。

   ```bash
   git clone https://github.com/<you>/zenn C:\Users\<user>\zenn
   mkdir C:\Users\<user>\zenn\{articles,books,images}
   ```

   既定パスは `C:\Users\<user>\zenn`。別パスなら後述 `BLOG_ZENN_REPO` を設定。

3. blog コマンドをインストール

   ```bash
   deno install -A -f -g .\blog.ts
   ```

   環境変数を設定して、どのディレクトリからでも記事の md ファイルを開けるように。

   ```bash
   export BLOG_ZENN_REPO="/c/Users/<user>/zenn"
   ```

4. 記事を書く

   ```bash
   blog <slug> --update   # または -u
   ```

   この状態で記事を書いて、 vscode の md を閉じると push される。

## Git Bash でのセットアップ

1. Deno は PowerShell でインストール
2. Git Bash で PATH を通す:
   ```bash
   export PATH="$HOME/.deno/bin:$PATH"
   deno --version
   ```
   恒久化したい場合は `~/.bashrc` に追記:
   ```bash
   echo 'export PATH="$HOME/.deno/bin:$PATH"' >> ~/.bashrc
   source ~/.bashrc
   ```
3. `blog` コマンドがグローバルに入っていれば、そのまま使える。

## 基本の使い方

### ローカルだけで記事を書く

```bash
blog mizchi-blog-test
```

スラグを指定する場合 12 文字以上必須。`articles/<slug>.md` が自動で作られ、VS Code が `--wait` 付きで開く。

「スラグ」とは zenn において、記事や本のユニークな ID のような文字列を表す。
https://zenn.dev/zenn/articles/what-is-slug

実行するとこうなる。

```bash
$ blog mizchi-blog-test
記事が存在しないため、雛形を作成します: C:\Users\jupei\zenn\articles\mizchi-blog-test.md
雛形を作成しました
VSCodeでファイルを開きます。編集が終わったらVSCodeを閉じてください...
> code --wait 'C:\Users\jupei\zenn\articles\mizchi-blog-test.md'
```

この状態で閉じると push は実行されない。

```bash
VSCodeが閉じられました
```

### 編集後に push まで自動で実行する

`-u` オプションを付ける。

```bash
blog mizchi-blog-test --update   # または -u
```

実行するとこうなる。

```bash
$ blog mizchi-blog-test -u
既存の記事を開きます: C:\Users\jupei\zenn\articles\mizchi-blog-test.md
VSCodeでファイルを開きます。編集が終わったらVSCodeを閉じてください...
> code --wait 'C:\Users\jupei\zenn\articles\mizchi-blog-test.md'
```

変更があれば `git add/commit/push` まで流れるので、push の認証設定だけ事前に済ませておく。

```bash
ファイル mizchi-blog-test.md の変更を検出しました
Git操作を実行します...
> git add 'C:\Users\jupei\zenn\articles\mizchi-blog-test.md'
warning: in the working copy of 'articles/mizchi-blog-test.md', LF will be replaced by CRLF the next time Git touches it
git add: 成功
> git commit -m 'Update mizchi-blog-test'
[main (root-commit) 60ce66e] Update mizchi-blog-test
1 file changed, 91 insertions(+)
create mode 100644 articles/mizchi-blog-test.md
git commit: 成功
> git push origin
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 24 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (4/4), 1.65 KiB | 1.65 MiB/s, done.
Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/JunpeiKatayama/zenn
* [new branch] main -> main
git push: 成功
Git 操作が完了しました
```

## その他の使い方

### list サブコマンド

記事一覧を Git の変更状況付きで確認できる。

```bash
blog list
```

```bash
$ blog list
記事一覧 (Git更新履歴順):
~\articles\mizchi-blog-test.md
```

### スラグなしの blog コマンド

リポジトリ全体の変更を commit -> push。

```bash
blog
```

- diff ない場合

  ```bash
  $ blog
  スラグが指定されていないため、全ての変更をコミットします
  リポジトリの変更をコミットします...
  > git status --porcelain
  コミットする変更はありませんでした
  ```

- diff ある場合

  ```bash
  $ blog
  スラグが指定されていないため、全ての変更をコミットします
  リポジトリの変更をコミットします...
  > git status --porcelain
  > git add .
  warning: in the working copy of 'articles/mizchi-blog-test.md', LF will be replaced by CRLF the next time Git touches it
  git add: 成功
  > git commit -m 'Update zenn repository'
  [main 83bf477] Update zenn repository
  1 file changed, 14 insertions(+)
  git commit: 成功
  > git push origin
  Enumerating objects: 7, done.
  Counting objects: 100% (7/7), done.
  Delta compression using up to 24 threads
  Compressing objects: 100% (2/2), done.
  Writing objects: 100% (4/4), 554 bytes | 554.00 KiB/s, done.
  Total 4 (delta 1), reused 0 (delta 0), pack-reused 0
  remote: Resolving deltas: 100% (1/1), completed with 1 local object.
  To https://github.com/JunpeiKatayama/zenn
  1861757..83bf477  main -> main
  git push: 成功
  Git操作が完了しました
  ```

## 使ってみた所感

- 作業メモを残すようにして後から記事の形に整えたりが結構気軽にできて便利無きがする。
- 発信することがあまりないことには、今は気付かないふりをする・・・。
