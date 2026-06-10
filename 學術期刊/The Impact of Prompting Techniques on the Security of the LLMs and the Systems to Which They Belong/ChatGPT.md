# The Impact of Prompting Techniques on the Security of the LLMs and the Systems to Which They Belong

## 總結

> 這篇研究探討提示工程（Few-shot、CoT、PaL、ReAct）對大型語言模型安全性的影響，用文獻分析、攻擊實驗與多種 GPT 模型測試方法，發現提示技術雖能提升效能，但同時提高 Prompt Injection 成功率，並讓 PaL、ReAct 等技術引入新的攻擊面，使系統整體安全風險增加。 

### 盲區

> 閱讀盲區

* 提示工程提升能力的同時，也可能削弱安全性
* Jailbreak 與 Prompt Injection 雖常被混用，但實際攻擊目標與結果不同
* 安全問題不只存在於 LLM 本身，也可能擴散到資料庫、API、程式執行環境等外部元件
* CoT、PaL、ReAct 等推理增強技術並非純粹效能工具，也會改變攻擊面
* LLM 的上下文學習能力同時是優勢與弱點

> 本研究盲區

* 僅使用 OpenAI GPT 系列模型，缺乏其他開源模型比較
* 攻擊樣本數量有限，部分實驗僅重複數次
* 未深入分析多模態模型（圖片、文件）是否存在類似 Prompt Injection
* 未完整研究長期記憶（Memory）對安全性的影響
* 防禦策略主要停留在架構與設計層面，缺乏大規模驗證
* 未建立標準化安全評測基準與統計顯著性分析

---

## IMRaD

> 問題是什麼?為何重要?研究動機與背景

* Prompt Engineering 已被證明能顯著提升 LLM 表現
* 現有研究大多關注效能提升，而忽略安全影響
* OWASP 已將 Prompt Injection 列為 LLM 應用的重要風險
* 隨著 LLM 被整合到企業系統與代理系統中，安全問題影響範圍擴大
* 作者質疑提示技術是否可能成為隱藏於系統中的安全弱點
* 希望建立 Prompting 技術與安全風險之間的因果關係

> 他們怎麼做?用了哪些數據、模型與技術

* 回顧 Few-shot、CoT、PaL、ReAct 等主流提示技術
* 分析 OpenAI 內容審核案例與相關安全研究
* 使用 OpenAI API 與 LangChain 建立實驗環境
* 測試 GPT-3.5、GPT-4、GPT-4o 等模型
* 區分並重新定義 Jailbreak 與 Prompt Injection
* 設計多種攻擊情境，包括：

  * DAN Jailbreak
  * Few-shot Prompt Injection
  * HOUYI 攻擊
  * 間接 Prompt Injection
  * Thought Injection
* 比較不同 Prompting 技術下的攻擊成功率
* 分析防禦方法與系統設計策略

> 做出什麼成果?圖表顯示了什麼發現?

* Few-shot Prompting 普遍提高 Prompt Injection 成功率
* HOUYI 攻擊在多數任務上具有極高成功率
* Prompt Injection 通常比 Jailbreak 更具工程風險
* PaL 因可產生程式碼並與執行環境互動，形成新的攻擊入口
* ReAct 因可操作外部工具與 API，使攻擊面擴大
* GPT 模型在部分情況下會自發產生防禦行為
* Few-shot 雖提升任務表現，但削弱模型對惡意輸入的抵抗能力
* Prompting 技術與安全性之間存在明顯權衡關係

> 結果代表什麼?有那些限制與未來延伸?

* 提示工程不能只評估效能，也必須納入安全考量
* 代理型架構（Agent）比單純聊天機器人更容易受到攻擊
* 防禦應採多層次策略而非單一方法
* 程式執行環境應與核心系統隔離
* 工具權限應最小化配置
* 未來可研究圖片、文件等多模態 Prompt Injection
* 未來可研究長期記憶與上下文管理對安全性的影響
* 可建立專門偵測惡意提示的模型
* 需要持續 Red Teaming 與安全測試機制

---

## 其他視角

> 審稿人角度

1. 研究問題具有新穎性，因為過去 Prompt Engineering 研究較少關注安全性
2. Jailbreak 與 Prompt Injection 的重新定義有助於釐清研究範疇
3. 僅採用 GPT 系列模型，外部效度有限
4. 實驗樣本量偏小，可能影響結果穩定性
5. 缺少與開源模型（Llama、Mistral 等）的比較
6. 防禦策略多屬概念性建議，缺少量化驗證
7. 對 PaL 與 ReAct 的安全分析具有實務價值
8. 可進一步建立標準化安全 Benchmark 作為後續研究基礎

---

> 如果應用到教育領域，需要哪些修改

* 建立教育專用安全提示模板
* 限制學生可操作的工具權限
* 將 ReAct 代理限制在封閉學習環境中
* 加入教材與作業資料的 Prompt Injection 檢測
* 建立學生與教師雙重審核機制
* 對 AI 回答進行事後驗證與引用來源檢查
* 設計教育版 Red Team 測試流程
* 強化 AI 素養教育，讓學生理解攻擊與防禦概念

---

> 教學式解釋（給初學者）

* 可以把 LLM 想成一位非常聰明但容易被說服的助理
* Prompt Engineering 就像給助理更詳細的工作說明
* Few-shot 是先給範例再讓助理模仿
* CoT 是要求助理一步一步思考
* PaL 是讓助理把數學或邏輯問題交給程式執行
* ReAct 是讓助理能查資料、呼叫工具與執行行動
* 問題在於：當助理越有能力，駭客也越容易利用這些能力
* Prompt Injection 就像有人偷偷把假指令夾進工作說明中
* Jailbreak 則像讓助理忘記原本的規則與限制
* 因此提升能力與維持安全必須同時考量

---

初學者最容易誤解的地方：

* 認為 Prompt Engineering 只會提升效能，不會影響安全
* 認為 Jailbreak 與 Prompt Injection 完全相同
* 認為更強大的模型一定更安全
* 認為 CoT、PaL、ReAct 只是推理技巧，不會增加攻擊面
* 認為只要保護 LLM 本身就足夠，忽略外部工具與資料庫風險
* 認為 Prompt Injection 一定來自使用者輸入，忽略文件、郵件、資料庫等間接來源
* 認為 AI 會自動辨識所有惡意指令，實際上許多攻擊仍可成功繞過防護機制

---

ChatGPT (GPT-5.5)
