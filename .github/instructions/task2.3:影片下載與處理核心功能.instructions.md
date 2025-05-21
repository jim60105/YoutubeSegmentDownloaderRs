### Task 2.3: 影片下載與處理核心功能

1. **實作影片下載功能**:
   - 建立 `src-tauri/src/downloader.rs` 模組
   - 使用 `yt-dlp` crate 實作下載功能，參照: https://github.com/boul2gom/yt-dlp/blob/develop/README.md
   - 實作進度回報機制，透過事件通知前端

2. **實作影片分段處理功能**:
   - 使用 `ffmpeg-sidecar` 處理影片分段，參照: https://docs.rs/ffmpeg-sidecar/2.0.5/ffmpeg_sidecar
   - 確保精確的時間切割
   - 處理不同影片格式和編碼

4. **實作輸出檔案名稱生成邏輯**:
   - 建立檔案名稱生成函數，包含影片標題、上傳日期、時間段資訊、自訂格式化選項
   - 處理檔案名稱中的非法字元

5. **實作臨時檔案管理功能**:
   - 建立臨時檔案管理模組
   - 確保程式結束時自動清理臨時檔案
   - 處理 OS 權限問題和檔案鎖定情況
