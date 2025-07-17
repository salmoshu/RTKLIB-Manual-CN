---
sidebarDepth: 2
---

# 附录E. 模型与算法

## E.5 对流层与电离层模型

### E.5.1. 对流层模型

标准大气可以表示为：

$\begin{equation}
p = 1013.25 \times (1 - 2.2557 \times 10^{-5} h)^{5.2568} \tag{E.5.1}
\end{equation}$

$\begin{equation}
T = 15.0 - 6.5 \times 10^{-3} h + 273.15 \tag{E.5.2}
\end{equation}$

$\begin{equation}
e = 6.108 \times \exp\left(\frac{17.15 T - 4684.0}{T - 38.45}\right) \times \frac{h_{rel}}{100} \tag{E.5.3}
\end{equation}$

其中 $p$ 是总压强（hPa），$T$ 是空气的绝对温度（K），$h$ 是大地高程（高于平均海平面，单位：m），$e$ 是水蒸气的偏压（hPa），$h_{rel}$ 是相对湿度。对流层延迟 $T^s$ 由Saastamoinen模型表示，基于从标准大气导出的 $p$、$T$ 和 $e$。

$\begin{equation}
T_r^s = \frac{0.002277}{\cos z} \left\{ p + \left(\frac{1255}{T} + 0.05\right) e - \tan^2 z \right\} \tag{E.5.4}
\end{equation}$

其中 $z$ 是天顶角（弧度），定义为 $z = \pi/2 - E l_r$。

标准大气和Saastamoinen模型在处理选项“Troposphere Correction”设置为“Saastamoinen”时适用，其中大地高程由椭球高程近似，且相对湿度固定为70%。

### E.5.2 SBAS 对流层模型

如果处理选项“对流层校正”设置为“SBAS”，则应用 SBAS 接收器规范中定义的 SBAS 对流层模型。该模型常被称为“MOPS 模型”。参考 [8] A.4.2.4 获取详情。

### E.5.3 精确对流层模型

如果处理选项“对流层校正”设置为“估计 ZTD”或“估计 ZTD+Grad”，则应用更精确的对流层模型，并使用严格的映射函数如下：

$\begin{equation}
m(El^s) = m_w(El_r^s)[1 + \cot El_r^s(G_{N,r} \cos AZ_r^s + G_{E,r} \sin AZ_r^s)] \tag{E.5.5}
\end{equation}$

$\begin{equation}
T_r^s = m_H(El_r^s)Z_{H,r} + m_v(El_r^s)(Z_{T,r} - Z_{H,r}) \tag{E.5.6}
\end{equation}$

其中：

$Z_{T,r}$ : 对流层天顶总延迟 (m)<br>
$Z_{H,r}$ : 对流层天顶静水延迟 (m)<br>
$m_H(El)$ : 静水映射函数<br>
$m_W(El)$ : 湿映射函数

在 RTKLIB 中，对流层天顶静水延迟由 Saastamoinen 模型（E.5.4）给出，假设天顶角 $z = 0$ 且相对湿度 $P_{rel} = 0$。对于映射函数，RTKLIB 默认使用 Niell 映射函数（Niell mapping function）[70]。天顶总延迟 $Z_{T,r}$ 和梯度参数 $G_{N,r}$、$G_{E,r}$（在“估计 ZTD+Grad”情况下）作为未知参数在参数估计过程中被估算。对于映射函数，RTKLIB 自版本 2.4.2 起可以通过设置编译器选项 -DIERS_MODEL 使用 GMF [71]。

### E.5.4 广播电离层模型

对于单频GNSS用户的电离层校正，GPS和QZSS导航数据包括以下广播电离层参数。

$\begin{equation}
\mathbf{p}_{ion} = (\alpha_0, \alpha_1, \alpha_2, \alpha_3, \beta_0, \beta_1, \beta_2, \beta_3)^T \tag{E.5.5}
\end{equation}$

通过使用这些电离层参数，可以按照以下过程推导出$L_1$电离层延迟$I_r^s$（m）。该模型通常称为Klobuchar模型。

$\begin{equation}
\psi = 0.0137/(El + 0.11) - 0.022 \tag{E.5.6}
\end{equation}$

$\begin{equation}
\varphi_i = \varphi + \psi \cos Az \tag{E.5.7}
\end{equation}$

$\begin{equation}
\lambda_i = \lambda + \psi \sin Az/\cos \varphi_i \tag{E.5.8}
\end{equation}$

$\begin{equation}
\varphi_m = \varphi_i + 0.064 \cos(\lambda_i - 1.617) \tag{E.5.9}
\end{equation}$

$\begin{equation}
t = 4.32 \times 10^4 \lambda_i + t \tag{E.5.10}
\end{equation}$

$\begin{equation}
F = 1.0 + 16.0 \times (0.53 - El)^3 \tag{E.5.11}
\end{equation}$

$\begin{equation}
x = 2\pi(t - 50400)/\sum_{n=0}^{3} \beta_n \varphi_m^n \tag{E.5.12}
\end{equation}$

$\begin{equation}
\small
I_r^s = 
\begin{cases} 
F \times 5 \times 10^{-9} & (|x| > 1.57) \\
F \times (5 \times 10^{-9} + \sum_{n=1}^{4} \alpha_n \varphi_m^n \times (1 - \frac{x^2}{2} + \frac{x^4}{24})) & (|x| \leq 1.57)
\end{cases} \tag{E.5.13}
\end{equation}$

如果处理选项 "Ionosphere Correction" 设置为 "Broadcast" 或 "QZSS Broadcast"。

### E.5.5 SBAS 电离层模型

SBAS 对电离层延迟的校正由消息类型 18（电离层网格点掩模）和消息类型 26（电离层延迟校正）提供。如果处理选项 "Ionosphere Correction" 设置为 "SBAS" 并且这些 SBAS 消息在输入文件中提供，RTKLIB 使用 SBAS 电离层校正。关于模型的算法和 IGPs（电离层网格点）的定义，请参阅 SBAS 接收器规范 [8] 的 A.4.4.9 和 A.4.4.10。

### E.5.6 单层模型

电离层通常被建模为图 E.5-1 所示的简单单层模型。单层模型也被称为薄壳模型。

![Single Layer Ionosphere Model](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250220-165458.jpg)

在模型中，IPP（电离层刺点）的纬度$\phi_{IPP}$（rad）和经度$\lambda_{IPP}$（rad）可以从以下公式推导：

$\begin{equation}
z = \pi/2 - El_r^s \tag{E.5.14}
\end{equation}$

$\begin{equation}
z' = \arcsin\left(\frac{R_E}{R_E+H}\sin z\right) \tag{E.5.15}
\end{equation}$

$\begin{equation}
\alpha = z - z' \tag{E.5.16}
\end{equation}$

$\begin{equation}
\phi_{IPP} = \arcsin(\cos \alpha \sin \phi_r + \sin \alpha \cos \phi_r \cos A z_r^s) \tag{E.5.17}
\end{equation}$

（$\phi_r > 70^\circ$ 且 $\tan \alpha \cos A z_r^s > \tan(\pi/2 - \phi_r)$）或（$\phi_r < -70^\circ$ 且 $-\tan \alpha \cos A z_r^s > \tan(\pi/2 + \phi_r)$）

$\begin{equation}
\lambda_{IPP} = \lambda_r + \pi - \arcsin\frac{\sin \alpha \sin A z_r^s}{\cos \phi_{IPP}} \tag{E.5.18a}
\end{equation}$

（否则）

$\begin{equation}
\lambda_{IPP} = \lambda_r + \arcsin\frac{\sin \alpha \sin A z_r^s}{\cos \phi_{IPP}} \tag{E.5.18b}
\end{equation}$

其中$R_E$是地球的半径（m），$H$是电离层壳体的高度（m）。RTKLIB通常使用$R_E = 6378137$和$H = 350000$。请注意，在该模型中，地球表面或电离层壳体被近似为一个球体。

如果在IPP和时间$t$处给出了VTEC（垂直总电子含量）值$TEC(t, \phi_{IPP}, \lambda_{IPP})$，则$L_i$电离层延迟$I_{r,i}^s)（m）可以表示为：

$\begin{equation}
I_{r,i}^s = \frac{1}{\cos z'}\frac{40.3 \times 10^{16}}{f_i} TEC(t,\phi_{IPP},\lambda_{IPP}) \tag{E.5.19}
\end{equation}$

其中$f_i$是信号的载波频率（Hz）。

VTEC值以几种格式或方程提供。RTKLIB目前仅支持IONEX格式提供的VTEC值。在IONEX格式中，VTEC表示为纬度和经度网格中的点值。RTKLIB通过简单的双线性插值将IONEX数据中的这些网格点值插值到合适的IPP位置。这些VTEC值在IONEX文件的每个历元时间间隔提供。时间插值也应应用于太阳固定坐标系，如下所示：

$\begin{flalign}
\scriptsize
TEC(t,\phi_{IPP},\lambda_{IPP}) = \frac{(t-t_i)TEC(t_i,\phi_{IPP},\lambda_{IPP}+\omega(t-t_i)) + (t_{i+1}-t)TEC(t_{i+1},\phi_{IPP},\lambda_{IPP}+\omega(t-t_{i+1}))}{t_{i+1}-t_i} \tag{E.5.20}
\end{flalign}$

其中，$t_i$ 和 $t_{i+1}$ ($t_i \leq t < t_{i+1}$ ) 是提供的TEC数据的时间，$\omega = 2\pi/86400$ 是太阳相对于地球的旋转速度。如果处理选项 "Ionosphere Correction" 设置为 "IONEX TEC" 并且IONEX数据仅在后处理模式下作为输入文件提供，则应用单层模型和IONEX数据的校正。

### E.5.7 无电离层组合（线性组合）

为了消除GNSS信号测量中的电离层影响，通常在GNSS数据处理中利用双频测量的LC（线性组合）。$L_i$和$L_j$伪距和相位差的无电离层LC表示为：

$\begin{equation}
P_{r,LC}^s = C_i P_{r,i}^s + C_j P_{r,j}^s \tag{E.5.21}
\end{equation}$

$\begin{equation}
\Phi_{LC}^s = C_i \Phi_{r,i}^s + C_j \Phi_{r,j}^s \tag{E.5.22}
\end{equation}$

其中$C_i$和$C_j$是无电离层影响的LC的系数。$C_i$和$C_j$由以下公式导出：

$\begin{equation}
C_i = \frac{{f_i}^2}{{f_i}^2 - {f_j}^2} \tag{E.5.23}
\end{equation}$

$\begin{equation}
C_j = \frac{-{f_j}^2}{{f_i}^2 - {f_j}^2} \tag{E.5.24}
\end{equation}$

其中$f_i$和$f_j$是$L_i$和$L_j$测量的频率（Hz）。当前版本的RTKLIB总是对GPS、GLONASS和QZSS使用$L_1$和$L_2$，对Galileo使用$L_1$和$L_5$进行无电离层影响的LC。如果在单一或PPP模式下将处理选项 "Ionosphere Correction" 设置为 "Iono-Free LC"，则无电离层影响的LC用于基本测量以消除电离层项。请注意，无电离层影响的LC模型不适用于Kinematic、Static或Moving-base模式。详情请参阅E.7(7.1)。

<GiscusTalk />
