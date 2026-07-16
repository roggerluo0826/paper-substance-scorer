# 實戰打分範例:Frontier 資料中心數位孿生論文

一個完整走過 rubric 的真實案例,示範「每個分數都要引用證據」長什麼樣。

**論文**：Digital Twin-Based Cooling System Optimization for Data Center
**作者/出處**：Shrenik Amol Jadhav & Zheng Liu(University of Michigan-Dearborn),arXiv:2603.01198v3,2026-03-07,**preprint(投稿審查中)**

## 總評分卡

| 維度 | 0–10 | 權重 | 加權 | 一句話 |
|---|---|---|---|---|
| ① 真實數據驗證 + 深度 | 8.5 | 20% | 17.0 | 4.7 萬筆 Frontier 真實數據 + 比標準更嚴的門檻 + 分項驗證 |
| ② 基準比較公平性 | 8.0 | 15% | 12.0 | 用真實 2023 定速運轉當 baseline,非稻草人 |
| ③ 數字內部自洽 | 8.0 | 10% | 8.0 | ΔT 比 (1.72) 對照 Q/f 比 (1.725) 吻合 |
| ④ 假設合理 + 可重現 | 5.0 | 15% | 7.5 | 假設實在,但程式/數據未公開 → 現無法重現 |
| ⑤ 宣稱不超出證據 | 8.5 | 15% | 12.75 | 誠實區分理論最佳 vs 實務可行(92.4% 回收) |
| ⑥ Limitations 誠實度 | 9.0 | 15% | 13.5 | 專節 4 條具體缺陷,含 cp 6% 瑕疵 |
| ⑦ 不確定性量化 | 5.0 | 10% | 5.0 | 只做泵指數敏感度(21–28%),無信賴區間 |
| **綜合** | | | **≈ 75.75** | **B 級:有料的誠實 preprint,但邊界清楚** |

## 逐項證據(節錄)

**① 驗證(8.5)**：47,186 筆 Frontier 全年 10 分鐘真實數據;主動把 Guideline 14 門檻從 30%/±10% 收緊到 5%/±5%。Table 1:子迴路 CV-RMSE 1.96/1.97/2.67%,NMBE +1.69/+1.72/+2.43%,分項驗證。
- 🟡 扣分點:三個 NMBE **全正**=系統性偏差(非隨機),源頭見 Limitations。

**② baseline(8.0)**：對照 = 2023 真實定速運轉(冬 ~200、夏 350–400 kg/s),真實數據非稻草人。層層比 A vs baseline、B vs A、C vs B。

**③ 自洽(8.0)**：自行反推——用熱佔比/流量佔比算 Q₃/f₃=1.263、Q₁/f₁=0.736,比值 1.72;對照論文自報 ΔT₃/ΔT₁ = 13.8/8 = 1.725,吻合。

**④ 可重現(5.0)**：Section 7 明說「程式與數據接受發表時才公開」→ 目前無法重現(硬傷)。冷卻塔風扇模型自承簡化。

**⑤ 宣稱(8.5)**：A 20.4% / B 30.1% / C 27.8%,並註明 C 是 B 的 92.4% 回收;敏感度給「21–28% 穩健區間」;CT 省電標「方向正確非精確」。

**⑥ Limitations(9.0)**：Section 4.4 四條——CT 模型簡化、供水溫度獨立性假設、缺完整 MPC、**採用 cp 與數據集內部換算常數差 6%**。

**⑦ 不確定性(5.0)**：Table 4 泵指數 n∈{2.0,2.5,3.0} 敏感度;但無信賴區間、無蒙地卡羅、6% 偏差未傳遞。

## 掃到的紅旗

- 🚩 preprint 未同行評審
- 🚩 程式/數據未公開 → 無法重現
- 🚩 **⭐ 省電 20–30% 是數字孿生「模擬算」的,不是真的部署到 Frontier 跑一年驗證的**
- 🟡 headline 一大塊來自 CT 省電(B/C 減 15.8–17.9%),而 CT 正是作者自承「簡化不精確」的模型 → headline 要降一格
- 🟡 NMBE 全正的系統性偏差(源自 cp 6% 落差)未修正/未傳遞

## 白話結論與引用建議

- ✅ **可信賴**：數位孿生的**建模與驗證方法論**(大數據 + 嚴格 Guideline 14 + 分項驗證)——這套方法可借鑑。
- ⚠️ **要打折**:省電百分比是**模擬上限,非實測績效**。
- 🔑 **引用措辭**:寫「文獻模擬顯示潛力可達 20–30%」,**不可**寫「已驗證可省 30%」。
- 💬 可用金句:「baseline 系統運轉在最低熱安全流量的 2.9 倍」——點出定速泵浦嚴重超量=節能空間所在。

## JSON 輸出

```json
{
  "paper": { "title": "Digital Twin-Based Cooling System Optimization for Data Center", "arxiv_id": "2603.01198v3", "peer_reviewed": false },
  "rigor_score": 76,
  "grade": "B",
  "dimensions": { "validation": 8.5, "baseline": 8.0, "internal_consistency": 8.0, "reproducibility": 5.0, "claims_vs_evidence": 8.5, "limitations_honesty": 9.0, "uncertainty": 5.0 },
  "red_flags": ["preprint_not_peer_reviewed", "no_code_data", "modeled_not_deployed", "headline_rests_on_weak_model", "uncorrected_systematic_bias"],
  "one_liner": "誠實紮實的 preprint,但省電為模擬上限、程式未公開、部分 headline 靠簡化的 CT 模型",
  "citation_advice": "引用寫成『模擬顯示潛力可達 20–30%』,勿寫成『已驗證省 30%』"
}
```
