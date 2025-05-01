# Learning Word Vectors for Sentiment Analysis (RoBERTa-based)

## 📌 專案簡介

本專案旨在透過微調（fine-tuning）預訓練語言模型（如 BERT、RoBERTa）來進行情感分析（Sentiment Analysis）。  
透過結合資料前處理、資料增強（EDA）、自定義模型架構與訓練流程，提升模型在情感分類任務中的表現。

## 📊 資料處理與增強
- 資料清理：將文本轉為小寫，移除 HTML 標籤與特殊符號，壓縮多餘的空白與標點。
- 資料增強（EDA）：使用 nlpaug 套件進行同義詞替換與隨機刪除，增加訓練資料的多樣性。
- 資料切分：將資料切分為訓練集、驗證集與測試集，並確保各類別比例一致。

## 🧠 模型架構
- BERT 與 RoBERTa 分類器：在預訓練模型的基礎上，添加兩層線性層與 ReLU 激活函數，並使用 Dropout 防止過擬合。
- 損失函數：使用交叉熵損失（CrossEntropyLoss）。
- 優化器：使用 Adam 優化器，並搭配學習率調整策略（如 Cosine 或 Linear）。

## 🏋️‍♂️ 訓練與評估
- 訓練流程：每個 epoch 包含訓練與驗證階段，並記錄損失與各項評估指標（準確率、F1 分數、召回率、精確率）。
- 早停（Early Stopping）：若驗證集的表現未提升，則提前停止訓練以防止過擬合。
- 模型儲存：在驗證集表現最佳的 epoch 儲存模型權重。

## 🔍 推論與應用
- 單句推論：使用 predict_one 函數對單一輸入句子進行情感預測，回傳機率分佈與預測標籤。
- 批次推論：使用 predict 函數對整個資料集進行推論，回傳所有樣本的預測結果。

## ⚖️ 模型比較：BERT vs. RoBERTa

| 特性             | BERT                                           | RoBERTa                                             |
|------------------|------------------------------------------------|-----------------------------------------------------|
| 預訓練目標       | Masked LM + Next Sentence Prediction (NSP)     | 僅 Masked LM，移除 NSP                              |
| 訓練語料         | BookCorpus + Wikipedia                         | 更大規模語料（BooksCorpus、CC-News 等）            |
| Tokenizer        | WordPiece，使用 `token_type_ids`               | Byte-Pair Encoding，不使用 `token_type_ids`         |
| 訓練策略         | 固定遮罩模式，較小批次與訓練步數               | 動態遮罩，更大批次與訓練步數                        |
| 效能表現         | 作為基準模型，表現穩定                         | 在多數下游任務中表現優於 BERT                      |

> 📌 本專案的模型架構設計相同，主要差異在於選用的預訓練模型（BERT 或 RoBERTa），以便進行公平的比較與分析。


## 📈 成果與觀察
- 透過資料增強與適當的模型設計，提升了模型在情感分類任務中的表現。
- RoBERTa 在本專案中的表現優於 BERT，特別是在驗證集的 F1 分數與準確率上。