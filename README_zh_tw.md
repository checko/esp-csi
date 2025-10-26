# ESP-CSI [[English]](./README.md) [[简体中文]](./README_cn.md)

本專案主要展示 ESP-WIFI-CSI 的使用方式，提供 CSI 資料的擷取流程、處理演算法與應用範例。人體偵測演算法仍在持續優化中。透過原始 CSI 資料，使用者可以結合機器學習、神經網路等方法，取得更精準的感測成效。

## CSI 介紹

通道狀態資訊（CSI, Channel State Information）是描述無線通道特性的關鍵參數，涵蓋訊號幅度、相位、訊號延遲等指標。在 Wi-Fi 通訊中，CSI 用於量測無線網路的通道狀態。分析 CSI 的變化可推測物理環境的改變，達成非接觸式智慧感測。CSI 對環境變化極為敏感，不只可偵測人或動物行走、奔跑等大幅度動作，也能捕捉靜態環境中的微幅動作，例如呼吸、咀嚼等。這些特性讓 CSI 可廣泛應用於智慧環境監測、人體活動偵測、無線定位等領域。

## 基礎知識

為協助理解 CSI 技術，我們提供相關的基礎知識文件（將持續更新）：

- [訊號處理基礎](./docs/zh_TW/Signal-Processing-Fundamentals.md)
- [OFDM 介紹](./docs/zh_TW/OFDM-introduction.md)
- [無線通道基礎](./docs/zh_TW/Wireless-Channel-Fundamentals.md)
- [無線測距與定位技術介紹](./docs/zh_TW/Introduction-to-Wireless-Location.md)
- [無線通訊指標 CSI 與 RSSI](./docs/zh_TW/Wireless-indicators-CSI-and-RSSI.md)
- [CSI 的應用與案例分析](./docs/zh_TW/CSI-Applications.md)

## Espressif CSI 優勢

- **全系列支援：** 所有 ESP32 系列皆支援 CSI，包括 ESP32 / ESP32-S2 / ESP32-C3 / ESP32-S3 / ESP32-C6。
- **強大生態系：** Espressif 作為 Wi-Fi MCU 市場的領導者，能將 CSI 與既有物聯網裝置完美整合。
- **資訊更豐富：** ESP32 提供完整的通道資訊，涵蓋 RSSI、RF 噪聲底限、接收時間以及天線的 `rx_ctrl` 欄位。
- **藍牙輔助：** ESP32 亦支援 BLE，可掃描周邊裝置以加強偵測能力。
- **強勁處理效能：** ESP32 搭載雙核心 240 MHz CPU，支援 AI 指令集，可執行機器學習與神經網路。
- **OTA 更新：** 既有專案可透過軟體 OTA 升級取得 CSI 新功能，無需額外硬體成本。

## 範例介紹

### [get-started](./examples/get-started)

協助使用者快速上手 CSI 功能，透過基礎範例示範 CSI 資料的擷取與初步分析，詳情見 [README](./examples/get-started/README.md)。

- [csi_recv](./examples/get-started/csi_recv) 示範 ESP32 作為接收端。
- [csi_send](./examples/get-started/csi_send) 示範 ESP32 作為發送端。
- [csi_recv_router](./examples/get-started/csi_recv_router) 示範以路由器為發送端，ESP32 透過 Ping 觸發路由器送出含 CSI 的封包。
- [tools](./examples/get-started/tools) 提供協助分析 CSI 資料的腳本，例如 `csi_data_read_parse.py`。

### [esp-radar](./examples/esp-radar)

提供多個 CSI 應用案例，包括 RainMaker 雲端回報與人體活動偵測。

- [connect_rainmaker](./examples/esp-radar/connect_rainmaker) 示範擷取 CSI 資料並上傳至 Espressif RainMaker 雲端。
- [console_test](./examples/esp-radar/console_test) 提供互動式主控台，可動態設定與擷取 CSI 資料，並示範人體活動偵測演算法。

## 如何取得 CSI

### 4.1 取得路由器 CSI

<img src="docs/_static/get_router_csi.png" width="550">

- **實作方式：** ESP32 向路由器送出 Ping 封包，並接收路由器回應中的 CSI 資訊。
- **優點：** 只需一個 ESP32 搭配路由器即可完成。
- **缺點：** 受路由器條件影響，例如位置、支援的 Wi-Fi 協定等。
- **適用情境：** 環境中僅有一台 ESP32，且具備可用的路由器。

### 4.2 取得裝置間 CSI

<img src="docs/_static/get_device_csi.png" width="550">

- **實作方式：** ESP32 A 與 B 同時向路由器送出 Ping，ESP32 A 接收 ESP32 B 封包中的 CSI，作為第一種情境的補充。
- **優點：** 不受路由器位置影響，也較不受其他連線裝置干擾。
- **缺點：** 仍受路由器支援的 Wi-Fi 協定與環境條件限制。
- **適用情境：** 需要兩台以上 ESP32 的環境。

### 4.3 取得特定裝置 CSI

<img src="docs/_static/get_broadcast_csi.png" width="550">

- **實作方式：** 封包發送裝置持續切換頻道並廣播封包，ESP32 A、B、C 皆可擷取廣播封包中的 CSI。此方式具最高的偵測精度與可靠度。
- **優點：** 不受路由器影響，偵測精度高；即使有多台裝置，僅需一個封包發送裝置即可，對網路干擾小。
- **缺點：** 除了一般 ESP32，還需要額外的專用封包發送裝置，成本較高。
- **適用情境：** 需要高精度與多裝置群集定位的應用。

## 注意事項

1. 外接 IPEX 天線表現優於 PCB 天線，後者具有方向性。
2. 建議在無人環境進行測試，避免其他活動影響結果。

## 相關資源

- [ESP-IDF Programming Guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/index.html) 是 Espressif 物聯網開發框架的官方文件。
- [ESP-WIFI-CSI Guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/wifi.html#wi-fi-channel-state-information) 說明如何使用 ESP-WIFI-CSI。
- 若發現問題或有功能需求，請於 GitHub [Issues](https://github.com/espressif/esp-csi/issues) 回報，提交前請先確認是否已有相關議題。

## Reference

1. [Through-Wall Human Pose Estimation Using Radio Signals](http://rfpose.csail.mit.edu/)
2. [A list of awesome papers and cool resources on WiFi CSI sensing](https://github.com/Marsrocky/Awesome-WiFi-CSI-Sensing#awesome-wifi-sensing)
