# CSI Radar 介面說明

以下說明對應於 `esp_csi_tool.py` 視覺化工具的各個圖表與面板，協助使用者理解雷達介面所呈現的資料。

## 指令與路由器面板

- `ssid`、`password` 文字框：設定 ESP32-S3 Station 端要連線的無線基地台（AP）。
- `autoconnect` 核取方塊：勾選後裝置重開機會自動重連最後一次成功的 AP。
- `disconnect` 按鈕：中斷目前的 Wi-Fi 連線。
- `custom` 指令輸入：可直接送出自訂主機端命令（例如 `reboot`、`wifi_scan`）。

## 子載波振幅圖（Raw data 區左上）

- 多條彩色曲線分別代表 CSI 封包內不同子載波的振幅。
- X 軸為封包接收序列（時間），Y 軸為振幅大小。曲線波動顯示空間中人體或物件移動造成的多重路徑變化。
- 行為：振幅快速增減通常代表環境中有移動物體或人員。

## 波形濾波與 RSSI（Raw data 區左下）

- `wave filtering` 勾選後會套用平滑濾波，方便觀察長期趨勢。
- RSSI 細條圖顯示接收訊號強度隨時間變化，可用於對照 CSI 波動是否來自整體訊號衰減。

## 日誌面板（log）

- 顯示韌體送出的即時紀錄：例如封包序號、可用記憶體、`gain`、`jitter`、`wander` 等統計值，或錯誤訊息。
- 有助於偵錯（例如計算失敗、連線狀態改變）。

## 資訊表格（info）

- 列出最新接收 CSI 封包的關鍵欄位：序號、時間戳、目標 MAC、PHY 模式、資料率、RSSI 等。
- 可檢查封包是否有漏失、PHY 參數是否如預期。

## 雷達模型圖（Radar model 區右上）

- 綠色曲線 `jitter` 代表高頻 CSI 波動，紫色曲線 `wander` 代表低頻漂移。
- 水平線為「有人」與「移動」的感度閾值，可透過 `someone sensitivity`、`move sensitivity` 兩個欄位調整。
- 標題列會即時顯示模型判定（如 `none move` 代表目前偵測到無人移動）。

## 統計圖（Statistics）

- 以長條圖方式統計一定時間內（`mode` 選單可選分鐘、時、日）偵測到的移動次數。
- 搭配 `time` 可回顧某個時間點的活動量，支援 `auto update` 自動刷新。

## 人員狀態表格與資料蒐集控制（Collect）

- 表格列出每次判定的房間狀態 (`room`)、人員狀態 (`human`)、停留時間 (`spend_time`) 與時間戳。
- `target`、`delay`、`duration`、`number` 用於錄製標記動作的 CSI 資料，按下 `start` 後依設定進行擷取。
- `clean` 可清除先前的資料記錄，方便重新標記。

> **提示**：在執行 Python 工具前請先關閉 `idf.py monitor`，避免序列埠被占用。
