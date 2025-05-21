### Task 2.2: 影片資訊處理功能

1. **實作 YouTube 影片 ID 解析功能**:
   - 在 `src-tauri/src/utils/url_parser.rs` 建立 URL 解析模組
   - 使用正則表達式從不同格式的 YouTube 連結中提取影片 ID

2. **實作 YouTube 片段連結解析功能**:
   - 解析 YouTube Clip 連結，提取其中的影片 ID、開始時間、結束時間
   - 支援時間戳參數 (t=xxx, start=xxx, end=xxx)

3. **實作影片基本資訊獲取功能**:
   - 建立 `src-tauri/src/models/video_info.rs` 定義資料結構
   - 使用 `yt-dlp` crate 擷取影片資訊
   - 實作 Tauri 命令介面向前端提供資料

4. **實作時間格式處理函式**:
   - 在 `src-tauri/src/utils/time_utils.rs` 中實作時間格式處理
     ```rust
     // HH:MM:SS 格式轉換為秒數
     pub fn hhmmss_to_seconds(time_str: &str) -> Result<f64, String> {
         // 解析邏輯
     }
     
     // 秒數轉換為 HH:MM:SS 格式
     pub fn seconds_to_hhmmss(seconds: f64) -> String {
         // 轉換邏輯
     }
     ```
   - 處理各種時間格式輸入，支援小數點秒數

```csharp
#region Regex

[GeneratedRegex(
   @"https?:\/\/(?:[\w-]+\.)?(?:youtu\.be\/|youtube(?:-nocookie)?\.com\S*[^\w\s-])([\w-]{11})(?=[^\w-]|$)(?![?=&+%\w.-]*(?:['""][^<>]*>|<\/a>))[?=&+%\w.-]*")]
private static partial Regex GetYoutubeId();

[GeneratedRegex(@"^.*[?&]t=([^&smh]*).*$")]
private static partial Regex ExtractYoutubeStartTime();

[GeneratedRegex(@"https?:\/\/(?:[\w-]+\.)?(?:youtu\.be\/|youtube(?:-nocookie)?\.com\/)clip\/[?=&+%\w.-]*")]
private static partial Regex IsYoutubeClipLink();

[GeneratedRegex(@"clipConfig"":{""postId"":""(?:[\w-]+)"",""startTimeMs"":""(\d+)"",""endTimeMs"":""(\d+)""}")]
private static partial Regex ParseYoutubeClipInfo();

[GeneratedRegex(@"{""videoId"":""([\w-]+)""")]
private static partial Regex ParseYoutubeClipVideoId();

#endregion
```
