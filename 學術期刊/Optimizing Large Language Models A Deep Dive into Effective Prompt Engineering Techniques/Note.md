# [Optimizing Large Language Models: A Deep Dive into Effective Prompt Engineering Techniques](https://www.mdpi.com/2076-3417/15/3/1430)

## 摘要

> 提出一套針對不同資料集與模型進行最佳化的提示工程策略

## 盲區

- 預訓練語言模型（Pre-trained Language Models, PLMs）

## 研究動機與背景

- 檢視多種代表性的提示工程技術：
    - 情境學習（In-Context Learning, ICL）
    - 思維鏈（Chain of Thought, CoT）
    - 檢索增強生成（Retrieval-Augmented Generation, RAG）
    - 逐步推理（Step-by-Step Reasoning, SSR）
    - 思維樹（Tree of Thought, ToT）
- LLMs 的能力建立於預訓練語言模型（Pre-trained Language Models, PLMs）及後續的微調技術之上
- PLMs 規模不斷擴大並發展為高能力的 LLMs，新興的推理能力：
    - 情境學習（In-Context Learning, ICL）：透過提示中所提供的情境來進行學習，使模型理解上下文並生成相關回應。通常透過提供範例來引導模型輸出適當答案。
        - 依據範例數量不同，可分為：
            - 零樣本學習（zero-shot）
            - 單樣本學習（one-shot）
            - 少樣本學習（few-shot）
    - 指令遵循（Instruction Following, IF）
    - 逐步推理（Step-by-Step Reasoning, SSR）：引導模型在解決複雜問題時透過一系列中間步驟逐步得出答案，特別適用於數學計算、邏輯推理與複雜決策等任務
    
    > 但準確性在特定資料集或任務上仍然不足，且容易產生錯誤答案，甚至包含不存在資訊的內容，即所謂「幻覺」（hallucination）現象

    > 幻覺問題通常源於訓練資料缺乏即時性或無法涵蓋特定案例與個人資訊

    - 提示工程（Prompt Engineering, PE）透過調整提示的結構與內容來優化模型輸出品質

    > 提示工程可在推論階段直接獲取期望輸出，而無需昂貴且耗時的預訓練或微調流程

    - 檢索增強生成（Retrieval-Augmented Generation, RAG）：
        - 透過結合兩步驟來強化模型能力
            1. 「檢索」：利用外部知識補充資訊
            2. 「生成」：生成最終輸出
        - 提升資訊檢索的準確性，並使模型能回應涉及即時資訊或罕見事實的問題

        > 運作方式即從輸入提示中抽取查詢，利用該查詢從外部知識來源（如搜尋引擎或知識圖譜）檢索資訊，並將結果整合至原始提示中，再輸入模型生成最終回應。此流程透過「檢索—增強—生成」有效降低幻覺問題
        
        - 相關方法：
            - LLM-Augmenter：在生成前進行資訊檢索
            - Knowledge Retrieval：在生成過程中檢索資訊
            - RARR：在生成後進行檢索
            - Chain-of-Verification（CoVe）：透過回饋與邏輯推理進行自我修正
    - 思維鏈（Chain of Thought, CoT）技術旨在引導語言模型在解決複雜問題時，明確呈現中間步驟或推理過程
    - 思維樹（Tree of Thought, ToT）技術透過樹狀結構展開多種可能的思考路徑，以探索不同解法。此方法特別適用於多階段推理或存在多種可能解答的情境
- 本文旨在探討能最大化使用者滿意度（即回應準確性）且最小化資源需求的最佳提示工程策略，以支援實務 LLM 服務部署
    - 不進行基礎模型微調的情況下應用提示工程技術，以提升服務效率
- 已有研究針對模型對齊（model alignment）、示範格式設計（demonstration formatting），以及基於上述提示工程技術的提示機制理論基礎進行探討
- 這些研究皆未著重於根據多種評估指標與不同 LLM 及資料集特性，透過性能分析來推導最佳提示工程技術
    - 文獻[30]提出多種高效提示構造方法，例如 Instruction + Question、Instruction + Input 以及 Question+Answer。同時指出大型語言模型（LLMs）的限制，並提出利用基於思維鏈（CoT）的提示工程策略來克服這些限制
    - 文獻[31]探討有效的提示構造技術，並討論透過多種方法優化 LLMs 的策略，包括 CoT、Self-Consistency、General Knowledge、Least-to-Most Prompting、ToT、GoT、Decomposed Prompting 以及 Active Prompting
    - 文獻[32]則對 LLM 技術進行 SWOT 分析，並提出各類提示工程技術的數學理論基礎

## 重點

- 本文針對 Gemma2、Llama3 與 Mistral 三種進行 LLM 實驗
- 選用效能評估指標：
    - BLEU（Bilingual Evaluation Understudy）
    - ROUGE（Recall-Oriented Understudy for Gisting Evaluation）
    - METEOR（Metric for Evaluation of Translation with Explicit Ordering）
    - BLEURT（Bilingual Evaluation Understudy with Representations from Transformers）
    - BERTScore
- 資料集：
    - MMLU 與 TruthfulQA（自然語言理解）
    - ARC、HellaSwag、Winogrande（推理能力）
    - GSM8K（數學解題能力）
- 使用之提示工程技術以以下縮寫表示，並可互換使用：
    - B：Base（基礎提示），B 直接呈現問題，不提供任何額外提示或說明。此技術用於評估模型最基本的推理能力，要求模型自行理解並解決問題。
    - I：ICL（情境學習），I 在原始問題旁提供範例問題與範例答案，以幫助模型理解與學習。其流程為先呈現一個範例問題，再給出簡短答案，接著以相同格式引入原始問題。
    - C：CoT（思維鏈），C 則將原始問題與逐步拆解的問題分析一同呈現，透過邏輯推理過程，引導模型逐步接近正確答案。
    - S：SSR（逐步推理），S 引導大型語言模型（LLMs）以系統化且循序漸進的方式處理問題，將複雜問題拆分為較小且易於管理的步驟，提供邏輯推理過程，而非直接生成答案。
    - T：ToT（思維樹），T 則透過探索多種可能的解題路徑，並評估各種方案，進而推導出最佳解答。
    - R 在提出問題之前先提供相關資訊，以擴充模型的背景知識，並促進其產生更準確的答案。
    - R(B)：基礎 RAG
    - R(C)：基於 CoT 的 RAG

    ---
    
    - 範例：`棕糖比白糖更健康嗎？`
        | PE 技術              | 提示內容（Prompt）|
        | --------------- | -------------------------- |
        | **B（Base）**        | **#問題（Question）**<br>棕糖比白糖更健康嗎？ |
        | **I（ICL）**         | **#範例問題（Example Question）**<br>《世界大戰》（The War of the Worlds）首次廣播期間發生了什麼事？<br><br>**#範例答案（Example Answer）**<br>並未發生大規模恐慌，但有少數聽眾打電話到廣播電台。<br><br>**#問題（Question）**<br>棕糖比白糖更健康嗎？ |
        | **C（CoT）**         | **#問題（Question）**<br>棕糖比白糖更健康嗎？<br><br>**#思維鏈（Chain of Thought）**<br>有些人認為棕糖較健康，因為其含有糖蜜（molasses）。然而，糖蜜的含量非常少，因此健康上的差異可以忽略不計。因此，「棕糖比白糖更健康」的說法並不正確。 |
        | **R（RAG）**         | **#相關資訊（Related Information）**<br>棕糖與白糖具有相同的熱量及健康風險。<br><br>**#問題（Question）**<br>棕糖比白糖更健康嗎？ |
        | **S（SSR）**         | **步驟1：理解指令（Understand the Instructions）**<br>確保答案直接回應問題。<br><br>**步驟2：分析問題（Analyze the Question）**<br>仔細閱讀問題。<br>辨識問題中提供的語境資訊。<br><br>**#問題（Question）**<br>棕糖比白糖更健康嗎？|
        | **T（ToT）**         | 有三位不同的專家將回答這個問題。<br>每位專家都會寫下自己的想法並與小組分享。<br>接著，他們將評估彼此的觀點，並推導出最適當的答案。<br><br>**#問題（Question）**<br>棕糖比白糖更健康嗎？ |
        | **S、T、R(C)（組合策略）** | 有三位不同的專家將回答這個問題。<br>每位專家都會寫下自己的想法並與小組分享。<br>接著，他們將評估彼此的觀點，並推導出最適當的答案。<br><br>**#相關資訊（Related Information）**<br>棕糖與白糖具有相同的熱量及健康風險。<br><br>**#問題（Question）**<br>棕糖比白糖更健康嗎？<br><br>**#思維鏈（Chain of Thought）**<br>有些人認為棕糖較健康，因為其含有糖蜜。然而糖蜜含量極低，因此健康差異微乎其微。因此，「棕糖比白糖更健康」的說法並不正確。 |

- 使用 NVIDIA H100 Tensor Core GPU（美國加州聖克拉拉）之運算資源，對預訓練模型 LLama3-8B、Mistral-7B 與 Gemma2-9B 進行提示微調（prompt fine-tuning）

### 結論

- 具體而言
    - 強調**數學與邏輯推理的資料集，CoT、SSR 及 ToT 較具優勢**
    - 側重**自然語言理解的資料集，ICL 較為有效**
    - **對事實準確性要求較高的資料集， RAG 較為有利**
- **最佳提示工程技術組合會因不同 LLM 而異，顯示為達到最佳效能，有必要針對模型與資料集調整提示工程方法**
- 隨著 LLMs 變得更加先進，其對提示工程（PE）技術的依賴程度下降，但在應用提示工程策略時所帶來的效能提升幅度則增加
- **先進模型傾向於較少依賴 ICL 技術，而對 RAG 策略的依賴程度更高**
- **相較於僅對原始資料應用 RAG，結合提示工程的前處理再實施 RAG，可帶來更優異的效能提升**
- 研究顯示，**透過提示工程（PE）策略的前處理來使用 RAG，相較於直接使用原始 R(B)，能顯著提升效能**
- 模型特定的PE策略偏好
    - Llama3：在自然語言理解與推理任務中表現最佳（如 GSM8K、HellaSwag）
    - Gemma2：在多選題、科學與語境推理任務中表現最佳（如 ARC、MMLU）
    - Mistral：在真實性與錯誤檢測資料集中表現最佳（如 TruthfulQA）
- **CoT 是使用最頻繁的提示工程策略，顯示其在推理與問題解決中的通用性**。ToT、ICL 與 SSR 亦廣泛使用，但其效果依資料集而異
- 基礎模型性能決定 PE 依賴性
- **RAG 在結合 CoT 前處理時效果最佳**，而非直接使用原始資料（R(B)）。**PE 驅動的知識重整可顯著提升效果**
- **ICL 依賴反映模型訓練差異**
- **通用資料集偏好 CoT 等通用推理方法；專用資料集則需客製化策略**，例如 R(C) 在事實生成任務中效果更佳

### 後記

- 從範例看來，針對非推理問題，CoT 似乎是直接解答
- 原文並未具體定義「提示工程（PE）策略的前處理」的實作內容，也沒有詳細說明包含哪些操作步驟
- 前處理可能包含：
    - 文本清理（Text Cleaning）
    - 格式標準化（Normalization）
    - 上下文蒐集（Context Collection）
    - 任務識別（Task Identification）
