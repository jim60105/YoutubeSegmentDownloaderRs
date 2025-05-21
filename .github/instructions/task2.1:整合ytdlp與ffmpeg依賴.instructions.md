### Task 2.1: 整合 yt-dlp 與 ffmpeg 依賴

1. **實作依賴套件檢查機制**:
   - 在 Rust 中建立 `src-tauri/src/dependencies.rs` 模組
   - 實作檢查系統中是否已安裝 yt-dlp 和 ffmpeg
   - 建立函數檢查檔案是否存在且可執行
   - 透過 IPC 向前端回報依賴狀態

2. **實作自動下載 yt-dlp 及 ffmpeg 功能**:
   ```rust
    use yt_dlp::Youtube;
    use std::path::PathBuf;

    #[tokio::main]
    pub async fn main() -> Result<(), Box<dyn std::error::Error>> {
        let executables_dir = PathBuf::from("libs");
        let output_dir = PathBuf::from("output");

        let fetcher = Youtube::with_new_binaries(executables_dir, output_dir).await?;
        Ok(())
    }
    ```
    - 使用 `yt-dlp` crate 下載 yt-dlp 和 ffmpeg
    - 確保下載的檔案存放在指定的目錄中
    - 實作進度回報機制，透過事件通知前端目前下載狀態

3. **實作重新下載依賴功能**:
   - 建立 Tauri 命令接口
   - 在前端建立對應呼叫按鈕
   - 實作進度回報機制，透過事件通知前端目前下載狀態

4. **整合 `yt-dlp` crate 與 `ffmpeg-sidecar` crate**:
   - 在 `Cargo.toml` 中添加相依套件: 
     ```toml
     [dependencies]
     yt-dlp = "1.3.4"
     ffmpeg-sidecar = "2.0.5"
     ```
