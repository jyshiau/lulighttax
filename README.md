# 璐光咖啡廳記帳

手機版記帳網頁：掃描台灣電子發票 QR Code（左右兩個）、手動輸入傳統發票與收據、費用分類、月報表、CSV 匯出。

- 網址：https://jyshiau.github.io/lulighttax/
- 資料存在各支手機的瀏覽器（localStorage），請到「設定 → 資料備份」定期下載備份。
- 用 Chrome 打開後可從選單「加到主畫面」，像 App 一樣使用。

`index.html` 已內嵌 [jsQR](https://github.com/cozmo/jsQR)（Apache-2.0）做 QR 辨識，Android Chrome 另會使用內建的 BarcodeDetector。
