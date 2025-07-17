---
sidebarDepth: 2
---

# 附录E. 模型与算法

## E.3 信号测量模型

### E.3.1 GNSS信号结构

图E.3-1展示了一个典型的GNSS信号结构。GNSS信号通常由载波（Carrier）、伪码（Code）和导航数据码（Data）的乘积组成。具体的，伪码和数据码一起先通过调制而依附在正弦波形式的载波上，然后卫星将调制后的载波信号播发出去。

GPS、GLONASS、Galileo、QZSS、北斗和SBAS提供的GNSS信号详细规格可以在[附录G](/algorithm/RTKLIB-Manual-CN/11-appendixG)中找到。

![](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250218-224511.jpg)
<p style="text-align: center;">图E.3-1 GNSS信号结构</p> 

### E.3.2 伪距测量模型

伪距指的“接收机天线到卫星天线之间的距离，其中包含了卫星钟差和接收机钟差（及其他偏差，如大气层延迟）”[9]。$L_i$频段的伪距 $P_{r,i}^s$ 可以用接收机测量的信号接收时间 $\overline{t}_r(s)$ 和卫星测量的信号发射时间 $\overline{t}^s(s)$ 来表示：

$\begin{equation}
P_{r,i}^s = c(\overline{t}_r - \overline{t}^s) \tag{E.3.1}
\end{equation}$

该方程也可以用卫星与接收机天线之间的几何距离 $\rho_r^s$，接收机和卫星的钟差 $dt_r$、$dT^s$，电离层和对流层延迟 $I_{r,i}^s$、$T_r^s$，以及测量误差 $\varepsilon_P$ 来表示：[64]

$\begin{align}
P_{r,i}^s 
&= c( (t_r + dt_r(t_r)) - (t^s + dT^s(t^s)) ) + \varepsilon_P \\
&= c(t_r - t^s) + c(dt_r(t_r) - dT^s(t^s)) + \varepsilon_P \\
&= (\rho_r^s + I_{r,i}^s + T_r^s) + c(dt_r(t_r) - dT^s(t^s)) + \varepsilon_P \\
&= \rho_r^s + c(dt_r(t_r) - dT^s(t^s)) + I_{r,i}^s + T_r^s + \varepsilon_P \tag{E.3.2}
\end{align}$

![Pseudorange Model ](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250219-215841.jpg)
<p style="text-align: center;">图E.3-2 伪距模型</p> 

### E.3.3 载波相位测量模型

“载波相位其实是测量卫星发来的信号载波和接收机自己产生的参考信号之间的一种节拍差（beat frequency）”[9]。$L_i$频段的载波相位 $\phi_{r,i}^s$ 可以表示为：

$\begin{align}
\small \phi_{r,i}^s
& \small = \phi_{r,i}(t_r) - \phi_i^s(t^s) + N_{r,i}^s + \varepsilon_\phi \\
& \small = (f_i(t_r + dt_r(t_r) - t_0) + \phi_{r, 0, i}) - (f_i(t^s + dT^s(t^s) - t_0) + \phi_{0,i}^s) + N_{r,i}^s + \varepsilon_\phi \\ 
& \small = \frac{c}{\lambda_i}(t_r - t^s) + \frac{c}{\lambda_i}(dt_r(t_r) - dT^s(t^s)) + (\phi_{r,0,i} - \phi_{0,i}^s + N_{r,i}^s) + \varepsilon_\phi \tag{E.3.3}
\end{align}$

其中，$t_0$ 是初始时间（s），$\phi_{r,i}(t)$ 是接收机本地振荡 $L_i$ 频段的相位（cycle），$\phi_i^s$ 是 $t$ 时刻发射的导航信号的 $L_i$ 相位（cycle）。$\phi_{r,0,i}$ 是接收机本地振荡器在 $t_0$ 时刻的 $L_i$ 初始相位（cycle），$\phi_{r,0,i}$ 是 $t_0$ 时刻发射的导航信号的 $L_i$ 初始相位（cycle）。

$L_i$ 相位距 $\Phi_{r,i}^s$ 定义为载波相位与载波波长 $\lambda_i$（m）的乘积，也可以通过载波相位偏差 $B_{r,i}^s$ 和载波相位校正项 $d\Phi_{r,i}^s$ 来表达，后者包括天线相位中心偏移和变化、地球潮汐导致的站点位移、相位缠绕效应以及卫星钟的相对论校正：

$
\begin{align}
\small \Phi_{r,i}^s 
& \small = \lambda_i \phi_{r,i}^s \\
& \small = c(t_r - t^s) + c(dt_r(t_r) - dT^s(t^s)) + \lambda_i (\phi_{r,0,i} - \phi_{0,i}^s + N_{r,i}^s) + \lambda_i \varepsilon_\phi \\
& \small = \rho_r^s + c(dt_r(t_r) - dT^s(t^s)) - I_{r,i}^s + T_r^s + \lambda_i B_{r,i}^s + d\Phi_{r,i}^s + \varepsilon_\phi
\tag{E.3.4}
\end{align}
$

其中：

$\begin{flalign}
B_{r,i}^s = \phi_{r,0,i} - \phi_{0,i}^s + N_{r,i}^s \tag{E.3.5}
\end{flalign}$

$\begin{flalign}
\small
d\phi_{r,i}^s = -d_{r,pco,i}^T e_{r,enu}^s + (E^s d_{pco,i}^s)^T e_r^s + d_{r,pcv,i}(El) + d_{pcv,i}^s(\theta) - d_{r,disp}^T e_{r,enu}^s + \lambda_i \phi_{pw} \tag{E.3.6}
\end{flalign}$

$N_{r,i}^s$ 通常被称为载波相位整数模糊度、载波整周模糊度或简称为模糊度。

:::info GPS L1频段的载波波长是19cm，为什么通常说载波相位的测量精度是cm或mm级别？
虽然 L1 载波的波长是 19 cm，但测量精度远高于波长，这是因为载波相位测量的精度取决于相位分辨能力，而不是波长本身。

接收机的相位测量是通过接收机锁相环实现的，它能分辨非常小的相位变化，实际测量精度通常在 0.01 周期左右（即1.9mm），主要受噪声和硬件限制。
:::

### E.3.4 卫地距（接收机与卫星天线）

接收机与卫星天线几何距离（卫地距）指卫星天线（相位中心）与接收机天线（相位中心）在惯性坐标系中的物理距离。首先，信号传输时间 $t^s$ 可以通过以下方式推导出来：

$
\begin{equation}
t^s = \overline{t}_r - \frac{P_{r,i}^s}{c} - dT(t^s) \tag{E.3.7}
\end{equation}
$

方程的两边都包含了 $t^s$。因此，需要进行几次迭代来解这个方程。几何距离可以通过 ECEF坐标系中 $t_r$ 时刻的接收机位置 $\mathbf{r}_r(t_r) = (x_r, y_r, z_r)^T$ 与 $t^s$ 时刻的卫星位置 $\mathbf{r}^s(t^s) = (x^s, y^s, z^s)^T$ 来表示：

$
\begin{equation}
\rho_r^s = \| \mathbf{U}(t_r) \mathbf{r}_r(t_r) - \mathbf{U}(t^s) \mathbf{r}^s(t^s) \| \tag{E.3.8}
\end{equation}
$

其中，$\mathbf{U}(t)$ 是 $t$ 时刻从 ECEF（地心地固）到 ECI（地心惯性）坐标系的转换矩阵。为了在 ECEF 坐标系中表达几何距离，需要考虑地球自转效应以获得几何距离。该方程可以用以下方程之一近似，精度在1毫米级别以内。当前版本的 RTKLIB 使用方程 (F.3.8b) 来计算几何距离。(F.3.8b) 中的最后一项有时被称为萨格纳克效应（Sagnac effect）。

$
\begin{equation}
\rho_r^{s} \approx \| \mathbf{r}_r(t_r) - R_z\left(\omega_p \frac{\rho_r^s}{c}\right) \mathbf{r}^s(t^s) \| \tag{E.3.8a}
\end{equation}
$
$
\begin{equation}
\rho_r^{s} \approx \| \mathbf{r}_r(t_r) - \mathbf{r}^s(t^s) \| + \frac{\omega_e}{c} (x^s y_r - y^s x_r) \tag{E.3.8b}
\end{equation}
$
$
\begin{equation}
\rho_r^{s} \approx \| \mathbf{r}_r(t_r) - R_z\left(\omega_e (t_r - t^s)\right) \mathbf{r}^s(t^s) \| \tag{E.3.8c}
\end{equation}
$

![Geometric Range and Earth Rotation Correction](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250219-234443.jpg)
<p style="text-align: center;">图E.3-3 几何距离和地球自转校正</p> 

### E.3.5 卫星方位角与仰角

接收机到卫星的单位视距（LOS）向量在 ECEF（地心地固）坐标系中可以表示为：

$
\begin{equation}
\mathbf{e}_r^s = \frac{\mathbf{r}^s(t^s) - \mathbf{r}_r(t_r)}{\|\mathbf{r}^s(t^s) - \mathbf{r}_r(t_r)\|} \tag{E.3.9}
\end{equation}
$

在方程中，忽略了地球自转效应。从接收机到卫星方向的方位角 $Az_r^s$ 和仰角 $El_r^s$ 可以从以下公式推导出来：

$
\begin{equation}
\mathbf{e}_{r,enu}^s = \mathbf{E}_r \mathbf{e}_r^s = (e_e, e_n, e_u)^T \tag{E.3.10}
\end{equation}
$
$
\begin{equation}
Az_r^s = \text{ATAN2}(e_e, e_n) \tag{E.3.11}
\end{equation}
$
$
\begin{equation}
El_r^s = \arcsin(e_u) \tag{E.3.12}
\end{equation}
$

其中，$\mathbf{E}_r$ 是从 ECEF 坐标系到接收机位置的局部坐标系的坐标旋转矩阵。有关该矩阵的详细形成，请参考 [附录 E.2](/algorithm/RTKLIB-Manual-CN/09-appendixE-E.2)。

![ Local Coordinates and Azimuth and Elevation Angles](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250219-235235.jpg)
<p style="text-align: center;">图E.3-4 局部坐标系与方位角和仰角</p>

<GiscusTalk />
