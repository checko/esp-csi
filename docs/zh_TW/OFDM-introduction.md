# OFDM 介紹[[English]](../en/OFDM-introduction.md) [[简体中文]](../zh_CN/OFDM-introduction.md)

要理解 CSI 的原理，必須先掌握 Wi-Fi 實體層的基礎知識。OFDM 中的 "O" 代表 Orthogonal（正交），因此先從正交的定義開始說明。

## 正交的定義

在數學上，若兩個函數（或向量）的內積在某區間為零，即稱為正交。具體而言，對於函數 $f(x)$ 與 $g(x)$ 在區間 $[a, b]$ 上，其內積定義如下：
$\langle f, g \rangle = \int_{a}^{b} f(x) \cdot g(x) \, dx$

當此積分為零時，表示 $f(x)$ 與 $g(x)$ 在 $[a, b]$ 上正交。

## 簡單的正交模型

從最簡單的模型出發：只要 $\omega_1 \ne \omega_2$，$\sin(\omega_1 t)$ 與 $\sin(\omega_2 t)$ 在一個週期內互相正交。

為了證明在一個週期內的正交性，考慮這兩個正弦函數在 $[0, T]$ 區間的內積：
$\int_{0}^{T} \sin(\omega_1 t) \sin(\omega_2 t) \, dt$

運用三角恆等式，可將積分化簡。利用
$$\sin(A) \sin(B) = \frac{1}{2}[\cos(A-B) - \cos(A+B)]$$
可將積分展開為兩個餘弦函數的差：
$$\int_{0}^{T} \sin(\omega_1 t) \sin(\omega_2 t) \, dt = \frac{1}{2} \int_{0}^{T} [\cos((\omega_1 - \omega_2) t) - \cos((\omega_1 + \omega_2) t)] \, dt$$

由於 $\omega_1$ 與 $\omega_2$ 為不同頻率，$(\omega_1 - \omega_2)$ 與 $(\omega_1 + \omega_2)$ 均不為零，因此這兩個餘弦函數在一個週期內的積分皆為零。代表 $\sin(\omega_1 t)$ 與 $\sin(\omega_2 t)$ 在一個週期內的內積為零，即互相正交。

## 時域 OFDM

在簡化模型中，$\sin(\omega_1 t)$ 與 $\sin(\omega_2 t)$ 互相正交。在時間區間 $[0, T]$ 內，最簡單的傳輸方式是振幅調變：以 $\sin(\omega_1 t)$ 傳送訊號 $a$，得到 $a \cdot \sin(\omega_1 t)$；以 $\sin(\omega_2 t)$ 傳送訊號 $b$，得到 $b \cdot \sin(\omega_2 t)$。這些正弦波作為載波，是發射端與接收端事先約定的資訊，亦即子載波。**調變到子載波上的振幅訊號 $a$ 與 $b$ 才是實際要傳輸的資訊。** 經過通道的訊號為 $a \cdot \sin(\omega_1 t) + b \cdot \sin(\omega_2 t)$。在接收端，透過與 $\sin(\omega_1 t)$、$\sin(\omega_2 t)$ 進行積分即可解調出 $a$ 與 $b$。下列為正交性應用於 OFDM 的數學說明：

接收訊號時，正交性讓不同頻率的訊號可以獨立解碼。具體來說，傳輸訊號 $s(t)$ 由兩個子訊號組成：
$a \sin(\omega_1 t) + b \sin(\omega_2 t)$
其中 $a$ 與 $b$ 為振幅，$\omega_1$ 與 $\omega_2$ 為兩個子訊號的頻率。

以下示範解碼子訊號 $a$。

接收端將訊號乘上解調頻率 $\sin(\omega_1 t)$ 並積分，以還原子訊號 $a \sin(\omega_1 t)$。

由於正交性，$\sin(\omega_1 t)$ 與 $\sin(\omega_2 t)$ 在一個週期的內積為零：
$\int_{0}^{T} \sin(\omega_1 t) \sin(\omega_2 t) \, dt = 0$

因此在積分過程中，$\sin(\omega_1 t) \sin(\omega_2 t)$ 項會消失，僅剩下 $a \sin^2(\omega_1 t)$。

我們知道 $\sin^2(\omega_1 t)$ 在一個週期內的積分為週期的一半：
$\int_{0}^{T} \sin^2(\omega_1 t) \, dt = \frac{T}{2}$

因此解碼後的訊號為：
$a \times \frac{T}{2}$

透過此方式，接收端可分別獨立解碼每個子訊號 $a$ 與 $b$，因為正交性保證其他子訊號不會對積分造成影響。

以 $\sin(\omega_1 t)$ 與 $\sin(\omega_2 t)$ 為例，可展示從直觀模型到抽象概念的轉換。簡單的正交模型是所有複雜理論的基礎。

接著，將 $\sin(t)$ 與 $\sin(2t)$ 的模型擴展為更多子載波序列 $\{ \sin(2T \Delta f \cdot t), \sin(2T \Delta f \cdot 2t), \sin(2T \Delta f \cdot 3t), \ldots, \sin(2T \Delta f \cdot kt) \}$（例如 $k = 16, 256, 1024$），其中 $2\pi$ 為常數，$\Delta f$ 為預先選定的載波頻率間隔，$T$ 為週期，$k$ 為序列的最大索引。

多函數的正交性建立在兩兩正交之上。若一組函數 $\{f_1(t), f_2(t), \ldots, f_n(t)\}$ 在區間內兩兩正交，則任意 $i \ne j$ 皆滿足：
$\int_{a}^{b} f_{i}(t) f_{j}(t) dt = 0$
代表任何兩個不同函數 $f_i(t)$ 與 $f_j(t)$ 的內積為零。只要 $\Delta f \ne 0$，上述子載波序列的正交性即可輕易證明。

頻率間隔的影響：若 $\Delta f$ 足夠小，使得 $2T \Delta f \cdot k$ 覆蓋整個頻譜，就能確保所有正弦波在頻率上不重疊。因此每個正弦波在完整週期內的積分為零，維持正交性。

下一步引入 $\cos(t)$。可輕易證明 $\cos(t)$ 與 $\sin(t)$ 以及整個 $\sin(kt)$ 正交族互相正交。同樣地，$\cos(kt)$ 與整個 $\sin(kt)$ 正交族互相正交。

因此，序列模型得以擴展為 $\{\sin(2T \Delta f t), \sin(2T \Delta f \cdot 2t), \ldots, \sin(2T \Delta f kt), \cos(2T \Delta f t), \cos(2T \Delta f \cdot 2t), \ldots, \cos(2T \Delta f kt)\}$。

在擴展正交序列 $\sin(kt)$ 與 $\cos(kt)$ 後，它們只是傳輸的「媒介」。實際需要傳輸的資訊仍須調變到這些載波上，也就是 $\sin(t), \sin(2t), \ldots, \sin(kt)$ 分別以訊號 $a_1, a_2, \ldots, a_k$ 進行振幅調變，而 $\cos(t), \cos(2t), \ldots, \cos(kt)$ 則以訊號 $b_1, b_2, \ldots, b_k$ 調變。當這 $2n$ 個正交通道同時傳輸時，形成的波形 $f(t)$ 如下（式 1-1）：
$f(t)=a_1 \sin(\Delta f \cdot t)+a_2 \sin(\Delta f \cdot 2t)+ \cdots + a_k \sin(\Delta f \cdot kt)+ b_1 \cos(\Delta f \cdot t)+b_2 \cos(\Delta f \cdot 2t)+ \cdots + b_k \cos(\Delta f \cdot kt)$

由於正交通道可分離不同頻率的子訊號，接收端透過傅立葉轉換即可分別取得振幅訊號 $a_i$ 與 $b_i$。

考量現代訊號處理同時在時間與頻率域採離散化，式 (1-1) 可寫為：

$s[n]= \sum_{k=0}^{N-1} x[k] e^{j2\pi nk/N}$

其中 $x[k]$ 為子訊號的振幅，$N$ 為傅立葉轉換點數。

因此，OFDM 的原理是透過傅立葉轉換將時間訊號分解為不同頻率的子訊號，藉由正交性達成平行傳輸，並在接收端透過反向傅立葉轉換還原原始訊號。此作法可顯著提升頻譜效率與抗干擾能力。

## OFDM 在無線通訊中的應用

OFDM 廣泛應用於無線通訊，尤其是 Wi-Fi、4G 與 5G。其主要優點包括：

1. **抗多重路徑干擾：** OFDM 將訊號分解為多個在不同頻率傳輸的子訊號，多重路徑干擾僅會影響部分子訊號，接收端可透過等化演算法降低干擾。
2. **高頻譜效率：** OFDM 的正交性確保子訊號在頻譜上可互相重疊而不互相干擾，提高頻譜利用率。
3. **彈性子載波配置：** OFDM 可依通道狀況動態配置子載波，增進傳輸的彈性與效率。

總結來說，OFDM 利用正交分頻多工技術顯著提升頻譜效率與抗干擾能力，是現代無線通訊的核心技術之一。
