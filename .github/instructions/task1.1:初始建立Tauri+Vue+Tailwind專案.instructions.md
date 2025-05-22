### Task 1.1: 初始建立 Tauri + Vue + Tailwind 專案

1. **安裝必要的開發工具**:

    - 確保已安裝 Node.js (v18+) 和 pnpm
    - 安裝 Rust 和 Cargo (`rustup`)
    - 安裝系統相依套件 (Linux 需要安裝 `libwebkit2gtk-4.0-dev`, `build-essential` 等)

    ```
    sudo dnf check-update
    sudo dnf install webkit2gtk4.1-devel \
      openssl-devel \
      curl \
      wget \
      file \
      libappindicator-gtk3-devel \
      librsvg2-devel
    sudo dnf group install "c-development"
    ```

2. **建立新專案**:

    - 執行 `pnpm create tauri-app@latest youtube-segment-downloader-rs
 --template vue-ts`，並將專案建立在當前目錄
    - 將專案目錄 `youtube-segment-downloader-rs` 的內容移動到當前目錄

3. **整合 Vue.js**:

    - 確認 Vue.js 3 已正確安裝
    - 設定 Vue Router 用於頁面導航: `pnpm install vue-router@4`
    - 設定 Pinia 用於狀態管理: `pnpm install pinia`

4. **設定 Tailwind CSS**:

    - 安裝 Tailwind CSS: `pnpm install -D tailwindcss postcss autoprefixer`
    - 初始化 Tailwind: `npx tailwindcss init -p`
    - 配置 `tailwind.config.js`，設定內容路徑

    ```js
    /** @type {import('tailwindcss').Config} */
    export default {
        content: ['./index.html', './src/**/*.{vue,js,ts,jsx,tsx}'],
        theme: {
            extend: {},
        },
        plugins: [],
    };
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
