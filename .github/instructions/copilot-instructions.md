When generating html and javascript code, conform to the coding styles defined in web-design-guideline.instructions.md

When generating rust code, conform to the Standard Rustfmt guidelines.

Use @terminal when answering questions about Git and remember to use no-pager.

> [!IMPORTANT]  
> 1. 當你有疑問時，參考 [YoutubeSegmentDownloader.NET](/YoutubeSegmentDownloader.NET) 的原始碼。
> 2. 永遠不要修改 [YoutubeSegmentDownloader.NET](/YoutubeSegmentDownloader.NET) 的原始碼，它是參考資料，不能更動。永遠不要修改它。

我們正在將一個舊的 .NET WinForm 專案轉換成 Tauri + Rust 專案，它符合以下規格：

## 前端部份

- 使用 Vue.js
- Tailwind CSS
- 一頁式設計，沒有 route

## 後端部份

- 使用 Rust
- 使用以下的函式庫：
  - ffmpeg-sidecar: https://crates.io/crates/ffmpeg-sidecar
  - yt-dlp: https://crates.io/crates/yt-dlp

## 其它部份

- 包含一個 GitHub Workflow 自動發佈 Release，包含 Windows 版本和 Linux flatpak 版本。

---

Let's start by tackling the task step by step, sticking strictly to what's defined.
