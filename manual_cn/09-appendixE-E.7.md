---
sidebarDepth: 2
---

# 附录E. 模型与算法

## E.7 Kinematic, Static 与 Moving-Baseline

RTKLIB在DGPS/DGNSS、Static、Kinematic、Moving-Base工作模式下采用了扩展卡尔曼滤波（EKF），同时也使用了附录E.3中的GNSS信号观测模型以及附录E.5中的对流层和电离层模型。

### E.7.1 EKF公式

**EKF量测更新**。EKF在 $t_k$ 时刻（历元时间）通过观测向量 $y_k$ 估计未知模型参数的状态向量 $\mathbf{x}$ 及其协方差矩阵 $\mathbf{P}$ ：

$\begin{flalign}
& \hat{\mathbf{x}}_k(+) = \hat{\mathbf{x}}_k(-) + \mathbf{K}_k(\mathbf{y}_k - \mathbf{h}(\hat{\mathbf{x}}_k(-))) \tag{E.7.1} \\
& \mathbf{P}_k(+) = (\mathbf{I} - \mathbf{K}_k \mathbf{H}(\hat{\mathbf{x}}_k(-)))\mathbf{P}_k(-) \tag{E.7.2} \\
& \mathbf{K}_k = \mathbf{P}_k(-)(\mathbf{H}(\hat{\mathbf{x}}_k(-))\mathbf{P}_k(-)\mathbf{H}(\hat{\mathbf{x}}_k(-))^T + \mathbf{R}_k)^{-1} \tag{E.7.3}
\end{flalign}$

其中 $\hat{\mathbf{x}}_k$ 和 $\mathbf{P}_k$ 是历元时间的 $t_k$ 估计状态向量和其协方差矩阵。 $−$ 和 $+$ 表示EKF量测更新之前（先验）和之后（后验）。 $\mathbf{h(x)}$ ， $\mathbf{H(x)}$ 和 $\mathbf{R_k}$ 分别是观测模型向量、偏导数矩阵（雅可比矩阵）和观测误差协方差矩阵。

**EKF时间更新**。假设系统模型是线性的，EKF的状态向量及其协方差矩阵的时间更新可以表示为：

$\begin{flalign}
& \hat{\mathbf{x}}_{k+1}(-) = \mathbf{F}_k^{k+1} \hat{\mathbf{x}}_k(+) \tag{E.7.4} \\
& \mathbf{P}_{k+1}(-) = \mathbf{F}_k^{k+1} \mathbf{P}_k(+) \mathbf{F}_k^{k+1^T} + \mathbf{Q}_k^{k+1} \tag{E.7.5}
\end{flalign}$

其中 $\mathbf{F}_k^{k+1}$ 和 $\mathbf{Q}_k^{k+1}$ 是从历元时间 $t_k$ 到 $t_{k+1}$ 的系统状态转移矩阵和系统噪声协方差矩阵。

### E.7.2 双差观测模型

对于短基线（<10公里）的 RTK 场景，通常使用以下双差（DD）观测方程来处理 $L_i$ 的相位差和伪距差。在这些方程中，通过使用双差技术，卫星和接收机的钟差、电离层与对流层影响以及其他次要修正项几乎被完全消除。

$\begin{flalign}
& \Phi_{rb,i}^{jk} = \rho_{rb}^{jk} + \lambda_i (B_{rb,i}^j - B_{rb,i}^k) + d\Phi_{r,i}^s + \varepsilon_\Phi \\
& P_{rb,i}^{jk} = \rho_{rb}^{jk} + \varepsilon_P \tag{E.7.6}
\end{flalign}$

![DD (double-difference) Formulation](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250220-223859.jpg)
<p style="text-align: center;">图E.7-1 双差模型</p> 

其中 $d\Phi_{r,i}^s$ 是载波相位修正项，在短基线情况下可以忽略，除了 PCV项 （因使用不同接收机天线所导致）。为了在方程中获得几何距离 $\rho_r^s$ ，基站位置 $r_b$ 需要设置为固定值，除非工作模式为动基线模式（Moving-Base）。

需要注意的是，接收机之间的单差（SD）最好在有相同历元时间的观测量之间进行。然而由于不同的接收机钟差，接收机并不能完全同步。在一些典型情况下，流动站的采样间隔与基站不同，例如很多流动站接收机支持1~10 Hz。RTKLIB在处理单差时，会选择在流动站历元时间之前或等于该历元时间的最后一个基站观测量。流动站和基站之间的历元时间差可以称为差分龄期（Age of Differential），相关的配置选项为“MAX Age of Diff”。随着差分龄期的增加，由于卫星钟漂和电离层延迟，解的精度会逐渐降低。RTKLIB使用广播星历种的的卫星时钟参数对单差观测量中的卫星钟漂进行修正。

至于卫星间单差（也即流动站双差），RTKLIB在每个历元的基础上选择一个具有最大仰角的参考卫星。请注意，不同导航系统之间的卫星会单独进行卫星单差的运算，这是因为即使不同导航系统的信号具有相同的载波频率，接收机通常对这些信号有不同的接收机群延迟ISB（inter system bias）。

假设流动站和基站均使用三频GPS/GNSS接收机，待估计的未知状态向量 $x$ 可定义为：

$\begin{equation}
x = (r_r^T, v_r^T, B_1^T, B_2^T, B_5^T)^T \tag{E.7.7}
\end{equation}$

其中 $B_i = (B_{rb,i}^1, B_{rb,i}^2, \ldots, B_{rb,i}^m)^T$ 是 $L_i$ 单差（SD）载波相位模糊度（cycle）。在RTKLIB的实现中，它内部使用SD载波相位模糊度而非双差（DD），主要是为了避免参考卫星切换所造成的麻烦。SD模糊度还有助于解决GLONASS FDMA信号中的整周模糊度。

观测向量 $y$ 中包含了双差相位和双差伪距观测值：

$\begin{equation}
y = (\Phi_1^T, \Phi_2^T, \Phi_5^T, P_1^T, P_2^T, P_5^T)^T \tag{E.7.8}
\end{equation}$

其中：

$\Phi_i = (\phi_{rb,i}^{12}, \phi_{rb,i}^{13}, \phi_{rb,i}^{14}, \ldots, \phi_{rb,i}^{1m})^T$ <br>
$P_i = (p_{rb,i}^{12}, p_{rb,i}^{13}, p_{rb,i}^{14}, \ldots, p_{rb,i}^{1m})^T$

### E.7.3 EKF量测更新

通过式(E.7.6)，观测模型向量 $\mathbf{h(x)}$ 、偏导数矩阵 $\mathbf{H(x)}$ （雅可比矩阵）和观测误差协方差矩阵 $\mathbf{R}$ 可以表示为：

$\begin{equation}
\mathbf{h(x)} = (\mathbf{h}_{\Phi,1}^T, \mathbf{h}_{\Phi,2}^T, \mathbf{h}_{\Phi,5}^T, \mathbf{h}_{p,1}^T, \mathbf{h}_{p,2}^T, \mathbf{h}_{p,5}^T)^T \tag{E.7.9}
\end{equation}$

$\begin{equation}
\mathbf{H(x)} = \left.\frac{\partial \mathbf{h(x)}}{\partial \mathbf{x}}\right|_{\mathbf{x}=\hat{\mathbf{x}}} = 
\begin{pmatrix}
-\mathbf{DE} & 0 & \lambda_1 \mathbf{D} & 0 & 0 \\
-\mathbf{DE} & 0 & 0 & \lambda_2 \mathbf{D} & 0 \\
-\mathbf{DE} & 0 & 0 & 0 & \lambda_5 \mathbf{D} \\
-\mathbf{DE} & 0 & 0 & 0 & 0 \\
-\mathbf{DE} & 0 & 0 & 0 & 0
\end{pmatrix} \tag{E.7.10}
\end{equation}$

$\begin{equation}
\small
\mathbf{R} = 
\begin{pmatrix}
\mathbf{DR}_{\Phi,1}\mathbf{D}^T & & & & \\
& \mathbf{DR}_{\Phi,2}\mathbf{D}^T & & & \\
& & \mathbf{DR}_{\Phi,5}\mathbf{D}^T & & \\
& & & \mathbf{DR}_{p,1}\mathbf{D}^T & \\
& & & & \mathbf{DR}_{p,2}\mathbf{D}^T & \\
& & & & & \mathbf{DR}_{p,5}\mathbf{D}^T
\end{pmatrix} \tag{E.7.11}
\end{equation}$

其中：

$h_{\Phi,i} = 
\begin{pmatrix}
\rho_{rb}^{12} + \lambda_i (B_{rb}^1 - B_{rb}^2) \\
\rho_{rb}^{13} + \lambda_i (B_{rb}^1 - B_{rb}^3) \\
\vdots \\
\rho_{rb}^{1m} + \lambda_i (B_{rb}^1 - B_{rb}^m)
\end{pmatrix}
, \quad
h_{P,i} = 
\begin{pmatrix}
\rho_{rb}^{12} \\
\rho_{rb}^{13} \\
\vdots \\
\rho_{rb}^{1m}
\end{pmatrix}$

$D = 
\begin{pmatrix}
1 & -1 & 0 & \cdots & 0 \\
1 & 0 & -1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
1 & 0 & 0 & \cdots & -1
\end{pmatrix}$： SD (单差) 矩阵

$R_{\Phi,i} = \text{diag}(2\sigma_{\Phi,i}^{1^2}, 2\sigma_{\Phi,i}^{2^2}, \ldots, 2\sigma_{\Phi,i}^{m^2})$ <br>
$R_{P,i} = \text{diag}(2\sigma_{P,i}^{1^2}, 2\sigma_{P,i}^{2^2}, \ldots, 2\sigma_{P,i}^{m^2})$ <br>
$\sigma_{\Phi,i}^S$： $L_i$ 相位差观测误差的标准差（m） <br>
$\sigma_{P,i}^S$： $L_i$ 伪距观测误差的标准差（m）

通过求解式(E.7.1)中的量测更新，可以获得 $t_k$ 时刻估计的流动站天线位置、速度和浮点单差（载波相位）模糊度。

### E.7.4 EKF时间更新

在RTKLIB中，设置了接收机动态的 Kinematic 模式（Positioning Mode = Kinematic， REC Dynamics = ON），EKF的时间更新（E.7.2）表示为：

$\begin{flalign}
\small
F_k^{k+1} = 
\begin{pmatrix}
I_{3 \times 3} & I_{3 \times 3} \tau_r &                       \\
               & I_{3 \times 3}        &                       \\
               &                       & I_{(3m-3)\times(3m-3)}
\end{pmatrix}, \quad
Q_k^{k+1} = 
\begin{pmatrix}
0_{3 \times 3} & \\
& Q_v & \\
& & 0_{(3m-3) \times (3m-3)}
\end{pmatrix} \tag{E.7.12}
\end{flalign}$

其中：

$Q_v = E_r^T \text{diag}(\sigma_{ve}^2 \tau_r, \sigma_{vn}^2 \tau_r, \sigma_{vu}^2 \tau_r) E_r$

$\tau_r = t_{k+1} - t_k$ 是GPS/GNSS接收机采样间隔（s）， $(\sigma_{ve}, \sigma_{vn}, \sigma_{vu})$ 是流动站速度噪声的东西、南北、垂直分量的标准差（m/s/√s）。

对于不考虑接收机动态的 Kinematic 模式（Positioning Mode = Kinematic, REC Dynamics = OFF），方程(E.7.9)被替换为：

$\begin{flalign}
\small
F_k^{k+1} = 
\begin{pmatrix}
I_{3 \times 3} & \\
& I_{3 \times 3} & \\
&& I_{(3m-3) \times (3m-3)}
\end{pmatrix}, \quad
Q_k^{k+1} = 
\begin{pmatrix}
\infty_{3 \times 3} & \\
& 0_{3 \times 3} & \\
&& 0_{(3m-3) \times (3m-3)}
\end{pmatrix} \tag{E.7.13}
\end{flalign}$

为了避免因将无限大的过程噪声添加到接收机位置的方差而导致数值不稳定，接收机位置状态在每个历元被重置为初始猜测值，并在 RTKLIB 中添加了足够大的过程噪声（$10^4 m^2$）。初始位置来源于单点定位，这用于避免非线性测量模型的迭代（如果没有设置初始位置，并且位置方差设置无穷大，就要靠量测更新多次迭代算出一个位置初值）。

在 Static 模式（Positioning Mode = Static）中，方程(E.7.10)被简单地替换为：

$\begin{flalign}
\small
F_k^{k+1} = 
\begin{pmatrix}
I_{3 \times 3} & \\
& I_{3 \times 3} & \\
&& I_{(3m-3) \times (3m-3)}
\end{pmatrix}, \quad
Q_k^{k+1} = 
\begin{pmatrix}
0_{3 \times 3} & \\
& 0_{3 \times 3} & \\
&& 0_{(3m-3) \times (3m-3)}
\end{pmatrix} \tag{E.7.14}
\end{flalign}$

在 Instantaneous 糊度解算模式（nteger Ambiguity Resolution = Instantaneous）下，单差载波相位模糊度 $B_i$ 的时间更新处理方式与上述略有不同。在这种模式下，载波相位模糊度状态的值不会通过EKF时间更新传递到下一个历元。模糊度在每个历元被重置为初始猜测值，并且在方差中添加了足够大的过程噪声（$10^4 m^2$）。如果在测量数据中检测到周跳，相应的单差载波相位模糊度状态也会被重置为初始值。RTKLIB通过输入测量数据中的LLI（失锁指示）和双频测量的几何无关LC（线性组合）相位来探测周跳。周跳阈值可以通过配置选项“Slip Thres”来更改。

### E.7.5 整周模糊度解算

EKF量测更新获得估计状态后，浮点载波相位模糊度（简称浮点模糊度）可以被解算为整数值，以提高精度和收敛速度。目前相关的配置选项是Integer Ambiguity Res，它可以设置为Continuous, Instantaneous 或 Fix and Hold 模式。首先，估计状态（单差和位置）及其协方差矩阵通过以下方式转换为双差（DD）形式：

$\begin{equation}
\hat{x}_k^{\prime} = G \hat{x}_k(+) = (\hat{r}_r^T, \hat{v}_r^T, \hat{N}^T)^T \tag{E.7.15}
\end{equation}$

$\begin{equation}
P_k^{\prime} = G P_k(+) G^T = 
\begin{pmatrix}
Q_R & Q_{NR} \\
Q_{RN} & Q_N
\end{pmatrix} \tag{E.7.16}
\end{equation}$

其中：

$G = 
\begin{pmatrix}
I_{6 \times 6} &  & & \\
& D & & \\
& & D & \\
& & & D
\end{pmatrix}$： SD-DD 转换矩阵

在这个转换过程中，单差载波相位模糊度被转换为双差形式，以消除接收机初始相位项（initial  phase  terms），从而获得双差整数模糊度 $\hat{N}$ 及其协方差 $Q_N$。

::: info “单差转双差以消除接收机初始相位项”的含义
载波相位测量值不仅包含几何距离、模糊度和钟差，还包括接收机硬件引入的初始相位偏差（Initial Phase Bias）。这是由于接收机的本地振荡器（Local Oscillator）在测量载波相位时有一个未知的起始相位。它应该可以被吸收到接收机钟差项中。

单差能消除卫星钟差和短基线情形下的大部分大气延时误差，而单差转双差我们更关心的是它能进一步消除掉接收机钟差。
:::

在上述公式中，通过求解一个整数最小二乘（ILS）问题，可以得到最合适的整数模糊度向量 $\stackrel{\smile}{N}$，该问题表示为：

$\begin{equation}
\stackrel{\smile}{N} = \arg\min_{N \in \mathbb{Z}} (N - \hat{N})^T Q_N^{-1} (N - \hat{N}) \tag{E.7.17}
\end{equation}$

其中，$\mathbb{Z}$ 表示整数集。

为了解决 ILS 问题， RTKLIB 采用了一种高效且应用广泛的搜索策略 LAMBDA [66]及其扩展 MLAMBDA [67]。 LAMBDA 和 MLAMBDA 提供了线性变换和变换空间中进行树搜索的组合，从而缩小整数向量搜索空间。

解算得到的整数向量解通过以下简单的“Ratio-Test”进行验证。在“Ratio-Test”中，比率因子 $R$ 定义为次优解 $N_2$ 的加权残差平方和最优 $N$ 的加权残差平方和的比值，用于检查解的可靠性。验证阈值 $R_{thres}$ 可以通过配置选项“Min Ratio to Fix Ambiguity”进行设置。原始版本的 RTKLIB 仅支持固定的阈值，而在 demo5 改进版本中，能够支持自适应 Ratio 。

$\begin{equation}
R = \frac{(\stackrel{\smile}{N}_2 - \hat{N})^T Q_N^{-1} (\stackrel{\smile}{N}_2 - \hat{N})}{(\stackrel{\smile}{N} - \hat{N})^T Q_N^{-1} (\stackrel{\smile}{N} - \hat{N})} > R_{thres} \tag{E.7.18}
\end{equation}$

验证通过后，Rover的固定（FIX）解 $\stackrel{\smile}{r}_r$ 和 $\stackrel{\smile}{v}_r$ 通过求解以下方程获得。如果验证失败，RTKLIB 则输出浮点（FLOAT）解 $\hat{r}_r$ 和 $\hat{v}_r$ 代替。

$\begin{equation}
\left( \begin{array}{c} \stackrel{\smile}{r}_r \\ \stackrel{\smile}{v}_r \end{array} \right) = \left( \begin{array}{c} \hat{r}_r \\ \hat{v}_r \end{array} \right) - Q_{RN} Q_N^{-1} (\hat{N} - \stackrel{\smile}{N}) \tag{E.7.19}
\end{equation}$

如果配置选项设置为“Fix and Hold”模式（Integer Ambiguity Res = Fix and Hold），并且固定解通过之前的验证通过，则双差载波相位模糊度参数会被严格约束到固定的整数值。为此，RTKLIB 会输入以下“伪（pseudo"）”观测值到 EKF，并通过公式 (E.7.1) 更新 EKF：

$\begin{equation}
y = \stackrel{\smile}{N} \tag{E.7.20}
\end{equation}$

$\begin{equation}
h(x) = G x \tag{E.7.21}
\end{equation}$

$\begin{equation}
H(x) = G \tag{E.7.22}
\end{equation}$

$\begin{equation}
R = \text{diag}(\sigma_c^2, \sigma_c^2, \sigma_c^2, ...) \tag{E.7.23}
\end{equation}$

其中：

$G = 
\begin{pmatrix}
0 & D & & \\
0 & & D & \\
0 & & & D
\end{pmatrix}$：SD to DD 转换矩阵

$\sigma_c$ : 约束到固定的整数模糊度误差（≈ 0.001 cycle）

“Fix and Hold”模式最初在 RTKLIB 2.4.0 版本中引入，它能显著提高接收机在运动状态下的固定率。

### E.7.6 长基线双差观测模型

对于接收机r和基站b之间的长基线处理，可以形成类似于短基线双差（DD）模型的以下双差测量方程：

$\begin{align}
& \phi_{rb,i}^{jk} = \rho_{rb}^{jk} - I_{rb,k}^{jk} T_{rb}^{jk} + \lambda_i (B_{rb,i}^j - B_{rb,i}^k) d\Phi_{r,i}^s + \varepsilon_\phi \\
& P_{rb,i}^{jk} = \rho_{rb}^{jk} + I_{rb,i}^{jk} T_{rb}^{jk} + \varepsilon_P \tag{E.7.24}
\end{align}$

其中$I_{r,i}^s$和$T_r^s$作为$L_i$电离层延迟（m）和对流层延迟（m）被添加到短基线双差模型中。对于超过100公里的基线，应使用精确的星历书来减轻广播星历误差。在载波相位修正项$\Phi_{r,i}^s$中，对于超过500公里的基线应考虑地球潮效应。为了消除电离层项，有时会形成无电离层LC（线性组合）。然而，RTKLIB不使用这种显式LC，而是通过EKF直接估计基线处理的双频或三频测量的电离层项。

长基线情况下的未知状态向量 $x$ 也可以设定为：

$\begin{flalign}
\small
x = \left(r_r{}^T, v_r{}^T, Z_r, G_{N, r}, G_{E, r}, Z_b, G_{N, b}, G_{E, b}, I^T, B_1{}^T, B_2{}^T, B_5{}^T\right)^T \tag{E.7.25}
\end{flalign}$

其中 $Z_r$ 和 $Z_b$ 是 ZTD（zenith total delay）在Rover和基站位置的值，$G_{N, r}$、$G_{E, r}$、$G_{N, b}$ 和 $G_{E, b}$ 是对流层梯度的北向和东向分量。$I = (I_{rb}^{1}, I_{rb}^{2}, \ldots, I_{rb}^{m})^{T}$ 是 $L_1$ 频率（$m$）下的SD垂直电离层延迟。

观测模型向量 $h(x)$ 和偏导数矩阵 $H(x)$ 可以表示为：

$\begin{flalign}
h(x) = (h_{\Phi,1}^T, h_{\Phi,2}^T, h_{\Phi,5}^T, h_{p,1}^T, h_{p,2}^T, h_{p,5}^T)^T \tag{E.7.26}
\end{flalign}$

$\begin{flalign}
\small
h_{\phi,i} = \begin{pmatrix}
\rho_{rb}^{12} + T_{rb}^{12} - \gamma_k (m_I^1 I_{rb}^1 - m_I^2 I_{rb}^2) + \lambda_i (B_{rb,i}^1 - B_{rb,i}^2) + d\Phi_{rb,i}^{12} \\
\rho_{rb}^{13} + T_{rb}^{13} - \gamma_k (m_I^1 I_{rb}^1 - m_I^3 I_{rb}^3) + \lambda_i (B_{rb,i}^1 - B_{rb,i}^3) + d\Phi_{rb,i}^{13} \\
\vdots \\
\rho_{rb}^{1m} + T_{rb}^{1m} - \gamma_k (m_I^1 I_{rb}^1 - m_I^m I_{rb}^m) + \lambda_i (B_{rb,i}^1 - B_{rb,i}^m) + d\Phi_{rb,i}^{1m}
\end{pmatrix} \tag{E.7.27}
\end{flalign}$

$\begin{flalign}
h_{p,i} = \begin{pmatrix}
\rho_{rb}^{12} + T_{rb}^{12} + \gamma_k (m_I^1 I_{rb}^1 - m_I^2 I_{rb}^2) \\
\rho_{rb}^{13} + T_{rb}^{13} + \gamma_k (m_I^1 I_{rb}^1 - m_I^3 I_{rb}^3) \\
\vdots \\
\rho_{rb}^{1m} + T_{rb}^{1m} + \gamma_k (m_I^1 I_{rb}^1 - m_I^m I_{rb}^m)
\end{pmatrix} \tag{E.7.28}
\end{flalign}$

$\begin{flalign}
\small
H(x) = \begin{pmatrix}
-DE & 0 & DM_{T,r} & DM_{T,b} & -\gamma_1 DM_I & \lambda_1 D & &  \\
-DE & 0 & DM_{T,r} & DM_{T,b} & -\gamma_2 DM_I & & \lambda_2 D &  \\
-DE & 0 & DM_{T,r} & DM_{T,b} & -\gamma_5 DM_I & & & \lambda_5 D  \\
-DE & 0 & DM_{T,r} & DM_{T,b} & \gamma_1 DM_I  & & & \\
-DE & 0 & DM_{T,r} & DM_{T,b} & \gamma_2 DM_I  & & & \\
-DE & 0 & DM_{T,r} & DM_{T,b} & \gamma_5 DM_I  & & &
\end{pmatrix} \tag{E.7.29}
\end{flalign}$

其中：

$\gamma_k = \lambda_k^2 / \lambda_1^2$

$
\small
M_{T,r} = \begin{pmatrix}
m_{W G,r}^1(E l_r^1) & m_{W,r}^1(E l_r^1) \cot E l_r^1 \cos A z_r^1 & m_{W,r}^1(E l_r^1) \cot E l_r^1 \sin A z_r^1 \\
m_{W G,r}^2(E l_r^2) & m_{W,r}^2(E l_r^2) \cot E l_r^2 \cos A z_r^2 & m_{W,r}^2(E l_r^2) \cot E l_r^2 \sin A z_r^2 \\
\vdots & \vdots & \vdots \\
m_{W G,r}^m(E l_r^m) & m_{W,r}^m(E l_r^m) \cot E l_r^m \cos A z_r^m & m_{W,r}^m(E l_r^m) \cot E l_r^m \sin A z_r^m
\end{pmatrix}
$

$M_I = (m_I^1, m_I^2, \ldots, m_I^m)^T$

对于长基线情况，EKF 的时间更新表达式为：

$\begin{equation}
\small
F_k^{k+1} = \begin{pmatrix}
I_{3 \times 3} & I_{3 \times 3} \tau_r &                &                & \\
               & I_{3 \times 3}        &                &                & \\
               &                       & I_{6 \times 6} &                & \\
               &                       &                & I_{m \times m} &  \\
               &                       &                & & I_{(3m-3) \times (3m-3)}
\end{pmatrix} \tag{E.7.30}
\end{equation}$

$\begin{equation}
Q_k^{k+1} = \begin{pmatrix}
0_{3 \times 3} & & & & \\
& Q_v & & & \\
& & Q_T & & \\
& & & Q_I & \\
& & & & 0_{(3m-3) \times (3m-3)}
\end{pmatrix} \tag{E.7.31}
\end{equation}$

其中，$Q_T$和$Q_I$分别为电离层和对流层项的过程噪声协方差矩阵。在该方程中，Rover和基站的ZTD（天顶总延迟）及梯度参数，以及每颗卫星的SD（单差）垂直电离层延迟，均被简单地建模为随机游走过程。此外，为了估计电离层和对流层项，在2.4.1版本中为长基线处理增加了一个“部分固定”功能。这意味着只有部分模糊度被解算为整数值，其余未固定的模糊度仍保持为浮点值。为了确定模糊度是否固定，RTKLIB中实现了一个使用卫星仰角的简单标准。如果卫星的仰角低于设定的阈值，则该卫星的模糊度不会被固定。只有仰角高于阈值的卫星的模糊度才会被解算为整数。模糊度解算的仰角阈值可以通过配置选项“最小仰角固定模糊度”（Min Elevation to Fix Amb）以及“最小仰角保持模糊度”（Min Elevation to Hold Amb）来设置，以控制“固定和保持”（Fix and Hold）功能。

### E.7.7 动态基线模型

移动基线模式通常在Rover和基站接收器都在移动且仅需要Rover相对于基站的相对位置时使用。通过在移动平台上安装两个天线，可以利用移动基线模式确定精确姿态。在 RTKLIB 中，如果将配置选项“Positioning Mode”设置为“Moving-Base”，则应用移动基线模式。

在移动基线模式下，基站位置不是固定的，而是通过逐历元的单点定位过程估计的。一旦获得基站位置，将基站位置固定为估计位置，并通过 (1)-(5) 中描述的短基线运动模式估计Rover位置。在这种情况下，只有相对位置是有意义的，也就是说，Rover和基站的绝对位置解的精度仅与点定位模式的解相同。

除了对移动基线模式的简单实现外，RTKLIB 还校正了Rover与基站之间的时间差异。Rover接收机和基站接收机不同步。接收机时钟差通常最大可达 2 毫秒。对于非常快速移动的平台，不同步的时钟会导致精度下降。为了校正时钟差，在基线处理之前，基站位置$r_b$通过以下公式进行校正：

$\begin{equation}
\boldsymbol{r}_b(t_r) = \boldsymbol{r}_b(t_b) + \boldsymbol{v}_b(t_b)(t_r - t_b) \tag{E.7.32}
\end{equation}$

其中，$t_r$和 $t_b$分别是通过单点定位过程估计的Rover和基站的信号接收时间。$v_b{t_b}$也是通过多普勒测量估计的基站速度。对于通过移动基线模式进行姿态确定的情况，如果启用配置选项“Baseline Length Constraint”，则可以应用基线长度约束。该约束在 EKF 量测更新中应用以下伪测量：

$\begin{equation}
\boldsymbol{y} = (\boldsymbol{r}_{\text{baseline}}) \tag{E.7.33}
\end{equation}$

$\begin{equation}
\boldsymbol{h(x)} = (|\boldsymbol{r}_r(t_r) - \boldsymbol{r}_b(t_r)|) \tag{E.7.34}
\end{equation}$

$\begin{equation}
\boldsymbol{H} = \left( \frac{(\boldsymbol{r}_r(t_r) - \boldsymbol{r}_b(t_r))^T}{|\boldsymbol{r}_r(t_r) - \boldsymbol{r}_b(t_r)|} \right) \tag{E.7.35}
\end{equation}$

$\begin{equation}
\boldsymbol{R} = (\sigma_r^2) \tag{E.7.36}
\end{equation}$

其中，$r_{baseline}$ 是给定的预设基线长度（单位：m），$\sigma_r$是基线长度的约束（单位：m）。为了应对非常短的基线长度情况下的非线性问题，可以通过将配置选项“Number of Filter Iteration”（滤波器迭代次数）设置为大于1，来支持扩展卡尔曼滤波器（EKF）的迭代量测更新。

<GiscusTalk />
