# VSCode Markdown ドキュメント環境テンプレート

[![Made with Markdown](https://img.shields.io/badge/Made%20with-Markdown-blue.svg)](https://daringfireball.net/projects/markdown/)
[![PDF Exportable](https://img.shields.io/badge/PDF%20-Exportable-green)](README.md)
[![Mermaid Supported](https://img.shields.io/badge/Mermaid-supported-brightgreen)](https://mermaid.js.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## ✅ 機能

| 機能                   | 説明                                             |
| ---------------------- | ------------------------------------------------ |
| PDF 出力               | Markdown PDF 拡張によって一発変換                |
| 目次                   | Markdown All in Oneによる目次の自動生成          |
| 改ページ指定           | `<div class="page"/>` を挿入するだけ             |
| ファイル分割執筆       | `markdown-it-include` で複数 Markdown を結合可能 |
| フローチャート図       | mermaid.js によるフローチャート図も記述可        |
| 文書フォーマット       | markdownlint 拡張で Markdown の文法チェック      |
| 画像のコピー＆ペースト | 文書内に貼り付けると自動的に画像保存             |
| カスタムデザイン       | CSSによるフォントや装飾はカスタム可能            |

## 📁 プロジェクト構成

```txt
markdown-project/
├── .vscode/
│   ├── extensions.json      ← 推奨拡張一覧
│   └── settings.json        ← Markdown PDF 設定
├── docs/
│   ├── index.md             ← メイン文書
│   ├── cover.md             ← 表紙ページ（HTML）
│   ├── section1.md          ← メイン文書を構成する部分文書
│   ├── section2.md          ← 同上
│   └── section3.md          ← 同上
├── css/
│   ├── page-style.css       ← 本文スタイル
│   └── cover-style.css      ← 表紙スタイル
├── images/
│   └── section3/
│       └── section3.png     ← 画像貼付テスト用画像
└── README.md
```

## 🛠️ セットアップ手順

### 1. VSCode 拡張をインストール

- [Markdown PDF](https://marketplace.visualstudio.com/items?itemName=yzane.markdown-pdf)
- [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)
- [Markdown Preview Mermaid Support](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-mermaid)
- [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)

### 2. `page-style.css`, `cover-style.css` の設定

PDF 出力時のデザインを記載します。デザインはお好みで変更してください。

### 3. `settings.json` の設定

Markdown を快適に扱うための設定を記述しています。主な内容は以下の通りです。

#### 基本編集設定

```json
"editor.formatOnSave": true,
"editor.formatOnType": true
```

- 保存時と入力時に自動整形を実行（JSON・Markdownに対応）

#### Markdown専用整形・Lint設定

```json
"[markdown]": {
  "editor.defaultFormatter": "yzhang.markdown-all-in-one"
},
"editor.codeActionsOnSave": {
  "source.fixAll.markdownlint": "explicit"
}
```

- Markdownは `Markdown All in One` で整形
- 保存時に `markdownlint` のルールを自動適用

#### 画像貼り付けの保存先

```json
"markdown.copyFiles.destination": {
  "*": "../images/${documentBaseName}/${documentBaseName}.${fileExtName}"
}
```

- `Ctrl+V` で貼り付けた画像を `images/ファイル名/ファイル名.png` に自動保存
- パスはMarkdown内に相対指定で埋め込まれ、PDF出力にも対応

#### PDF出力の見た目・構造

```json
"markdown-pdf.breaks": true,
"markdown-pdf.styles": [
  "../css/cover-style.css",
  "../css/page-style.css"
],
"markdown-pdf.includeDefaultStyles": false,
"markdown-pdf.stylesRelativePathFile": true,
"markdown-pdf.outputDirectory": "output",
"markdown-pdf.margin.top": "0cm",
"markdown-pdf.margin.bottom": "2cm",
```

- 改ページタグ (`<div class="page"/>`) に対応
- 表紙・本文のCSSを個別適用
- 出力先を `output/` フォルダに指定
- PDF上下余白を調整（上0cm／下2cm）

#### ヘッダー・フッター設定

```json
"markdown-pdf.displayHeaderFooter": true,
"markdown-pdf.headerTemplate": "<div></div>",
"markdown-pdf.footerTemplate": "<div style=...>"
```

- ヘッダーなし／フッターにページ番号と著作表示（© SAMPLE）を表示

#### その他

```json
"markdown-pdf.mermaidServer": "https://unpkg.com/mermaid@10.0.3-alpha.1/dist/mermaid.min.js",
"markdown-pdf.markdown-it-include.enable": true,
```

- Mermaid.js による図表出力に対応
- `@[include](...)` によるMarkdownファイル結合を有効化

```json
"markdownlint.config": {
  "MD033": false, // HTMLタグ許可
  "MD041": false  // 見出しから開始しなくてもOK
}
```

- 表紙などHTML記述を許容しつつ lint を適用

---

## 📝 Markdown 記述例（index.md）

```markdown
:[表紙](cover.md)

# 目次

- [目次](#目次)
- [１章](#１章)
  - [Markdown PDF 拡張機能の設定](#markdown-pdf-拡張機能の設定)
  - [Mermaid フローチャートとシーケンス図](#mermaid-フローチャートとシーケンス図)
- [２章](#２章)
  - [画像の貼り付け](#画像の貼り付け)

<div class="page"/>

# １章

## Markdown PDF 拡張機能の設定

:[セクション１](section1.md)

## Mermaid フローチャートとシーケンス図

:[セクション２](section2.md)  

<div class="page"/>

# ２章

## 画像の貼り付け

:[セクション３](section3.md)

```

## 🚀 PDF出力手順

1. `index.md` を VSCode で開きます
2. 右クリック → `Markdown PDF: Export (pdf)`
3. `output/index.pdf` が生成されます

### 参考

- [VSCODE+MarkdownでキレイなPDFとHTMLを出力して、TRPGシナリオリリース当日まで寝て過ごす。](https://tthrr.com/article/15168c8f-2401-8194-9a60-cdc3d8df5e35#15168c8f240181e6a0fee7031e9365dd)
- [VSCode＋MarkdownでPDF資料作成する際に表紙とフッターを良い感じにした備忘録](https://qiita.com/matcha_kinako/items/5c62784db4919e048925)
- [Visual Studio Code で Markdown に画像を貼り付けられる markdown.copyFiles.destination 設定メモ 2024 年 5 月改良版](https://www.1ft-seabass.jp/memo/2024/04/26/vscode-current-markdown-copyfiles-destination-setting-ver2/)
