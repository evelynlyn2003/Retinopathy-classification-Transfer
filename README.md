# 糖尿病視網膜病變影像分類

### A. 數據 (Dataset & DataLoader)

#### 1. 資料來源
Kaggle「Diabetic Retinopathy Classification 3」  
https://www.kaggle.com/competitions/diabetic-retinopathy-classification-3

#### 2. 資料規模
- 訓練 + 驗證集：2,197 張  
- 測試集：1,465 張  
- 分類標籤：0～4 等級（五類）

#### 3. 影像增強（Data Augmentation）
使用 `torchvision.transforms`：
- 水平翻轉（Horizontal Flip）  
- 垂直翻轉（Vertical Flip）  
- 隨機仿射變換（旋轉、縮放、剪切）

---

### B. 模型 (Model)

#### 1. 使用深度遷移學習 (Deep Transfer Learning)
主架構採用 **ResNet50**，並載入 ImageNet 預訓練權重。

#### 2. 殘差網路 (Residual Blocks)
Residual Blocks 有效解決深層網路的梯度消失問題，使訓練更穩定。

#### 3. 凍結部分層（Freeze Layers）
將基礎模型底層（例如 layer1）凍結，降低小型資料集上的過度擬合風險。

---

### C. 訓練與優化 (Training)

#### 1. 損失函數
`CrossEntropyLoss`

#### 2. 優化器
Adam（初始學習率：`1e-5`）

#### 3. 其他訓練細節（可選）
- 使用 GPU（`.to(device)`）進行訓練  
- batch-wise 訓練搭配 DataLoader  
- 訓練/驗證時分別使用 `model.train()` / `model.eval()`  
- 測試時使用 `torch.no_grad()` 以節省記憶體

---

### D. 模型結果與效能評估 (Results)

#### 1. Kaggle Public Score
**0.79863**

#### 2. AUC Score（One-vs-Rest）
**0.9347**

#### 3. 敏感度（Sensitivity - Weighted）
**80.91%**
