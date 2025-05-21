# YoutubeSegmentDownloaderRs 專案轉換詳細實作計畫

以下是將 YoutubeSegmentDownloader.NET 轉換為 Tauri + Rust 專案的詳細實作計畫，按照實作順序和領域進行分類。此計畫包含每個任務的詳細步驟和技術考量，以確保實作過程順利且完整。

> [!IMPORTANT]  
> 1. 當你有疑問時，參考 [YoutubeSegmentDownloader.NET](/YoutubeSegmentDownloader.NET) 的原始碼。
> 2. 永遠不要修改 [YoutubeSegmentDownloader.NET](/YoutubeSegmentDownloader.NET) 的原始碼，它是參考資料，不能更動。永遠不要修改它。

## 1. 專案初始化與基礎架構建立

### Task 1.1: 初始建立 Tauri + Vue + Tailwind 專案
1. **安裝必要的開發工具**:
   - 確保已安裝 Node.js (v18+) 和 npm
   - 安裝系統相依套件 (Linux 需要安裝 `libwebkit2gtk-4.0-dev`, `build-essential` 等)

2. **建立新專案**:
   - 執行 `npm create tauri-app@latest`，選擇 Vue 作為前端框架
   - 專案命名為 `YoutubeSegmentDownloaderRs`
   - 進入專案目錄 `cd YoutubeSegmentDownloaderRs`

3. **整合 Vue.js**:
   - 確認 Vue.js 3 已正確安裝
   - 設定 Vue Router 用於頁面導航: `npm install vue-router@4`
   - 設定 Pinia 用於狀態管理: `npm install pinia`

4. **設定 Tailwind CSS**:
   - 安裝 Tailwind CSS: `npm install -D tailwindcss postcss autoprefixer`
   - 初始化 Tailwind: `npx tailwindcss init -p`
   - 配置 `tailwind.config.js`，設定內容路徑
   ```js
   /** @type {import('tailwindcss').Config} */
   export default {
     content: [
       "./index.html",
       "./src/**/*.{vue,js,ts,jsx,tsx}",
     ],
     theme: {
       extend: {},
     },
     plugins: [],
   }
   ```
   - 在主要 CSS 文件中加入 Tailwind 指令:
   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

5. **建立專案目錄結構**:
   - `src/`: 前端 Vue 程式碼
     - `components/`: 元件目錄
     - `views/`: 頁面視圖
     - `stores/`: Pinia 狀態存放位置
     - `assets/`: 靜態資源
     - `i18n/`: 國際化資源
   - `src-tauri/`: Rust 後端程式碼
     - `src/`: Rust 原始碼
       - `main.rs`: 主程式進入點
       - `commands/`: Tauri 命令模組
       - `utils/`: 工具函數
       - `models/`: 資料模型
     - `Cargo.toml`: Rust 專案配置

6. **配置基本檔案**:
   - 修改 `src-tauri/tauri.conf.json` 設定應用程式基本資訊
   - 設定視窗大小、標題和權限
   - 設定在開發模式下啟用 DevTools

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
   示意圖: ![Layout](/YoutubeSegmentDownloader.NET/assets/preview.png)

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

## 2. 後端核心功能實作

### Task 2.1: 整合 yt-dlp 與 ffmpeg 依賴

1. **實作依賴套件檢查機制**:
   - 在 Rust 中建立 `src-tauri/src/dependencies.rs` 模組
   - 實作檢查系統中是否已安裝 yt-dlp 和 ffmpeg:
     ```rust
     pub fn check_dependencies() -> (Option<String>, Option<String>) {
         let ytdlp_path = find_executable("yt-dlp");
         let ffmpeg_path = find_executable("ffmpeg");
         
         (ytdlp_path, ffmpeg_path)
     }
     ```
   - 建立函數檢查檔案是否存在且可執行
   - 透過 IPC 向前端回報依賴狀態

2. **實作自動下載 yt-dlp 及 ffmpeg 功能**:
   - 添加 `reqwest` crate 用於下載功能
   - 在 Rust 中實作下載函數，支援從官方來源取得最新版本:
     ```rust
     pub async fn download_ytdlp(target_dir: &Path) -> Result<String, Error> {
         // 下載實作
     }
     
     pub async fn download_ffmpeg(target_dir: &Path) -> Result<String, Error> {
         // 下載實作
     }
     ```
   - 實作平台特定操作 (Windows/Linux/macOS)
   - 對於 ffmpeg，需處理解壓縮功能 (使用 `zip` 或 `tar` crate)

3. **實作重新下載依賴功能**:
   - 建立 Tauri 命令接口:
     ```rust
     #[tauri::command]
     pub async fn update_dependencies() -> Result<bool, String> {
         // 實作強制重新下載邏輯
     }
     ```
   - 在前端建立對應呼叫按鈕
   - 實作進度回報機制，透過事件通知前端目前下載狀態

4. **整合 `yt-dlp` crate 與 `ffmpeg-sidecar` crate**:
   - 在 `Cargo.toml` 中添加相依套件:
     ```toml
     [dependencies]
     yt-dlp = "0.3"  # 或更新版本
     ffmpeg-sidecar = "0.5"  # 或更新版本
     ```
   - 建立包裝器模組整合這些套件的功能
   - 處理路徑管理和執行權限問題

### Task 2.2: 影片資訊處理功能

1. **實作 YouTube 影片 ID 解析功能**:
   - 在 `src-tauri/src/utils/url_parser.rs` 建立 URL 解析模組
   - 使用正則表達式從不同格式的 YouTube 連結中提取影片 ID:
     ```rust
     pub fn extract_youtube_id(url: &str) -> Option<String> {
         // 使用正則表達式匹配各種 YouTube URL 格式並提取 ID
     }
     ```
   - 支援標準 YouTube 連結、短網址 (youtu.be)、嵌入連結等

2. **實作 YouTube 片段連結解析功能**:
   - 解析 YouTube Clip 連結，提取其中的:
     - 影片 ID
     - 開始時間
     - 結束時間
   - 支援時間戳參數 (t=xxx, start=xxx, end=xxx)

3. **實作影片基本資訊獲取功能**:
   - 建立 `src-tauri/src/models/video_info.rs` 定義資料結構:
     ```rust
     pub struct VideoInfo {
         pub id: String,
         pub title: String,
         pub upload_date: Option<String>,
         pub duration: f64,
         pub thumbnail_url: String,
         // 其他欄位
     }
     ```
   - 使用 `yt-dlp` crate 擷取影片資訊:
     ```rust
     pub async fn fetch_video_info(url: &str) -> Result<VideoInfo, String> {
         // 使用 yt-dlp 擷取影片基本資訊
     }
     ```
   - 實作 Tauri 命令介面向前端提供資料

4. **實作時間格式處理函式**:
   - 在 `src-tauri/src/utils/time_utils.rs` 中實作時間格式處理:
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

### Task 2.3: 影片下載與處理核心功能

1. **實作影片下載功能**:
   - 建立 `src-tauri/src/downloader.rs` 模組
   - 使用 `yt-dlp` crate 實作下載功能:
     ```rust
     pub async fn download_video(
         url: &str,
         format: &str,
         output_path: &Path,
         progress_callback: impl Fn(f32),
     ) -> Result<String, String> {
         // 下載實作
     }
     ```
   - 實作進度回報機制，透過事件通知前端

2. **實作影片分段處理功能**:
   - 使用 `ffmpeg-sidecar` 處理影片分段:
     ```rust
     pub async fn cut_video(
         input_path: &Path,
         output_path: &Path,
         start: f64,
         end: f64,
     ) -> Result<(), String> {
         // 截取影片邏輯
     }
     ```
   - 確保精確的時間切割
   - 處理不同影片格式和編碼

3. **實作檔案格式轉換功能**:
   - 實作 ffmpeg 轉換命令:
     ```rust
     pub async fn convert_video(
         input_path: &Path,
         output_path: &Path,
         format: &str,
     ) -> Result<(), String> {
         // 格式轉換邏輯
     }
     ```
   - 支援不同影片編碼選項 (H.264, HEVC 等)
   - 實作音訊處理選項

4. **實作輸出檔案名稱生成邏輯**:
   - 建立檔案名稱生成函數，包含:
     - 影片標題
     - 上傳日期
     - 時間段資訊
     - 自訂格式化選項
   - 處理檔案名稱中的非法字元

5. **實作臨時檔案管理功能**:
   - 建立臨時檔案管理模組:
     ```rust
     pub struct TempFileManager {
         files: Vec<PathBuf>,
     }
     
     impl TempFileManager {
         pub fn new() -> Self { /* 實作 */ }
         pub fn create_temp_file(&mut self, ext: &str) -> PathBuf { /* 實作 */ }
         pub fn cleanup(&self) -> Result<(), std::io::Error> { /* 實作 */ }
     }
     ```
   - 確保程式結束時自動清理臨時檔案
   - 處理 OS 權限問題和檔案鎖定情況

## 3. 前端 UI 與使用者體驗

### Task 3.1: 主介面功能實作

1. **實作影片連結輸入與解析功能**:
   - 建立 `src/components/VideoUrlInput.vue` 元件
   - 當使用者輸入 URL 時即時驗證
   - 實作自動取得影片資訊功能:
     ```javascript
     async function handleUrlInput(url) {
       if (!url || url.trim() === "") return;
       
       try {
         loading.value = true;
         const videoInfo = await invoke("get_video_info", { url });
         // 更新相關資訊
       } catch (error) {
         // 處理錯誤
       } finally {
         loading.value = false;
       }
     }
     ```
   - 顯示縮圖和影片基本資訊

2. **實作時間區段設定功能**:
   - 建立 `src/components/TimeRangeSelector.vue` 元件
   - 提供兩種輸入模式:
     - 直接輸入時間格式 (HH:MM:SS)
     - 使用滑桿調整
   - 輸入時即時驗證並格式化
   - 確保結束時間大於開始時間

3. **實作格式選擇功能**:
   - 建立 `src/components/FormatSelector.vue` 元件
   - 提供常用格式的快速選項
   - 支援自訂格式輸入
   - 顯示格式說明文字

4. **實作輸出目錄選擇功能**:
   - 建立 `src/components/DirectorySelector.vue` 元件
   - 使用 Tauri API 實現目錄選擇:
     ```javascript
     async function openDirectoryPicker() {
       const selected = await invoke("open_directory_dialog");
       if (selected) {
         outputDirectory.value = selected;
       }
     }
     ```
   - 顯示已選擇目錄的路徑
   - 驗證目錄可寫入性

5. **實作下載按鈕與取消功能**:
   - 建立 `src/components/DownloadButton.vue` 元件
   - 實作下載狀態管理:
     ```javascript
     const downloading = ref(false);
     
     async function startDownload() {
       downloading.value = true;
       try {
         await invoke("download_video_segment", {
           url: videoUrl.value,
           start: startTime.value,
           end: endTime.value,
           format: format.value,
           outputDir: outputDir.value
         });
       } catch (error) {
         // 處理錯誤
       } finally {
         downloading.value = false;
       }
     }
     
     function cancelDownload() {
       invoke("cancel_download");
     }
     ```
   - 加入取消機制與進度顯示

### Task 3.2: 使用者介面即時反饋

1. **實作下載進度顯示功能**:
   - 建立 `src/components/ProgressBar.vue` 元件
   - 監聽後端進度事件:
     ```javascript
     onMounted(() => {
       const unlisten = listen("download-progress", (event) => {
         progress.value = event.payload.percentage;
         stage.value = event.payload.stage;
       });
       
       onUnmounted(() => {
         unlisten();
       });
     });
     ```
   - 顯示目前階段 (下載中、處理中)
   - 顯示百分比和預計剩餘時間

2. **實作日誌即時更新功能**:
   - 建立 Rust 端的日誌管理系統，使用 `tracing` crate
   - 將關鍵事件透過 Tauri 事件傳送至前端:
     ```rust
     fn emit_log(level: &str, message: &str, window: Window) {
         let _ = window.emit("log-event", LogMessage {
             level,
             message: message.to_string(),
             timestamp: chrono::Local::now().to_rfc3339(),
         });
     }
     ```
   - 在前端即時顯示接收到的日誌
   - 實作詳細日誌選項切換功能

3. **實作依賴檢查狀態顯示**:
   - 建立 `src/components/DependencyStatus.vue` 元件
   - 顯示 yt-dlp 和 ffmpeg 的狀態圖示
   - 提供重新下載按鈕
   - 顯示版本資訊

4. **實作錯誤訊息顯示功能**:
   - 建立 `src/components/ErrorDisplay.vue` 元件
   - 使用 Vue 的 `teleport` 功能建立彈出式訊息:
     ```html
     <teleport to="body">
       <div v-if="error" class="error-modal">
         <h3>錯誤</h3>
         <p>{{ error.message }}</p>
         <button @click="dismissError">關閉</button>
       </div>
     </teleport>
     ```
   - 分類顯示不同等級的錯誤
   - 提供問題解決建議

### Task 3.3: 進階使用者體驗優化

1. **實作下載完成後自動開啟資料夾功能**:
   - 建立設定選項
   - 使用 Tauri API 開啟檔案資料夾:
     ```javascript
     async function openContainingFolder(path) {
       await invoke("open_folder", { path });
     }
     ```
   - 加入設定開關

2. **實作下載完成通知功能**:
   - 使用 Tauri 通知 API:
     ```javascript
     function showNotification(title, body) {
       invoke("show_notification", { title, body });
     }
     ```
   - 在下載完成時顯示通知
   - 可點擊通知開啟檔案

3. **實作 UI 狀態管理**:
   - 建立 Pinia store 管理全域 UI 狀態:
     ```javascript
     export const useAppStore = defineStore('app', () => {
       const isDownloading = ref(false);
       const isBusy = ref(false);
       
       function disableInterface() {
         isBusy.value = true;
       }
       
       function enableInterface() {
         isBusy.value = false;
       }
       
       return { isDownloading, isBusy, disableInterface, enableInterface };
     });
     ```
   - 在特定操作期間禁用相關元件
   - 實作統一的載入指示器

4. **優化輸入驗證與即時回饋**:
   - 實作 URL 格式即時驗證
   - 實作時間格式即時驗證
   - 加入輸入提示文字
   - 透過視覺效果強調無效輸入

## 4. 設定管理與持久化

### Task 4.1: 使用者設定實作

1. **實作設定檔儲存與讀取功能**:
   - 使用 Tauri 的 Store 管理設定 https://v2.tauri.app/plugin/store/
   - 安裝 Tauri Store 插件: `cargo tauri add store`
   - 範本程式碼:
     ```javascript
     import { load } from '@tauri-apps/plugin-store';
     // when using `"withGlobalTauri": true`, you may use
     // const { load } = window.__TAURI__.store;

     // Create a new store or load the existing one,
     // note that the options will be ignored if a `Store` with that path has already been created
     const store = await load('store.json', { autoSave: false });

     // Set a value.
     await store.set('some-key', { value: 5 });

     // Get a value.
     const val = await store.get<{ value: number }>('some-key');
     console.log(val); // { value: 5 }

     // You can manually save the store after making changes.
     // Otherwise, it will save upon graceful exit
     // And if you set `autoSave` to a number or left empty,
     // it will save the changes to disk after a debounce delay, 100ms by default.
     await store.save();
     ```
   - 設計設定結構和預設值
   - 實作設定驗證和遷移機制

2. **實作輸出目錄記憶功能**:
   - 將使用者上次選擇的目錄保存至設定檔
   - 下次啟動時自動載入
   - 檢查目錄是否仍存在

3. **實作格式設定記憶功能**:
   - 將使用者選擇的影片格式保存至設定檔
   - 提供預設格式選項
   - 儲存常用自訂格式

4. **實作詳細日誌選項記憶功能**:
   - 保存日誌顯示等級設定
   - 保存日誌視窗大小和位置
   - 實作日誌篩選器設定

## 5. 國際化支援

### Task 5.1: i18n 基礎架構

1. **設定 Vue i18n 框架**:
   - 安裝 Vue I18n: `npm install vue-i18n`
   - 在 `src/main.js` 中設定:
     ```javascript
     import { createI18n } from 'vue-i18n';
     
     const i18n = createI18n({
       legacy: false,
       locale: 'zh-TW',
       fallbackLocale: 'en',
       messages: {}
     });
     
     app.use(i18n);
     ```
   - 建立語言偵測機制

2. **建立語言資源檔結構**:
   - 建立 `src/i18n` 目錄
   - 建立語言檔結構:
     - `src/i18n/en.json`: 英文資源
     - `src/i18n/zh-TW.json`: 繁體中文資源
   - 設計鍵值命名規則

3. **實作語言切換功能**:
   - 建立 `src/components/LanguageSelector.vue` 元件
   - 實作切換邏輯:
     ```javascript
     function changeLanguage(locale) {
       i18n.global.locale.value = locale;
       localStorage.setItem('language', locale);
     }
     ```
   - 使用國旗或語言名稱作為選項
   - 記憶使用者選擇

### Task 5.2: 多語言資源實作

1. **實作英文語言資源**:
   - 建立完整的英文介面文字資源
   - 確保所有 UI 元素都有對應的翻譯
   - 加入錯誤訊息的翻譯

2. **實作中文語言資源**:
   - 建立完整的中文介面文字資源
   - 確保專業術語翻譯準確
   - 本地化時間和數字格式

3. **確保所有 UI 元素都支援多語言**:
   - 修改所有元件使用 i18n:
     ```html
     <button>{{ $t('download.start') }}</button>
     ```
   - 處理動態內容的翻譯
   - 確保日期和時間格式本地化

## 6. 測試與品質保證

### Task 6.1: 單元測試

1. **為 Rust 後端功能編寫單元測試**:
   - 使用 Rust 的標準測試框架
   - 為每個關鍵功能編寫測試:
     ```rust
     #[cfg(test)]
     mod tests {
         use super::*;
         
         #[test]
         fn test_extract_youtube_id() {
             assert_eq!(extract_youtube_id("https://www.youtube.com/watch?v=dQw4w9WgXcQ"), Some("dQw4w9WgXcQ".to_string()));
             // 更多測試案例
         }
     }
     ```
   - 使用模擬物件測試網路和檔案系統操作

## 7. 打包與發佈

### Task 7.1: 應用程式打包配置

1. **配置 Tauri 打包設定**:
   - 修改 `src-tauri/tauri.conf.json`:
     ```json
     {
       "build": {
         "beforeBuildCommand": "npm run build",
         "beforeDevCommand": "npm run dev",
         "devPath": "http://localhost:5173",
         "distDir": "../dist"
       },
       "bundle": {
         "active": true,
         "identifier": "com.youtubesegmentdownloader.rs",
         "icon": ["icons/32x32.png", "icons/128x128.png", "icons/icon.icns", "icons/icon.ico"]
       }
     }
     ```
   - 設定版本號和更新機制
   - 配置打包選項

2. **設定應用程式圖示與詳細資訊**:
   - 準備各種尺寸的圖示文件
   - 配置應用程式元數據
   - 設定安裝程式資訊和授權資訊

### Task 7.2: CI/CD 工作流程建立

1. **設定 GitHub Actions 工作流程**:
   - 建立 `.github/workflows/build.yml`
   - 設定觸發條件 (推送到主分支、發布標籤)
   - 配置 Rust 和 Node.js 建構環境

2. **配置 Windows 版本自動構建**:
   - 設定 Windows 構建工作流程
   - 配置代碼簽名
   - 產生 Windows 安裝程式

3. **配置 Linux Flatpak 版本自動構建**:
   - 設定 Flatpak 構建流程
   - 配置 Linux 相依套件
   - 產生 AppImage 和 Flatpak 包
   - 使用 Action upload-artifact@v3 上傳構建的檔案

4. **實作自動發佈 Release 功能**:
   - 僅在由發佈標籤觸發時執行
   - 設定 GitHub Release 自動發佈
   - 上傳構建的安裝檔案
   - 產生更新日誌
   - 配置版本編號和標籤

### Task 7.3: 文件與說明

1. **撰寫 README.md**:
   - 描述專案功能與特色
   - 添加安裝說明
   - 添加使用範例
   - 添加螢幕截圖

2. **編寫安裝與使用說明**:
   - 建立詳細的使用者指南
   - 描述主要功能的操作方式
   - 提供疑難排解章節
   - 解釋常用格式和選項

