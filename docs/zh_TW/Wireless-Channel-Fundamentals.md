# 無線通道概述[[English]](../en/Wireless-Channel-Fundamentals.md) [[简体中文]](../zh_CN/Wireless-Channel-Fundamentals.md)

無線通訊系統透過無線通道傳輸資料，訊號會受到衰減、多重路徑效應與干擾等多種因素影響。理解無線通道特性是設計與優化無線通訊系統的關鍵。

## 訊號表示方式

**複數表示：** 以複數形式描述訊號，可簡化分析流程。

**幅度與相位：** 幅度代表訊號強度，相位則對應訊號位置。

## 無線通道特性

無線通道的主要特性包含：

- **衰減與路徑損耗：** 訊號在傳播過程中會衰減並產生路徑損耗。
- **多重路徑效應：** 訊號沿不同路徑前進，造成相位與幅度變化，進而影響通訊品質。
- **延遲展開（Delay Spread）：** 多重路徑效應導致訊號抵達時間被拉長。
- **多使用者干擾：** 多個使用者共用頻譜時會彼此干擾，降低通訊可靠度與效率。
- **遮蔽（Shadowing）：** 障礙物會削弱訊號強度。

## CSI 與無線通道特性的關係

Channel State Information (CSI) 提供完整的通道資訊，有助於掌握並運用各項無線通道特性，以最佳化無線通訊系統的效能與可靠度。CSI 與多重路徑效應的結合在無線通訊上有多項重要應用，包括：

### 1. Multipath Beamforming

Multipath Beamforming 利用多重路徑效應在特定方向增強或抑制訊號，應用方式包含：

- **Beam Tracking：** 藉由 CSI 追蹤多重路徑通道變化，優化波束形狀並最大化接收訊號強度。
- **Interference Suppression：** 精準量測與分析多重路徑通道的 CSI，可在空間上抑制干擾來源，提升 Signal-to-Interference Ratio (SIR) 與系統容量。

### 2. Localization and Tracking

多重路徑效應是精準定位與移動追蹤的關鍵。CSI 在定位與追蹤的應用包括：

- **Multipath Imaging：** 分析 CSI 資料以建構物體周遭的多重路徑影像，達成高解析度的位置估測。
- **Attitude Estimation：** 借助多重路徑效應準確估計行動裝置的方向與姿態，提高導航系統的準確度。

### 3. Multi-User MIMO Systems

在 Multi-User MIMO 系統中，結合 CSI 與多重路徑效應可帶來下列效益：

- **Multi-User Diversity：** 運用多重路徑傳播的 CSI 接收不同路徑上的使用者資料流，提升頻譜效率與系統容量。
- **Spatial Multi-User Scheduling：** 利用多重路徑通道的 CSI 實現空間多使用者排程，最大化系統吞吐量與資源運用。

### 4. Dynamic Spectrum Access 與 Spectrum Sensing

多重路徑效應與 CSI 亦應用於動態頻譜存取（Dynamic Spectrum Access, DSA）與 Spectrum Sensing：

- **最佳化頻譜利用：** 分析多重路徑通道的 CSI，以精準評估並優化頻譜資源利用，包括在頻譜空洞中實現動態頻譜存取。
- **Spectrum Interference Detection：** 結合多重路徑效應與 CSI 快速偵測與定位干擾來源，強化系統抗干擾能力。

### 5. 高速移動通訊

在高速移動環境中，多重路徑效應與 CSI 的應用包含：

- **Mobile Channel Modeling：** 解析多重路徑效應與 CSI，以建立精準的行動通道模型，支援高速移動通訊。
- **Mobile User Tracking：** 運用多重路徑效應與 CSI 資訊快速追蹤並定位高速移動使用者，提高通訊系統的穩定度與可靠度。

上述應用展現無線通道基礎與 CSI 之間的緊密關聯，有助於持續提升無線通訊系統的整體效能。
