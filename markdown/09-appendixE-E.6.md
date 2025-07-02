---
sidebarDepth: 2
---

# 附录E. 模型与算法

## E.6 单点定位

RTKLIB采用迭代加权最小二乘法（LSE）用于“Single”（单点定位）模式（无论是否包含SBAS校正）。

### E.6.1 线性最小二乘

假设给定测量向量$y$，它可以被建模为一个包含未知参数向量$x$和一个随机观测误差向量$v$的线性方程：

$\begin{equation}
\mathbf{y} = \mathbf{Hx} + \mathbf{v} \tag{E.6.1}
\end{equation}$

最小二乘代价函数$J_{LS}$定义为观测误差的平方和：

$\begin{equation}
J_{LS} = v_1^2 + v_2^2 + \ldots + v_m^2 = \mathbf{v}^T\mathbf{v} \tag{E.6.2}
\end{equation}$

通过使用 (E.6.1) 和 (E.6.2) ，代价函数可以重写为（计算时注意代价值为标量，而标量的转置为自身）：

$\begin{align}
J_{LS} 
&= (\mathbf{y} - \mathbf{Hx})^T(\mathbf{y} - \mathbf{Hx}) \\
&= \mathbf{y}^T\mathbf{y} - \mathbf{y}^T\mathbf{Hx} - \mathbf{x}^T\mathbf{H}^T\mathbf{y} + \mathbf{x}^T\mathbf{H}^T\mathbf{Hx} \\
&= \mathbf{y}^T\mathbf{y} - 2\mathbf{y}^T\mathbf{Hx} + \mathbf{x}^T\mathbf{H}^T\mathbf{Hx} \tag{E.6.3}
\end{align}$

为了最小化代价函数，$J_{LS}$的梯度应该为零。然后

$\begin{align}
\frac{\partial J_{LS}}{\partial \mathbf{x}} 
&= -2\mathbf{y}^T\mathbf{H} + 2\mathbf{H}^T\mathbf{H}\mathbf{x} = \mathbf{0} \tag{E.6.4}
\end{align}$

这就是最小二乘法的“正规方程”（Normal Equation）：

$\begin{equation}
\mathbf{H}^T\mathbf{Hx} = \mathbf{H}^T\mathbf{y} \tag{E.6.5}
\end{equation}$

为了解正规方程，我们可以通过最小二乘法得到估计的未知参数向量$\hat{x}$：

$\begin{equation}
\hat{\mathbf{x}} = (\mathbf{H}^T\mathbf{H})^{-1}\mathbf{H}^T\mathbf{y} \tag{E.6.6}
\end{equation}$

如果给定了每个测量的权重，可以使用权重矩阵$\mathbf{W}$重写代价函数 (E.6.3) 。

$\begin{equation}
J_{WLS} = \mathbf{v}^T \mathbf{W v} \tag{E.6.7}
\end{equation}$

为了最小化代价函数$J_{WLS}$，我们可以通过加权最小二乘法（WLS）以类似最小二乘法原本的方式获得估计的未知参数向量$\hat{\mathbf{x}}$：

$\begin{equation}
\hat{\mathbf{x}} = (\mathbf{H}^T \mathbf{W H})^{-1} \mathbf{H}^T \mathbf{W y} \tag{E.6.8}
\end{equation}$

加权最小二乘法（WLS）的权重矩阵$\mathbf{W}$通常定义为：

$\begin{equation}
\mathbf{W} = diag(\sigma_1^{-2}, \sigma_2^{-2}, \ldots, \sigma_m^{-2})
\end{equation}$

其中$\sigma_i$是第$i$个观测误差的先验标准差。

### E.6.2 高斯-牛顿迭代

如果观测值不是以线性模型给出的，测量方程可以由一个通用的非线性向量函数表示为：

$\begin{equation}
\mathbf{y} = \mathbf{h(x)} + \mathbf{v} \tag{E.6.9}
\end{equation}$

其中$\mathbf{h(x)}$是参数向量$\mathbf{x}$的测量向量函数。该方程可以通过在初始参数向量$\mathbf{x_0}$处使用泰勒（级数）展开：

$\begin{equation}
\mathbf{h(x)} = \mathbf{h(x_0)} + \mathbf{H(x - x_0)} + \ldots \tag{E.6.10}
\end{equation}$

其中$H$是$h(x)$关于$x$在$x = x_0$处的偏导数矩阵：

$\begin{equation}
\mathbf{H} = \left.\frac{\partial \mathbf{h(x)}}{\partial \mathbf{x}}\right|_\mathbf{{x=x_0}} \tag{E.6.11}
\end{equation}$

假设初始参数足够接近真实值，并且忽略泰勒级数的高阶项（第二项及其后续项）。我们可以将 (E.6.9) 近似为：

$\begin{equation}
\mathbf{y} \approx \mathbf{h(x_0)} + \mathbf{H(x - x_0)} + \mathbf{v} \tag{E.6.12}
\end{equation}$

然后可以得到以下线性方程：

$\begin{equation}
\mathbf{y} - \mathbf{h(x_0)} = \mathbf{H(x - x_0)} + \mathbf{v} \tag{E.6.13}
\end{equation}$

通过将线性加权最小二乘法 (E.6.8) 应用于 (E.6.13) ，我们可以得到非线性加权最小二乘法的正规方程：

$\begin{equation}
\mathbf{H}^T \mathbf{W H} (\hat{\mathbf{x}} - \mathbf{x_0}) = \mathbf{H}^T \mathbf{W} (\mathbf{y} - \mathbf{h(x_0)}) \tag{E.6.14}
\end{equation}$

因此，我们可以通过以下方式获得估计的未知参数向量$\hat{x}$：

$\begin{equation}
\hat{\mathbf{x}} = \mathbf{x_0} + (\mathbf{H}^T \mathbf{W H})^{-1} \mathbf{H}^T \mathbf{W (y - h(x_0)}) \tag{E.6.15}
\end{equation}$

如果初始参数$\mathbf{x_0}$不够接近真实值，我们可以迭代改进估计参数如下：

$\begin{equation}
\hat{\mathbf{x}}_0 = \mathbf{x_0} \tag{E.6.16}
\end{equation}$

$\begin{equation}
\hat{\mathbf{x}}_{i+1} = \hat{\mathbf{x}}_i + (\mathbf{H}^T \mathbf{W H})^{-1} \mathbf{H}^T \mathbf{W} (\mathbf{y} - \mathbf{h}(\hat{\mathbf{x}}_i)) \tag{E.6.17}
\end{equation}$

如果迭代收敛，我们可以得到最终的估计参数为：

$\begin{equation}
\hat{\mathbf{x}} = \lim_{i \to \infty} \hat{\mathbf{x}}_i \tag{E.6.18}
\end{equation}$

迭代最小二乘法通常被称为高斯-牛顿法。请注意，对于具有较大非线性的病态（ill-conditioned）测量方程，简单的高斯-牛顿方法并不总是能够收敛。在这种情况下，我们应该采用另一种策略来处理这种非线性最小二乘问题。对于非线性最小二乘法，最流行的方法是非线性最小二乘的LM（Levenberg-Marquardt）方法。

### E.6.3. 接收机位置和钟差估计

如果定位模式 (Positioning Mode) 设置为 “Single” ，将会通过以下单点定位过程，通过逐历元的方式获得最终解。对于一个历元时间，未知参数向量 $\mathbf{x}$ 定义为：

$\begin{equation}
\mathbf{x} = (\mathbf{r}_r^T, cdt_r)^T \tag{E.6.19}
\end{equation}$

伪距测量向量 $\mathbf{y}$ 可以表示为：

$\begin{equation}
\mathbf{y} = (P_r^1, P_r^2, P_r^3, \ldots, P_r^m)^T \tag{E.6.20}
\end{equation}$

其中 $P_r^s$ 是伪距测量。如果配置选项"Ionosphere Correction"设置为"Iono-Free LC"，则使用附录 [E.5(E.5.7)](/algorithm/RTKLIB-Manual-CN/09-appendixE-E.5#e-5-7-无电离层组合-线性组合) 中定义的无电离层组合（线性组合）伪距。其他情况下，则仅使用 $L_1$ 伪距。

![ Satellite Geometry for Single Point Positioning](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250220-204634.jpg)
<p style="text-align: center;">图E.6-1 单点定位的卫星几何结构</p> 

单点定位的测量方程及其偏导数矩阵构成如下：

$\begin{equation}
\mathbf{h(x)} = 
\begin{pmatrix} 
\rho_r^1 + cdt_r - cdT^1 + I_r^1 + T_r^1 \\ 
\rho_r^2 + cdt_r - cdT^2 + I_r^2 + T_r^2 \\ 
\rho_r^3 + cdt_r - cdT^3 + I_r^3 + T_r^3 \\ 
\vdots \\
\rho_r^m + cdt_r - cdT^m + I_r^m + T_r^m 
\end{pmatrix} \mathbf{H} = 
\begin{pmatrix} 
-\mathbf{e}_r^{1^T} & 1 \\ 
-\mathbf{e}_r^{2^T} & 1 \\ 
-\mathbf{e}_r^{3^T} & 1 \\ 
\vdots & \vdots \\ 
-\mathbf{e}_r^{m^T} & 1
\end{pmatrix} \tag{E.4.45}
\end{equation}$

其中几何距离 $\rho_r^s$ 和视距（LOS）向量 $\mathbf{e}_r^s$ 由 E.3 (3.4) 和 E.3 (3.5) 给出，结合卫星和接收机的位置。卫星位置 $\mathbf{r}^s$ 和钟差 $dT^s$ 则根据配置选项“Satellite Ephemeris/Clock”从 E.4 中描述的GNSS卫星星历和时钟推导而来。

为了求解测量方程以获得接收机位置和接收机钟差，RTKLIB采用了迭代加权最小二乘法（LSE），如下所示：

$\begin{equation}
\hat{\mathbf{x}}_{i+1} = \hat{\mathbf{x}}_i + (\mathbf{H}^T \mathbf{W H})^{-1} \mathbf{H}^T \mathbf{W} (\mathbf{y} - \mathbf{h}(\hat{\mathbf{x}}_i)) \tag{E.6.22}
\end{equation}$

对于迭代加权最小二乘法的初始参数向量 $\mathbf{x}_0$ ，在单点定位的第一个历元中可以全部设置为0。一旦获得解，该位置将用于下一个历元接收机位置的迭代初值。对于权重矩阵 $\mathbf{W}$ ，RTKLIB使用以下公式：

$\begin{equation}
\mathbf{W} = diag(\sigma_1^{-2}, \sigma_2^{-2}, \ldots, \sigma_m^{-2}) \tag{E.6.23}
\end{equation}$

$\begin{equation}
\sigma^2 = {F^s}^2 R_r^2 \left(a_\sigma^2 + b_\sigma^2 / \sin El_r^s \right) + \sigma_{eph}^2 + \sigma_{ion}^2 + \sigma_{trop}^2 + \sigma_{bias}^2 \tag{E.6.24}
\end{equation}$

其中：

$F^s$：卫星系统误差因子<br>
（1: GPS, Galileo, QZSS和Beidou, 1.5: GLONASS, 3.0: SBAS）<br>
$R_r$：码/载波相位误差比率，伪距定位和载波相位定位使用的是同样的加权公式，这里以载波相位为基础，然后通过系数转换到了伪距上<br>
$a_\sigma, b_\sigma$：载波相位误差因子 $a$ 和 $b$（m）<br>
$\sigma_{eph}$：星历和钟差标准差（m）<br>
$\sigma_{ion}$：电离层校正模型误差标准差（m）<br>
$\sigma_{trop}$：对流层校正模型误差标准差（m）<br>
$\sigma_{bias}$：码偏差误差标准差（m）

::: info 手册与代码中权计算的差异
RTKLIB代码中是这样的：<br>
```c
// var = fact^2*R^2*(a^2 + (b^2/sin(el) + c^2*(10^(0.1*(snr_max-snr_rover)))) + (d*rcv_std)^2)
```
注意，这里的$R$和$F^s$都需要带上平方，手册中并没有很好的说明这一点，另外代码中还包含SNR的部分。
::: 

对于星历和钟差的标准差，RTKLIB中使用了URA（用户测距精度）或类似的指标。通过几次迭代，通常情况下解会收敛，并获得估计的接收机位置 $\hat{\mathbf{r}}_r$ 和接收机钟差 $\hat{d}t_r$ 。

$\begin{equation}
\hat{\mathbf{x}} = \lim_{i \to \infty} \hat{\mathbf{x}}_i = (\hat{\mathbf{r}}_r^T, cdt_r)^T \tag{E.6.25}
\end{equation}$

估计的接收机钟差 $\hat{d}t_r$ 没有明确输出到结果文件中。它被合并在定位解的时间标签中。这意味着定位解的时间标签表示的并不是接收机的时间标签，而是GPST中测量到的真实信号接收时间（包含钟差修正的观测时间）。

### E.6.4 接收机速度和钟漂估计

如果给出了多普勒频率观测值，可以按照以下过程估计接收机速度和钟漂。对于一个历元时间，速度估计的未知参数向量 $\mathbf{x}$ 定义为：

$\begin{equation}
\mathbf{x} = (\mathbf{v}_r^T, cd\dot{t}_r)^T \tag{E.6.26}
\end{equation}$

其中 $\mathbf{v}_r$ 和 $d\dot{t}_r$ 分别是接收机在ECEF（地心地固）坐标系中的速度（m/s）和接收机钟漂（s/s）。相应的速率测量向量 $\mathbf{y}$ 可以表示为：

$\begin{equation}
\mathbf{y} = (-\lambda_i D_{r,i}^1, -\lambda_i D_{r,i}^2, -\lambda_i D_{r,i}^3, \ldots, -\lambda_i D_{r,i}^m)^T \tag{E.6.27}
\end{equation}$

其中 $D_{r,i}^s$ 是卫星 $s$ 的 $L_i$ 多普勒频率观测值。

::: info Q1：多普勒观测值的正负

RINEX 中的多普勒观测值 $D^s$ 可以表示为：

$\begin{equation}
D_r^s  = \frac{(\mathbf{v}_r - \mathbf{v}^s) \cdot \mathbf{e}_r^s}{\lambda} = - \frac{(\mathbf{v}^s - \mathbf{v}_r) \cdot \mathbf{e}_r^s}{\lambda} = - \frac{\dot{\mathbf{r}}}{\lambda}
\end{equation}$

根据多普勒效应的物理定义：

- 当接收机和卫星靠近时， $\dot{\mathbf{r}} < 0$， $D_r^s > 0$ ，多普勒记录为正值。
- 当接收机和卫星远离时， $\dot{\mathbf{r}} > 0$， $D_r^s < 0$ ，多普勒记录为负值。

绝大部分接收机满足上述规律，可以通过查看观测文件中前后两个历元的伪距或载波相位是增加还是减小来判断多普勒的符号定义。

:::

RTKLIB总是使用 $L_1$ 频段的多普勒频率观测值。这些测量方程及其偏导数矩阵构成如下：

$\begin{equation}
\mathbf{h(x)} = \begin{pmatrix}
r_r^1 + cd\dot{t}_r - cd\dot{T}^1 \\
r_r^2 + cd\dot{t}_r - cd\dot{T}^2 \\
r_r^3 + cd\dot{t}_r - cd\dot{T}^3 \\
\vdots \\
r_r^m + cd\dot{t}_r - cd\dot{T}^m
\end{pmatrix} \quad
\mathbf{H} = \begin{pmatrix}
-\mathbf{e}_r^{1^T} & 1 \\
-\mathbf{e}_r^{2^T} & 1 \\
-\mathbf{e}_r^{3^T} & 1 \\
\vdots & \vdots \\
-\mathbf{e}_r^{m^T} & 1
\end{pmatrix} \tag{E.6.28}
\end{equation}$

这些方程中卫星相对于接收机的速度 $\mathbf{r}_r^s$ 从以下公式推导：

$\begin{equation}
\mathbf{r}_r^s = \mathbf{e}_r^{s^T} (\mathbf{v}^s(t^s) - \mathbf{v}_r) + \frac{\omega_e}{c} (v_y^s x_r + y^s v_{x,r} - v_x^s y_r - x^sv_{y,r}) \tag{E.6.29}
\end{equation}$

其中 $\mathbf{v}^s = (v_x^s, v_y^s, v_z^s)^T$ ， $\mathbf{v}_r = (v_{x,r}, v_{y,r}, v_{z,r})^T$ 。通过使用与位置估计类似的迭代最小二乘法，我们可以获得接收机的速度和钟漂：

$\begin{equation}
\hat{\mathbf{x}} = \lim_{i \to \infty} \hat{\mathbf{x}}_i = (\dot{\mathbf{v}}_r^T, c\hat{d}\dot{t}_r)^T \tag{E.6.30}
\end{equation}$

其中权重矩阵 $\mathbf{W}$ 设置为 $\mathbf{I}$ （非加权最小二乘法）。

### E.6.5 解的验证与RAIM FDE

6.3中描述的接收机位置估计可能由于未建模的观测误差而产生无效解。为了检验结果是否有效并防止无效解，RTKLIB在完成位置估计后可以采用以下验证方式。如果验证失败，将发出警告信息并拒绝该解。

$\begin{equation}
v_s = \frac{p_r^s - (\hat{\rho}_i^s + c\hat{d}t_r - cdT^s + I_r^s + T_r^s)}{\sigma_s} \tag{E.6.31}
\end{equation}$

$\begin{equation}
\mathbf{v} = (v_1, v_2, v_3, \ldots, v_m)^T \tag{E.6.32}
\end{equation}$

$\begin{equation}
\frac{v^T v}{m-n-1} < \chi^2_\alpha(m - n - 1) \tag{E.6.33}
\end{equation}$

$\begin{equation}
GDOP < GDOP_{thres} \tag{E.6.34}
\end{equation}$

其中 $n$ 是待估参数的数目， $m$ 是观测量的数目。$\chi_a^2(n)$ 是自由度 $n$ 和 $\alpha=0.001 (0.1\%)$ 的卡方分布。GDOP是几何精度因子（dilution of precision）。$GDOP_{thres}$ ，可以配置选项“Reject Threshold of GDOP”中进行相关设置。

除了上述解的检验之外，RTKLIB在版本2.4.2中还增加了RAIM（接收机自主完好性监控）FDE（故障检测与排除）功能。如果启用了配置选项“RAIM FDE”，那么在式(E.6.33)中的卡方检验失败后，RTKLIB将通过逐个排除可见卫星来进行重新估计。在重新进行了所有尝试后，选择具有最小归一化平方残差 $\mathbf{v}^T\mathbf{v}$ 的估计位置作为最终解。在这种方案中，由于卫星故障、接收机故障或较严重的多路径所导致的无效观测值将被视为异常值并被剔除掉。请注意，此功能需要2颗冗余的可见卫星，这意味着至少需要6颗可见卫星才能获得最终解。
