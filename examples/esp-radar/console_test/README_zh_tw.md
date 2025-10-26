# esp-csi console_test [[English]](./README.md) [[简体中文]](./README_cn.md)
----------
## 1 介紹
此範例提供 Wi-Fi CSI 的測試平台，整合資料顯示、資料擷取與資料分析等功能，協助您快速了解 Wi-Fi CSI。
- **Display**：即時顯示 Wi-Fi RF 噪聲底限、CSI、RSSI 與噪聲底等資訊，可觀察不同天線、人員移動與裝置定位對訊號的影響。
- **Acquisition**：所有收集到的 Wi-Fi CSI 會存入檔案，並可為不同動作標記，以利後續的神經網路或機器學習訓練。
- **Analysis**：可偵測室內是否有人與是否有移動，協助您快速了解 Wi-Fi CSI 的應用情境。

## 2 設備準備
### 2.1 設備
![equipment](./docs/_static/2.1_equipment.png)
本範例提供「esp32-s3 開發板」與「路由器」兩種 Wi-Fi CSI 發訊來源。以 `esp32-s3` 作為發訊端可更彈性地調整發包速率、射頻功率與頻道。兩種模式下，`esp32-s3` 均為 Wi-Fi CSI 接收端。

### 2.2 編譯環境
目前專案使用的 ESP-IDF 版本為 [ESP-IDF Release v5.0.2](https://github.com/espressif/esp-idf/releases/tag/v5.0.2)
```bash
cd esp-idf
git checkout v5.0.2
git submodule update --init --recursive
./install.sh
. ./export.sh
```
> 注意：ESP-IDF v5.0.0 以上版本支援目的位址過濾，效果更佳，建議使用 v5.0.0 或更新版本。

## 3 啟動程式
### 3.1 傳送 Wi-Fi CSI
- **使用 esp32-s3 傳送 CSI**：將 `csi_send` 專案燒錄至 esp32-s3 開發板。
  
    ```bash
    cd esp-csi/examples/get-started/csi_send
    idf.py set-target esp32s3
    idf.py flash -b 921600 -p /dev/ttyUSB0 monitor
    ```
- **使用路由器傳送 CSI**：請避免路由器同時連接其他智慧裝置，以免網路壅塞影響測試。

### 3.2 接收 Wi-Fi CSI
- 將 `console_test` 專案燒錄至另一塊 esp32-s3 開發板
    ```bash
    cd esp-csi/examples/console_test
    idf.py set-target esp32s3
    idf.py flash -b 921600 -p /dev/ttyUSB1
    ```

### 3.3 啟動 `esp-csi-tool` 並開啟 CSI 視覺化介面
- 在 `csi_recv` 目錄執行 `esp_csi_tool.py` 進行資料分析。執行前請先關閉 `idf.py monitor`，並務必使用 UART 埠而非 USB Serial/JTAG。
    ```bash
    cd esp-csi/examples/console_test/tools
    # 安裝 Python 相依套件
    pip install -r requirements.txt
    # 啟動圖形化介面
    python esp_csi_tool.py -p /dev/ttyUSB1
    ```
- 執行成功後會開啟 CSI 視覺化介面。介面左側為資料顯示區 `Raw data`，右側為資料模型區 `Raw model`：![csi tool](./docs/_static/3.3_csi_tool.png)

## 4 介面介紹
即時視覺化介面包含 `Raw data` 與 `Radar model` 兩部分。`Raw data` 顯示原始 CSI 子載波資料，`Radar model` 則透過演算法分析 CSI。您可以透過右上角 `Raw data` 與 `Radar model` 按鈕切換顯示。

### 4.1 路由器連線視窗
左上角為路由器連線設定視窗，透過此視窗可讓裝置連上路由器並接收兩者之間的 CSI。

![connection window](./docs/_static/4.1_connect_windows.png)

- **SSID**：路由器帳號
- **password**：路由器密碼
- **auto connect**：勾選後下次開啟會自動連線至上一次的路由器
- **connect / disconnect**：連線／斷線按鈕
- **custom**：可傳送更多控制指令，例如重新啟動、查詢版本，或在裝置端執行自訂指令

### 4.2 CSI 波形顯示視窗
即時顯示特定頻道的 CSI 波形。勾選 `wave filtering` 可查看濾波後的波形。
![csi_waveform window](./docs/_static/4.2_csi_waveform_windows.png)

### 4.3 RSSI 波形顯示視窗
顯示 RSSI 波形，可搭配 CSI 波形觀察室內不同狀態下的 RSSI 變化。
![RSSI_waveform window](./docs/_static/4.3_rssi_waveform_windows.png)

### 4.4 日誌顯示視窗
顯示時間、記憶體等系統日誌。
![log window](./docs/_static/4.4_log_windows.png)

### 4.5 Wi-Fi 頻道資料視窗
顯示 Wi-Fi 頻道狀態資訊。
![Wi-Fi_data window](./docs/_static/4.5_wi-fi_data_windows.png)

### 4.6 房間狀態視窗
此視窗用於校準房間狀態閾值並顯示房間狀態（有人／無人、移動／靜止）。![room_state window](./docs/_static/4.6_room_state_windows.png)

- **delay**：校準開始前的延遲時間，啟動後請在延遲內離開房間，確保校準期間無人。
- **duration**：校準流程持續時間。
- **add**：勾選後，重新校準的閾值會累加至目前閾值。
- **start**：啟動校準按鈕。
- **wander(someone) threshold**：校準後自動設定的有人／無人門檻，也可手動調整。
- **jitter(move) threshold**：判斷移動／靜止的門檻，可自動或手動設定。
- **config**：手動設定閾值後請點選 `config` 套用。
- **display table**：勾選後會在波形視窗右側顯示房間與人員狀態表格。表格欄位如下：
  
    |status|threshold|value|max|min|mean|std|
    |---|---|---|---|---|---|---|
    |房間／人員狀態|判斷閾值|即時數值|最大值|最小值|平均值|標準差|

### 4.7 人員移動資料視窗
顯示室內人員移動的詳細資料。左側長條圖即時顯示移動次數，右側表格記錄每次移動的時間。
![people_movedata window](./docs/_static/4.7_people_movedata_windows.png)

- **mode**：觀測模式分為 `minute`、`hour`、`day`，分別統計每分鐘／每小時／每日的移動次數。
- **time**：觀測起始時間，預設為目前時間，可手動設定。
- **auto update**：勾選後長條圖會自動更新顯示。
- **update**：手動更新長條圖顯示。

表格欄位說明如下：
|room|human|spend_time|start_time|stop_time|
|---|---|---|---|---|
|房間狀態|人員狀態|移動持續時間|移動開始時間|移動結束時間|

### 4.8 動作收集視窗
用於收集特定動作下的 CSI 資料。資料會存放在 `esp-csi/examples/console_test/tools/data`，可用於機器學習或神經網路訓練。
![collect window](./docs/_static/4.8_collect_windows.png)

- **target**：選擇要收集的動作。
- **delay**：自按下 `start` 起延遲多久開始收集。
- **duration**：每個動作的收集時間。
