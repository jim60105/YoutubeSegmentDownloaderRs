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
