# CVSS 3.1 計算機

一個基於網頁的CVSS (Common Vulnerability Scoring System) 3.1版本計算工具，支援中文界面，提供直觀的漏洞評分計算功能。

## 功能特色

### 🎯 核心功能
- **向量字串輸入**：支援直接輸入CVSS向量字串進行計算
- **下拉選單操作**：提供直觀的圖形化界面選擇各項指標
- **即時計算**：輸入時即時顯示計算結果
- **多語言支援**：完整的中英文對照界面

### 📊 計算指標
- **基礎指標 (Base Metrics)**
  - AV (Attack Vector) - 攻擊途徑
  - AC (Attack Complexity) - 攻擊複雜度  
  - PR (Privileges Required) - 所需權限
  - UI (User Interaction) - 使用者互動
  - S (Scope) - 影響範圍
  - C (Confidentiality) - 機密性影響
  - I (Integrity) - 完整性影響
  - A (Availability) - 可用性影響

- **時間指標 (Temporal Metrics)**
  - E (Exploit Code Maturity) - 攻擊程式成熟度
  - RL (Remediation Level) - 修補狀態
  - RC (Report Confidence) - 報告可信度

### 🎨 評分等級
- **Critical (嚴重)**: 9.0-10.0
- **High (高)**: 7.0-8.9
- **Medium (中)**: 4.0-6.9
- **Low (低)**: 0.0-3.9

## 使用方法

### 方法一：向量字串輸入
1. 在「直接輸入向量」欄位中輸入完整的CVSS向量字串
2. 範例：`CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H/E:U/RL:O/RC:C`
3. 系統會即時驗證並計算結果

### 方法二：下拉選單操作
1. 使用各個下拉選單選擇對應的指標值
2. 點擊「計算」按鈕獲得結果
3. 系統會自動生成對應的向量字串

## 技術規格

- **前端技術**：純HTML5 + CSS3 + JavaScript (ES6+)
- **瀏覽器支援**：現代瀏覽器 (Chrome, Firefox, Safari, Edge)
- **響應式設計**：支援桌面和行動裝置
- **無需後端**：完全基於客戶端運行

## 計算公式

### 基礎分數計算
```
Exploitability = 8.22 × AV × AC × PR × UI
ISC_Base = 1 - ((1-C) × (1-I) × (1-A))

Impact = 6.42 × ISC_Base (當S=U時)
Impact = 7.52 × (ISC_Base-0.029) - 3.25 × (ISC_Base-0.02)^15 (當S=C時)

Base Score = min(Impact + Exploitability, 10) (當S=U時)
Base Score = min(1.08 × (Impact + Exploitability), 10) (當S=C時)
```

### 時間分數計算
```
Temporal Score = Base Score × E × RL × RC
```

## 檔案結構

```
CVSS3.1計算機/
├── CVSS3.1.html          # 主要應用檔案
├── README.md             # 專案說明文檔
└── docs/                 # 技術文檔目錄
    ├── API.md            # API參考文檔
    └── CHANGELOG.md      # 版本更新記錄
```

## 開發計劃

### 已實現功能 ✅
- [x] 基礎CVSS 3.1計算功能
- [x] 向量字串解析和驗證
- [x] 圖形化界面操作
- [x] 中英文對照界面
- [x] 即時計算和結果顯示

### 計劃中功能 🚧
- [ ] 歷史記錄功能
- [ ] 結果匯出 (JSON/CSV)
- [ ] 批量計算功能
- [ ] 更多視覺化圖表
- [ ] 離線使用支援
- [ ] 自定義評分規則

## 貢獻指南

歡迎提交Issue和Pull Request來改善這個專案！

### 開發環境設置
1. 克隆專案到本地
2. 使用現代瀏覽器開啟 `CVSS3.1.html`
3. 開始開發和測試

### 提交規範
- 使用清晰的commit訊息
- 確保代碼符合現有風格
- 添加適當的註解和文檔

## 授權條款

本專案採用 MIT 授權條款，詳見 LICENSE 檔案。

## 聯絡資訊

如有問題或建議，請透過以下方式聯絡：
- 提交 GitHub Issue
- 發送電子郵件至專案維護者

---

**注意**：本工具僅供參考，實際的漏洞評分應結合專業的安全評估和業務環境進行綜合判斷。
