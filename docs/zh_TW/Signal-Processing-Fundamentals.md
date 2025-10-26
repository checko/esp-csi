# 無線通訊系統的組成 [[English]](../en/Signal-Processing-Fundamentals.md) [[简体中文]](../zh_CN/Signal-Processing-Fundamentals.md)

**發射端與接收端：** 產生、傳輸與接收訊號的裝置。
**通道：** 訊號傳遞的媒介，可能是自由空間、空氣或其他介質。

## 訊號的取樣與量化

**取樣：** 將連續訊號轉換為離散訊號的過程。
**量化：** 將離散訊號的幅度值轉換為有限精度的程序。
**奈奎斯特取樣定理：** 取樣頻率必須至少為訊號最高頻率的兩倍。

## 傅立葉轉換

**離散傅立葉轉換（DFT）：** 將離散時間訊號轉換為頻域表示。
**快速傅立葉轉換（FFT）：** 高效率計算 DFT 的演算法。

## 調變與解調技術

**調變：** 將基頻訊號轉換為載波訊號的過程。
**解調：** 從載波訊號中還原基頻訊號的過程。
**常見調變技術：** 例如 Quadrature Amplitude Modulation (QAM)、Phase Shift Keying (PSK)、Orthogonal Frequency Division Multiplexing (OFDM)。

## CSI 子載波的運用

CSI 透過子載波掌握通道資訊的能力，奠基於 Orthogonal Frequency Division Multiplexing (OFDM) 與 Orthogonal Frequency Division Multiplexing Multiple Input Multiple Output (OFDM-MIMO) 等技術。

OFDM 技術將整體頻譜切分為多個彼此正交的子載波，每個子載波獨立傳輸資料。子載波在頻率域保持正交，代表它們之間的干擾極小。藉由在不同子載波上送出資料，OFDM 技術可提升頻譜效率並增強抗干擾能力。

在 OFDM-MIMO 系統中，多根天線同步傳送不同的資料流。這些資料流通過通道到達接收端，並受到多重路徑傳播與衰落等通道效應影響。由於通道對不同子載波的影響各異，可藉由分析這些子載波上的訊號特性取得通道資訊。觀察發射與接收訊號的差異，即可推論通道狀態資訊，包括通道衰落與相位偏移等。

因此，CSI 能夠透過多個子載波與多根天線之間的訊號差異來推斷通道狀態，充分運用 OFDM 與 MIMO 技術的特性。
