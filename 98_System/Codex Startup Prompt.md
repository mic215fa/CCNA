# Codex Startup Prompt

tags: #codex #startup #vault/rules #acting-ccna

> [!info] 用途
> 這份筆記用來在 Windows / VS Code / 新的 Codex session 中快速貼上，讓 Codex 先讀取本 Vault 的規則並接續工作。

## 啟動提示

```text
請先閱讀並遵守本 Obsidian Vault 的以下檔案：

1. AGENTS.md
2. 98_System/Vault Rules.md
3. 99_Change Log/Ver 0.0.2.md

請以繁體中文為主回覆與撰寫筆記，專有名詞、Cisco / CCNA 術語與必要英文原文可以保留英文。

這個 Vault 是我用來閱讀、整理與複習 Acting CCNA exam 的 Obsidian Vault。請把它當成 Obsidian Markdown Vault 處理，善用 Obsidian 語法，例如 wikilinks、callouts、tags、表格與清楚的標題層級。

目前重要規則：

- 依照 AGENTS.md 的 Unit Processing Workflow 處理 Unit。
- Source files 在 00_Source，是 source-of-truth；除非我明確要求，請不要修改原始來源檔。
- 建立或更新 Unit 時，請產出或維護：
  - Source Note：02_Source_Notes
  - Concepts：03-Concepts
  - Maps：04_Maps
  - Questions：05_Questions
  - REVIEW：06_review
  - Cross-document / Cross-unit relationships
- Relationships Added 的內容必須寫入 04_Maps 中的 durable Knowledge Map，不要只放在最後回覆。
- REVIEW 的內容必須寫入 06_review，不要只放在最後回覆。
- 03-Concepts 中的 Concept Notes 應盡可能加入有助理解的來源圖片，圖片需來自 00_Source 或 00_Source/images，並在圖片下方加底標，標註來源 Chapter 與 Figure / caption。
- 每次對 Vault 進行操作，都要記錄到目前版本的 Change Log。
- 目前版本是 0.0.2；除非我明確要求更新版本號，否則不要改版本號。
- Change Log 目前寫入：99_Change Log/Ver 0.0.2.md。
- 我的偏好是：筆記以繁體中文為主、結構清楚、可在 Obsidian 中好讀好複習。

開始工作前，請先確認目前工作目錄就是這個 Vault 的根目錄，並簡短回報你讀到的 Vault 狀態。如果目錄名稱和我輸入略有不同，例如 01-Units vs 01_Units，請以實際存在的目錄為準。
```

## 使用方式

1. 在 Windows / VS Code 中開啟這個 Vault folder。
2. 開啟新的 Codex session。
3. 複製上方 `啟動提示` 區塊貼給 Codex。
4. 等 Codex 確認已讀取 `AGENTS.md`、`Vault Rules` 與目前 Change Log 後，再下達具體處理任務。

## 注意事項

> [!warning] 路徑差異
> macOS / iCloud 與 Windows 的實體路徑會不同；請讓 Codex 以「目前 VS Code 開啟的 workspace root」為準，不要硬套 macOS 路徑。

> [!tip] 穩定接續任務
> 如果新 session 接續之前的 Unit 處理，請在任務中明確指定 Unit 檔案，例如：`請依照 AGENTS.md，處理 01_Units/Unit04-xxx.md`。
