# 📊 論文有料打分器 · Paper Substance Scorer

一個 [Claude Code](https://claude.com/claude-code) skill,判斷一篇研究論文是否**「有料」**——不是看它切不切題、夠不夠新,而是看它**紮不紮實、可不可信、宣稱有沒有超出證據**——並給一個 **0–100 的嚴謹度分數**。

> 核心心法:**「數字漂亮」不代表有料;「敢講缺點、宣稱不超出證據」才是有料的信號。**

## 這跟「關鍵字打分」有什麼不同?

搭配姊妹專案 [`hvac-ai-paper-daily`](https://github.com/roggerluo0826/hvac-ai-paper-daily) 使用效果最好——那個網站的分數是**切題度**(關鍵字加權 + 近期加成,判斷「夠不夠切題、夠不夠新」);本 skill 給的是**嚴謹度**(讀全文,逐維度評「紮不紮實、可不可信」)。

| | 切題度分數 | 嚴謹度分數(本 skill) |
|---|---|---|
| 需要 | 只看標題/摘要 | 讀全文、方法、驗證、Limitations |
| 角色 | 第一關快篩 | 第二關深評 |

兩關都過,才是真有料。

## 評分維度(7 維加權,0–100)

| 維度 | 權重 |
|---|---|
| 真實數據驗證 + 深度 | 20% |
| 基準比較公平性 | 15% |
| 數字內部自洽 | 10% |
| 假設合理 + 可重現 | 15% |
| 宣稱不超出證據 | 15% |
| Limitations 誠實度 | 15% |
| 不確定性量化 | 10% |

外加**紅旗硬上限**(如「完全無真實驗證 → 封頂 40 分」)與**必須點明的陷阱**(如「省電是模擬算的、不是實測部署的」)。

## 怎麼用

安裝後,在 Claude Code 對話裡:

- 「這篇論文有料嗎?幫我打分」+ 貼上 arXiv 連結 / PDF
- 「這篇 preprint 可不可信?」
- 「幫我評這篇文獻值不值得引用」

Claude 會抓全文、逐維度打分、掃紅旗,輸出**評分卡 + 逐項證據 + 白話結論 + 引用建議**,並附一段結構化 JSON 方便程式讀取。

### 安裝

把這個 repo 放進你的 Claude Code skills 目錄(或用你慣用的 skill 安裝方式),Claude 會依 `SKILL.md` 的 description 自動在合適時機觸發。

## 檔案結構

```
paper-substance-scorer/
├── SKILL.md                              # 主流程與心法
└── references/
    ├── rubric.md                         # 7 維加權評分細則 + 輸出 JSON 格式
    ├── red-flags.md                      # 紅旗清單與硬上限規則
    └── worked-example-frontier.md        # 真實論文完整打分範例
```

## 附:與 hvac-ai-paper-daily 的整合方向

`references/rubric.md` 末段記錄了日後把「嚴謹度分數」融進論文網站的建議做法(兩軸並存 `relevanceScore` + `rigorScore`,而非相乘覆蓋)。本次僅建立 skill,網站程式未改動。

---

🤖 與 [Claude Code](https://claude.com/claude-code) 協作建立
