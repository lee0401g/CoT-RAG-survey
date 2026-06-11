# [The Impact of Prompting Techniques on the Security of the LLMs and the Systems to Which They Belong](https://www.mdpi.com/2076-3417/14/19/8711)

## 摘要

> 研究最受歡迎的提示技術對大型語言模型，以及間接對其所屬系統之安全性的影響。透過迄今部署最廣泛的三種 GPT 模型，我們在不同設定情境下執行了一些最常見的攻擊，並展示提示技術可能對 LLM 的安全性產生負面影響

## 盲區

- 

## 研究動機與背景

- 嘗試建立提示技術特性與潛在漏洞之間的因果關係
- 根據攻擊者意圖與攻擊結果，清楚區分提示注入與越獄的定義
- OWASP 識別基於 LLM 應用程式的主要問題：
    - Prompt Injection（提示注入）
    - Insecure Output Handling（不安全輸出處理）
    - Training Data Poisoning（訓練資料汙染）
    - Model Denial of Service（模型阻斷服務）
    - Supply Chain Vulnerabilities（供應鏈漏洞）
    - Sensitive Information Disclosure（敏感資訊洩漏）
    - Insecure Plugin Design（不安全外掛設計）
    - Excessive Agency（過度自主性）
    - Overreliance（過度依賴）
    - Model Theft（模型竊取）
- 語言模型的功能建立在以下原則之上：序列中下一個詞彙出現的機率僅取決於特定數量的前置詞彙，這被稱為上下文（context），利用 N-grams 、Hidden Markov Models（隱馬可夫模型）或神經網路等方法識別文本語料中詞彙之間的模式與關聯
- LLM 並不具備任何形式的內部記憶，因此它們高度依賴所接收的上下文
- **要調整模型上下文以正確完成任務，還必須設法確保模型的行為不會被外部人士修改或操控，使其服務於與原始目的不同的用途**
- 
- ?

## 重點

- 

##  結論

- 

## 後記

- `...兩者的差異在於 LLM 的底層模型是一種採用 Transformer 架構的人工神經網路。Transformer 架構源自於《Attention Is All You Need》[12] 一文，本文不進一步介紹，但建議讀者參閱...`???
- ?
