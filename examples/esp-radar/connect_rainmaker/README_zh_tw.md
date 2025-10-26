# 在 ESP RainMaker 中加入 Wi-Fi CSI 功能 [[English]](./README.md) [[简体中文]](./README_cn.md)

## 建置與燒錄
請依照 ESP RainMaker 文件的 [Getting Started](https://rainmaker.espressif.com/docs/get-started.html) 章節建置並燒錄韌體。

## 範例操作指南

### 參數說明
- **someone_status**：false 表示無人，true 表示有人。
- **someone_timeout**：在此時間內若偵測到移動，判定為有人；單位為秒。
- **move_status**：false 表示無移動，true 表示有人移動。
- **move_count**：自上次 ESP RainMaker 上報以來偵測到的移動次數。
- **move_threshold**：判斷有人移動所使用的閾值。
- **filter_window**：Wi-Fi CSI 波形抖動值的緩衝佇列大小，用於濾除離群值。
- **filter_count**：若在緩衝佇列中有 `filter_count` 次抖動值超過 `move_threshold`，即判定有人移動。
- **threshold_calibrate**：是否啟用閾值自動校準。
- **threshold_calibrate_timeout**：自動校準逾時時間，單位為秒。

### App 版本
- ESP RainMaker App：[v2.11.1](https://m.apkpure.com/p/com.espressif.rainmaker)+  
> 注意：2.11.1 以前的 App 版本不支援 `time series` 功能，`move_count` 無法正常顯示。

### App 操作
1. 開啟 RainMaker App。
2. 點選 `+` 新增裝置。
3. 等待裝置連線至雲端。
4. 啟用 `threshold_calibrate` 進行自動校準，校準期間請確保室內無人或無移動。
5. 校準完成後，`move_threshold` 會顯示偵測閾值；偵測到移動時 `move_status` 會變為 true。

### 裝置
- [x] ESP32-S3-DevKitC-1
- [x] ESP32-C3-DevKitC

### 裝置操作
1. **恢復原廠設定**：長按 `BOOT` 鍵超過 5 秒可將開發板重置為預設值。

## 裝置狀態指示
- 人體移動偵測  
  - 綠燈：房間內偵測到移動  
  - 白燈：房間內無移動

- 人體存在偵測  
  > 目前演算法對靜態存在偵測仍待優化，因此以最近 1 分鐘是否有人移動作為判斷依據；只要偵測到移動就視為有人在場。
  - LED 亮起：判定有人
  - LED 熄滅：判定無人

- 人體移動閾值  
  > - 可透過手機 App 設定，或啟用自動校準取得；若未設定則使用預設值。  
  > - 校準時務必保持室內無人移動。校準後靈敏度會提高，若過程中有人活動可能造成誤判，因此建議在無人時進行。  
  > - 校準後的閾值會儲存在 NVS，下次開機會沿用。
  - 進行移動閾值校準期間，LED 會閃爍黃燈。

## 常見問題

### RainMaker 上報失敗
------
- **問題**：裝置端日誌出現：
    ```shell
    E (399431) esp_rmaker_mqtt: Out of MQTT Budget. Dropping publish message.
    ```
- **原因**：裝置端上傳的資料量超出 ESP RainMaker 限制。

------
- **問題**：持續偵測到有人移動但實際無人，或完全偵測不到移動。

- **解決方式**：
  1. 移動偵測閾值設定不當導致誤判  
     - 預設 Wi-Fi CSI 閾值可能不符實際需求，請依情境調整或啟用自動校準。  
     - 預設的離群值濾波視窗可能不適用，可在 App 依情況調整。

  2. 網路不穩定影響偵測結果  
     - 嘗試更換路由器後是否改善。  
     - 將路由器放置於更適合的位置。

  3. 若仍無法解決，請調整 LOG 等級並於 GitHub 提交 [issue](https://github.com/espressif/esp-csi/issues)
     ```c
     esp_log_level_set("esp_radar", ESP_LOG_DEBUG);
     ```
