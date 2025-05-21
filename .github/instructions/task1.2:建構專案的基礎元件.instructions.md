### Task 1.2: 建構專案的基礎元件

1. **建立主視窗元件**:
   - 在 `src/App.vue` 中建立應用程式基本架構
   - 設計頂部標題欄，包含應用程式名稱和基本功能按鈕
   - 建立 `src/components/MainWindow.vue` 作為主要內容容器

2. **實作基本 Layout 佈局**:
   - 使用 Tailwind CSS 的 flex 或 grid 系統建立基本佈局
   - 分割畫面為幾個主要區域：
     - 頂部：標題和選項
     - 左側：輸入表單區域
     - 右側：日誌顯示區域或預覽區
     - 底部：狀態和控制按鈕
   - 示意圖: ![Layout](/YoutubeSegmentDownloader.NET/assets/preview.png)

3. **設計輸入表單元件**:
   - 建立 `src/components/InputForm.vue`
   - 實作以下表單元素:
     - 影片連結輸入框 (`TextInput` 元件)
     - 時間區段開始和結束輸入框 (`TimeInput` 元件)
     - 輸出目錄選擇元件 (`DirectoryPicker` 元件)
     - 格式選擇下拉選單 (`FormatSelector` 元件)
     - 瀏覽器選擇下拉選單 (`BrowserSelector` 元件)
   - 為每個輸入元件加入適當的驗證規則
   - 實作資料雙向綁定並連接到 Pinia store

4. **實作日誌顯示區域元件**:
   - 建立 `src/components/LogDisplay.vue`
   - 加入自動滾動功能
   - 實作日誌等級顯示 (資訊、警告、錯誤)
   - 加入日誌清除按鈕
   - 加入詳細日誌切換選項
   - 使用 Pinia store 管理日誌資料流
