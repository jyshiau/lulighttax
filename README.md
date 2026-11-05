# 璐光咖啡廳記帳

手機版記帳網頁：掃描台灣電子發票 QR Code（左右兩個）、手動輸入傳統發票與收據、費用分類、月報表、CSV 匯出。

- 網址：https://jyshiau.github.io/lulighttax/
- 開啟後要先登入（帳號 `admin`）。
- 用 Chrome 打開後可從選單「加到主畫面」，像 App 一樣使用。

## 跨裝置同步（Firebase）

`index.html` 最上方的 `FIREBASE_CONFIG` 填入 Firebase 專案設定後：

- 登入改由 Firebase 驗證，密碼不再寫在網頁裡。
- 所有帳存在 Firestore，任何手機或電腦用同一組帳號密碼登入都會看到同一份明細，離線時也能記，連上網路後自動上傳。
- 每台裝置第一次登入時，原本存在那台裝置上的帳會自動上傳到雲端（同一筆保留較新的版本），本機那份改名保留為 `luguang-acct-v1-before-cloud` 當備份。

設定步驟：

1. 到 https://console.firebase.google.com 建立專案（免費的 Spark 方案即可）。
2. 「Build → Authentication → 開始使用」，啟用「電子郵件/密碼」登入。到「Users → 新增使用者」，電子郵件填 `admin@luguang.example`，密碼填登入用的密碼。到「Settings → 使用者動作」關閉「啟用建立帳戶（註冊）」。
3. 「Build → Firestore Database → 建立資料庫」，選 `asia-east1`（台灣），正式版模式。在「規則」貼上：

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if request.auth != null && request.auth.token.email == "admin@luguang.example";
       }
     }
   }
   ```

4. 「專案設定 → 一般 → 你的應用程式 → 網頁（</>）」新增網頁應用程式，複製 `firebaseConfig`，貼到 `index.html` 的 `FIREBASE_CONFIG`。

`FIREBASE_CONFIG` 為 `null` 時是本機模式：登入只在網頁裡比對密碼雜湊，資料只存在那台裝置的瀏覽器（localStorage），請到「設定 → 資料備份」定期下載備份。

`index.html` 已內嵌 [jsQR](https://github.com/cozmo/jsQR)（Apache-2.0）做 QR 辨識，Android Chrome 另會使用內建的 BarcodeDetector。
