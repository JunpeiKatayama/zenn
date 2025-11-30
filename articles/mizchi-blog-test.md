---
title: mizchi さんの blog コマンドを Windows + Git Bash で試したメモ
emoji: "🚀"
type: "tech"
topics: ["zenn", "deno", "cli", "windows", "gitbash"]
published: false
---

Windows 環境で mizchi さんの Deno 製 `blog` コマンドを試したときのメモです。Git Bash からも動かせるように PATH を通して、記事作成から Git の push までを CLI 一本で回しました。

## 環境

- OS: Windows
- シェル: Git Bash（必要に応じて PowerShell も併用）
- エディタ: VS Code（`code` が PATH に通っている前提）
- リポジトリ: `C:\Users\jupei\zenn`（必要なら `BLOG_ZENN_REPO` で変更可）

## Git Bash でのセットアップ

1. Deno は PowerShell でインストール済み（`C:\Users\jupei\.deno\bin\deno.exe`）。
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

## 使い方メモ

- 記事を開く（無ければ雛形を作成）:

  ```bash
  blog mizchi-blog-test
  ```

  スラグは 12 文字以上必須。`articles/<slug>.md` が自動で作られ、VS Code が `--wait` 付きで開く。

  こうなる。

  ```
  $ blog mizchi-blog-test
  記事が存在しないため、雛形を作成します: C:\Users\jupei\zenn\articles\mizchi-blog-test.md
  雛形を作成しました
  VSCodeでファイルを開きます。編集が終わったらVSCodeを閉じてください...
  > code --wait 'C:\Users\jupei\zenn\articles\mizchi-blog-test.md'
  ```

  この状態で閉じると push は実行されない。

  ```
  VSCodeが閉じられました
  ```

- 編集後に Git まで自動で実行:

  ```bash
  blog mizchi-blog-test --update   # または -u
  ```

  こうなる。

  ```
  $ blog mizchi-blog-test -u
  既存の記事を開きます: C:\Users\jupei\zenn\articles\mizchi-blog-test.md
  VSCodeでファイルを開きます。編集が終わったらVSCodeを閉じてください...
  > code --wait 'C:\Users\jupei\zenn\articles\mizchi-blog-test.md'
  ```

  変更があれば `git add/commit/push` まで流れるので、push の認証設定だけ事前に済ませておく。

  ```
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

````

- 記事一覧を Git の変更状況付きで確認:

```bash
blog list
````

- スラグ省略でリポジトリ全体の変更を commit/push:
  ```bash
  blog
  ```

## 使ってみた所感

- 雛形生成と VS Code 起動が一発で済むので、記事を書くまでの儀式が減った。
- Git Bash でも `PATH` を通せばそのまま動作。環境変数 `BLOG_ZENN_REPO` を使えば別パスのリポジトリにも対応できる。
- `--update` で Git 操作まで自動化できるので、書いたらすぐ push まで持っていける。
