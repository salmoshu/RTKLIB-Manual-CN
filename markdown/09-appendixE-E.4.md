---
sidebarDepth: 2
---

# 附录E. 模型与算法

## E.4 GNSS卫星星历与钟差

RTKLIB 支持 GPS、GLONASS、Galileo、QZSS、BeiDou 和 SBAS 的广播星历和钟差数据。它还支持作为 SP3-c 文件和钟差 RINEX 文件提供的精确星历和钟差数据，这些文件包括 Galileo、QZSS 和 BeiDou，用于后处理模式。对于实时模式，支持使用 SBAS 长期和快速修正后的广播星历和钟差数据，以及使用 RTCM 3 SSR（状态空间表示）修正的数据。以下方程展示了 RTKLIB 中使用的星历和钟差模型。

### E.4.1 GPS、Galileo 和 QZSS 的广播星历和钟差数据 [1][5][6]

GPS、Galileo 和 QZSS 的广播星历和卫星钟差参数在导航消息中给出如下：

$
\begin{equation}
\scriptsize
p_{eph}(t_{oe}, t_{oc}, IOD) = (a, e, \Delta n, \Omega_0, \omega, M_0, \Delta t, I_0, \dot{\Omega}, C_{us}, C_{uc}, C_{rs}, C_{rc}, C_{is}, C_{ic}, a_f, a_{f1}, a_{f2}, T_{gd})^T \tag{E.4.1}
\end{equation}
$

通过使用这些参数，可以计算出卫星在 ECEF 坐标系中的位置（天线相位中心位置）$r^s(t)$、卫星钟差 $dT^s(t)$ 和钟漂 $dT'^s(t)$。

$
\begin{equation}
t_k = t - t_{oe} \tag{E.4.2}
\end{equation}
$
$
\begin{equation}
M_k = M_0 + \left( \sqrt{\frac{\mu}{a^3}} + \Delta n \right) t_k \tag{E.4.3}
\end{equation}
$
$
\begin{equation}
M = E - e \sin E \tag{E.4.4}
\end{equation}
$
$
\begin{equation}
a = \frac{\sqrt{1-e^2}sinE}{cosE-e} \tag{E.4.5}
\end{equation}
$
$
\begin{equation}
\phi = \arctan\alpha + \omega \tag{E.4.6}
\end{equation}
$
$
\begin{equation}
\delta u = C_{us} \sin 2 \phi + C_{uc} \cos 2 \phi \tag{E.4.7}
\end{equation}
$
$
\begin{equation}
\delta r = C_{rs} \sin 2 \phi + C_{rc} \cos 2 \phi \tag{E.4.8}
\end{equation}
$
$
\begin{equation}
\delta i = C_{is} \sin 2 \phi + C_{ic} \cos 2 \phi \tag{E.4.9}
\end{equation}
$
$
\begin{equation}
u = \phi + \delta u \tag{E.4.10}
\end{equation}
$
$
\begin{equation}
r = a(1 - e \cos E) + \delta r \tag{E.4.11}
\end{equation}
$
$
\begin{equation}
i = i_0 + \delta i + \dot{i} t_k \tag{E.4.12}
\end{equation}
$
$
\begin{equation}
\Omega = \Omega_0 + (\dot{\Omega} - \omega_e) t_k - \omega_e t_{oe} \tag{E.4.13}
\end{equation}
$
$
\begin{equation}
\mathbf{r}^s(t) = r \begin{pmatrix}
\cos u \cos \Omega - \sin u \cos i \sin \Omega \\
\cos u \sin \Omega + \sin u \cos i \cos \Omega \\
\sin u \sin i
\end{pmatrix} \tag{E.4.14}
\end{equation}
$
$
\begin{equation}
t_{c} = t - t_{oc} \tag{E.4.15}
\end{equation}
$
$
\begin{equation}
dT^s(t) = a f_0 + a f_1 t_{c} + a f_2 t_{c}^2 - \frac{2 \sqrt{\mu}}{c^2} e \sqrt{A} \sin E - b T_{GD} \tag{E.4.16}
\end{equation}
$
$
\begin{equation}
d\dot{T}^s(t) = a f_1 + 2 a f_2 t_{c} \tag{E.4.17}
\end{equation}
$

其中：

$\mu$ : 地球引力常数（$3.9860050 × 10^{14} \, \text{m}^3/\text{s}^2$ 用于 GPS 和 QZSS，$3.986004418 × 10^{14} \, \text{m}^3/\text{s}^2$ 用于 Galileo）<br>
$\omega_e$ : 地球角速度（$7.292115467 × 10^{-5}$ rad/s）<br>
$b = f_1^2 / f_2^2$ for $L_1$ pseudorange<br>
$T_{GD}$ : 群延迟参数，用于 GPS 和 QZSS，$T_{GD}$ for Galileo (s)

开普勒方程（E.4.4）可以通过牛顿法迭代求解。

$\begin{equation}
E_0 = M \tag{E.4.18}
\end{equation}$

$\begin{equation}
E_{i+1} = E_i - \frac{E_i - e \sin E_i - M}{1 - e \cos E_i} \tag{E.4.19}
\end{equation}$

$\begin{equation}
E = \lim_{i \to \infty} E_i \tag{E.4.20}
\end{equation}$

广播星历和时钟在处理选项“Satellite Ephemeris/Clock”设置为“Broadcast”时适用，同样适用于GLONASS、BeiDou和SBAS。

### E.4.2 GLONASS的广播星历和时钟 [4]

GLONASS的广播星历和时钟参数在导航消息中给出如下：

$\begin{equation}
p_{eph}(t_b) = (x, y, z, v_x, v_y, v_z, a_x, a_y, a_z, \tau_n, \gamma_n) \tag{E.4.21}
\end{equation}$

卫星位置 $\mathbf{r}^s(t) = (x, y, z)^T$ 和速度 $\mathbf{v}^s(t) = (v_x, v_y, v_z)^T$ 在地心固定坐标系（ECEF，PZ90.02）中的微分方程可以表示为：

$\begin{equation}
\frac{dx}{dt} = v_x, \quad \frac{dy}{dt} = v_y, \quad \frac{dz}{dt} = v_z \tag{E.4.22}
\end{equation}$

$\begin{equation}
\frac{dv_x}{dt} = -\frac{\mu}{r^3} x - \frac{3}{2} J_2 \frac{\mu a_e^2}{r^5} x \left(1 - \frac{5z^2}{r^2}\right) + \omega_e^2 x + 2\omega_e v_y + a_x \tag{E.4.23}
\end{equation}$

$\begin{equation}
\frac{dv_y}{dt} = -\frac{\mu}{r^3} y - \frac{3}{2} J_2 \frac{\mu a_e^2}{r^5} y \left(1 - \frac{5z^2}{r^2}\right) + \omega_e^2 y - 2\omega_e v_x + a_y \tag{E.4.24}
\end{equation}$

$\begin{equation}
\frac{dv_z}{dt} = -\frac{\mu}{r^3} z - \frac{3}{2} J_2 \frac{\mu a_e^2}{r^5} z \left(3 - \frac{5z^2}{r^2}\right) + a_z \tag{E.4.25}
\end{equation}$

其中：

$a_e$：地球半长轴（6378136.0 m）<br>
$\mu$：地球引力常数（398600.444 × 10⁹ m³/s²）<br>
$\omega_e$：地球角速度（7.292115 × 10⁻⁵ rad/s）<br>
$J_2$：地球引力场的二阶球谐系数（1082625.7 × 10⁻⁹）<br>
$r = \sqrt{x^2 + y^2 + z^2}$

注意，GLONASS ICD 5.1 [4] 的 A.3.1.2 中的两个勘误已在上述模型中得到修正。

卫星位置 $\mathbf{r}^s(t)$ 和速度 $\mathbf{v}^s(t)$ 在时间 $t$ 可以通过 RK4（Runge-Kutta 4阶和4阶段）数值积分方法求解这些微分方程，初始条件为参考时间 $t_b$ 时的卫星位置 $\mathbf{r}^s(t_b)$ 和速度 $\mathbf{v}^s(t_b)$。对于卫星时钟偏差 $dT^s(t)$ 和漂移 $d\dot{T}^s(t)$ 在历元时间 $t$ 也同样推导如下：

$\begin{equation}
dT^s(t) = -\tau_n + \gamma_n (t - t_b) \tag{E.4.26}
\end{equation}$

$\begin{equation}
d\dot{T}^s(t) = \gamma_n \tag{E.4.27}
\end{equation}$

卫星钟中的相对论效应已包含在GLONASS时钟参数中。因此，在此情况下不应用相对论校正。

### E.4.3 北斗卫星的广播星历和时钟 [7]

对于北斗卫星，与GPS、Galileo和QZSS相似的星历和时钟参数在导航消息中提供如下：

$\begin{equation}
\scriptsize
P_{eph}(t_{oe}, t_{oc}) = (a, e, i_0, \Omega_0, \omega, M_0, \Delta n, \dot{I}, \Omega, C_{us}, C_{uc}, C_{rs}, C_{rc}, C_{is}, C_{ic}, a_{f0}, a_{f1}, a_{f2}, T_{GD})^T \tag{E.4.28}
\end{equation}$

对于北斗的MEO和IGSO卫星，使用的公式与(1)中GPS星历和时钟相同，只是$\mu = 3.986004418 \times 10^{14}$，$\omega_e = 7.2921150 \times 10^{-5}$ rad/s，且时间 $t$ 以BDT（北斗时间）表示。

为了在BDT（北斗时间）的时间 $t$ 获取北斗GEO卫星的位置 $\mathbf{r}^s(t)$，应替换方程 (E.5.13) 和 (E.5.14)，具体如下：

$\begin{equation}
\Omega = \Omega_0 + \dot{\Omega} t_k - \omega_e t_{oe} \tag{E.4.29}
\end{equation}$

$\begin{equation}
\mathbf{r}^s(t) = r R_z(\omega_e t_k) R_X(-5^\circ) \begin{pmatrix} \cos u \cos \Omega - \sin u \cos i \sin \Omega \\ \cos u \sin \Omega + \sin u \cos i \cos \Omega \\ \sin u \sin i \end{pmatrix} \tag{E.4.30}
\end{equation}$

其中：

$\begin{equation}
R_X(\theta) = \begin{pmatrix} 1 & 0 & 0 \\ 0 & \cos \theta & \sin \theta \\ 0 & -\sin \theta & \cos \theta \end{pmatrix}, \quad R_Z(\theta) = \begin{pmatrix} \cos \theta & \sin \theta & 0 \\ -\sin \theta & \cos \theta & 0 \\ 0 & 0 & 1 \end{pmatrix} \tag{}
\end{equation}$

### E.4.4 SBAS的广播星历和时钟 [8]

SBAS GEO卫星的导航消息参数在SBAS消息（消息类型9）中提供如下：

$\begin{equation}
P_{eph}(t_0) = (x_i, y_i, z_i, v_{x}, v_{y}, v_{z}, a_{x}, a_{y}, a_{z}, a_{GF0}, a_{GF1}) \tag{E.4.31}
\end{equation}$

通过使用这些参数，可以计算出在ECEF坐标系中的卫星位置 $\mathbf{r}^s(t)$ 和卫星时钟偏差 $dT^s(t)$。

$\begin{equation}
\mathbf{r}^s(t) = \begin{pmatrix} x \\ y \\ z \end{pmatrix} + \begin{pmatrix} v_x \\ v_y \\ v_z \end{pmatrix} (t - t_0) + \frac{1}{2} \begin{pmatrix} a_x \\ a_y \\ a_z \end{pmatrix} (t - t_0)^2 \tag{E.4.32}
\end{equation}$

$\begin{equation}
dT^s(t) = a_{GF0} + a_{GF1}(t - t_0) \tag{E.4.33}
\end{equation}$

### E.4.5 SBAS轨道和时钟改正 [8]

SBAS轨道和时钟改正被定义为以下参数：

$\begin{equation}
\Delta_{sbas}(t_0, IOD) = (\delta x, \delta y, \delta z, \delta \dot{x}, \delta \dot{y}, \delta \dot{z}, \delta a_{f0}, \delta a_{f1}) \tag{E.4.38}
\end{equation}$

IOD表示目标广播星历和时钟参数。修正后的卫星位置 $\mathbf{r}^s(t)$ 在时间 $t$ 时计算如下：

$\begin{equation}
\mathbf{r}^s(t) = \mathbf{r}^s_{\text{broadcast}}(t) + \begin{pmatrix} \delta x \\ \delta y \\ \delta z \end{pmatrix} + \begin{pmatrix} \delta \dot{x} \\ \delta \dot{y} \\ \delta \dot{z} \end{pmatrix} (t - t_0) \tag{E.4.39}
\end{equation}$

其中：

$\mathbf{r}^s_{\text{broadcast}}(t)$ : 由带IOD的广播星历在时间 $t$ 计算的卫星位置（米）

校正后的卫星时钟偏差 $dT^s(t)$ 在时间 $t$ 时通过SSR校正计算如下：

$\begin{equation}
dT^s(t) = dT^s_{\text{broadcast}}(t) + \delta a_{f0} + \delta a_{f1}(t - t_0) \tag{E.4.40}
\end{equation}$

其中：

$dT^s_{\text{broadcast}}(t)$ : 由带IOD的广播时钟在时间 $t$ 计算的卫星时钟偏差。

SBAS校正与广播星历和时钟结合使用的情况是在处理选项“Satellite Ephemeris/Clock”设置为“Broadcast+SBAS”时。

### E.4.6 精密星历和时钟

GPS、GLONASS、Galileo、QZSS和BeiDou的精确星历通常以SP3-c文件的形式提供，这些文件包含每15分钟或5分钟历元的卫星位置和速度（可选）。为了在时间 $t$ 获得卫星位置，需要适当的插值。RTKLIB使用固定阶数（$n = 10$）的Newton-Neville多项式插值算法，如下所示：

$\begin{equation}
P_{ij}(t) = x_{j} \quad (i \leq j \leq i + n) \tag{E.4.34}
\end{equation}$

$\begin{equation}
\small
P_{jk}(t) = \frac{(t_k - t) P_{j,k-1}(t) + (t - t_j) P_{j+1,k}(t)}{t_k - t_j} \quad (i \leq j < k \leq i + n) \tag{E.4.35}
\end{equation}$

其中 $n$ 是插值的多项式阶数，$x(t_i), x(t_{i+1}), x(t_{i+2}), ..., x(t_{i+n})$ 是历元时间 $t_i, t_{i+1}, t_{i+2}, ..., t_{i+n}$ 处每个组件的星历值。例如，在 $n=4$ 的情况下，时间 $t$ 处的插值值 $x(t)$ 可以推导为：

![interpolate equations](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250220-154054.jpg)

请注意，精确星历通常提供卫星的质心（CoM，中心质量）位置，而非天线相位中心位置。因此，用户应校正卫星天线相位中心偏移以使用精确星历。详情请参阅附录E.8。

尽管精确星历通常使用高阶多项式插值，但对于以SP3-c或时钟RINEX文件形式提供的精确时钟，采用了简单的线性插值，表达式如下：

$\begin{equation}
\small
dT^s(t) = \frac{(t_{i+1} - t) dT^s(t_i) + (t - t_i) dT^s(t_{i+1})}{t_{i+1} - t_i} \quad (t_i \leq t < t_{i+1}) \tag{E.4.36}
\end{equation}$

对于由IGS（国际GNSS服务）提供的精确时钟，应进行相对论效应的校正：[68]

卫星时钟偏差 $dT^s(t)$ 的相对论校正表达式如下：

$\begin{equation}
\small
dT^s(t) = \frac{(t_{i+1} - t) dT^s(t_i) + (t - t_i) dT^s(t_{i+1})}{t_{i+1} - t_i} - \frac{2 \, \mathbf{r}^s(t)^T \mathbf{v}^s(t)}{c^2} \tag{E.4.37}
\end{equation}$

其中，$\mathbf{r}^s(t)$ 和 $\mathbf{v}^s(t)$ 是从精确星历推导出的卫星位置和速度。

精确星历和时钟在处理选项“Satellite Ephemeris/Clock”设置为“Precise”时适用。

IOD 表示目标广播星历和时钟参数。校正后的卫星位置 $\mathbf{r}^s(t)$ 在时间 $t$ 时计算如下：

$\begin{equation}
e_{along} = \frac{\mathbf{v}^s_{\text{broadcast}}(t)}{|\mathbf{v}^s_{\text{broadcast}}(t)|} \tag{E.4.42}
\end{equation}$

$\begin{equation}
e_{cross} = \frac{\mathbf{r}^s_{\text{broadcast}}(t) \times \mathbf{v}^s_{\text{broadcast}}(t)}{|\mathbf{r}^s_{\text{broadcast}}(t) \times \mathbf{v}^s_{\text{broadcast}}(t)|} \tag{E.4.43}
\end{equation}$

$\begin{equation}
e_{radial} = e_{along} \times e_{cross} \tag{E.4.44}
\end{equation}$

$\begin{equation}
\delta\mathbf{O} = \begin{pmatrix} \delta O_{radial} \\ \delta O_{along} \\ \delta O_{cross} \end{pmatrix} + \begin{pmatrix} \delta \dot{O}_{radial} \\ \delta \dot{O}_{along} \\ \delta \dot{O}_{cross} \end{pmatrix} (t - t_0) \tag{E.4.45}
\end{equation}$

$\begin{equation}
\mathbf{r}^s(t) = \mathbf{r}^s_{\text{broadcast}}(t) + (e_{radial}, e_{along}, e_{cross}) \delta\mathbf{O} \tag{E.4.46}
\end{equation}$

其中：

$\mathbf{r}^s_{\text{broadcast}}(t)$ : 由带IOD的广播星历在时间 $t$ 计算的卫星位置（米）<br>
$\mathbf{v}^s_{\text{broadcast}}(t)$ : 由带IOD的广播星历在时间 $t$ 计算的卫星速度（米/秒）

卫星速度 $\mathbf{v}^s_{\text{broadcast}}(t)$ 通过以下微分近似计算，其中 $\Delta t = 0.001$（秒）。

$\begin{equation}
\mathbf{v}^s_{\text{broadcast}}(t) = \frac{\mathbf{r}^s_{\text{broadcast}}(t + \Delta t) - \mathbf{r}^s_{\text{broadcast}}(t)}{\Delta t} \tag{E.4.47}
\end{equation}$

校正后的卫星时钟偏差 $dT^s(t)$ 在时间 $t$ 时通过SSR校正计算如下：

$\begin{equation}
\delta C = C_0 + C_1 (t - t_0) + C_2 (t - t_0)^2 \tag{E.4.48}
\end{equation}$

$\begin{equation}
dT^s(t) = dT^s_{\text{broadcast}}(t) + \frac{\delta C}{c} \tag{E.4.49}
\end{equation}$

其中：

$dT^s_{\text{broadcast}}(t)$ : 由带IOD的广播时钟在时间 $t$ 计算的卫星时钟偏差，带有以下相对论校正：

$\begin{equation}
\Delta t_{rel} = -\frac{2 \mathbf{r}^s_{\text{broadcast}}(t)^T \mathbf{v}^s_{\text{broadcast}}(t)}{c^2} \tag{E.4.50}
\end{equation}$

SSR校正与广播星历和时钟结合使用的情况是在处理选项“Satellite Ephemeris/Clock”设置为“Broadcast+SSR APC”或“Broadcast+SSR CoM”时。
