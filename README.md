<!--
  このREADMEは「try」プロジェクトの概要を説明します。新しい実験やアイデアごとに新しいディレクトリを簡単に作成・管理できるツールです。セットアップ手順、使い方の例、プロジェクトの目的や構成に関する注意事項を含みます。
-->
# try - すべての「ひらめき」は新しいディレクトリで

*実験には、適切な場所が必要です* 🏠

> ちょっとした実験のたびに新しいプロジェクトを作ってしまう人へ。このワンファイルRubyスクリプトで、散らかりがちなディレクトリを素早く管理・移動して、少しだけ整理された環境を提供します。

ファイルシステムに `test`, `test2`, `new-test`, `actually-working-test` などのディレクトリが50個も散らばってしまったことはありませんか？あるいは `/tmp` でコードを書いて、すべて消えてしまったことは？

**try** は、そんな混沌としたあなたのためのツールです。

# できること

[![asciicast](https://asciinema.org/a/ve8AXBaPhkKz40YbqPTlVjqgs.svg)](https://asciinema.org/a/ve8AXBaPhkKz40YbqPTlVjqgs)

すべての実験ディレクトリを瞬時に移動：
- **あいまい検索**で素早く見つかる
- **スマートソート**で最近使ったものが上位に
- **自動日付付与**で `2025-08-17-redis-experiment` のようなディレクトリ名を作成
- **設定不要** Rubyファイル1つ、依存なし

## クイックスタート

```bash
curl -sL https://raw.githubusercontent.com/tobi/try/refs/heads/main/try.rb > ~/.local/try.rb

# "try" を実行可能にする
chmod +x ~/.local/try.rb

# シェル(bash/zsh)に追加
echo 'eval "$(~/.local/try.rb init ~/src/tries)"' >> ~/.zshrc
```

## よくある問題

Redisを学んでいるとき、`/tmp/redis-test` を作り、次は `~/Desktop/redis-actually`、さらに `~/projects/testing-redis-again`。3週間後、深夜2時に書いたあの素晴らしいコネクションプールのコードがどこにあるか分からなくなります。

## 解決策

すべての実験を一箇所にまとめ、あいまい検索で即座にアクセス：

```bash
$ try pool
→ 2025-08-14-redis-connection-pool    2時間前, 18.5
  2025-08-03-thread-pool              3日前, 12.1
  2025-07-22-db-pooling               2週間前, 8.3
  + 新規作成: pool
```

入力して、下矢印、<kbd><kbd>Enter</kbd></kbd>ですぐ移動。

## 主な機能

### 🎯 スマートあいまい検索
部分一致だけでなく、賢く検索：
- `rds` で `redis-server` に一致
- `connpool` で `connection-pool` に一致
- 最近使ったものほど上位
- 名前が短いほど同点なら上位

### ⏰ 時間認識
- 最終更新からの経過時間を表示
- 最近アクセスしたディレクトリが上位
- 「昨日何してたっけ？」に最適

### 🎨 シンプルなTUI
- クリーンでミニマルなインターフェース
- 入力中に一致箇所をハイライト
- スコア表示で順位理由が分かる
- デフォルトでダークモード

### 📁 整理されたカオス
- すべて `~/src/tries` に保存（`TRY_PATH`で変更可）
- 日付で自動プレフィックス：`2025-08-17-your-idea`
- 名前を入力済みなら日付入力をスキップ

### シェル連携

`~/.bashrc` や `~/.zshrc` に追加：

```bash
# デフォルトは ~/src/tries
eval "$(~/.local/try.rb init)"
```

保存場所を変更したい場合：

```bash
eval "$(~/.local/try.rb init ~/src/tries)"
```

## 使い方

```bash
try                 # すべての実験を一覧
try redis           # redis実験に移動、なければ新規作成
try new api         # "2025-08-17-new-api" で新規開始
try --help          # オプション一覧
```

### キーボードショートカット

- <kbd><kbd>↑</kbd></kbd>/<kbd><kbd>↓</kbd></kbd> または <kbd><kbd>Ctrl</kbd></kbd>-<kbd><kbd>P</kbd></kbd>/<kbd><kbd>N</kbd></kbd> - 移動
- <kbd><kbd>Enter</kbd></kbd> - 選択または新規作成
- <kbd><kbd>Backspace</kbd></kbd> - 文字削除
- <kbd><kbd>ESC</kbd></kbd> - キャンセル
- 入力するだけで絞り込み

## 設定

実験ディレクトリの保存場所を変更するには `TRY_PATH` を設定：

```bash
export TRY_PATH=~/code/sketches
```

デフォルト: `~/src/tries`

## Nix

### クイックスタート

```bash
nix run github:tobi/try
nix run github:tobi/try -- --help
nix run github:tobi/try init ~/my-tries
```

### Home Manager

```nix
{
  inputs.try.url = "github:tobi/try";
  
  imports = [ inputs.try.homeManagerModules.default ];
  
  programs.try = {
    enable = true;
    path = "~/experiments";  # 任意。デフォルトは ~/src/tries
  };
}
```

## Rubyを選んだ理由

- ファイル1つ、依存なし
- Rubyが入っていればどこでも動作（macOSは標準搭載）
- 数千ディレクトリでも十分高速
- 改造が簡単

## 哲学

人の脳はきれいなフォルダ構造では動きません。アイデアが浮かび、試し、カフェインでテンションが上がったリスのようにコンテキストを切り替えます。このツールはその混沌を受け入れます。

すべての実験に居場所を。すべての居場所はすぐ見つかる。深夜2時のコードも、もう消えません。

## よくある質問

**Q: `cd` や `ls` じゃダメなの？**
A: ディレクトリが200個もあると、`test-redis` か `redis-test` か `new-redis-thing` か覚えていられません。

**Q: `fzf` じゃダメ？**
A: fzfはファイル検索には最適ですが、これはプロジェクトディレクトリ専用。時間認識や自動作成機能付きです。

**Q: 本番プロジェクトにも使える？**
A: 使えますが、これは実験用。正式なプロジェクトには正式な名前と場所を。

**Q: 実験が何千個もあったら？**
A: ようこそ仲間へ。スコアリングアルゴリズムで関連性の高いものが常に上位に来ます。

## コントリビュート

ファイルは1つだけ。変更したい場合は直接編集してください。他の人にも役立ちそうならPRを送ってください。

## ライセンス

MIT - ご自由にどうぞ。

*ADHDな開発者による、ADHDな開発者のためのツール*

*あなたの実験に居場所を* 🏠
