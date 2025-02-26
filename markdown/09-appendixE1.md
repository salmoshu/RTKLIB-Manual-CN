---
sidebarDepth: 2
---

# 附录E1. 模型与算法（E.1-E.5）

该附录简要描述了RTKLIB中涉及的模型和算法（E.1-E.5）。附录中使用的常见模型或方程的缩写如下所示。

<style>
  .align-colon {
    display: inline-block;
    width: 100px; /* 根据最长内容调整宽度 */
  }
</style>
<span class="align-colon">$c$</span>
: 光速 (m/s)<br>
<span class="align-colon">$P_{r,i}^s$</span>
: $L_i$ 频段伪距 (m)<br>
<span class="align-colon">$D_{r,i}^s$</span>
: $L_i$ 频段多普勒频率 (Hz)<br> 
<span class="align-colon">$\phi_{r,i}^s$</span>
: $L_i$ 频段载波相位 (cycle)<br>
<span class="align-colon">$\Phi_{r,i}^s$</span>
: $L_i$ 频段载波相位 (m)<br>
<span class="align-colon">$t_r$</span>
: 导航信号发送至接收机时间，即接收机收到信号的时间 (s)<br>
<span class="align-colon">$t_s$</span>
: 卫星发送导航信号时间 (s)<br>
<span class="align-colon">$\rho_r^s$</span>
: 卫星和接收机天线之间的几何距离 (m)<br>
<span class="align-colon">$r_r^s$</span>
: 卫星和接收机天线之间的伪距率 (m/s)<br>
<span class="align-colon">$\boldsymbol{r}^s(t)$</span>
：t时刻ECEF下卫星位置（m）<br>
<span class="align-colon">$\boldsymbol{v}^s(t)$</span>
：t时刻ECEF下卫星速度(m/s)<br>
<span class="align-colon">$\boldsymbol{r}_r(t)$</span>
：t时刻ECEF下接收机天线位置（m）<br>
<span class="align-colon">$\boldsymbol{v}_r(t)$</span>
：t时刻ECEF下接收机天线速度（m/s）<br>
<span class="align-colon">$\boldsymbol{e}^s_r$</span>
：ECEF下，接收机到卫星的LOS单位向量<br>
<span class="align-colon">$\boldsymbol{e}^s_{r,enu}$</span>
：当地坐标系下，接收机到卫星的单位向量<br>
<span class="align-colon">$\boldsymbol{E}_r$</span>
：ECEF坐标系到大地坐标系的旋转变换矩阵<br>
<span class="align-colon">$\boldsymbol{E}^s$</span>
：ECEF坐标系到卫星载体坐标系的旋转变换矩阵<br>
<span class="align-colon">$\varphi_r$</span>
：接收机天线位置的纬度/大地维度<br>
<span class="align-colon">$\lambda_r$</span>
：接收机天线位置的经度/大地经度<br>
<span class="align-colon">$Az^s_r$</span>
：当前位置观测，卫星的方位角（rad）<br>
<span class="align-colon">$El_r^s$</span>
：当前位置观测，卫星的高度角（rad）<br>
<span class="align-colon">$dt_r(t)$</span>
：t时刻接收机钟差（s）<br>
<span class="align-colon">$d\dot{t}_r(t)$</span>
：t时刻接收机钟漂——频差（s/s）<br>
<span class="align-colon">$dT^s(t)$</span>
：t时刻卫星钟差（s）<br>
<span class="align-colon">$d\dot{T}^s(t)$</span>
：t时刻卫星钟漂——频差（s/s）<br>
<span class="align-colon">$T_r^s$</span>
：对流层延迟（m）<br>
<span class="align-colon">$I_{r,i}^s$</span>
：L频段电离层延迟（m）<br>
<span class="align-colon">$f_i$</span>
：$L_i$频段载波频率（Hz）<br>
<span class="align-colon">$\lambda_i$</span>
：$L_i$频段波长（m）<br>
<span class="align-colon">$B_{r,i}^s$</span>
：L频段载波偏差（cycle）<br>
<span class="align-colon">$N_{r,i}^s$</span>
：L频段载波整周模糊度（cycle）<br>
<span class="align-colon">$\varepsilon_P$</span>
：伪距测量误差（m）<br>
<span class="align-colon">$\varepsilon_\phi$</span>
：载波相位测量误差（cycle）<br>
<span class="align-colon">$\varepsilon_\Phi$</span>
：载波相位测量误差（m）<br>
<span class="align-colon">$\omega_e$</span>
：地球自转角速度（rad/s）<br>
<span class="align-colon">$\boldsymbol{d}_{r,pco,i}$</span>
：当地坐标系下，L频段接收机天线相位中心偏差(PCO)（m）<br>
<span class="align-colon">$\boldsymbol{d}_{pco,i}^s$</span>
：卫星载体坐标系下，卫星天线L波段相位中心偏差(PCO)（m）<br>
<span class="align-colon">$d_{r,pcv,i}(El)$</span>
：$L_i$频段接收机天线相位中心变化PCV（m）<br>
<span class="align-colon">$d_{pcv,i}^s(\theta)$</span>
：$L_i$频段卫星天线相位中心变化PCV（m）<br>
<span class="align-colon">$\alpha$</span>
：接收机天线到卫星的天底角（$\alpha$）<br>
<span class="align-colon">$\boldsymbol{d}_{r,disp}$</span>
：当地坐标系下，接收机天线位置处的地球潮偏移<br>
<span class="align-colon">$\phi_{pw}$</span>
：相位缠绕效应（cycle）<br>
<span class="align-colon">$dT_{rel}^s$</span>
：卫星时钟的相对论改正/Trel<br>
<span class="align-colon">$\boldsymbol{U}(t)$</span>
：t时刻ECI和ECEF的坐标系转换矩阵<br>
<span class="align-colon">$\boldsymbol{R}_x(\theta)$</span>
：绕X轴旋转θ得到的坐标旋转矩阵<br>
<span class="align-colon">$\boldsymbol{R}_y(\theta)$</span>
：绕Y轴旋转θ得到的坐标旋转矩阵<br>
<span class="align-colon">$\boldsymbol{R}_z(\theta)$</span>
：绕Z轴旋转θ得到的坐标旋转矩阵<br>
<span class="align-colon">$()^{jk}$</span>
：卫星端，卫星j和k之间的单差<br>
<span class="align-colon">$()_{rb}$</span>
：地面端，基准站b和移动站/r之间的单差<br>
<span class="align-colon">$Z_{T,r}$</span>
：对流层天顶总延迟（m）<br>
<span class="align-colon">$Z_{H,r}$</span>
：对流层天顶干延迟（m）<br>
<span class="align-colon">$G_{E,r}$</span>
：对流层东向梯度分量<br>
<span class="align-colon">$G_{N,r}$</span>
：对流层北向梯度分量 <br>
<span class="align-colon">$m_{H,r}^s$</span>
：天顶对流层干延迟映射函数<br>
<span class="align-colon">$m_{W,r}^s$</span>
：天顶对流层湿延迟映射函数<br>
<span class="align-colon">$m_{I,r}^s$</span>
：电离层延迟映射函数<br>

## E.1 时间系统

RTKLIB内部采用GPS时间进行GNSS数据处理和定位；UTC时间（协调世界时）是一个不连续时间，简而言之，它和GPS时间之间存在一个跳秒（或闰秒）的关系。GPS时间通常采用周数和周内秒来表示其起始历元为1980年1月6日00:00:00 UTC。但是对于double类型变量来说，其距离精度只有0.04米。因此，在实际运算时，RTKLIB调用C标准库中的时间结构体并定义了`gtime_t`类型：

```c
typedef struct {
    time_t time; /* time (s) expressed by standard time_t */
    double sec;  /* fraction of second under 1 s */
} gtime_t;
```

`time_t` 通常通过一个无符号的32位整数来实现，表示从1970年1月1日00:00:00开始的总秒数。由于整数位的长度限制，，`gtime_t` 无法处理1970年1月1日之前或2038年1月19日之后的时间。

`gtime_t` 中的`sec`字段用于存储小数秒。`sec`在距离上的分辨率为6.7×10⁻⁸米，足以满足GNSS高精度定位计算的需求。RTKLIB还提供了几个有用的API来处理`gtime_t`，包括加、减以及时间格式的转换。

不同导航系统的时间定义有所差异，以下是所有时间系统特性的汇总。

<p style="text-align: center;">表E.1-1 时间系统汇总</p>
<table class="tg"><thead>
  <tr>
    <th class="tg-9wq8">时间系统</th>
    <th class="tg-9wq8">连续性</th>
    <th class="tg-9wq8">起始历元</th>
    <th class="tg-0pky">与UTC的关系</th>
    <th class="tg-0pky">转换公式</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-lboi">GPST</td>
    <td class="tg-lboi">连续</td>
    <td class="tg-0pky">1980年1月6日00:00:00 UTC</td>
    <td class="tg-0pky">UTC比GPST慢，差值为累积的闰秒数（Δt<sub>LS</sub>）</td>
    <td class="tg-0pky">t<sub>UTC</sub>=t<sub>GPST</sub>-Δt<sub>LS</sub></td>
  </tr>
  <tr>
    <td class="tg-lboi">GLONASST</td>
    <td class="tg-lboi">不连续</td>
    <td class="tg-0pky">与UTC（SU）对齐</td>
    <td class="tg-0pky">UTC比GLONASST慢3小时，差值为固定偏移和闰秒数</td>
    <td class="tg-0pky">t<sub>UTC</sub>=t<sub>GLONSASST</sub>-10800</td>
  </tr>
  <tr>
    <td class="tg-0pky">GST</td>
    <td class="tg-0pky">连续</td>
    <td class="tg-0pky">1999年8月22日00:00:00 UTC</td>
    <td class="tg-0pky">GST比UTC快13秒，与GPST对齐，但有1024周的差异和小时间偏移（GGTO）</td>
    <td class="tg-0pky">无直接公式，需考虑与GPST的对齐关系</td>
  </tr>
  <tr>
    <td class="tg-0pky">QZSST</td>
    <td class="tg-0pky">连续</td>
    <td class="tg-0pky">与GPST对齐</td>
    <td class="tg-0pky">QZSST与GPST具有相同的起始点和秒定义</td>
    <td class="tg-0pky">无直接公式，可直接处理为GPST</td>
  </tr>
  <tr>
    <td class="tg-0pky">BDT</td>
    <td class="tg-0pky">连续</td>
    <td class="tg-0pky">2006年1月1日00:00:00 UTC</td>
    <td class="tg-0pky">BDT比UTC快14秒，时间差被控制在100纳秒以内（模1秒）</td>
    <td class="tg-0pky">t<sub>BDT</sub>=t<sub>GPST</sub>-14</td>
  </tr>
</tbody></table>

**1. GPST（GPS时间）和UTC（协调世界时）**

现在的UTC时间比GPST慢，因为它当前闰掉的时间为负数。GPST与UTC（协调世界时）之间的粗略转换可以简单地表示为：  

$\begin{equation}
t_{UTC}=t_{GPST}-\Delta{t}_{LS} \tag{E.1.1}
\end{equation}$
$\begin{equation}
t_{GPST}=t_{UTC}+\Delta{t}_{LS} \tag{E.1.2}
\end{equation}$
 
其中，$t_{UTC}$和$t_{GPST}$分别是以秒为单位的UTC时间和GPS时间。$\Delta{t}_{LS}$是由于自1980年1月6日以来累积的闰秒（或跳秒）导致的UTC与GPST之间的时间差（秒）。$\Delta{t}_{LS}$的值如表E.1-1所示。

<p style="text-align: center;">表E.1-2 GPST-UTC值（截至2025年2月）</p>

```text
Time Since (in UTC)     𝛥𝑡𝐿𝑆 (s)
-----------------------------
1980-01-06 00:00:00       0
1981-07-01 00:00:00       1
1982-07-01 00:00:00       2
1983-07-01 00:00:00       3
1987-01-01 00:00:00       4
1988-01-01 00:00:00       5
1990-01-01 00:00:00       6
1991-01-01 00:00:00       7
1992-07-01 00:00:00       8
1993-07-01 00:00:00       9
1994-07-01 00:00:00      10
1996-01-01 00:00:00      11
1999-01-01 00:00:00      13
1997-07-01 00:00:00      12
2006-01-01 00:00:00      14
2009-01-01 00:00:00      15
2012-07-01 00:00:00      16
2015-07-01 00:00:00      17
2017-01-01 00:00:00      18
```

公式（E.1.1）或（E.1.2）的近似精度在几十纳秒以内。通过使用GPS导航消息中的UTC参数，我们可以更精确地将GPST与UTC之间的转换，公式如下：  

$\begin{equation}
\small t_{𝑈𝑇𝐶} = 𝑡_{𝐺𝑃𝑆𝑇} − \{\Delta𝑡_{𝐿𝑆} + 𝐴_0 + 𝐴_1(𝑡_E − 𝑡_{𝑜𝑡} + 604800(𝑊𝑁 − 𝑊𝑁_𝑡))\} \tag{E.1.3}
\end{equation}$

其中，$𝐴_0$、$𝐴_1$、$𝑡_𝐸$、$𝑡_{𝑜𝑡}$、$𝑊𝑁$和$𝑊𝑁_𝑡$是GPS导航消息中提供的UTC参数。更严格地说，公式（E.1.3）中的UTC是UTC(USNO)，即美国本地实现的UTC。UTC与UTC(USNO)之间的差异可以在BIPM提供的T公报中获得，这种差异通常在纳秒级别。

**2. GLONASST (GLONASS时间)**

GLONASST（GLONASS时间）基于UTC（SU），因此它同样包含了闰秒。GLONASST还与当地时间对齐。因此，大致上，GLONASST中的时间$t_{GLONASST} (s)$可以转换为UTC中的时间$t_{UTC} (s)$。

$\begin{equation}
t_{UTC}=t_{GLONASST}-10800 \tag{E.1.4}
\end{equation}$

更准确地说，GLONASS导航信息中的GLONASST的UTC参数应类似于GPST（GPS时间）和UTC转换的方式使用。忽略闰秒和3小时的时差，GPST与GLONASST之间的差异通常为100纳秒或几个100纳秒的水平。

**3. GST (Galileo时间)**

伽利略系统时间（Galileo System Time，GST）与GPST类似，同样有周与周内秒组成。GST的起始历元为1999年8月22日00:00:00 UTC（这个时间刚好和GPS的起始时间1980年1月6日00:00:00 UTC相隔1024周）。在起始历元时，GST比UTC快13秒。GST是连续时间，不插入或删除闰秒。因此，GST与GPS时间（GPST）对齐，除了时间系统原点的1024周差异和一个小的时间偏移（Galileo/GPS时间差异，GGTO）。根据RINEX约定，伽利略周数与GPS周数相等。

**4. QZSST (QZSS时间)**

QZSST（QZSS时间）与GPST（GPS时间）对齐。它与GPST具有相同的起始点和相同的秒定义，QZSST可以像GPST一样处理。

**5. BDT (北斗时间)**

北斗时间（BDT）也是一个连续时间系统（没有闰秒的插入或删除）。BDT的起始历元为2006年1月1日00:00:00 UTC。BDT与UTC之间的时间差被控制在100纳秒以内（模1秒）。因此，GPS时间（GPST）中的时间$𝑡_{𝐺𝑃𝑆}(s)$可以大致通过以下公式转换为BDT中的时间$𝑡_{BDT}(s)$，精度约为200纳秒：

$\begin{equation}
𝑡_{𝐵𝐷𝑇} = 𝑡_{𝐺𝑃𝑆} − 14 \tag{E.1.5}
\end{equation}$

BDT同样需要使用北斗导航信息中的UTC参数，然后进行类似GPST与UTC之间的转换。

::: tip Q1：为什么使用double类型表示GPS时间，距离精度是0.04米？
首先搞清楚要干嘛。这里是想说明以double类型来表示GPS时间，数值精度会很低。我们可以进行如下分析：

**1. double 类型的精度**
- double 类型是 64 位浮点数，遵循 IEEE 754 标准。  
- 其尾数部分有 52 位，因此精度为：  
  $精度=2^{−52}≈2.22×10^{−16}$

**2. GPS 时间的表示**
- GPS 时间通常以周数（week）和周内秒（second of week）表示。
- 一周的总秒数为：  
  604800s (7天×86400s/天)  
- 用 double 类型表示 GPS 时间时，其最小分辨率为：  
  $时间分辨率=604800×2^{−52}≈1.34×10^{−10}s$

**3. 时间分辨率转换为距离误差**
- 光速 c 为：  
  $c=3×10^8m/s$
- 时间分辨率对应的距离误差为（使用 double 类型表示 GPST 时的最小分辨率）：  
  $距离误差=c×时间分辨率=3×10^8×1.34×10^{−10} ≈0.04m$
- 如果用double类型表示一个比604800更大的数值时候，距离误差还会比0.04m更大（当时间需要参与运算时，要考虑这个误差的；当时间只是用来标识与显示时，则可以不用管这个误差）。
:::

## E.2 坐标系统

在RTKLIB中，接收机和卫星的位置以ECEF（地心地固坐标系）的X、Y、Z分量表示。ECEF框架的选择取决于卫星星历和基准站位置，并未明确固定。例如，使用GPS信号时，单点定位结果通常基于WGS84坐标系。在基线分析中，移动站的位置会被转换到与基准站相同的ECEF框架中。实际上，常用的ECEF框架（如WGS84、PZ90.02和ITRF）在广播星历或单点定位精度范围内是等效的。然而，对于基线分析或PPP（精密单点定位），需要更严格地处理坐标系，使用统一的坐标系可以有效减少处理误差。

(1) 从大地坐标转换为ECEF XYZ坐标
大地坐标是基于参考椭球体定义的，如图E.2-1所示。大地纬度$\phi_r$、经度$\lambda_r$和椭球高$h$可以转换为ECEF XYZ坐标$rr = (x, y, z)^T$，转换公式如下：

$\begin{equation}
e^2 = f(2-f) \tag{E.2.1}
\end{equation}$

$\begin{equation}
v = \frac{a}{\sqrt{1-e^2sin{\phi_r}^2}} \tag{E.2.2}
\end{equation}$

$\begin{equation}
r_r=\begin{pmatrix}
(v+h)cos\phi_r cos\lambda_r  \\
(v+h)cos\phi_r sin\lambda_r  \\
v(1-e^2)sin\phi_r
\end{pmatrix}\tag{E.2.3}
\end{equation}$

其中，<br>
$a$：地球参考椭球体的长半轴（米）<br>
$f$：地球参考椭球体的扁率

当前版本的RTKLIB始终使用WGS84坐标系提供的参考椭球体的$a$和$f$值如下：

$\begin{equation}
a = 6378137.0 (m)
\end{equation}$ <br>
$\begin{equation}
f = 1.0/298.257223563
\end{equation}$

![Reference Ellipsoid](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250218-224823.jpg)
<p style="text-align: center;">图E.2-1  参考椭球体</p> 

(2) 将ECEF坐标转换为大地坐标
将ECEF中的XYZ位置 $r_r ​= (x,y,z)^T$ 转换为大地坐标，需采用以下步骤。在此过程中，大地纬度是通过迭代方法计算得出的。

$\begin{equation}
r = \sqrt{x^2 + y^2} \tag{E.2.4}
\end{equation}$

$\begin{equation}
\Phi_{r,0} = 0       \tag{E.2.5}
\end{equation}$

$\begin{equation}
\Phi_{r,i+1} = arctan(\frac{z}{r} - \frac{ae^2tan\phi_{r,i}}{r\sqrt{1+(1-e^2)tan^2\phi_{r,i}}}) \tag{E.2.6}
\end{equation}$

$\begin{equation}
\Phi_{r,0} = \lim\limits_{x \to \infty} \phi_{r,i} \tag{E.2.7}
\end{equation}$

$\begin{equation}
\lambda = ATAN2(y,x) \tag{E.2.8}
\end{equation}$

$\begin{equation}
h = \frac{r}{cos\phi_r} - \frac{a}{\sqrt{(1-e^2)sin^2\phi_r}} \tag{E.2.9}
\end{equation}$

(3) 本地坐标与ECEF坐标之间的转换
接收机位置的局部坐标（也称为ENU坐标）在GNSS导航处理中经常被使用。从ECEF坐标到局部坐标的旋转矩阵 $E_r$​ 表示为：

$\begin{equation}
E_r=\begin{pmatrix}
-sin\lambda_r            & cos\lambda_r            & 0                 \\
-sin{\phi_r}cos_{\lambda_r} & -sin{\phi_r}sin{\lambda_r} & cos{\phi_r} \\
cos{\phi_r}cos{\lambda_r}   & cos{\phi_r}sin{\lambda_r}  & sin\phi_r
\end{pmatrix}\tag{E.2.10}
\end{equation}$

其中，$\phi_r$和$\lambda_r$分别是接收机位置的大地纬度和经度。通过使用旋转矩阵$E_r$以及接收机在ECEF坐标系中的位置$r_r$，可以将ECEF坐标系中的位置$r_{ECEF}$转换为局部坐标系中的位置$r_{local}$，转换公式如下：

$\begin{equation}
r_{local} = E_r(r_{ECEF} - r_r) \tag{E.2.11}
\end{equation}$

(4)  似大地水准面模型
为了从椭球高得到大地高，我们需要考虑似大地水准面高。大地高 hgeod​（单位：米）表示为：

$\begin{equation}
h_{geod} = h - geod(\phi_r, \lambda_r) \tag{E.2.12}
\end{equation}$

其中，$geoid{\phi_r, \lambda_r}$ 是在纬度 $\phi_r$ 和经度 $\lambda_r$​ 处的似大地水准面高（单位：米）。RTKLIB支持通过处理选项“似大地水准面模型”选择以下几种似大地水准面模型。所有这些模型均以纬度和经度网格点上的似大地水准面高提供。RTKLIB使用双线性插值来获取适用于特定纬度和经度位置的值。除了内部模型外，使用这些似大地水准面模型时，需要下载相应的似大地水准面数据文件，并将其文件路径设置为处理选项“似大地水准面数据文件”。

(a) 内置模型：基于EGM96的1度×1度网格似大地水准面模型
(b) EGM96：15" x 15" 网格的EGM96似大地水准面模型
(c) EGM2008：2.5" x 2.5" 网格或1弧秒×1弧秒网格的EGM2008似大地水准面模型
(d) GSI 2000：1" x 1.5" 弧秒网格的GSI 2000似大地水准面模型（仅适用于日本列岛周边）

## E.3 GNSS信号测量模型

### 3.1 GNSS信号结构

图E.3-1展示了一个典型的GNSS信号结构。GNSS信号通常由载波（Carrier）、伪码（Code）和导航数据码（Data）的乘积组成。具体的，伪码和数据码一起先通过调制而依附在正弦波形式的载波上，然后卫星将调制后的载波信号播发出去。

GPS、GLONASS、Galileo、QZSS、北斗和SBAS提供的GNSS信号详细规格可以在[附录G](/algorithm/RTKLIB手册/11-appendixG)中找到。

![](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250218-224511.jpg)
<p style="text-align: center;">图E.3-1 GNSS信号结构</p> 

### 3.2 伪距测量模型

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

### 3.3 载波相位测量模型

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

$N_{r,i}^s$ 通常被称为载波相位整数模糊度、载波整周模糊度或简称为模糊度。有关载波相位校正项的详细公式，请参考[附录 E.8](/algorithm/RTKLIB手册/09-appendixE2#e-8-ppp)。

### 3.4 卫地距（接收机与卫星天线）

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

### 3.5 卫星方位角与仰角

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

其中，$\mathbf{E}_r$ 是从 ECEF 坐标系到接收机位置的局部坐标系的坐标旋转矩阵。有关该矩阵的详细形成，请参考 [附录 E.2](/algorithm/RTKLIB手册/09-appendixE1#e-2-坐标系统)。

![ Local Coordinates and Azimuth and Elevation Angles](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250219-235235.jpg)
<p style="text-align: center;">图E.3-4 局部坐标系与方位角和仰角</p>

## E.4 GNSS卫星星历与钟差

RTKLIB 支持 GPS、GLONASS、Galileo、QZSS、BeiDou 和 SBAS 的广播星历和钟差数据。它还支持作为 SP3-c 文件和钟差 RINEX 文件提供的精确星历和钟差数据，这些文件包括 Galileo、QZSS 和 BeiDou，用于后处理模式。对于实时模式，支持使用 SBAS 长期和快速修正后的广播星历和钟差数据，以及使用 RTCM 3 SSR（状态空间表示）修正的数据。以下方程展示了 RTKLIB 中使用的星历和钟差模型。

(1) GPS、Galileo 和 QZSS 的广播星历和钟差数据 [1][5][6]

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

**2. GLONASS的广播星历和时钟** [4]

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

**3. 北斗卫星的广播星历和时钟** [7]

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

**4. SBAS的广播星历和时钟** [8]

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

**5. SBAS轨道和时钟改正** [8]

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

**6. 精密星历和时钟** 

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

## E.5 对流层与电离层模型

**1. 对流层模型**

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

**2. SBAS 对流层模型**

如果处理选项“对流层校正”设置为“SBAS”，则应用 SBAS 接收器规范中定义的 SBAS 对流层模型。该模型常被称为“MOPS 模型”。参考 [8] A.4.2.4 获取详情。

**3. 精确对流层模型**

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

**4. 广播电离层模型**

对于单频GNSS用户的电离层校正，GPS和QZSS导航数据包括以下广播电离层参数。

$\begin{equation}
\mathbf{p}_{ion} = (\alpha_0, \alpha_1, \alpha_2, \alpha_3, \beta_0, \beta_1, \beta_2, \beta_3)^T \tag{E.5.5}
\end{equation}$

通过使用这些电离层参数，可以按照以下过程推导出$L_1$电离层延迟$I_r^s$（米）。该模型通常称为Klobuchar模型。

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

**5. SBAS 电离层模型**

SBAS 对电离层延迟的校正由消息类型 18（电离层网格点掩模）和消息类型 26（电离层延迟校正）提供。如果处理选项 "Ionosphere Correction" 设置为 "SBAS" 并且这些 SBAS 消息在输入文件中提供，RTKLIB 使用 SBAS 电离层校正。关于模型的算法和 IGPs（电离层网格点）的定义，请参阅 SBAS 接收器规范 [8] 的 A.4.4.9 和 A.4.4.10。

**6. 单层模型**

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

其中$R_E$是地球的半径（米），$H$是电离层壳体的高度（米）。RTKLIB通常使用$R_E = 6378137$和$H = 350000$。请注意，在该模型中，地球表面或电离层壳体被近似为一个球体。

如果在IPP和时间$t$处给出了VTEC（垂直总电子含量）值$TEC(t, \phi_{IPP}, \lambda_{IPP})$，则$L_i$电离层延迟$I_{r,i}^s)（米）可以表示为：

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

**7. 无电离层影响的LC（线性组合）**

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

其中$f_i$和$f_j$是$L_i$和$L_j$测量的频率（Hz）。当前版本的RTKLIB总是对GPS、GLONASS和QZSS使用$L_1$和$L_2$，对Galileo使用$L_1$和$L_5$进行无电离层影响的LC。如果在单一或PPP模式下将处理选项 "Ionosphere Correction" 设置为 "Iono-Free LC"，则无电离层影响的LC用于基本测量以消除电离层项。请注意，无电离层影响的LC模型不适用于Kinematic、Static或Moving-base模式。详情请参阅E.7(6)。
