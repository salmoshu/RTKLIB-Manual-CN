---
sidebarDepth: 2
---

# 附录E. 模型与算法

该附录简要描述了RTKLIB中涉及的模型和算法。附录中使用的常见模型或方程的缩写如下所示。

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

**1. GNSS信号结构**

图E.3-1展示了一个典型的GNSS信号结构。GNSS信号通常由载波频率（Carrier）、扩频码（Code）和导航数据（Data）的乘积组成。扩频码也称为伪随机噪声（PRN）码。GPS、GLONASS、Galileo、QZSS、北斗和SBAS提供的这些GNSS信号的详细规格可以在附录G中找到。

![GNSS Signal Structure](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250218-224511.jpg)
<p style="text-align: center;">图E.3-1 GNSS信号结构</p> 

**2. 伪距测量模型**

伪距指的“从接收机天线到卫星天线的距离，其中包含了接收机和卫星的时钟偏移（以及其他偏差，如大气延迟）”[9]。$L_i$伪距 $P_{r,i}^s$ 可以用接收机时钟测量的接收信号时间 $\overline{t}_r(s)$ 和卫星时钟测量的发送信号时间 $\overline{t}^s(s)$ 来表达：

$\begin{equation}
P_{r,i}^s = c(\overline{t}_r - \overline{t}^s) \tag{E.3.1}
\end{equation}$

该方程可以用卫星与接收机天线之间的几何距离 $\rho_r^s$，接收机和卫星的时钟偏差 $dt_rdT^s$，电离层和对流层延迟 $I_{r,i}^s$，$T_r^s$，以及测量误差 $\varepsilon_P$ 来表示如下：[64]

$\begin{align}
P_{r,i,s} 
&= c( (t_r + dt_r(t_r)) - (t^s + dT^s(t^s)) ) + \varepsilon_P \\
&= c(t_r - t^s) + c(dt_r(t_r)) + \varepsilon_P \\
&= (\rho_r^s + I_{r,i}^s + T_r^s) + c(dt_r(t_r) - dT^s(t^s)) + \varepsilon_P \\
&= \rho_r^s + c(dt_r(t_r) - dT^s(t^s)) + I_{r,i}^s + T_r^s + \varepsilon_P \tag{E.3.2}
\end{align}$

![Pseudorange Model ](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250219-215841.jpg)
<p style="text-align: center;">图E.3-2 伪距模型</p> 

**3. 载波相位测量模型**

载波相位是“对卫星信号接收载波与接收机生成的参考频率之间的拍频进行测量”[9]。第 $L_i$ 个载波的载波相位 $\phi_{r,i}^s$ 可以表示为：

$\begin{align}
\small \phi_{r,i}^s
& \small = \phi_{r,i}(t_r) - \phi_i^s(t^s) + N_{r,i}^s + \varepsilon_\phi \\
& \small = (f_i(t_r + dt_r(t_r) - t_0) + \phi_{r, 0, i}) - (f_i(t^s + dT^s(t^s) - t_0) + \phi_{0,i}^s) + N_{r,i}^s + \varepsilon_\phi \\ 
& \small = \frac{c}{\lambda_i}(t_r - t^s) + \frac{c}{\lambda_i}(dt_r(t_r) - dT^s(t^s)) + (\phi_{r,0,i} - \phi_{0,i}^s + N_{r,i}^s) + \varepsilon_\phi \tag{E.3.3}
\end{align}$

其中，$t_0$ 是初始时间（秒），$φ\phi_{r,i}(t)$ 是接收机本地振荡器的 $L_i$ 相位（周期），$\phi_i^s$ 是时间 $t$ 时发射的导航信号的 $L_i$ 相位（周期）。$\phi_{r,0,i}$ 是接收机本地振荡器在时间 $t_0$ 时的 $L_i$ 初始相位（周期），$\phi_{r,0,i}$ 是时间 $t_0$ 时发射的导航信号的 $L_i$ 初始相位（周期）。

$L_i$ 相位范围 $\Phi_{r,i}^s$，定义为载波相位乘以载波频率 $\lambda_i$（以米为单位），也可以通过使用载波相位偏差 $B_{r,i}^s$ 和载波相位校正项 $d\Phi_{r,i}^s$ 来表达，后者包括天线相位中心偏移和变化、地球潮汐导致的站点位移、相位缠绕效应以及卫星钟的相对论校正：

$
\begin{align}
\small \Phi_{r,i}^s 
& \small = \lambda \phi_{r,i}^s \\
& \small = c(t_r - t^s) + c(dt_r(t_r) - dT^s(t^s)) + \lambda_i (\phi_{r,0,i} - \phi_{0,i}^s + N_{r,i}^s) + \lambda_i \varepsilon_\phi \\
& \small = \rho_r^s + c(dt_r(t_r) - dT^s(t^s)) - I_{r,i}^s + T_r^s + \lambda_i B_{r,i}^s + d\Phi_{r,i}^{s\prime} + \varepsilon_\phi
\tag{E.3.4}
\end{align}
$

$N_{r,i}^s$ 通常被称为载波相位整数模糊度、载波周期模糊度或简称为模糊度。有关载波相位校正项的详细公式，请参考附录 E.9。

**4. 接收机与卫星天线之间的几何距离**

几何距离被定义为卫星天线相位中心位置与接收机天线相位中心位置在惯性坐标系中的物理距离。首先，信号传输时间 $t^s$ 可以通过以下方式推导出来：

$
\begin{equation}
t^s = \overline{t}_r - \frac{P_{r,i}^s}{c} - dT(t^s) \tag{E.3.7}
\end{equation}
$

方程的两边都包含了 $t^s$。因此，需要进行几次迭代来解这个方程。几何距离可以通过在 ECEF（地心地固）坐标系中使用接收机和卫星天线相位中心位置在时间 $t_r$ 时的 $\mathbf{r}_r(t_r) = (x_r, y_r, z_r)^T$ 和在时间 $t_r$ 时的 $\mathbf{r}^s(t^s) = (x^s, y^s, z^s)^T$ 来表示：

$
\begin{equation}
\rho_r^s = \| \mathbf{U}(t_r) \mathbf{r}_r(t_r) - \mathbf{U}(t^s) \mathbf{r}^s(t^s) \| \tag{E.3.8}
\end{equation}
$

其中，$U(t)$ 是时间 $t$ 时从 ECEF（地心地固）到 ECI（地心惯性）坐标系的转换矩阵。为了在 ECEF 坐标系中表达几何距离，必须考虑地球自转效应以获得几何距离。该方程可以用以下方程之一近似，精度在1毫米级别以内。当前版本的 RTKLIB 始终使用方程 (F.3.8b) 来计算几何距离。(F.3.8b) 中的最后一个项有时被称为萨格纳克效应（Sagnac effect）。

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

**5. 卫星方向的方位角和仰角**

接收机到卫星的单位视线（LOS）向量在 ECEF（地心地固）坐标系中可以表示为：

$
\begin{equation}
\mathbf{e}_r^s = \frac{\mathbf{r}^s(t^s) - \mathbf{r}_r(t_r)}{\|\mathbf{r}^s(t^s) - \mathbf{r}_r(t_r)\|} \tag{E.3.9}
\end{equation}
$

在方程中，忽略了地球自转效应。卫星方向从接收站点出发的方位角 $Az_r^s$ 和仰角 $El_r^s$ 可以从以下公式推导出来：

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

其中，$\mathbf{E}_r$ 是从 ECEF 坐标系到接收机位置的局部坐标系的坐标旋转矩阵。有关该矩阵的详细形成，请参考 E.2。

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

## E.6 单点定位

RTKLIB采用迭代加权最小二乘法（LSE）用于“Single”（单点定位）模式，无论是否使用SBAS校正。

**1. 线性LSE**

假设给定测量向量$y$，它可以被建模为未知参数向量$x$和一个随机测量误差向量$v$的以下线性方程：

$\begin{equation}
y = Hx + v \tag{E.6.1}
\end{equation}$

最小二乘代价函数$J_{LS}$定义为测量误差的平方和：

$\begin{equation}
J_{LS} = v_1^2 + v_2^2 + \ldots + v_m^2 = v^Tv \tag{E.6.2}
\end{equation}$

通过使用(E.6.1)和(E.6.2)，代价函数可以重写为：

$\begin{align}
J_{LS} 
&= (y - Hx)^T(y - Hx) \\
&= y^Ty - y^THx - x^TH^Ty + x^TH^THx \tag{E.6.3}
\end{align}$

为了最小化代价函数，$J_{LS}$的梯度应该为零。然后

$\begin{align}
\frac{\partial J_{LS}}{\partial x} 
&= 0^T - y^TH - (H^Ty)^T + (H^THx)^T + x^TH^TH \\
&= -2y^TH + 2x^TH^TH = 0 \tag{E.6.4}
\end{align}$

这给出了所谓的“正规方程”：

$\begin{equation}
H^THx = H^Ty \tag{E.6.5}
\end{equation}$

为了解正规方程，我们可以通过最小二乘法得到估计的未知参数向量$\hat{x}$：

$\begin{equation}
\hat{x} = (H^TH)^{-1}H^Ty \tag{E.6.6}
\end{equation}$

如果给定了每个测量的权重，可以使用权重矩阵$W$重写代价函数(E.6.3)。

$\begin{equation}
J_{WLS} = v^T W v \tag{E.6.7}
\end{equation}$

为了最小化代价函数$J_{WLS}$，我们可以通过加权最小二乘法（WLS）以类似简单最小二乘法的方式获得估计的未知参数向量$\hat{x}$：

$\begin{equation}
\hat{x} = (H^T W H)^{-1} H^T W y \tag{E.6.8}
\end{equation}$

加权最小二乘法（WLS）的权重矩阵$W$通常定义为：

$\begin{equation}
W = diag(\sigma_1^{-2}, \sigma_2^{-2}, \ldots, \sigma_m^{-2})
\end{equation}$

其中$\sigma_i$是第$i$个测量误差的先验标准差。

**2. 非线性最小二乘的高斯-牛顿迭代**

如果测量值不是以线性模型给出的，测量方程可以由一个通用的非线性向量函数表示为：

$\begin{equation}
y = h(x) + v \tag{E.6.9}
\end{equation}$

其中$h(x)$是参数向量$x$的测量向量函数。该方程可以通过在初始参数向量$x_0$周围使用泰勒级数展开来扩展：

$\begin{equation}
h(x) = h(x_0) + H(x - x_0) + \ldots \tag{E.6.10}
\end{equation}$

其中$H$是$h(x)$关于$x$在$x = x_0$处的偏导数矩阵：

$\begin{equation}
H = \left.\frac{\partial h(x)}{\partial x}\right|_{x=x_0} \tag{E.6.11}
\end{equation}$

假设初始参数足够接近真实值，并且可以忽略泰勒级数的第二项和后续项。我们可以将(E.6.9)近似为：

假设初始参数足够接近真实值，并且可以忽略泰勒级数的第二项和后续项。我们可以将(E.6.9)近似为：

$\begin{equation}
y \approx h(x_0) + H(x - x_0) + v \tag{E.6.12}
\end{equation}$

然后我们可以得到以下线性方程。

$\begin{equation}
y - h(x_0) = H(x - x_0) + v \tag{E.6.13}
\end{equation}$

通过将线性加权最小二乘法（E.6.8）应用于(E.6.13)，我们可以得到非线性加权最小二乘法的正规方程：

$\begin{equation}
H^T W H (\hat{x} - x_0) = H^T W (y - h(x_0)) \tag{E.6.14}
\end{equation}$

因此，我们可以通过以下方式获得估计的未知参数向量$\hat{x}$：

$\begin{equation}
\hat{x} = x_0 + (H^T W H)^{-1} H^T W (y - h(x_0)) \tag{E.6.15}
\end{equation}$

如果初始参数$x_0$不够接近真实值，我们可以迭代改进估计参数如下：

$\begin{equation}
\hat{x}_0 = x_0 \tag{E.6.16}
\end{equation}$

$\begin{equation}
\hat{x}_{i+1} = \hat{x}_i + (H^T W H)^{-1} H^T W (y - h(\hat{x}_i)) \tag{E.6.17}
\end{equation}$

如果迭代收敛，我们可以得到最终的估计参数为：

$\begin{equation}
\hat{x} = \lim_{i \to \infty} \hat{x}_i \tag{E.6.18}
\end{equation}$

迭代最小二乘法通常被称为高斯-牛顿方法。请注意，对于具有较大非线性的病态测量方程，简单的高斯-牛顿方法并不总是能够收敛。在这些情况下，我们应该采用另一种策略来处理这种非线性最小二乘问题。对于非线性最小二乘法，最流行的方法是非线性最小二乘的LM（Levenberg-Marquardt）方法。

**3. 接收机位置和时钟偏差的估计**

对于“Single”模式作为“Positioning Mode”，应用以下单点定位过程，通过逐历元的方式获得最终解。对于一个历元时间，未知参数向量$x$定义为：

$\begin{equation}
x = (r_r^T, cdt_r)^T \tag{E.6.19}
\end{equation}$

伪距测量向量$y$可以表示为：

$\begin{equation}
y = (P_r^1, P_r^2, P_r^3, \ldots, P_r^m)^T \tag{E.6.20}
\end{equation}$

其中$P_r^s$是伪距测量。如果处理选项"Ionosphere Correction"设置为"Iono-Free LC"，则使用附录E.5 (7)中定义的无电离层影响的LC（线性组合）伪距。在其他情况下，仅使用$L_1$伪距。

![ Satellite Geometry for Single Point Positioning](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250220-204634.jpg)
<p style="text-align: center;">图E.6-1 单点定位的卫星几何结构</p> 

单点定位的测量方程及其偏导数矩阵构成如下：

$\begin{equation}
\delta\mathbf{O} = 
\begin{pmatrix} 
\rho_r^1 + cdt_r - cdT^1 + I_r^1 + T_r^1 \\ 
\rho_r^2 + cdt_r - cdT^2 + I_r^2 + T_r^2 \\ 
\rho_r^3 + cdt_r - cdT^3 + I_r^3 + T_r^3 \\ 
\vdots \\
\rho_r^m + cdt_r - cdT^m + I_r^m + T_r^m 
\end{pmatrix} H = 
\begin{pmatrix} 
-e_r^{1^T} & 1 \\ 
-e_r^{2^T} & 1 \\ 
-e_r^{3^T} & 1 \\ 
\vdots & \vdots \\ 
-e_r^{m^T} & 1
\end{pmatrix} \tag{E.4.45}
\end{equation}$

其中几何距离$\rho_r^s$和视线（LOS）向量$e_r^s$由E.3 (4)和E.3 (5)给出，结合卫星和接收机的位置。卫星位置$r^s$和时钟偏差$dT^s$也根据处理选项“Satellite Ephemeris/Clock”从E.4中描述的GNSS卫星星历和时钟导出。

为了求解测量方程以获得最终估计的接收机位置和接收机时钟偏差，RTKLIB采用迭代加权最小二乘法（LSE），如下所示：

$\begin{equation}
\hat{x}_{i+1} = \hat{x}_i + (H^T W H)^{-1} H^T W (y - h(\hat{x}_i)) \tag{E.6.22}
\end{equation}$

对于迭代加权最小二乘法的初始参数向量$x_0$，在单点定位的第一个历元中仅使用全0。一旦获得解，该位置将用作下一个历元的初始接收机位置。对于权重矩阵$W$，RTKLIB使用以下公式：

$\begin{equation}
W = diag(\sigma_1^{-2}, \sigma_2^{-2}, \ldots, \sigma_m^{-2}) \tag{E.6.23}
\end{equation}$

$\begin{equation}
\sigma^2 = F^s R_r \left(a_\sigma^2 + b_\sigma^2 / \sin El_r^s \right)^2 + \sigma_{eph}^2 + \sigma_{ion}^2 + \sigma_{trop}^2 + \sigma_{bias}^2 \tag{E.6.24}
\end{equation}$

其中：

$F^s$：卫星系统误差因子<br>
（1: GPS, Galileo, QZSS和Beidou, 1.5: GLONASS, 3.0: SBAS）<br>
$R_r$：码/载波相位误差比<br>
$a_\sigma, b_\sigma$：载波相位误差因子$a$和$b$（米）<br>
$\sigma_{eph}$：星历和时钟误差的标准差（米）<br>
$\sigma_{ion}$：电离层校正模型误差的标准差（米）<br>
$\sigma_{trop}$：对流层校正模型误差的标准差（米）<br>
$\sigma_{bias}$：码偏差误差的标准差（米）

对于星历和时钟误差的标准差，RTKLIB中使用了URA（用户范围精度）或类似的指标。通过几次迭代，通常情况下解会收敛，并获得估计的接收机位置$\hat{r}_r$和接收机时钟偏差$\hat{d}t_r$。

$\begin{equation}
\hat{x} = \lim_{i \to \infty} \hat{x}_i = (\hat{r}_r^T, cdt_r)^T \tag{E.6.25}
\end{equation}$

估计的接收机时钟偏差$\hat{d}t_r$没有明确输出到解文件中。相反，它被合并在解的时间标签中。这意味着解的时间标签指示的不是接收机的时间标签，而是GPST中测量到的真实信号接收时间。

**4. 接收机速度和时钟漂移的估计**

如果给出了GNSS信号的多普勒频率测量值，可以按照以下过程估计接收机速度和时钟漂移。对于一个历元时间，速度估计的未知参数向量$x$定义为：

$\begin{equation}
x = (v_r^T, cdt_r)^T \tag{E.6.26}
\end{equation}$

其中$v_r$和$dt_r$分别是接收机速度在ECEF（地心地固坐标系）中的速度（米/秒）和接收机时钟漂移（秒/秒）。相对应的速率测量向量$y$可以表示为：

$\begin{equation}
y = (-\lambda_i D_{r,i}^1, -\lambda_i D_{r,i}^2, -\lambda_i D_{r,i}^3, \ldots, -\lambda_i D_{r,i}^m)^T \tag{E.6.27}
\end{equation}$

其中$D_{r,i}^s$是卫星$s$的$L_i$多普勒频率测量值。RTKLIB总是使用$L_1$多普勒频率测量值。这些测量方程及其偏导数矩阵构成如下：

$\begin{equation}
h(x) = \begin{pmatrix}
r_r^1 + cd\dot{t}_r - cd\dot{T}^1 \\
r_r^2 + cd\dot{t}_r - cd\dot{T}^2 \\
r_r^3 + cd\dot{t}_r - cd\dot{T}^3 \\
\vdots \\
r_r^m + cd\dot{t}_r - cd\dot{T}^m
\end{pmatrix} \quad
H = \begin{pmatrix}
-e_r^{1^T} & 1 \\
-e_r^{2^T} & 1 \\
-e_r^{3^T} & 1 \\
\vdots & \vdots \\
-e_r^{m^T} & 1
\end{pmatrix} \tag{E.6.28}
\end{equation}$

这些方程中接收机和卫星之间的距离速率$r_r^s$从以下公式推导：

$\begin{equation}
r_r^s = e_r^{s^T} (v^s(t^s) - v_r) + \frac{\omega_e}{c} (v_y^s x_r + y^s v_{x,r} - v_x^s y_r - x^sv_{y,r}) \tag{E.6.29}
\end{equation}$

其中$v^s = (v_x^s, v_y^s, v_z^s)^T$和$v_r = (v_{x,r}, v_{y,r}, v_{z,r})^T$。通过使用类似于接收机位置估计的迭代最小二乘法，我们可以获得接收机速度和时钟漂移：

$\begin{equation}
\hat{x} = \lim_{i \to \infty} \hat{x}_i = (\dot{v}_r^T, c\hat{d}\dot{t}_r)^T \tag{E.6.30}
\end{equation}$

其中权重矩阵$W$设置为$I$（非加权最小二乘法）。

**5. 解的验证和RAIM FDE**

在(3)中描述的估计接收机位置可能由于未建模的测量误差而包含无效解。为了测试是否为有效解并拒绝无效解，RTKLIB在获得接收机位置估计后应用以下验证。如果验证失败，将发出警告信息拒绝该解。

$\begin{equation}
v_s = \frac{(p_i^s - (\hat{\rho}_i^s + c\hat{d}t_r - cdT^s + I_r^s + T_r^s)}{\sigma_s} \tag{E.6.31}
\end{equation}$

$\begin{equation}
v = (v_1, v_2, v_3, \ldots, v_m)^T \tag{E.6.32}
\end{equation}$

$\begin{equation}
\frac{v^T v}{m-n-1} < \chi^2_\alpha(m - n - 1) \tag{E.6.33}
\end{equation}$

$\begin{equation}
GDOP < GDOP_{thres} \tag{E.6.34}
\end{equation}$

其中$n$是估计参数的数量，$m$是测量的数量。$\chi_a^2(n)$ 是自由度n和α=0.001（0.1%）的卡方分布。GDOP是几何精度因子（dilution of precision）。$GDOP_{thres}$
​
可以设置为处理选项“Reject Threshold of GDOP”。

除了上述解决方案验证之外，RTKLIB在版本2.4.4.2中增加了RAIM（接收机自主完整性监控）FDE（故障检测和排除）功能。如果启用了处理选项“RAIM FDE”并且(E.6.333)中的卡方检验失败，RTKLIB将通过逐个排除可见卫星来重试估计。在所有重试后，选择具有最小归一化平方残差$v^Tv$的估计接收机位置作为最终解。在这种方案中，由于卫星故障、接收机故障或大的多路径导致的无效测量值将被排除为异常值。请注意，此功能在两个或更多无效测量值时无效。它还需要两个冗余的可见卫星，这意味着至少需要6个可见卫星才能获得最终解。

## E.7 Kinematic, Static and Moving-Baseline

RTKLIB采用扩展卡尔曼滤波器（EKF）以获得DGPS/DGNSS、Static、Kinematic、Moving-base模式下的最终解，并结合附录E.3中的GNSS信号测量模型以及附录E.5中的对流层和电离层模型。

### 7.1 EKF公式

通过使用扩展卡尔曼滤波滤波器（EKF），可以在历元时间$t_k$通过测量向量$y_k$估计未知模型参数的状态向量$x$及其协方差矩阵$P$：

$\begin{equation}
\hat{x}_k(+) = \hat{x}_k(-) + K_k(y_k - h(\hat{x}_k(-))) \tag{E.7.1}
\end{equation}$

$\begin{equation}
P_k(+) = (I - K_k H(\hat{x}_k(-)))P_k(-) \tag{E.7.2}
\end{equation}$

$\begin{equation}
K_k = P_k(-)(H(\hat{x}_k(-))P_k(-)H(\hat{x}_k(-))^T + R_k)^{-1} \tag{E.7.3}
\end{equation}$

其中$\hat{x}_k$和$P_k$是历元时间的$t_k$估计状态向量和其协方差矩阵。−和+表示EKF测量更新之前和之后。$h(x)$，$H(x)$和$R_k$分别是测量模型向量、偏导数矩阵和测量误差的协方差矩阵。假设系统模型是线性的，EKF的状态向量和其协方差矩阵的时间更新表示为：

$\begin{equation}
\hat{x}_{k+1}(-) = F_k^{k+1} \hat{x}_k(+) \tag{E.7.4}
\end{equation}$

$\begin{equation}
P_{k+1}(-) = F_k^{k+1} P_k(+) F_k^{k+1^T} + Q_k^{k+1} \tag{E.7.5}
\end{equation}$

其中$F_k^{k+1}$和$Q_k^{k+1}$是从历元时间$t_k$到$t_{k+1}$的系统噪声的转移矩阵和协方差矩阵。

### 7.2 双差观测模型

对于短基线（<10公里）的载波相对定位，通常使用以下双差（DD）测量方程来处理$L_i$相位差和伪距。在这些方程中，通过使用双差技术，卫星和接收机的时钟偏差、电离层和对流层影响以及其他次要修正项几乎被消除。

$\begin{equation}
\Phi_{rb,i}^{jk} = \rho_{rb}^{jk} + \lambda_i (B_{rb,i}^j - B_{rb,i}^k) + d\Phi_{r,i}^s + \varepsilon_\Phi \tag{E.7.6}
\end{equation}$

$\begin{equation}
P_{rb,i}^{jk} = \rho_{rb}^{jk} + \varepsilon_P \tag{E.7.6}
\end{equation}$

![DD (double-difference) Formulation](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250220-223859.jpg)
<p style="text-align: center;">图E.7-1 双差模型</p> 

其中$d\Phi_{r,i}^s$是载波相位修正项，在短基线情况下可以忽略，除非接收机PCV项因使用不同天线而需要考虑。为了在方程中获得几何距离$\rho_r^s$，基站位置$r_b$固定为预定值，除非在移动基线情况下。

请注意，接收机之间的单差（SD）最好在具有相同历元时间的测量之间进行。然而，由于不同的接收机时钟偏差，接收机并不能完全同步。在一些典型情况下，移动站的采样间隔与基站不同，例如10 Hz和1 Hz。为了控制单差，RTKLIB采用了一个简单的标准来选择测量对。RTKLIB简单地选择在移动站测量的历元时间之前或等于该历元时间的最后一个测量。移动站和基站之间的历元时间差有时被称为“Age of Differential”。随着时间差的增加，由于卫星时钟漂移和电离层延迟的变化，解的精度逐渐降低。为了仅补偿卫星时钟漂移，RTKLIB使用广播的卫星时钟参数对单差测量进行修正。最大“Age of Differential”设置为处理选项“MAX Age of Diff”。
至于卫星侧的单差生成，RTKLIB在每个历元的基础上选择一个具有最大仰角的参考卫星。请注意，不同导航系统之间的卫星不会生成卫星侧的单差，例如GPS和GLONASS之间。这是因为即使不同导航系统的信号具有相同的载波频率，接收机通常对这些信号有不同的群延迟。接收机中的群延迟差异被称为接收机ISB（inter system bias）。

假设移动站和基站均使用三频GPS/GNSS接收机，待估计的未知状态向量$x$可定义为：

$\begin{equation}
x = (r_r^T, v_r^T, B_1^T, B_2^T, B_5^T)^T \tag{E.7.7}
\end{equation}$

其中$B_i = (B_{rb,i}^1, B_{rb,i}^2, \ldots, B_{rb,i}^m)^T$是$L_i$单差（SD）载波相位偏差（周期）。在RTKLIB的实现中，它内部使用SD载波相位偏差而不是双差（DD），以避免繁琐的参考卫星切换处理。SD偏差还有助于解决GLONASS FDMA信号中的整数模糊度。

测量向量$y$也定义为包含双差相位差和伪距测量值：

$\begin{equation}
y = (\Phi_1^T, \Phi_2^T, \Phi_5^T, P_1^T, P_2^T, P_5^T)^T \tag{E.7.8}
\end{equation}$

其中：

$\Phi_i = (\phi_{rb,i}^{12}, \phi_{rb,i}^{13}, \phi_{rb,i}^{14}, \ldots, \phi_{rb,i}^{1m})^T$

$P_i = (p_{rb,i}^{12}, p_{rb,i}^{13}, p_{rb,i}^{14}, \ldots, p_{rb,i}^{1m})^T$

### 7.3 EKF量测更新

通过使用方程(E.7.6)，测量模型向量$h(x)$、偏导数矩阵$H(x)$和测量误差的协方差矩阵$R$可以表示为：

$\begin{equation}
h(x) = (h_{\Phi,1}^T, h_{\Phi,2}^T, h_{\Phi,5}^T, h_{p,1}^T, h_{p,2}^T, h_{p,5}^T)^T \tag{E.7.9}
\end{equation}$

$\begin{equation}
H(x) = \left.\frac{\partial h(x)}{\partial x}\right|_{x=\hat{x}} = 
\begin{pmatrix}
-DE & 0 & \lambda_1 D & 0 & 0 \\
-DE & 0 & 0 & \lambda_2 D & 0 \\
-DE & 0 & 0 & 0 & \lambda_5 D \\
-DE & 0 & 0 & 0 & 0 \\
-DE & 0 & 0 & 0 & 0
\end{pmatrix} \tag{E.7.10}
\end{equation}$

$\begin{equation}
\small
R = 
\begin{pmatrix}
DR_{\Phi,1}D^T & & & & \\
& DR_{\Phi,2}D^T & & & \\
& & DR_{\Phi,5}D^T & & \\
& & & DR_{p,1}D^T & \\
& & & & DR_{p,2}D^T & \\
& & & & & DR_{p,5}D^T
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
\end{pmatrix} : \text{SD (single-differencing) matrix}$

$R_{\Phi,i} = \text{diag}(2\sigma_{\Phi,i}^{1^2}, 2\sigma_{\Phi,i}^{2^2}, \ldots, 2\sigma_{\Phi,i}^{m^2})$

$R_{P,i} = \text{diag}(2\sigma_{P,i}^{1^2}, 2\sigma_{P,i}^{2^2}, \ldots, 2\sigma_{P,i}^{m^2})$

$\sigma_{\Phi,i}^S$：$L_i$相位差测量误差的标准差（米）

$\sigma_{P,i}^S$：$L_i$伪距测量误差的标准差（米）

通过求解EKF公式(E.7.1)，可以获得历元时间$t_k$处估计的移动站天线位置、速度和浮点单差载波相位偏差。

### 7.4 EKF时间更新

在RTKLIB中，对于具有接收机动态的运动定位模式（Positioning Mode = Kinematic 并且 REC Dynamics = ON），EKF的时间更新（E.7.2）表示为：

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

并且$\tau_r = t_{k+1} - t_k$是GPS/GNSS接收机采样间隔（s），$(\sigma_{ve}, \sigma_{vn}, \sigma_{vu})$是移动站速度系统噪声的东西、北、上分量的标准差（m/s/√s）。

对于纯运动学模式且不考虑接收机动态（Positioning Mode = Kinematic, REC Dynamics = OFF），方程(E.7.9)被替换为：

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

为了避免因将无限大的过程噪声添加到接收机位置的方差而导致数值不稳定，接收机位置状态在每个历元被重置为初始猜测值，并且在RTKLIB中向方差中添加了足够大的过程噪声（$10^4 m^2$）。初始位置是从单点定位过程导出的，该过程用于避免非线性信号测量模型的迭代。

在静态模式（Positioning Mode = Static）中，方程(E.7.10)被简单地替换为：

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

在瞬时模糊度解算模式（整数模糊度解算=瞬时）中，SD载波相位偏差$B_i$的时间更新处理方式与上述略有不同。在这种模式下，载波相位偏差状态的值不会通过EKF时间更新传递到下一个历元。偏差在每个历元被重置为初始猜测值，并且在方差中添加了足够大的过程噪声（$10^4 m^2$）。如果在测量数据中检测到周跳，相应的SD载波相位偏差状态也会被重置为初始值。RTKLIB通过输入测量数据中的LLI（失锁指示）和双频测量的无几何LC（线性组合）相位跳变来检测周跳。周跳阈值可以通过处理选项“Slip Thres”来更改。

### 7.5 整周模糊度解算

一旦在EKF测量更新中获得估计状态，浮点载波相位模糊度可以被解算为整数值，以提高精度和收敛时间（Integer Ambiguity Res = Continuous, Instantaneous or Fix and Hold）。首先，估计状态及其协方差矩阵通过以下方式转换为双差（DD）形式：

$\begin{equation}
\tilde{x}_k' = G \tilde{x}_k(+) = (\tilde{r}_r^T, \tilde{v}_r^T, \tilde{N}^T)^T \tag{E.7.15}
\end{equation}$

$\begin{equation}
P_k' = G P_k(+) G^T = 
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
\end{pmatrix} : \text{SD to DD transformation matrix}$

在这个转换过程中，SD 载波相位偏差被转换为 DD 载波相位形式，以消除接收机初始相位项，从而获得整数模糊度 $\tilde{N}$ 及其协方差 $Q_N$。在这些公式中，通过求解一个整数最小二乘（ILS）问题，可以得到最合适的整数模糊度向量 $\tilde{N}$，该问题表示为：

$\begin{equation}
\tilde{N} = \arg\min_{N \in \mathbb{Z}} (N - \hat{N})^T Q_N^{-1} (N - \hat{N}) \tag{E.7.17}
\end{equation}$

其中，$\mathbb{Z}$ 表示整数集。

为了解决ILS问题，RTKLIB采用了一种著名的高效搜索策略LAMBDA [66]及其扩展MLAMBDA [67]。LAMBDA和MLAMBDA提供了线性变换和变换空间中熟练的树搜索过程的组合，以缩小整数向量搜索空间。这些过程得到的整数向量解通过以下简单的“Ratio-Test”进行验证。在“Ratio-Test”中，比率因子 $R$ 定义为次优解 $N_2$ 的加权残差平方和最优 $N$ 的加权残差平方和的比值，用于检查解的可靠性。验证阈值 $R_{thres}$ 可以通过处理选项“Min Ratio to Fix Ambiguity”设置。当前版本的RTKLIB仅支持固定的阈值。

$\begin{equation}
R = \frac{(\tilde{N}_2 - \hat{N})^T Q_N^{-1} (\tilde{N}_2 - \hat{N})}{(\tilde{N} - \hat{N})^T Q_N^{-1} (\tilde{N} - \hat{N})} > R_{thres} \tag{E.7.18}
\end{equation}$

验证之后，Rover天线位置和速度的“FIXED”解 $\hat{r}_r$ 和 $\hat{v}_r$ 通过求解以下方程获得。如果验证失败，RTKLIB 则输出“FLOAT”解 $\hat{r}_r$ 和 $\hat{v}_r$ 代替。

$\begin{equation}
\left( \begin{array}{c} \tilde{r}_x \\ \tilde{r}_y \end{array} \right) = \left( \begin{array}{c} \hat{r}_x \\ \hat{r}_y \end{array} \right) - Q_{RN} Q_N^{-1} (\hat{N} - \tilde{N}) \tag{E.7.19}
\end{equation}$

如果处理选项设置为“Fix and Hold”模式（整周模糊度解析 = Fix and Hold），并且固定解通过之前的测试被正确验证，则双差载波相位偏差参数会被严格约束到固定的整数值。为此，RTKLIB 会输入以下“伪”测量值到 EKF，并通过公式 (E.7.1) 更新 EKF：

$\begin{equation}
y = \tilde{N} \tag{E.7.20}
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
\end{pmatrix} : \text{SD to DD transformation matrix}$

$\sigma_c$ : 约束到固定整数模糊度的误差（≈ 0.001 cycle）

“Fix and Hold”模式最初在 RTKLIB 版本 2.4.0 中引入，目的是在运动接收机的运动模式下特别提高固定解的比率。

### 7.6 长基线双差测量模型

对于接收机r和基站b之间的长基线处理，可以形成类似于短基线双差（DD）模型的以下双差测量方程：

$\begin{align}
& \phi_{rb,i}^{jk} = \rho_{rb}^{jk} - I_{rb,k}^{jk} T_{rb}^{jk} + \lambda_i (B_{rb,i}^j - B_{rb,i}^k) d\Phi_{r,i}^s + \varepsilon_\phi \\
& P_{rb,i}^{jk} = \rho_{rb}^{jk} + I_{rb,i}^{jk} T_{rb}^{jk} + \varepsilon_P \tag{E.7.24}
\end{align}$

其中$I_{r,i}^s$和$T_r^s$作为$L_i$电离层延迟（米）和对流层延迟（米）被添加到短基线双差模型中。对于超过100公里的基线，应使用精确的星历书来减轻广播星历误差。在载波相位修正项$\Phi_{r,i}^s$中，对于超过500公里的基线应考虑地球潮效应。为了消除电离层项，有时会形成无电离层LC（线性组合）。然而，RTKLIB不使用这种显式LC，而是通过EKF直接估计基线处理的双频或三频测量的电离层项。

长基线情况下的未知状态向量 $x$ 也可以设定为：

$\begin{flalign}
\small
x = \left(r_r{}^T, v_r{}^T, Z_r, G_{N, r}, G_{E, r}, Z_b, G_{N, b}, G_{E, b}, I^T, B_1{}^T, B_2{}^T, B_5{}^T\right)^T \tag{E.7.25}
\end{flalign}$

其中 $Z_r$ 和 $Z_b$ 是 ZTD（zenith total delay）在Rover和基站位置的值，$G_{N, r}$、$G_{E, r}$、$G_{N, b}$ 和 $G_{E, b}$ 是对流层梯度的北向和东向分量。$I = (I_{rb}^{1}, I_{rb}^{2}, \ldots, I_{rb}^{m})^{T}$ 是 $L_1$ 频率（$m$）下的SD垂直电离层延迟。

测量模型向量 $h(x)$ 和偏导数矩阵 $H(x)$ 可以表示为：

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

其中，$Q_T$和$Q_I$分别为电离层和对流层项的过程噪声协方差矩阵。在该方程中，Rover和基站的ZTD（天顶总延迟）及梯度参数，以及每颗卫星的SD（单差）垂直电离层延迟，均被简单地建模为随机游走过程。此外，为了估计电离层和对流层项，在2.4.1版本中为长基线处理增加了一个“部分固定”功能。这意味着只有部分模糊度被解算为整数值，其余未固定的模糊度仍保持为浮点值。为了确定模糊度是否固定，RTKLIB中实现了一个使用卫星仰角的简单标准。如果卫星的仰角低于设定的阈值，则该卫星的模糊度不会被固定。只有仰角高于阈值的卫星的模糊度才会被解算为整数。模糊度解算的仰角阈值可以通过处理选项“最小仰角固定模糊度”（Min Elevation to Fix Amb）以及“最小仰角保持模糊度”（Min Elevation to Hold Amb）来设置，以控制“固定和保持”（Fix and Hold）功能。

### 7.7 动态基线模型

移动基线模式通常在Rover和基站接收器都在移动且仅需要Rover相对于基站的相对位置时使用。通过在移动平台上安装两个天线，可以利用移动基线模式确定精确姿态。在 RTKLIB 中，如果将处理选项“定位模式”设置为“移动基线”，则应用移动基线模式。

在移动基线模式下，基站位置不是固定的，而是通过逐历元的单点定位过程估计的。一旦获得基站位置，将基站位置固定为估计位置，并通过 (1)-(5) 中描述的短基线运动模式估计Rover位置。在这种情况下，只有相对位置是有意义的，也就是说，Rover和基站的绝对位置解的精度仅与点定位模式的解相同。

除了对移动基线模式的简单实现外，RTKLIB 还校正了Rover与基站之间的时间差异。Rover接收机和基站接收机不同步。接收机时钟差通常最大可达 2 毫秒。对于非常快速移动的平台，不同步的时钟会导致精度下降。为了校正时钟差，在基线处理之前，基站位置$r_b$通过以下公式进行校正：

$\begin{equation}
\boldsymbol{r}_b(t_r) = \boldsymbol{r}_b(t_b) + \boldsymbol{v}_b(t_b)(t_r - t_b) \tag{E.7.32}
\end{equation}$

其中，$t_r$和 $t_b$分别是通过单点定位过程估计的Rover和基站的信号接收时间。$v_b{t_b}$也是通过多普勒测量估计的基站速度。对于通过移动基线模式进行姿态确定的情况，如果启用处理选项“基线长度约束”（Baseline Length Constraint），则可以应用基线长度约束。该约束在 EKF 测量更新中应用以下伪测量：

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

其中，$r_baseline$ 是给定的预设基线长度（单位：米），$\sigma_r$是基线长度的约束（单位：米）。为了应对非常短的基线长度情况下的非线性问题，可以通过将处理选项“滤波器迭代次数”（Number of Filter Iteration）设置为大于1，来支持扩展卡尔曼滤波器（EKF）的迭代测量更新。

## E.8 PPP

TBD
