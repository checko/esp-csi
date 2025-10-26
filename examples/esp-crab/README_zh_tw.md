# 共晶振 CSI 接收範例

* [English Version](./README.md)
* [简体中文版](./README_CN.md)

此範例提供 Wi-Fi CSI 的射頻相位同步方案，包含三個子專案：`MASTER_RECV`（主接收端）、`SLAVE_RECV`（從接收端）與 `SLAVE_SEND`（從發送端）。  
解決方案支援兩種工作模式：  
1. 自發自收模式  
2. 單發雙收模式

## 功能介紹

### 1. 自發自收模式

在此模式下，兩顆 ESP32-C5 晶片分別負責發送與接收。透過解析接收到的 Wi-Fi CSI 相位，可在毫米等級感知訊號路徑的干擾。  
搭配銅片調整射頻傳播路徑，也能控制感測範圍，進而支援高精度的近距離 Wi-Fi 感知。  
此模式讓 Wi-Fi 訊號感知更為細緻，適用於近距離與複雜環境的精準應用。

![Self-Transmission and Reception Amplitude](<doc/img/Self-Transmission and Reception Amplitude.gif>)  
![Self-Transmission and Reception Phase](<doc/img/Self-Transmission and Reception Phase.gif>)

#### 1.1 MASTER_RECV（主接收端）

將 `MASTER_RECV` 韌體燒錄至 `esp-crab` 裝置的 **Master** 晶片，功能包含：

* 接收 `SLAVE_SEND` 傳送的 Wi-Fi 封包並擷取 **CIR (Channel Impulse Response)** 資料。
* 依據 CIR 計算 Wi-Fi CSI 的 **幅度與相位**，並顯示結果。

#### 1.2 SLAVE_SEND（從發送端）

將 `SLAVE_SEND` 韌體燒錄至 `esp-crab` 裝置的 **Slave** 晶片，其功能為：

* 發送特定的 Wi-Fi 封包。

### 2. 單發雙收模式

此模式下，由一顆 ESP32-C5 晶片負責訊號發送，`esp-crab` 裝置上的兩顆 ESP32-C5 則同時接收。  
透過分散佈署發送端與接收端，可在**更大空間範圍**內實現 Wi-Fi 感知。  
`esp-crab` 取得的**共晶振 Wi-Fi CSI 資料**符合尖端研究需求，亦可直接串接進階演算法，進一步提升感測系統的**精度與應用價值**。  
此模式為**大範圍、複雜環境的無線感知與定位**提供強大的技術支援。

![Single-Transmission and Dual-Reception Phase](<doc/img/Single-Transmission and Dual-Reception Phase.gif>)

#### 2.1 MASTER_RECV（主接收端）

將 `MASTER_RECV` 韌體燒錄至 `esp-crab` 裝置的 **Master** 晶片，功能包含：

* 透過天線接收 `SLAVE_SEND` 的 Wi-Fi 封包並擷取 **CIR 資料**。
* 接收由 `SLAVE_RECV` 晶片傳回的 CIR 資料（該資料由其天線收集）。
* 計算 Wi-Fi CSI 的**幅度與相位差**，並顯示結果。

#### 2.2 SLAVE_RECV（從接收端）

將 `SLAVE_RECV` 韌體燒錄至 `esp-crab` 裝置的 **Slave** 晶片，支援兩種模式：

* 透過天線接收 `SLAVE_SEND` 的 Wi-Fi 封包並擷取 **CIR 資料**。
* 將收到的 CIR 資料傳送給 `MASTER_RECV`。

#### 2.3 SLAVE_SEND（從發送端）

將 `SLAVE_SEND` 韌體燒錄至額外的 **ESP32-C5 晶片**（如 `ESP32-C5-DevkitC-1`），其功能為：

* 發送特定 Wi-Fi 封包。

## 所需硬體

### `esp-crab` 裝置

#### PCB 概觀

射頻相位同步方案需執行於 `esp-crab` 裝置。下圖為 PCB 正面與背面：

| 編號 | 功能                         |
|------|------------------------------|
| 1    | Master 外接天線              |
| 2    | Master 板載天線              |
| 3    | Slave 外接天線               |
| 4    | Slave 板載天線               |
| 5    | Master BOOT 按鈕             |
| 6    | Master ADC 按鈕              |
| 7    | Slave BOOT 按鈕              |
| 8    | Master 2.4G 天線切換電阻     |
| 9    | Master 5G 天線切換電阻       |
| 10   | Slave 5G 天線切換電阻        |
| 11   | Slave 2.4G 天線切換電阻      |
| 12   | RST 按鈕                     |

![esp_crab_pcb_front](doc/img/esp_crab_pcb_front.png)  
![esp_crab_pcb_back](doc/img/esp_crab_pcb_back.jpg)

更多 PCB 腳位說明請參閱 [ESP-Crab Circuit Diagram Explanation](<doc/ESP-Crab circuit diagram explanation.pdf>)。

#### 外觀形式

`esp-crab` 依工作模式有兩種外殼：

* 自發自收模式：太空船造型  
  ![Alt text](doc/img/shape_style.png)
* 單發雙收模式：路由器造型  
  ![router_shape](doc/img/router_style.png)

### `ESP32-C5-DevkitC-1` 開發板

單發雙收模式需要一塊 `ESP32-C5-DevkitC-1` 作為 Wi-Fi 發送端。

## 使用方式

### 1. 自發自收模式

以 Type-C 為 `esp-crab` 供電後即開始運作，並顯示 CSI 幅度與相位：

* **幅度**：兩條曲線分別對應 -Nsr~0 與 0~Nsr 的 CIR 幅度。
* **相位**：標準正弦曲線，與螢幕中央紅線交點代表 0~Nsr 的 CIR 相位。

同時 `esp-crab` 會於序列埠輸出接收到的 CSI 資料，格式如下：  
`type,id,mac,rssi,rate,noise_floor,fft_gain,agc_gain,channel,local_timestamp,sig_len,rx_state,len,first_word,data`

範例：

``` text
CSI_DATA,3537,1a:00:00:00:00:00,-17,11,159,22,5,8,859517,47,0,234,0,"[14,9,13,...,-11]"
```

> 注意：上電後裝置會先收集前 100 個 Wi-Fi 封包，以決定 Wi-Fi 射頻接收增益。

### 2. 單發雙收模式

為 `esp-crab` 與 `ESP32-C5-DevkitC-1` 供電並置於適當距離後，系統即會顯示 CSI 幅度與相位，同時在序列埠輸出上述格式的 CSI 資料。
