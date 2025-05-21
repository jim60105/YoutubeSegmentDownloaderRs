### Task 3.1: 主介面功能實作

1. **實作影片連結輸入與解析功能**:
   - 建立 `src/components/VideoUrlInput.vue` 元件
   - 當使用者輸入 URL 時即時驗證
   - 實作自動取得影片資訊功能
   - 顯示縮圖和影片基本資訊

2. **實作時間區段設定功能**:
   - 建立 `src/components/TimeRangeSelector.vue` 元件
   - 提供兩種輸入模式：直接輸入時間格式 (HH:MM:SS) 或使用滑桿調整
   - 輸入時即時驗證並格式化
   - 確保結束時間大於開始時間

3. **實作格式選擇功能**:
   - 建立 `src/components/FormatSelector.vue` 元件
   - 提供常用格式的快速選項
   - 支援自訂格式輸入
   - 顯示格式說明文字

4. **實作輸出目錄選擇功能**:
   - 建立 `src/components/DirectorySelector.vue` 元件
   - 使用 Tauri API 實現目錄選擇
   - 顯示已選擇目錄的路徑
   - 驗證目錄可寫入性

5. **實作下載按鈕與取消功能**:
   - 建立 `src/components/DownloadButton.vue` 元件
   - 實作下載狀態管理
   - 加入取消機制與進度顯示
