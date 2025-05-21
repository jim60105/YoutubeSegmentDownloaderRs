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
   - 建立語言檔結構：`src/i18n/en.json`、`src/i18n/zh-TW.json`
   - 設計鍵值命名規則

3. **實作語言切換功能**:
   - 建立 `src/components/LanguageSelector.vue` 元件
   - 實作切換邏輯
     ```javascript
     function changeLanguage(locale) {
       i18n.global.locale.value = locale;
       localStorage.setItem('language', locale);
     }
     ```
   - 使用國旗或語言名稱作為選項
   - 記憶使用者選擇
