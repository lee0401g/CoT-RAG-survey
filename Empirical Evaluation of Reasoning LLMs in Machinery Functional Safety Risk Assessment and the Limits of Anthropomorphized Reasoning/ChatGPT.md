# Empirical Evaluation of Reasoning LLMs in Machinery Functional Safety Risk Assessment and the Limits of Anthropomorphized Reasoning

## 總結

> 這篇研究 探討大型語言模型在機械功能安全風險評估中的可靠性，用 多模型比較與多種提示策略（rule-based、CoT、RAG）實驗方法，發現 嚴格依規則的提示能穩定且高準確完成PLr判定，而推理式方法（CoT/RAG）反而引入不穩定與錯誤

### 盲區

> 閱讀盲區

- PLr（Performance Level required）與ISO 13849風險圖的背景知識門檻較高  
- S/F/P（Severity/Frequency/Possibility）參數如何從自然語言映射並不直觀  
- 多種 prompting strategy 差異需要具備 LLM prompt engineering 基礎  
- 評估指標（Macro-F1 / Weighted-F1 / per-class recall）理解成本較高  

> 本研究盲區

- 僅聚焦 PLr 分類，未涵蓋完整風險評估流程（如 hazard identification）  
- 資料集雖標準化，但仍屬合成與工程撰寫場景，未完全代表真實產線  
- 未納入 human-in-the-loop 實務流程比較  
- 模型選擇偏向主流商用模型，未涵蓋更多開源模型  
- 未深入分析模型內部機制（僅行為層級觀察）  

---

## IMRaD

> 問題是什麼?為何重要?研究動機與背景

- 工業機械安全評估需要高度可解釋且一致的決策  
- PLr 判定是安全控制系統設計的關鍵步驟  
- 傳統方法依賴專家，成本高且一致性不足  
- LLM 被認為具備「推理能力」，但其可靠性尚未驗證  
- 安全關鍵任務容錯極低，錯誤可能導致嚴重事故  
- 存在「擬人化偏誤」，容易誤以為模型真的在推理  

> 他們怎麼做?用了哪些數據、模型與技術

- 使用 7800 筆機械危害情境資料集（依 ISO 12100 與 ISO 13849 建立）  
- 建立兩種測試集：  
  - Variant 1：標準化 ISO 語言  
  - Variant 2：工程師自由描述（語言變形）  
- 評估六種 prompting 策略：  
  - ZERO_SHOT  
  - WITH_RULES  
  - PURE_CoT  
  - COT_WITH_RULES  
  - WITH_RULES_RAG  
  - COT_WITH_RULES_RAG  
- 測試六種 LLM 模型（不同廠商與架構）  
- 採用 deterministic decoding（消除隨機性）  
- 指標包含 accuracy、Macro-F1、per-class recall、延遲時間  
- RAG 採 hybrid retrieval（語意 + lexical + S/F/P 約束）  

> 做出什麼成果?圖表顯示了什麼發現?

- WITH_RULES 在所有模型上達到接近滿分準確率（≥0.92，甚至接近1.0）  
- PURE_CoT 表現高度不穩（0.40–0.99）  
- ZERO_SHOT 明顯不足，尤其小模型表現崩潰  
- CoT + rules 雖改善，但仍不如純 rule-based  
- RAG 在標準語境下幫助有限  
- CoT + RAG 甚至可能降低小模型表現  
- rule-based prompting 同時最佳化：  
  - 準確率  
  - 穩定性  
  - 計算效率  
- CoT 方法增加 2–10 倍延遲  
- 僅 rule-based 方法能穩定抓到最高風險 class e  

> 結果代表什麼?有那些限制與未來延伸?

- LLM 並未真正進行推理，而是語言模式延續  
- 「可解釋推理」多為表面現象（reasoning illusion）  
- 安全關鍵任務需依賴明確規則，而非自由推理  
- CoT 不適合用於 deterministic classification  
- RAG 必須嚴格控制，否則引入噪音  
- 未來需發展：  
  - 更嚴謹的 domain-specific benchmark  
  - human-in-the-loop 系統  
  - rule-grounded AI 架構  
- 強調不可過度擬人化 LLM 能力  

---

## 其他視角

> 審稿人角度（最可能被質疑的3點，若內容充分可更多）

1. 實驗是否過度依賴特定資料集，影響泛化能力  
2. rule-based prompting 的成功是否只是 trivial mapping，而非真正 AI 能力  
3. 未比較人類專家與模型的實際效能差距  

---

> 如果應用到教育領域，需要哪些修改

- 將 ISO 規則轉為教學式逐步引導（scaffolded learning）  
- 加入錯誤案例分析（讓學生理解誤判原因）  
- 將 S/F/P 判定轉為互動式練習  
- 結合模擬情境（case-based learning）  
- 使用 explainable AI 輔助，而非自動決策  

---

> 教學式解釋（給初學者）

- 想像你在評估一台機器有多危險  
- 你需要看三件事：  
  - 傷害嚴不嚴重（S）  
  - 人會不會常接觸（F）  
  - 能不能躲開（P）  
- 把這三個條件放進一個「表格」（ISO risk graph）  
- 就可以得到風險等級（PLr a–e）  
- 這篇研究在測試：  
  - AI 能不能自己做這件事  
- 結果發現：  
  - 如果直接給 AI 規則 → 表現很好  
  - 如果讓 AI「自己想」 → 容易出錯  
- 所以 AI 比較像是在「套公式」，不是「真的理解」  

---

初學者最容易誤解的地方：

- 認為 CoT（一步一步解釋）就代表真的在推理  
- 認為 LLM 可以取代專家做安全決策  
- 混淆「語言流暢」與「邏輯正確」  
- 忽略少數高風險案例（class e）的重要性  
- 以為 RAG 一定會提升模型表現  

---

**ChatGPT**