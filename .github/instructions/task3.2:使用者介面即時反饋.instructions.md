### Task 3.2: 使用者介面即時反饋

1. **實作下載進度顯示功能**:
   - 建立 `src/components/ProgressBar.vue` 元件
   - 監聽後端進度事件
   - 顯示目前階段 (下載中、處理中)
   - 顯示百分比和預計剩餘時間

2. **實作日誌即時更新功能**:
   - 建立 Rust 端的日誌管理系統，使用 `tracing` crate
   - 將關鍵事件透過 Tauri 事件傳送至前端
   - 在前端即時顯示接收到的日誌
   - 實作詳細日誌選項切換功能

3. **實作依賴檢查狀態顯示**:
   - 建立 `src/components/DependencyStatus.vue` 元件
   - 顯示 yt-dlp 和 ffmpeg 的狀態圖示
   - 提供重新下載按鈕
   - 顯示版本資訊

4. **實作錯誤訊息顯示功能**:
   - 建立 `src/components/ErrorDisplay.vue` 元件
   - 使用 Vue 的 `teleport` 功能建立彈出式訊息
   - 分類顯示不同等級的錯誤
   - 提供問題解決建議
