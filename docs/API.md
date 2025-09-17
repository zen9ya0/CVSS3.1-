# CVSS 3.1 計算機 API 文檔

## 概述

本文檔描述了CVSS 3.1計算機的JavaScript API接口，包括所有可用的函數、參數和返回值。

## 核心函數

### 1. 向量解析函數

#### `parseVector(vectorString)`
解析CVSS向量字串為物件格式。

**參數：**
- `vectorString` (string): CVSS向量字串，格式如 `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H/E:U/RL:O/RC:C`

**返回值：**
- `Object`: 包含所有向量指標的物件

**範例：**
```javascript
const vector = "CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H/E:U/RL:O/RC:C";
const parsed = parseVector(vector);
// 返回: { AV: "N", AC: "L", PR: "N", UI: "N", S: "U", C: "N", I: "N", A: "H", E: "U", RL: "O", RC: "C" }
```

### 2. 向量驗證函數

#### `validateVector(vectorString)`
驗證CVSS向量字串的格式和內容。

**參數：**
- `vectorString` (string): 要驗證的CVSS向量字串

**返回值：**
- `Object`: 驗證結果物件
  - `valid` (boolean): 是否有效
  - `error` (string): 錯誤訊息（如果無效）

**範例：**
```javascript
const result = validateVector("CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H");
if (result.valid) {
  console.log("向量有效");
} else {
  console.log("錯誤:", result.error);
}
```

### 3. CVSS計算函數

#### `calcCVSS(vectorObject)`
計算CVSS分數。

**參數：**
- `vectorObject` (Object): 包含所有必要向量指標的物件

**返回值：**
- `Object`: 計算結果物件
  - `BaseScore` (number): 基礎分數
  - `Temporal` (number): 時間分數
  - `level` (string): 嚴重程度等級 (Critical/High/Medium/Low)
  - `Exploitability` (number): 可利用性分數
  - `Impact` (number): 影響分數
  - `ISC_Base` (number): 基礎影響分數

**範例：**
```javascript
const vector = { AV: "N", AC: "L", PR: "N", UI: "N", S: "U", C: "N", I: "N", A: "H", E: "U", RL: "O", RC: "C" };
const result = calcCVSS(vector);
console.log(`Base Score: ${result.BaseScore}, Level: ${result.level}`);
```

### 4. 歷史記錄函數

#### `saveToHistory(vectorObject, resultObject)`
將計算結果保存到歷史記錄。

**參數：**
- `vectorObject` (Object): 向量物件
- `resultObject` (Object): 計算結果物件

**返回值：**
- `void`

#### `updateHistoryDisplay()`
更新歷史記錄顯示。

**參數：**
- 無

**返回值：**
- `void`

#### `clearHistory()`
清除所有歷史記錄。

**參數：**
- 無

**返回值：**
- `void`

### 5. 工具函數

#### `roundup(number)`
將數字向上取整到小數點後一位。

**參數：**
- `number` (number): 要取整的數字

**返回值：**
- `number`: 取整後的數字

#### `copyToClipboard(buttonElement)`
複製向量字串到剪貼簿。

**參數：**
- `buttonElement` (HTMLElement): 包含data-vector屬性的按鈕元素

**返回值：**
- `void`

## 資料結構

### 向量指標資料 (metrics)

```javascript
const metrics = {
  AV: { N: [0.85, "Network", "網路"], A: [0.62, "Adjacent Network", "相鄰網路"], L: [0.55, "Local", "本地"], P: [0.2, "Physical", "實體"] },
  AC: { L: [0.77, "Low", "低"], H: [0.44, "High", "高"] },
  PRu: { N: [0.85, "None", "無"], L: [0.62, "Low", "低"], H: [0.27, "High", "高"] },
  PRc: { N: [0.85, "None", "無"], L: [0.68, "Low", "低"], H: [0.5, "High", "高"] },
  UI: { N: [0.85, "None", "無"], R: [0.62, "Required", "需要"] },
  CIA: { N: [0.0, "None", "無"], L: [0.22, "Low", "低"], H: [0.56, "High", "高"] },
  E: { X: [1.0, "Not Defined", "未定義"], U: [0.91, "Unproven", "未證實"], P: [0.94, "Proof-of-Concept", "概念驗證"], F: [0.97, "Functional", "可行攻擊程式"], H: [1.0, "High", "已廣泛濫用"] },
  RL: { X: [1.0, "Not Defined", "未定義"], O: [0.95, "Official Fix", "官方修補"], T: [0.96, "Temporary Fix", "暫時修補"], W: [0.97, "Workaround", "替代方案"], U: [1.0, "Unavailable", "無修補"] },
  RC: { X: [1.0, "Not Defined", "未定義"], U: [0.92, "Unknown", "未知"], R: [0.96, "Reasonable", "可信"], C: [1.0, "Confirmed", "已確認"] }
};
```

### 歷史記錄項目結構

```javascript
{
  id: number,           // 時間戳記ID
  timestamp: string,    // ISO格式時間戳記
  vector: Object,       // 向量物件
  result: Object,       // 計算結果物件
  vectorString: string  // 完整向量字串
}
```

## 事件處理

### 輸入事件
- `vector` 輸入框的 `input` 事件會觸發 `calcFromVector()` 函數
- 下拉選單變更會觸發 `calcFromSelectors()` 函數

### 頁面載入事件
- `DOMContentLoaded` 事件會初始化速查表和歷史記錄顯示

## 本地儲存

歷史記錄使用 `localStorage` 儲存，鍵名為 `cvssHistory`。資料以JSON格式儲存，最多保留50筆記錄。

## 錯誤處理

所有函數都包含適當的錯誤處理機制：
- 向量驗證會返回詳細的錯誤訊息
- 複製功能會顯示成功或失敗狀態
- 歷史記錄操作包含確認對話框

## 瀏覽器相容性

- 支援現代瀏覽器 (Chrome 60+, Firefox 55+, Safari 12+, Edge 79+)
- 需要支援 ES6+ 語法
- 需要支援 `localStorage` API
- 需要支援 `navigator.clipboard` API
