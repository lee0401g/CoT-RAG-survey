# Domain-augmented large language models for automated root cause classification of offshore process incidents

## 總結

> 這篇研究針對離岸製程事故報告的根因分類問題，採用結合領域知識的 Chain-of-Thought 與 Retrieval-Augmented Generation 方法，發現可顯著提升分類準確度（F1-score 由 0.552 提升至 0.663）並降低過度分類現象

### 盲區

> 閱讀盲區

- 未深入說明 GPT-4o mini 與其他模型（如 GPT-4o）在此任務的具體差異
- 對於 RAG 檢索內容如何影響推理過程的細節描述有限
- 缺乏對錯誤案例（misclassification）的具體分析範例

> 本研究盲區

- 使用的事故報告（BSEE）本身資訊不完整，限制模型表現上限
- 僅評估頂層分類（11 類），未驗證更細粒度（第二、三層）能力
- 知識庫僅包含 ABS 指南與少量範例（30 筆），覆蓋不足
- 對設計（Design）與文件（Documentation）等需隱性推理的類別表現仍弱
- 未測試跨領域或不同法規環境的泛化能力

---

## IMRaD

> 問題是什麼?為何重要?研究動機與背景

- 製程工業事故會造成傷亡、經濟損失與聲譽損害
- 事故調查報告多為非結構化文本，人工分析效率有限
- 傳統 ML/NLP 方法依賴標註資料，且缺乏可解釋性與彈性
- LLM 雖具潛力，但缺乏領域知識會產生幻覺與不可靠結果
- 需要一種能結合推理能力與領域知識的自動化 RCA 方法

> 他們怎麼做?用了哪些數據、模型與技術

- 資料：1182 份 BSEE 離岸事故調查報告（2004–2024）
- 將報告與 ABS Root Cause Map™ 轉為 JSON 結構化資料
- 使用 embedding 建立 FAISS 向量資料庫（RAG 知識庫）
- 模型：GPT-4o mini
- 方法組合：
  - Zero-shot / Expert prompting
  - Chain-of-Thought（含領域特化 CoT）
  - Retrieval-Augmented Generation（RAG）
- 任務：多標籤分類（11 個頂層根因類別）
- 評估指標：Precision、Recall、F1-score（集合式）

> 做出什麼成果?圖表顯示了什麼發現?

- Domain CoT + RAG 表現最佳（F1 = 0.663）
- Precision 顯著提升（0.646），顯示誤判減少
- 預測類別數從 3.92 降至 2.78（接近人類 2.55）
- 高表現類別：
  - Equipment Reliability
  - Procedure
  - Human Factors（F1 > 0.75）
- 低表現類別：
  - Design
  - Documentation（需隱性推理）
- 無 RAG 模型傾向過度預測（高 Recall、低 Precision）
- RAG 能有效抑制 hallucination 與 over-classification
- 類別關聯分析顯示人因相關因素高度共現

> 結果代表什麼?有那些限制與未來延伸?

- 結合領域知識與 LLM 推理可提升安全分析可靠性
- RAG 是降低幻覺與提升可解釋性的關鍵
- 模型能學習接近人類的分類行為模式
- 限制：
  - 資料品質不足影響上限
  - 隱性因果推理能力不足
  - 知識庫規模小
- 未來方向：
  - 擴充多來源知識庫（如 CCPS）
  - 發展多層級 RCA（更細分類）
  - 引入 human-in-the-loop
  - 改進多步推理與約束推理機制
  - 建立解釋可信度評估指標

---

## 其他視角

> 審稿人角度（最可能被質疑的3點）

1. 評估僅限頂層分類，是否足以證明實務價值？
2. 知識庫規模過小，RAG 效果是否可泛化？
3. 資料來源（BSEE）品質不佳，是否影響結論可信度？

---

> 如果應用到教育領域，需要哪些修改

- 簡化 RCA 分類架構，降低學習門檻
- 提供更多逐步推理（step-by-step）教學範例
- 建立互動式案例分析平台（學生可模擬判斷）
- 強化錯誤分析回饋機制（解釋為何分類錯誤）
- 使用可視化工具呈現因果關係（如樹狀圖）

---

> 教學式解釋（給初學者）

- 想像你在分析一場事故原因（例如設備爆炸）
- 傳統方法像是「看關鍵字」來分類原因
- 但這篇研究讓 AI：
  - 先一步步推理（Chain-of-Thought）
  - 再查資料（RAG，就像開書找答案）
- 最後 AI 不只給答案，還會說「為什麼這樣判斷」
- 加入專業知識後，AI 比較不會亂猜（減少 hallucination）
- 就像一個「有查資料能力的專家助理」

---

初學者最容易誤解的地方：

- 誤以為 LLM 本身就懂專業知識（其實需要 RAG 補充）
- 認為準確率提升只是模型變強（其實是知識導入的效果）
- 忽略多標籤分類的困難（不是只有一個正確答案）
- 混淆「因果因素」與「根本原因」
- 以為 CoT 是答案，其實只是推理過程

---

**ChatGPT**
