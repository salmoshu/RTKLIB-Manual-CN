---
sidebarDepth: 2
---

# 附录E. 模型与算法

## E.8 PPP

在 PPP 模式下，RTKLIB 同样采用与附录 E.7 中描述的方案类似的扩展卡尔曼滤波（EKF）。与基线处理不同的是，PPP 采用了类似于单点定位模型的非差（ZD）观测方程，而不是双差（DD）观测方程。

### E.8.1 PPP 非差观测模型（ZD）

利用式 (E.3.4) 和 (E.3.2)，卫星 s 的无电离层 LC 载波相位距离 $Φ_{r,LC}^s$ 和伪距 $P_{r,LC}^s$ 观测可表示为：

$$
\begin{align}
\Phi_{r,\text{LC}}^s &= \rho_r^s + c(dt_r(t_r) - dT^s(t^s)) + T_r^s + B_{r,\text{LC}}^s + d\Phi_{r,\text{LC}}^s + \varepsilon_{\Phi} \\
P_{r,\text{LC}}^s &= \rho_r^s + c(dt_r(t_r) - dT^s(t^s)) + T_r^s + \varepsilon_{P} \tag{E.8.1}
\end{align}
$$

其中，通过形成 E.5 (7) 中描述的无电离层 LC 组合，消除了式 (E.3.4) 和 (E.3.2) 中的电离层项。$B_{r,\text{LC}}^s$ 是以米为单位的载波相位偏差，$d\Phi_{r,\text{LC}}^s$ 是 $L_i$ 和 $L_j$ 的无电离层 LC 载波相位改正项，表达式如下：

$$
\begin{align}
d\Phi_{r,LC}^s &= -\left(C_i d_{r,pco,i} + C_j d_{r,pco,i}\right)^T e_{r,enu}^s + \left(E^s \left(C_i d_{pco,i}^s + C_j d_{pco,j}^s\right)\right)^T e_{r}^s \\
&+ \left(C_i d_{r,pcv,i}(El) + C_j d_{r,pcv,j}(El)\right) + \left(d_{pcv,i}^s(\theta) + d_{pcv,j}^s(\theta)\right) \\
&- d_{r,disp}^T e_{r,enu}^s + \left(C_i \lambda_i + C_j \lambda_j\right) \phi_{pw}
\end{align}
$$

### E.8.2 接收机天线相位中心模型

图 E.8-1 展示了接收机天线的 PCO（相位中心偏移）和 PCV（相位中心变化）：

- PCO 定义为接收机天线相位中心相对于ARP（天线参考点）的相对位置。
- PCV 定义为天线因仰角和方位角而产生的额外相位延迟。

各种天线类型的 PCO 和 PCV 值已通过适当的天线校准过程进行测量，并以一些标准格式的文件呈现给用户。当前版本的 RTKLIB 支持用于包含 PCO 和 PCV 数据的天线模型的 NGS PCV 和 ANTEX 格式。PCO 值通常以接收机位置的本地坐标给出。因此，接收机天线在 ECEF 坐标系下的 $L_i$ 波段 PCO $d_{r,pco,i}$ 表示为：

$$
d_{r,pco,i} = E_r^T d_{r,pco,i,enu} \tag{E.8.3}
$$

其中 $E_r$ 是由式 (E.2.10) 给出的从地心地固（ECEF）坐标系到本地坐标系的旋转矩阵，$d_{r,pco,i,enu}$ 是用本地坐标系表示的 $L_i$ 频段接收机天线相位中心偏移（PCO）。仰角为 $El$ 时 $L_i$ 频段接收机天线相位中心变化（PCV）$d_{r,pcv,i} (El)$ 可根据给定的不同仰角下的 PCV 值通过线性插值得到，如下所示：

$$
d_{r,pcv,i} (El) = \frac{(El - El_i)d_{r,pcv,i} (El_i) + (El_{i+1} - El)d_{r,pcv,i} (El_{i+1})}{El_{i+1} - El_i} \tag{E.8.4}
$$

其中 $El_i \leq El < El_{i+1}$。

<img style="width: 100%; margin: 0 auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-142743.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图E.8-1 接收机天线相位中心</p>

### E.8.3 卫星天线相位中心模型

图 E.8-1 展示了 GNSS 卫星天线的 PCO 和 PCV，与接收机天线类似，这些数据通常也由 IGS（国际GNSS服务组织）以 ANTEX 文件的形式提供。RTKLIB 也可以导入此类卫星天线模型。卫星天线的 PCO 通常以相对于卫星质心（CoM，center of mass）的卫星固连坐标给出，如图 E.8-3 所示。因此，卫星固连坐标系下的卫星天线 PCO 需要转换到地心地固（ECEF）坐标系下。PCV 也以天底角（nadir angle）来表示。

<img style="width: 100%; margin: 0 auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-142940.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图E.8-2 卫星天线相位中心</p>

<img style="width: 100%; margin: 0 auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-142944.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图E.8-3 卫星固连坐标系</p>

假设采用标准卫星姿态控制模式，从卫星固连坐标系到地心地固（ECEF）坐标系的坐标转换矩阵 $E_s$ 如下所示：

$$
\begin{align}
e_z^s &= -\frac{r^s}{|r^s|} \tag{E.8.5} \\
e_s &= \frac{r_{\text{sun}} - r^s}{|r_{\text{sun}} - r^s|} \tag{E.8.6} \\
e_y^s &= \frac{e_z^s \times e_s}{|e_z^s \times e_s|} \tag{E.8.7} \\
e_x^s &= e_y^s \times e_z^s \tag{E.8.8} \\
E_s &= (e_x^s, e_y^s, e_z^s) \tag{E.8.9}
\end{align}
$$

其中 $r_{\text{sun}}$ 是太阳在地心地固（ECEF）坐标系下的位置。天底角 $\theta$ 也可推导如下：

$$
\theta = \arccos\left(\frac{(e_r^s)^T r^s}{|r^s|}\right) \tag{E.8.10}
$$

### E.8.4 地球潮汐引起的测站位移

固定在地面上的接收机位置会受到地球潮汐效应的影响。在基线分析中，潮汐效应通常会被忽略，因为双差（DD）几乎可以消除所有这些效应。然而，在精密单点定位（PPP）模式下，该效应所引起的误差需要考虑到模型中，因为其垂直分量的振幅有时可达几十厘米。图 E.8-2 展示了地球潮汐引起的测站位移效应示例。

<img style="width: 100%; margin: 0 auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-143146.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图E.8-4 Displacement by Earth Tides</p>

### E.8.5 相位缠绕改正

相位缠绕效应是由接收机天线和卫星天线之间的相对旋转引起的相位超前和延迟。相位缠绕建模如下：

$$
\begin{align}
E_r &= \begin{pmatrix} (e_{r,x})^T , (e_{r,y})^T , (e_{r,z})^T \end{pmatrix}^T \tag{E.8.11} \\
E^s &= \begin{pmatrix} (e_x^s)^T , (e_y^s)^T , (e_z^s)^T \end{pmatrix}^T \tag{E.8.12} \\
D^s &= e_x^s - e_u^s (e_u^s \cdot e_x^s) - e_u^s \times e_y^s \tag{E.8.13} \\
D_r &= e_{r,x} - e_r^s (e_r^s \cdot e_{r,x}) + e_r^s \times e_{r,y} \tag{E.8.14} \\
\phi_{pw} &= \text{sign}(e_r^s \cdot (D^s \times D_r)) \frac{\arccos\left(\frac{D^s \cdot D_r}{\|D^s\| \|D_r\|}\right)}{2\pi} + N \tag{E.8.15}
\end{align}
$$

其中 $N$ 为整周模糊度，其取值应确保避免周跳。

### E.8.6 使用 PPP 估计 接收机位置

通过使用扩展卡尔曼滤波（EKF），精密单点定位（PPP）情况下的未知状态向量 $x$ 确定如下：

$$
x = \begin{pmatrix} (r_r)^T , v_r^T , cdt_r , Z_r , G_{N,r} , G_{E,r} , (B_{LC})^T \end{pmatrix}^T \tag{E.8.16}
$$

其中 $Z_r$ 为天顶总延迟（ZTD，zenith total delay），$G_{N,r}$ 和 $G_{E,r}$ 分别为对流层梯度的北向和东向分量。$B_{LC} = (B_{r,LC}^1, B_{r,LC}^2, \cdots, B_{r,LC}^m)^T$ 为非差（ZD，zero-differenced）无电离层 LC 载波相位偏差（单位：米）。

观测向量 $y$ 同样根据式 (E.5.21) 和 (E.5.22)，由非差无电离层 LC 相位距离和伪距观测值定义为：

$$
y = \begin{pmatrix} (\Phi_{LC})^T , (P_{LC})^T \end{pmatrix}^T \tag{E.8.17}
$$

其中：

$$
\Phi_{LC} = (\Phi_{r,LC}^1, \Phi_{r,LC}^2, \Phi_{r,LC}^3, \cdots, \Phi_{r,LC}^m)^T
$$
$$
P_i = (P_{r,LC}^1, P_{r,LC}^2, P_{r,LC}^3, \cdots, P_{r,LC}^m)^T
$$

观测模型向量 $h(x)$ 和偏导数矩阵 $H(x)$ 可表示为：

$$
h(x) = \begin{pmatrix} (h_{\Phi})^T , (h_{P})^T \end{pmatrix}^T \tag{E.8.18}
$$

$$
h_{\Phi} = 
\begin{pmatrix}
\rho_r^1 + c(dt_r - dT^1) + T_r^1 + B_{r,LC}^1 + d\Phi_{r.LC}^1 \\
\rho_r^2 + c(dt_r - dT^2) + T_r^2 + B_{r,LC}^2 + d\Phi_{r.LC}^2 \\
\vdots \\
\rho_r^m + c(dt_r - dT^m) + T_r^m + B_{r,LC}^m + d\Phi_{r.LC}^m
\end{pmatrix} \tag{E.8.19}
$$

$$
h_{P} = 
\begin{pmatrix}
\rho_r^1 + c(dt_r - dT^1) + T_r^1 \\
\rho_r^2 + c(dt_r - dT^2) + T_r^2 \\
\vdots \\
\rho_r^m + c(dt_r - dT^m) + T_r^m
\end{pmatrix} \tag{E.8.20}
$$

$$
H(x) = 
\begin{pmatrix}
-DE & 0 & 1 & DM_T & I \\
-DE & 0 & 1 & DM_T & 0
\end{pmatrix} \tag{E.8.21}
$$

$$
R = 
\begin{pmatrix}
R_{\Phi,LC} &  \\
 & R_{P,LC}
\end{pmatrix} \tag{E.8.22}
$$

其中：

$$
1 = 
\begin{pmatrix}
1 \\
1 \\
\vdots \\
1
\end{pmatrix}
$$

$$
M_T = 
\begin{pmatrix}
m_{WG,r}^1 (El_r^1) & m_{W,r}^1 (El_r^1) \cot El_r^1 \cos Az_r^1 & m_{W,r}^1 (El_r^1) \cot El_r^1 \sin Az_r^1 \\
m_{WG,r}^2 (El_r^2) & m_{W,r}^2 (El_r^2) \cot El_r^2 \cos Az_r^2 & m_{W,r}^2 (El_r^2) \cot El_r^2 \sin Az_r^2 \\
\vdots & \vdots & \vdots \\
m_{WG,r}^m (El_r^m) & m_{W,r}^m (El_r^m) \cot El_r^m \cos Az_r^m & m_{W,r}^m (El_r^m) \cot El_r^m \sin Az_r^m
\end{pmatrix}
$$

$$
R_{\Phi,LC} = \text{diag}(3\sigma_{\Phi,1}^1{}^2, 3\sigma_{\Phi,1}^2{}^2, \cdots, 3\sigma_{\Phi,1}^m{}^2)
$$

$$
R_{P,LC} = \text{diag}(3\sigma_{P,1}^1{}^2, 3\sigma_{P,1}^2{}^2, \cdots, 3\sigma_{P,1}^m{}^2)
$$

$\sigma_{\Phi,1}^s$ ：$L_1$ 载波相位观测误差的标准差（m）

$\sigma_{P,1}^s$ ：$L_1$ 伪距观测误差的标准差（m）

利用 EKF 公式以及附录 E.7 中描述的类似时间更新过程，可估计包括接收机位置和速度、接收机钟差、对流层参数以及无电离层 LC 载波相位偏差在内的未知参数。

请注意，与基线处理情况类似，为了解决载波相位偏差中的整周模糊度问题，需要卫星端的 FCB（小数周偏差）附加信息。这一过程有时被称为 PPP-AR（模糊度解算）。当前版本的 RTKLIB 仅支持法国国家空间研究中心（CNES）提供的宽巷 FCB 和 IRC（整周恢复钟）产品。该实现尚处于试验阶段，并不稳定。仅在事后处理模式下，将处理选项 “Integer Ambiguity Resolution（整周模糊度解算）” 设置为 “PPP-AR” 时，才会启用 PPP-AR 功能。

<GiscusTalk />
