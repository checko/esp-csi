# 範例專案
[[English]](./README.md) [[简体中文]](./README_cn.md)  
此目錄收錄多個 esp-csi 範例專案，展示 esp-csi 的多項功能，並提供可複製、調整後直接套用於自有專案的程式碼。

- `get-started/csi_recv`：基本的 CSI 接收範例，示範如何透過 Wi-Fi 接收端取得 CSI 資訊。
- `get-started/csi_send`：基本的 CSI 傳送範例，搭配 `csi_recv` 以 Wi-Fi 封包供接收端擷取 CSI。
- `get-started/csi_recv_router`：在路由器通訊模式下接收 CSI，透過 ping 路由器並解析回覆封包中的 CSI。
- `esp-radar/connect_rainmaker`：將 CSI 數據上傳至 Espressif RainMaker 雲端平台，用於遠端視覺化或控制。
- `esp-radar/console_test`：提供主控台測試環境，可偵錯並評估 CSI 數據與演算法效能。
- `esp-crab/master_recv`：esp-crab 硬體平台的主接收端，負責擷取與解析 Wi-Fi CIR/CSI 資料。
- `esp-crab/slave_recv`：esp-crab 平台的從接收端，協助主接收端進行多通道資料收集。
- `esp-crab/slave_send`：esp-crab 平台的發送端，週期性傳送封包供其他節點提取 CSI。
