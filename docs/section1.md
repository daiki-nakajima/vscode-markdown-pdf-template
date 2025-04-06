Visual Studio CodeでMarkdownをPDFに変換する拡張「Markdown PDF」の最小構成例です。

### 設定ファイル（.vscode/settings.json）

```json
{
  "markdown-pdf.styles": ["style.css"],
  "markdown-pdf.breaks": true,
  "markdown-pdf.includeDefaultStyles": false,
  "markdown-pdf.mermaidServer": "https://unpkg.com/mermaid@10.0.3-alpha.1/dist/mermaid.min.js"
}
```
