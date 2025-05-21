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
