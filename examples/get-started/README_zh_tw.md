# Get Started 範例
[[English]](./README.md) [[简体中文]](./README_cn.md)

本範例示範如何透過兩顆 Espressif 晶片互相通訊以擷取 CSI 資料，並使用圖形介面即時顯示 CSI 子載波的數據。

## 硬體

請準備兩塊 Espressif 開發板，一塊作為發送端，另一塊作為接收端。

![example_display](./docs/_static/example_display.png)

為了獲得理想的 CSI 感知效果，建議：

1. 使用 ESP32-C5 / ESP32-S6：ESP32-C5 支援雙頻 Wi-Fi 通訊，射頻表現優異；ESP32-C6 是目前已上市型號中射頻效能最佳的晶片。
2. 改用外接天線：PCB 天線方向性較差，容易受到主板干擾。
3. 兩台裝置之間保持超過 1 公尺的距離。

## 綁定流程

1. 分別將 `csi_recv` 與 `csi_send` 韌體燒錄到兩塊開發板。

    ![device_log](./docs/_static/device_log.png)

    ```shell
    # csi_send
    cd esp-csi/examples/get-started/csi_send
    idf.py set-target esp32c3
    idf.py flash -b 921600 -p /dev/ttyUSB0 monitor

    # csi_recv
    cd esp-csi/examples/get-started/csi_recv
    idf.py set-target esp32c3
    idf.py flash -b 921600 -p /dev/ttyUSB1
    ```

2. 在 `csi_recv` 目錄下執行 `csi_data_read_parse.py` 進行資料分析。執行前請先關閉 `idf.py monitor`。

    ```shell
    cd esp-csi-gitlab/examples/get-started/tools

    # 安裝 Python 相依套件
    pip install -r requirements.txt

    # 啟動圖形化介面
    python csi_data_read_parse.py -p /dev/ttyUSB1
    ```

## CSI 資料格式

以下為一行 CSI 原始資料範例：

> type,id,mac,rssi,rate,sig_mode,mcs,bandwidth,smoothing,not_sounding,aggregation,stbc,fec_coding,sgi,noise_floor,ampdu_cnt,channel,secondary_channel,local_timestamp,ant,sig_len,rx_state,len,first_word,data
CSI_DATA,0,94:d9:b3:80:8c:81,-30,11,1,6,1,0,1,0,1,0,0,-93,0,13,2,2751923,0,67,0,128,1,"[67,48,4,0,0,0,0,0,0,0,5,0,20,1,20,1,19,0,17,1,16,2,15,2,14,1,12,0,12,-1,12,-3,12,-4,13,-6,15,-7,16,-8,16,-8,16,-8,16,-6,15,-5,15,-4,14,-4,13,-4,12,-4,11,-4,10,-4,9,-5,8,-6,4,-4,8,-9,9,-10,9,-10,10,-11,11,-10,11,-10,12,-9,11,-8,11,-7,10,-6,9,-6,7,-6,6,-7,5,-7,5,-8,5,-9,5,-10,5,-11,5,-11,6,-11,7,-11,8,-11,9,-10,9,-9,8,-8,8,-7,1,-2,0,0,0,0,0,0,0,0]"

*ESP32-C5 與 ESP32-C6 的資料格式為：*
>type,seq,mac,rssi,rate,noise_floor,fft_gain,agc_gain,channel,local_timestamp,sig_len,rx_state,len,first_word,data
CSI_DATA,7,1a:00:00:00:00:00,-23,11,-96,32,4,11,372852,47,0,256,0,"[0,0,0,0,0,0,0,0,0,0,0,0,-6,-13,-6,-14,-3,-15,-2,-16,-2,-18,-3,-17,-1,-18,2,-19,0,-21,3,-21,1,-20,4,-21,4,-23,6,-22,7,-21,8,-23,9,-23,10,-21,10,-22,11,-20,12,-19,11,-25,13,-22,12,-23,14,-23,14,-22,14,-21,13,-21,13,-19,14,-22,14,-19,14,-23,16,-22,14,-22,13,-22,14,-21,13,-22,13,-23,13,-23,12,-26,13,-24,13,-24,12,-25,14,-29,13,-26,14,-26,15,-26,13,-27,15,-28,16,-27,14,-30,15,-28,16,-28,18,-27,16,-31,18,-31,19,-31,0,0,0,0,0,0,-29,-23,-28,-21,-30,-18,-26,-20,-30,-21,-25,-23,-26,-21,-25,-22,-26,-19,-22,-22,-24,-19,-22,-20,-24,-20,-24,-18,-23,-18,-22,-18,-25,-17,-23,-18,-21,-18,-21,-17,-24,-14,-22,-16,-21,-14,-22,-15,-21,-19,-23,-16,-22,-17,-23,-13,-23,-16,-25,-15,-21,-17,-22,-15,-21,-17,-23,-16,-20,-16,-21,-20,-21,-19,-21,-19,-19,-20,-17,-20,-18,-20,-16,-21,-15,-21,-15,-20,-13,-21,-11,-20,-10,-20,-11,-19,-9,-20,-8,-22,-6,-19,-7,-20,-4,-19,-2,-18,-2,-18,1,-17,4,-18,0,0,0,0,0,0,0,0,0,0]"

- **中繼資料欄位**：包含 type、id、mac、rssi、rate、...、len、first_word 等。
- **CSI 資料**：最後一個 data 陣列使用 [...] 包住，內含每個子載波的通道狀態資訊。詳細格式請參考 [ESP-WIFI-CSI Guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/wifi.html#wi-fi-channel-state-information) 的 Long Training Field (LTF) 章節。每個子載波的資料順序為先虛部再實部（例：[子載波 1 虛部, 子載波 1 實部, 子載波 2 虛部, 子載波 2 實部, 子載波 3 虛部, 子載波 3 實部, ...]）。

LTF 的順序為：LLTF、HT-LTF、STBC-HT-LTF。依通道與分組設定不同，可能不會同時出現三種 LTF。

## 常見問題（A&Q）

### 1. `csi_send` 顯示記憶體不足
- **現象**：序列埠日誌出現：
  ```shell
    W (510693) csi_send: <ESP_ERR_ESPNOW_NO_MEM> ESP-NOW send error
  ```
- **原因**：目前 Wi-Fi 通道壅塞，ESP-NOW 傳送緩衝區已滿。
- **解決方式**：更換 Wi-Fi 通道或移至網路環境較佳的位置。

### 2. `csi_data_read_parse.py` 序列埠輸出異常
- **現象**：序列埠日誌出現：
    ```shell
        element number is not equal
        data is not incomplete
    ```
- **原因**：繪圖時 PyQt 佔用較多 CPU，導致電腦無法即時讀取序列埠緩衝區而造成資料錯亂。
- **解決方式**：調高序列埠鮑率。
