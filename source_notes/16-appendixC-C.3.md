---
sidebarDepth: 1
---

# 附录C: 配置参数解析
 
## C.3 Statistics

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250710-134735.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.3-1 观测误差模型与过程模型参数</p>

### C.3.1 观测模型原理

RTKLIB 为了统一数学与程序描述，观测误差模型是以载波相位为基础的，而后通过 $R_r$ 误差转换因子，将其转换为伪距误差数值水平。其中唯一的多普勒参数除外（Doppler Freq Error (Hz)）。

单点定位和相对定位定义中的模型基本相同，只存在稍微的差别，可以从 `varerr` 函数的定义中看到。

最后再次说明，本章节研究的内容是基于 RTKLIB demo5 版本。

#### C.3.1.1 单点定位中的模型

<!-- 放缩后水平：σ -->
$\begin{flalign}
\sigma^2 &= {(F^s)}^2 \cdot R_r^2 \cdot (\sigma_{el}^2 + \sigma_{snr}^2 + \sigma_{rcv}^2)
\end{flalign}$

<!-- 仰角模型 -->
$\begin{flalign}
\sigma_{el}^2 =  
e_{phase}^2 + \frac{e_{el}^2}{\sin^2(El_r^s)}
\end{flalign}$（高度角误差模型）

<!-- SNR 模型 -->
$\begin{flalign}
\sigma_{snr}^2 =  
e_{snr}^2 \cdot 10^{(snr_{max} - snr_{rover}) / 10}
\end{flalign}$（SNR 误差模型）

<!-- 接收机误差模型（仅适用于u-blox） -->
$\begin{flalign}
\sigma_{rcv}^2 &=  
e_{rcv}^2 \cdot \left(0.01 \cdot 2^{rcvstd_{P} + 5)} \right)^2
\end{flalign}$ （接收机误差模型，仅适用于u-blox）

**其中：**

<!-- 卫星系统误差因子：F -->
- $F^s$：卫星系统误差因子 fact
<!-- 伪距放缩因子：R -->
- $R_r$：伪距误差比例（相较于载波相位）
<!-- 仰角模型 -->
- $e_{phase}, e_{el}$：高度角误差模型误差因子（m）
<!-- SNR 模型 -->
- $e_{snr}$：信噪比误差模型误差因子（dB-Hz）
<!-- 接收机误差模型（仅适用于u-blox） -->
- $e_{rcv}$：接收机定义误差因子（仅适用于u-blox）
<!-- 高度角 el -->
- $El_r^s$：高度角（rad），若 $El_r^s < elmask$ ，则设为 $elmask$
<!-- 流动站 snr -->
- $\text{snr}_{\text{rover}}$：流动站信噪比（dB-Hz）
<!-- 流动站 snr -->
- $\text{snr}_{\text{max}}$：最大信噪比（dB-Hz）
<!--  -->
- $rcvstd_{P}$：接收机定义伪距误差标准差（仅适用于u-blox）

**注意：**

- **无电离层组合（IONOOPT_IFLC）**：$\sigma^2 = \sigma^2 \cdot 3^2$（该部分参考附录B.4）
- **其他误差**：`varerr` 函数外部还会加上大气层等误差（参考 [RTKLIB-Manual-CN E.6.3节](/algorithm/RTKLIB-Manual-CN/09-appendixE-E.6.html#e-6-3-接收机位置和钟差估计)）
- **$R_r$ 的设置**：低成本接收机设置较大的值效果会更好（如 300），而对于更昂贵的接收器，默认值则设定为 100，因为它们的伪距观测噪声较小。较大的值往往会使卡尔曼滤波器更快收敛，也会加速首次固定，但同时也增加了假固定的可能性。如果增加该值，建议将pos2-arthres1设置得足够低，以防止滤波器尚未收敛时就进行固定。

#### C.3.1.2 相对定位中的模型（单差）

$\begin{flalign}
\sigma^2 &= 
2.0 \cdot \left( {(F^s)}^2 \cdot \sigma_{el}^2 + \sigma_{bl}^2 \right) + 
\sigma_{clk}^2 + {(F^s)}^2 \cdot \sigma_{snr}^2 + \sigma_{rcv}^2
\end{flalign}$

<!-- 高度角误差模型 -->
$\begin{flalign}
\sigma_{el}^2 =  
a_\sigma^2 + \frac{b_\sigma^2}{\sin^2(El_r^s)}
\end{flalign}$（高度角误差模型）

<!-- 基线误差模型 -->
$\begin{flalign}
\sigma_{bl}^2 =  
{(e_{bl} \cdot \frac{\text{bl}}{10^4})} ^ 2
\end{flalign}$（基线误差模型）

<!-- 钟误差模型 -->
$\begin{flalign}
\sigma_{clk}^2 = \left( c_{\text{light}} \cdot e_{clk} \cdot \text{dt} \right)^2
\end{flalign}$（钟误差模型）

<!-- snr 误差模型 -->
$\begin{flalign}
\sigma_{snr}^2 =  
e_{snr}^2 \cdot \left( 10^{(\text{snr}_{\text{max}} - \text{snr}_{\text{rover}}) / 10} + 10^{(\text{snr}_{\text{max}} - \text{snr}_{\text{base}}) / 10} \right)
\end{flalign}$（SNR 误差模型）

<!-- 接收机误差模型（仅适用于u-blox） -->
$\begin{flalign}
\sigma_{rcv}^2 = 
\begin{cases}
e_{rcv}^2 \cdot (0.01 \cdot 2^{(rcvstd_{P} + 5)})^2, & \text{code} \\
e_{rcv}^2 \cdot (rcvstd_{L} \cdot 0.004 \cdot 0.2)^2, & \text{phase}
\end{cases}
\end{flalign}$（接收机误差模型，仅适用于u-blox）

**其中：**

<!-- 卫星系统误差因子：F -->
- $F^s$：卫星系统误差因子 fact
<!-- 仰角模型 -->
- $e_{phase}, e_{el}$：高度角误差模型误差因子（m）
<!-- 基线误差模型 -->
- $e_{bl}$：基线误差模型误差因子（m）
<!-- 钟误差模型 -->
- $e_{clk}$：钟误差模型误差因子（m/s）
<!-- SNR 模型 -->
- $e_{snr}$：信噪比误差模型误差因子（dB-Hz）
<!-- 接收机误差模型（仅适用于u-blox） -->
- $e_{rcv}$：接收机定义误差因子（仅适用于u-blox）
<!-- 高度角 el -->
- $El_r^s$：高度角（rad），若 $El_r^s < elmask$ ，则设为 $elmask$
<!-- 基线长度 bl -->
- $\text{bl}$：基线长度（米）
<!--  -->
- $\text{dt}$：时间差（s）
<!--  -->
- $\text{sclkstab}$：时钟稳定性（s/s）
<!--  -->
- $c_{\text{light}}$：光速（m/s）
<!-- 流动站 snr -->
- $\text{snr}_{\text{rover}}$：流动站信噪比（dB-Hz）
<!-- 基准值 snr -->
- $\text{snr}_{\text{base}}$：基准值信噪比（dB-Hz）
<!-- 流动站最大 snr -->
- $\text{snr}_{\text{max}}$：最大信噪比（dB-Hz）
<!-- 接收机定义伪距误差标准差 -->
- $rcvstd_{P}$：接收机定义误差标准差（仅适用于u-blox）
<!-- 接收机定义载波相位误差标准差 -->
- $rcvstd_{L}$：接收机定义误差标准差（仅适用于u-blox）

**注意：**

- **单差问题**：公式开始的因子 2.0 的存在是由于 RTK 定位中单差观测的特性，假设两者流动站和基站观测误差方差相等（$\sigma^2$），那么单差误差为 $\text{var}_{sd} = \sigma_{\text{rover}}^2 + \sigma_{\text{base}}^2 = 2 \cdot \sigma^2$；
- **无电离层组合（IONOOPT_IFLC）**：$\sigma^2 = \sigma^2 \cdot 3^2$（该部分参考附录B.4）
- **其他误差**：`varerr` 函数外部还会加上大气层等误差（参考 [RTKLIB-Manual-CN E.6.3节](/algorithm/RTKLIB-Manual-CN/09-appendixE-E.6.html#e-6-3-接收机位置和钟差估计)）

### C.3.2 观测模型源码解析

<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">表C.3-1 观测误差模型参数对应关系</p>

| 符号 | GUI 参数名 | 配置文件参数名 | 程序结构体 | 含义 | 默认值 |
| --- | --- | --- | --- | --- | --- |
| $R_r$ | Code/Carrier-Phase Error Ratio L1/L2/L5 | stats-eratio1<br>stats-eratio2<br>stats-eratio5 | opt.eratio[0]<br>opt.eratio[1]<br>opt.eratio[2] | 伪距误差比例 | 300.0 300.0 300.0 |
| $e_{phase}, e_{el}$ | Carrier-Phase Error: a+b/sinEl (m) | stats-errphase<br>stats-errphaseel | opt.err[1]<br>opt.err[2] | 高度角误差模型误差因子 | 0.003 0.003 |
| $e_{snr}, \text{snr}_{\text{max}}$ | Carrier Phase Error: SNR / SNR maxDb | stats-errsnr<br>stats-snrmax | opt.err[6]<br>opt.err[5] | SNR 加权模型 | 0.003 52.000 |
| $e_{rcv}$ | Carrier Phase Error: Rcv Errs | stats-errrcv | opt.err[7] | 接收机误差因子（仅适用于u-blox） | 0.000 |
| $e_{clk}$ | Satellite Clock Stability | stats-clkstab | opt.sclkstab | 卫星钟稳定性 | 5.00E-12 |
| - | Doppler Freq Error (Hz) | stats-errdoppler | opt.err[4] | 多普勒观测误差 | 1.000 |

上述参数中除了最后一项是描述多普勒的，其余均用于描述载波相位和伪距观测误差，相关函数如下：

**1. varerr (pntpos)**

```c
/* varerr(pntpos.c): pseudorange measurement error variance */
if (el<MIN_EL) el=MIN_EL;
varr=SQR(opt->err[1])+SQR(opt->err[2])/sin(el);
if (opt->err[6]>0.0) {  /* if snr term not zero */
    snr_rover=(ssat)?SNR_UNIT*ssat->snr_rover[0]:opt->err[5];
    varr+=SQR(opt->err[6])*pow(10,0.1*MAX(opt->err[5]-snr_rover,0));
}
varr*=SQR(opt->eratio[0]);
if (opt->err[7]>0.0) {
    varr+=SQR(opt->err[7]*0.01*(1<<(obs->Pstd[0]+5)));  /* 0.01*2^(n+5) m */
}
if (opt->ionoopt==IONOOPT_IFLC) varr*=SQR(3.0); /* iono-free */
return SQR(fact)*varr;
```

`sin(el)` 需要带上平方，原本手册与代码描述有误，不过相对定位中是正确的，即 rtkpos.c 中的 `varerr`；

**2. varerr (rtkpos.c)**

```c
/* varerr(rtkpos.c): single-differenced measurement error variance */
/* adjust variance for config parameters */
a=fact*opt->err[1];  /* base term */
b=fact*opt->err[2];  /* el term */
c=opt->err[3]*bl/1E4; /* baseline term */
d=CLIGHT*opt->sclkstab*dt; /* clock term */
/* calculate variance */
var=2.0*(a*a+b*b/sinel/sinel+c*c)+d*d;
if (opt->err[6]>0) {  /* add SNR term */
    e=fact*opt->err[6];
    var+=e*e*(pow(10,0.1*MAX(snr_max-snr_rover,0))+
              pow(10,0.1*MAX(snr_max-snr_base, 0)));
}
if (opt->err[7]>0.0) {   /* add rcvr stdevs term */
    if (code) var+=SQR(opt->err[7]*0.01*(1<<(obs->Pstd[frq]+5))); /* 0.01*2^(n+5) */
    else var+=SQR(opt->err[7]*obs->Lstd[frq]*0.004*0.2); /* 0.004 cycles -> m) */
}

var*=(opt->ionoopt==IONOOPT_IFLC)?SQR(3.0):1.0;
```

比起单点定位中的模型，相对定位中还有的内容：

- 包含基线项 `c=opt->err[3]*bl/1E4`；
- SNR 项还考虑了基站：`pow(10,0.1*MAX(snr_max-snr_base, 0))`；
- 方差乘以 `2.0`，反映单差观测（两站）。

**3. resdop(pntpos.c)**

```c
/* resdop(rtkpos.c) */
/* Std of range rate error (m/s) */
sig=(err<=0.0)?1.0:err*CLIGHT/freq;
```

`err` 即 `opt.err[4]`，用以表示多普勒观测误差，这里的作用较弱，基本与等权无异。笔者尝试过在单点定位中使用伪距误差模型（`varerr`）来表示多普勒误差，不过需要将 $R_r$ 参数设置得更小（如10~30），此时对测速的精度带来了提升。

### C.3.3 过程模型原理

**1. 状态递推**

$\hat{x}_k^{-}=f(\hat{x}_{k-1},u_{k-1},0)$

EKF 针对的是非线性高斯模型，因此 $f$ 和 $h$ 可以是非线性函数，不过在这里 RTKLIB 中的 $f$ 为如下的线性模型。

$\hat{x}_k^- = A_{k-1}   \hat{x}_{k-1}$

其中：

${x}=(r_r^T,v_r^T,a_r^T,B_1^T,B_2^T,B_5^T)^T$

$A_{k} =   \left[\begin{array}{1} I_{3\times3} & I_{3\times3} τ_r & \frac{1}{2}I_{3\times3}τ_r^2 & 0 \\  0 & I_{3\times3} & I_{3\times3} τ_r & 0 \\  0 & 0 & I_{3\times3} & 0 \\ 0 & 0 & 0 & I_{{(3m-3)}\times{(3m-3)}} \\  \end{array}\right]$

如果考虑了接收机动态（pos1-dynamics=on），那么还需要包含加速度（常加速度模型）。

$\hat{x}_k^- =   \left[\begin{array}{1} I_{3\times3} & I_{3\times3} τ_r & \frac{1}{2}I_{3\times3}τ_r^2 & 0 \\  0 & I_{3\times3} & I_{3\times3} τ_r & 0 \\  0 & 0 & I_{3\times3} & 0 \\ 0 & 0 & 0 & I_{{(3m-3)}\times{(3m-3)}} \\  \end{array}\right] \bullet  \left[\begin{array}{1}  r_{3}^T \\ v_{3}^T \\  a_{3}^T \\ B_{{3m-3}}^T  \\ \end{array}\right]  =  \left[\begin{array}{1}  r_{3}^T+v_{3}^Tτ_r + \frac{1}{2}a_3^T \tau_r^2 \\  v_{3}^T + a_3^T \tau_r \\ a_3^T\\  B_{{3m-3}}^T \\  \end{array}\right]$
 
上面的公式描述了 EKF 系统状态递推的过程，其中的 $\tau = t_k - t_{k-1}$，表示采样时间间隔。

简单起见，这里并没有在一些状态如 $r$ 、$v$ 和 $a$ 中添加下标 $k$ ，但它们实际上是随时间变化的。另外，单差的维度是3m-3，这里的3代表的是3频段，而m-1代表的是去除双差参考卫星以外的卫星数目，这也是为了方便和双差相关的矩阵进行联合运算。

状态更新过程从矩阵变换的角度来讲比较简单，不过实际源码中会有更多细节。

**2. 误差协方差矩阵递推**

$P_k^- = A_k P_{k-1} A_k^T + W_{k-1} Q_{k-1}W_{k-1}$

其中， $W$ 是 $f$ 对状态噪声 $w$ 偏导的雅可比矩阵，而由于 $f$ 为线性模型，因此状态噪声协方差矩阵不用进行线性变换，这里的递推公式可以表示为：

$P_k^- = A_k P_{k-1} A_k^T +  Q_{k-1}$

这里主要看其中的 $Q$ 矩阵。

$Q= \left[\begin{array}{1} 0_{6\times6} & 0 & 0 \\ 0 & Q_v & 0 \\ 0 & 0 & 0_{(3m-3)\times(3m-3)} \\ \end{array}\right]$

其中：

$Q_v = E_r^T \left[\begin{array}{1} \sigma_{ve}^2\tau_r & 0 & 0 \\ 0 & \sigma_{vn}^2\tau_r & 0 \\ 0 & 0 & \sigma_{vu}^2\tau_r \\ \end{array}\right] E_r$

RTKLIB手册中的 $Q_v$ 矩阵是从第4行4列开始的，但在RTKLIB源码中， $Q_v$ 矩阵式从7行7列开始的，我想这里可能是老版本的RTKLIB使用的是常速度模型，也因此少了3个维度，不过现在都是需要加上的。

$Q_v$ 中的 $\sigma_{ve}$ 、 $\sigma_{vn}$ 、 $\sigma_{vu}$ 为移动端加速度的系统噪声在东、北、天分量上的标准差，具体在 RTKNAVI 的配置选项Options/Statistics中的过程噪声项 $(\sigma/\sqrt{s})$ 中找到，其单位为 $(m/s^2/s)$，我想这里单位的意义可能是得到协方差之后是 $\sigma^2/s$ ，也即过程噪声或误差在时间尺度上的协方差变化。

$Q_v$ 中 $Er$ 矩阵表示的是 ECEF 坐标系到 ENU 坐标系的转换矩阵，那么 $E_r^T$ 则反之，表示从 ENU 坐标系到 ECEF 坐标系的转换矩阵。

对于这里的如 $Qv = E_r^T \Sigma E_r$ 的形态（ $\Sigma=E(ww^T)$ ），是由于：假定 ENU 坐标系下的测量误差向量为 $w$ ，其对应在 ECEF 坐标系上的测量误差向量为 $E_r^Tw$ ，则协方差矩阵为 :

$E((E_r^Tw)(E_r^Tw)^T)=E(E_r^Tww^TE_r)=E_r^TE(ww^T)E_r=E_r^T \Sigma E_r$

### C.3.4 过程模型源码解析

<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">表C.3-2 过程模型参数对应关系</p>

| 符号 | GUI 参数名 | 配置文件参数名 | 程序结构体 | 含义 | 默认值 |
| --- | --- | --- | --- | --- | --- |
| - | Receiver Accel Horiz/Vertical (m/s^2) | stats-prnaccelh<br>stats-prnaccelv | opt.prn[3]<br>opt.prn[4] | 接收机水平/垂直加速度 | 3.0 1.0 |
| - | Carrier-Phase Bias (cycle) | stats-prnbias | opt.prn[0] | 载波相位偏置 | 0.0001 |
| - | Vertical Ionospheric Delay (m/10km) | stats-prniono | opt.prn[1] | 电离层延迟 | 0.001 |
| - | Zenith Tropospheric Delay (m) | stats-prntrop | opt.prn[2] | 对流层延迟 | 0.0001 |

状态更新相关的函数是 `udstate`，而其中又分别包含了：
- `udpos`：更新位置、速度和加速度；
- `udbias`：更新载波相位偏置；
- `udion`：更新电离层延迟（电离层校正模型为 `IONOOPT_EST`，即 **Estimate TEC**）；
- `udtrop`：更新对流层延迟（对流层校正模型为 `TROPOPT_EST`，即 **Estimate ZTD**）；
- `udrcvbias`：更新接收机相位偏移（GLO AR模型为 `GLO_ARMODE_AUTOCAL`，即 **AutoCal**）；

#### C.3.4.1 udstate

`udstate` 是 RTKLIB 中卡尔曼滤波的时间更新函数，负责更新状态向量（`rtk->x`）和协方差矩阵（`rtk->P`）。它按以下顺序操作：

- 调用 `udpos` 更新位置、速度、加速度状态。
- 若启用电离层（`IONOOPT_EST`）或对流层（`TROPOPT_EST`）估计，调用 `udion` 和 `udtrop` 更新相关参数，基于基线长度和过程噪声。
- 若启用 GLONASS 自动校准（`GLO_ARMODE_AUTOCAL`），调用 `udrcvbias` 更新接收机硬件偏差。
- 若为 RTK 模式（> `PMODE_DGPS`），调用 `udbias` 更新单差相位偏移，包括周跳检测、状态重置和一致性校正。

该函数确保状态量在时间维度上正确演进，为后续量测更新（如 `ddres`）提供基础，重点可以关注 `udpos` 和 `udbias`。

::: details 点击查看代码
```c
/* Temporal update of states --------------------------------------------------*/
static void udstate(rtk_t *rtk, const obsd_t *obs, const int *sat,
                    const int *iu, const int *ir, int ns, const nav_t *nav)
{
    double tt=rtk->tt;

    /* Temporal update of position/velocity/acceleration */
    /* 更新位置、速度、加速度状态 */
    /* 调用 udpos 函数，根据运动模型（如静态、动态、运动学）更新状态向量中的位置（x,y,z）、速度（vx,vy,vz）和加速度（ax,ay,az） */
    /* 过程噪声根据 tt 和配置添加到协方差矩阵 */
    udpos(rtk,tt);

    /* Temporal update of ionospheric and tropospheric parameters */
    /* 如果启用了电离层或对流层估计（IONOOPT_EST 或 TROPOPT_EST 及以上），更新电离层和对流层参数 */
    if (rtk->opt.ionoopt==IONOOPT_EST || rtk->opt.tropopt>=TROPOPT_EST) {
        double dr[3], bl=baseline(rtk->x,rtk->rb,dr); /* 计算基线长度 bl（米），dr 为基线向量（流动站 - 基站） */
        
        /* 更新电离层参数 */
        /* 如果启用了电离层估计（IONOOPT_EST），调用 udion 更新每个卫星的电离层延迟状态 */
        /* 过程噪声基于 tt 和 opt->prn[3] 添加到协方差矩阵，考虑基线长度 bl 和共视卫星 sat, ns */
        if (rtk->opt.ionoopt==IONOOPT_EST) {
            udion(rtk,tt,bl,sat,ns);
        }
        
        /* 更新对流层参数 */
        /* 如果启用了对流层估计（TROPOPT_EST 或更高），调用 udtrop 更新对流层湿延迟状态 */
        /* 过程噪声基于 tt 和 opt->prn[4] 添加，基线长度 bl 可能影响模型尺度 */
        if (rtk->opt.tropopt>=TROPOPT_EST) {
            udtrop(rtk,tt,bl);
        }
    }
    
    /* Temporal update of receiver h/w bias for GLONASS */
    /* 如果启用了 GLONASS 自动校准（GLO_ARMODE_AUTOCAL）且系统包含 GLONASS，更新接收机硬件偏差（inter-channel bias, ICB） */
    /* 调用 udrcvbias 更新状态向量中 GLONASS 频率相关的硬件偏差，过程噪声基于 tt */
    if (rtk->opt.glomodear==GLO_ARMODE_AUTOCAL&&(rtk->opt.navsys&SYS_GLO)) {
        udrcvbias(rtk,tt);
    }
    
    /* Temporal update of phase-bias */
    /* 如果定位模式为 RTK（> PMODE_DGPS，如 PMODE_KINEMA、PMODE_STATIC 等），更新单差相位偏移状态 */
    /* 调用 udbias 执行以下操作： */
    /* 1. 检测周跳（LLI、几何无关组合、多普勒） */
    /* 2. 重置相位偏移状态（瞬时模式或观测中断超限） */
    /* 3. 为单差相位偏移添加过程噪声（基于 opt->prn[0] 和 tt） */
    /* 4. 处理周跳或异常值，重置状态 */
    /* 5. 使用单差载波相位和伪距估算相位偏移，校正相位-伪距一致性 */
    /* 6. 初始化未初始化的相位偏移状态 */
    if (rtk->opt.mode>PMODE_DGPS) {
        udbias(rtk,tt,obs,sat,iu,ir,ns,nav);
    }
}
```
:::

#### C.3.4.2 udpos

动态模式下的状态预测为 PVA 模型，相对较为简单。主要关注这里的 Q 矩阵。加速度状态的过程噪声为随机游走模型，而水平和垂直方向的噪声不同：

```c
Q[0] = Q[4] = SQR(opt->prn[3])*fabs(tt) // 水平分量
Q[8] = SQR(opt->prn[4])*fabs(tt)        // 垂直分量
```

最后还需要将过程噪声从本地坐标系（ENU）转换为地球固定系（ECEF）：

```c
ecef2pos(rtk->x,pos);
covecef(pos,Q,Qv);
```

::: details 点击查看代码
```c
/* Temporal update of position/velocity/acceleration -------------------------*/
static void udpos(rtk_t *rtk, double tt)
{
    double *F,*P,*FP,*x,*xp,pos[3],Q[9]={0},Qv[9],var=0.0;
    int i,j,*ix,nx;

    trace(3,"udpos   : tt=%.3f\n",tt); /* 调试日志：输出时间步长 tt（秒） */

    /* fixed mode */
    /* 如果定位模式为固定模式（PMODE_FIXED），用配置的流动站位置（opt->ru）初始化位置状态（x[0:2]） */
    /* 方差设为小值 VAR_POS_FIX = 1E-8（高精度固定位置），跳过动态更新 */
    if (rtk->opt.mode==PMODE_FIXED) {
        for (i=0;i<3;i++) initx(rtk,rtk->opt.ru[i],VAR_POS_FIX,i);
        return;
    }

    /* initialize position for first epoch */
    /* 首个历元初始化：如果位置状态范数为 0（norm(x,3) <= 0.0），表示尚未初始化 */
    /* 位置状态（x[0:2]）用单点定位结果（sol.rr[0:2]）初始化，方差为 VAR_POS = 60 */
    /* 若启用动态（opt.dynamics），速度（x[3:5]）用 sol.rr[3:5] 初始化，方差 VAR_VEL = 10 */
    /* 加速度（x[6:8]）设为 1E-6，方差 VAR_ACC = 10 */
    if (norm(rtk->x,3)<=0.0) {
        trace(3,"rr_init=");tracemat(3,rtk->sol.rr,1,6,15,6); /* 输出单点定位结果 */
        for (i=0;i<3;i++) initx(rtk,rtk->sol.rr[i],VAR_POS,i);
        if (rtk->opt.dynamics) {
            for (i=3;i<6;i++) initx(rtk,rtk->sol.rr[i],VAR_VEL,i);
            for (i=6;i<9;i++) initx(rtk,1E-6,VAR_ACC,i);
        }
    }

    /* static mode */
    /* 如果定位模式为静态（PMODE_STATIC 或 PMODE_STATIC_START），不进行动态预测，直接返回 */
    /* 静态模式假设位置不变，状态由量测更新调整 */
    if (rtk->opt.mode==PMODE_STATIC||rtk->opt.mode==PMODE_STATIC_START) return;

    /* kinematic mode without dynamics */
    /* 如果未启用动态（!opt.dynamics），每次用单点定位结果（sol.rr[0:2]）重置位置状态 */
    /* 方差设为 VAR_POS = 60，不更新速度和加速度，直接返回 */
    if (!rtk->opt.dynamics) {
        for (i=0;i<3;i++) initx(rtk,rtk->sol.rr[i],VAR_POS,i);
        return;
    }

    /* check variance of estimated position */
    /* 检查位置状态（x[0:2]）的平均协方差：var = (P[0,0] + P[1,1] + P[2,2]) / 3 */
    /* 如果 var > VAR_POS（60），表示位置发散，重置位置、速度、加速度状态 */
    for (i=0;i<3;i++) var+=rtk->P[i+i*rtk->nx];
    var/=3.0;

    if (var>VAR_POS) {
        /* 重置状态：位置用 sol.rr[0:2]，方差 VAR_POS；速度用 sol.rr[3:5]，方差 VAR_VEL；加速度设为 1E-6，方差 VAR_ACC */
        for (i=0;i<3;i++) initx(rtk,rtk->sol.rr[i],VAR_POS,i);
        for (i=3;i<6;i++) initx(rtk,rtk->sol.rr[i],VAR_VEL,i);
        for (i=6;i<9;i++) initx(rtk,1E-6,VAR_ACC,i);
        trace(2,"reset rtk position due to large variance: var=%.3f\n",var);
        return;
    }

    /* generate valid state index */
    /* 生成有效状态索引数组 ix，包含位置/速度/加速度（i<9）和其他非零状态（x[i] != 0.0 且 P[i,i] > 0.0） */
    /* 使用 b33 代码逻辑（注释提到 b34 有问题） */
    ix=imat(rtk->nx,1);
    for (i=nx=0;i<rtk->nx;i++) {
        if (i<9||(rtk->x[i]!=0.0&&rtk->P[i+i*rtk->nx]>0.0)) ix[nx++]=i;
    }

    /* state transition of position/velocity/acceleration */
    /* 构建状态转移矩阵 F（单位矩阵）和协方差矩阵 P，执行卡尔曼滤波时间更新 */
    F=eye(nx); P=mat(nx,nx); FP=mat(nx,nx); x=mat(nx,1); xp=mat(nx,1);

    /* 设置状态转移：位置更新 x_i = x_i + v_i * tt（i=0,1,2） */
    for (i=0;i<6;i++) {
        F[i+(i+3)*nx]=tt;
    }
    /* 如果位置协方差 var < opt->thresar[1]（滤波收敛），加入加速度项：x_i = x_i + (1/2) * a_i * tt^2 */
    if (var<rtk->opt.thresar[1]) {
        for (i=0;i<3;i++) {
            F[i+(i+6)*nx]=(tt>=0?1:-1)*SQR(tt)/2.0;
        }
    }
    else trace(3,"pos var too high for accel term: %.4f\n", var);

    /* 提取有效状态和协方差 */
    for (i=0;i<nx;i++) {
        x[i]=rtk->x[ix[i]];
        for (j=0;j<nx;j++) {
            P[i+j*nx]=rtk->P[ix[i]+ix[j]*rtk->nx];
        }
    }

    /* 卡尔曼滤波时间更新 */
    /* 状态预测：x_k(-) = F * x_{k-1}(+) */
    matmul("NN",nx,1,nx,F,x,xp);
    /* 协方差预测：P_k(-) = F * P_{k-1}(+) * F^T */
    matmul("NN",nx,nx,nx,F,P,FP);
    matmul("NT",nx,nx,nx,FP,F,P);

    /* 更新状态向量和协方差矩阵 */
    for (i=0;i<nx;i++) {
        rtk->x[ix[i]]=xp[i];
        for (j=0;j<nx;j++) {
            rtk->P[ix[i]+ix[j]*rtk->nx]=P[i+j*nx];
        }
    }

    /* process noise added to only acceleration  P=P+Q */
    /* 为加速度状态添加过程噪声，建模为随机游走噪声 */
    /* Q 为 3x3 协方差矩阵，水平分量（opt->prn[3]）和垂直分量（opt->prn[4]） */
    Q[0]=Q[4]=SQR(rtk->opt.prn[3])*fabs(tt); /* 水平加速度噪声：Q[0,0] = Q[1,1] = (prn[3])^2 * |tt| */
    Q[8]=SQR(rtk->opt.prn[4])*fabs(tt);      /* 垂直加速度噪声：Q[2,2] = (prn[4])^2 * |tt| */
    
    /* 将过程噪声从本地坐标系（ENU）转换为地球固定系（ECEF） */
    ecef2pos(rtk->x,pos); /* 将当前位置 x[0:2] 转为经纬高 pos */
    covecef(pos,Q,Qv);    /* 将 ENU 系的 Q 转换为 ECEF 系的 Qv */
    
    /* 将过程噪声添加到加速度状态的协方差（P[6:8,6:8]） */
    for (i=0;i<3;i++) for (j=0;j<3;j++) {
        rtk->P[i+6+(j+6)*rtk->nx]+=Qv[i+j*3];
    }

    free(ix); free(F); free(P); free(FP); free(x); free(xp);
}
```
:::

#### C.3.4.3 udbias

`udbias` 函数是卡尔曼滤波时间更新阶段的一部分，负责更新单差相位偏移（phase bias）状态量及其协方差，处理周跳检测、状态重置和相位-伪距一致性校正，具体的：
- 在 RTK 定位的卡尔曼滤波时间更新阶段，更新状态向量中的单差相位偏移状态（`rtk->x[IB(i,k)]`），表示流动站和基站之间的载波相位模糊度（单位：周或米，视配置而定）。
- 检测周跳（cycle slip）和异常值（outlier），重置或初始化相位偏移状态。
- 使用单差载波相位和伪距观测估算相位偏移，校正相位-伪距一致性（phase-code coherency）。
- 为协方差矩阵添加过程噪声，更新卫星状态（如锁定计数、周跳标志）。

`opt.prn[0]` 即 **stats-prnbias** 参数表示单差相位偏移（流动站和基站之间的载波相位模糊度）随时间变化的不确定性，建模为随机游走（random walk）过程。

```c
for (i=0;i<ns;i++) {
    j=IB(sat[i],k,&rtk->opt); /* 获取卫星 sat[i] 频率 k 的单差相位偏移状态索引 */
    rtk->P[j+j*rtk->nx]+=rtk->opt.prn[0]*rtk->opt.prn[0]*fabs(tt); /* 添加过程噪声 */
}
```

为单差相位偏移状态（`rtk->x[j]`，$j = \text{IB(sat[i],k)}$）的协方差矩阵对角元素（`rtk->P[j+j*rtk->nx]`）添加过程噪声。

$$\begin{flalign}
P[j,j] = P[j,j] + (\text{opt->prn[0]})^2 \cdot |\text{tt}|
\end{flalign}$$

- `rtk->opt.prn[0]`：过程噪声标准差（s/√s）。
- `tt`：时间步长（秒），表示当前历元与上一历元的时间差。
- `fabs(tt)`：取绝对值，确保噪声增量为正。
- `rtk->opt.prn[0]*rtk->opt.prn[0]`：将标准差平方，得到方差增量（m^2/s）。

::: details 点击查看代码
```c
/* temporal update of phase biases -------------------------------------------*/
static void udbias(rtk_t *rtk, double tt, const obsd_t *obs, const int *sat,
                   const int *iu, const int *ir, int ns, const nav_t *nav)
{
    double cp,pr,cp1,cp2,pr1,pr2,*bias,offset,freqi,freq1,freq2,C1,C2;
    int i,j,k,slip,rejc,reset,nf=NF(&rtk->opt),f2;

    trace(3,"udbias  : tt=%.3f ns=%d\n",tt,ns); /* 调试日志：输出时间步长 tt 和共视卫星数量 ns */

    /* clear cycle slips */
    /* 清除所有共视卫星的周跳标志（slip）低两位（0xFC = 11111100），为后续周跳检测准备 */
    for (i=0;i<ns;i++) {
        for (k=0;k<rtk->opt.nf;k++) rtk->ssat[sat[i]-1].slip[k]&=0xFC;
    }

    /* detect cycle slip by doppler and phase difference */
    /* 使用多普勒检测周跳（暂未启用，因时间跳变问题） */
    detslp_dop(rtk,obs,iu,ns,1,nav); /* 流动站周跳检测 */
    detslp_dop(rtk,obs,ir,ns,2,nav); /* 基站周跳检测 */

    /* 检测周跳和更新半周标志 */
    for (i=0;i<ns;i++) {
        /* 使用 LLI（失锁指示）检测周跳 */
        detslp_ll(rtk,obs,iu[i],1); /* 流动站 */
        detslp_ll(rtk,obs,ir[i],2); /* 基站 */
        
        /* 使用几何无关（GF）组合检测周跳（如 L1-L2 或 L1-L5） */
        detslp_gf(rtk,obs,iu[i],ir[i],nav);
        
        /* 更新半周标志：若流动站或基站 LLI 标志位 2（半周模糊）置位，则 half[k] = 0（无效） */
        for (k=0;k<nf;k++) {
            rtk->ssat[sat[i]-1].half[k]=
                !((obs[iu[i]].LLI[k]&2)||(obs[ir[i]].LLI[k]&2));
        }
    }
    
    /* 按频率循环处理相位偏移状态 */
    for (k=0;k<nf;k++) {
        /* reset phase-bias if instantaneous AR or expire obs outage counter */
        /* 检查所有卫星（1 到 MAXSAT）是否需要重置相位偏移状态 */
        for (i=1;i<=MAXSAT;i++) {
            /* 观测中断计数器递增，若超过 maxout（opt->maxout），则 reset = 1 */
            reset=++rtk->ssat[i-1].outc[k]>(uint32_t)rtk->opt.maxout;
            
            /* 瞬时模糊度解算（ARMODE_INST）：若状态非零，重置为 0 */
            if (rtk->opt.modear==ARMODE_INST&&rtk->x[IB(i,k,&rtk->opt)]!=0.0) {
                initx(rtk,0.0,0.0,IB(i,k,&rtk->opt));
            }
            /* 观测中断超限：若状态非零，重置为 0，清零计数器，记录日志 */
            else if (reset&&rtk->x[IB(i,k,&rtk->opt)]!=0.0) {
                initx(rtk,0.0,0.0,IB(i,k,&rtk->opt));
                trace(3,"udbias : obs outage counter overflow (sat=%3d L%d n=%d)\n",
                      i,k+1,rtk->ssat[i-1].outc[k]);
                rtk->ssat[i-1].outc[k]=0;
            }
            /* 非瞬时模式且超限：设置锁定计数为负（-minlock），需重新锁定 */
            if (rtk->opt.modear!=ARMODE_INST&&reset) {
                rtk->ssat[i-1].lock[k]=-rtk->opt.minlock;
            }
        }
        
        /* update phase bias noise and check for cycle slips */
        /* 为共视卫星的相位偏移状态添加过程噪声，处理周跳和异常值 */
        for (i=0;i<ns;i++) {
            j=IB(sat[i],k,&rtk->opt); /* 获取相位偏移状态索引 */
            
            /* 添加过程噪声：P[j,j] += (opt->prn[0])^2 * |tt| */
            rtk->P[j+j*rtk->nx]+=rtk->opt.prn[0]*rtk->opt.prn[0]*fabs(tt);
            
            /* 获取周跳标志和异常值计数 */
            slip=rtk->ssat[sat[i]-1].slip[k];
            rejc=rtk->ssat[sat[i]-1].rejc[k];
            
            /* 无电离层组合（IONOOPT_IFLC）：考虑第二频率（f2）的周跳 */
            if (rtk->opt.ionoopt==IONOOPT_IFLC) {
                f2=seliflc(rtk->opt.nf,rtk->ssat[sat[i]-1].sys);
                slip|=rtk->ssat[sat[i]-1].slip[f2];
            }
            
            /* 瞬时模式或无周跳（slip & 1 == 0）且异常值计数 < 2，继续循环 */
            if (rtk->opt.modear==ARMODE_INST||(!(slip&1)&&rejc<2)) continue;
            
            /* 检测到周跳或异常值：重置状态 */
            rtk->x[j]=0.0; /* 重置相位偏移状态为 0 */
            rtk->ssat[sat[i]-1].rejc[k]=0; /* 清零异常值计数 */
            rtk->ssat[sat[i]-1].lock[k]=-rtk->opt.minlock; /* 设置锁定计数为负 */
            /* 非 GLONASS 卫星：清零无电离层偏差 */
            if (rtk->ssat[sat[i]-1].sys!=SYS_GLO) rtk->ssat[sat[i]-1].icbias[k]=0;
        }
        
        /* estimate approximate phase-bias by delta phase - delta code */
        /* 使用单差载波相位和伪距估算相位偏移（bias[i]），校正一致性 */
        bias=zeros(ns,1);
        for (i=j=0,offset=0.0;i<ns;i++) {
            if (rtk->opt.ionoopt!=IONOOPT_IFLC) {
                /* 非无电离层模式 */
                cp=sdobs(obs,iu[i],ir[i],k); /* 单差载波相位（周） */
                pr=sdobs(obs,iu[i],ir[i],k+NFREQ); /* 单差伪距（米） */
                freqi=sat2freq(sat[i],obs[iu[i]].code[k],nav); /* 频率（Hz） */
                if (cp==0.0||pr==0.0||freqi==0.0) continue; /* 无效观测跳过 */
                /* 估算相位偏移（周）：bias[i] = phi - rho * f / c */
                bias[i]=cp-pr*freqi/CLIGHT;
            }
            else {
                /* 无电离层组合模式 */
                f2=seliflc(rtk->opt.nf,rtk->ssat[sat[i]-1].sys); /* 第二频率索引 */
                cp1=sdobs(obs,iu[i],ir[i],0); /* 第一频率单差载波相位 */
                cp2=sdobs(obs,iu[i],ir[i],f2); /* 第二频率单差载波相位 */
                pr1=sdobs(obs,iu[i],ir[i],NFREQ); /* 第一频率单差伪距 */
                pr2=sdobs(obs,iu[i],ir[i],NFREQ+f2); /* 第二频率单差伪距 */
                freq1=sat2freq(sat[i],obs[iu[i]].code[0],nav); /* 第一频率 */
                freq2=sat2freq(sat[i],obs[iu[i]].code[f2],nav); /* 第二频率 */
                if (cp1==0.0||cp2==0.0||pr1==0.0||pr2==0.0||freq1<=0.0||freq2<=0.0) continue;
                /* 无电离层组合系数：C1 = f1^2 / (f1^2 - f2^2), C2 = -f2^2 / (f1^2 - f2^2) */
                C1=SQR(freq1)/(SQR(freq1)-SQR(freq2));
                C2=-SQR(freq2)/(SQR(freq1)-SQR(freq2));
                /* 估算相位偏移（米）：bias[i] = (C1*phi1*c/f1 + C2*phi2*c/f2) - (C1*rho1 + C2*rho2) */
                bias[i]=(C1*cp1*CLIGHT/freq1+C2*cp2*CLIGHT/freq2)-(C1*pr1+C2*pr2);
            }
            /* 计算有效卫星（状态非零）的偏差均值：offset += bias[i] - x[IB(i,k)] */
            if (rtk->x[IB(sat[i],k,&rtk->opt)]!=0.0) {
                offset+=bias[i]-rtk->x[IB(sat[i],k,&rtk->opt)];
                j++;
            }
        }
        
        /* correct phase-bias offset to ensure phase-code coherency */
        /* 校正有效卫星的相位偏移状态：x[IB(i,k)] += offset/j，保持相位-伪距一致性 */
        if (j>0) {
            for (i=1;i<=MAXSAT;i++) {
                if (rtk->x[IB(i,k,&rtk->opt)]!=0.0) rtk->x[IB(i,k,&rtk->opt)]+=offset/j;
            }
        }
        
        /* set initial states of phase-bias */
        /* 为未初始化的共视卫星（x[IB(sat[i],k)] == 0.0）设置相位偏移初值 */
        for (i=0;i<ns;i++) {
            if (bias[i]==0.0||rtk->x[IB(sat[i],k,&rtk->opt)]!=0.0) continue;
            /* 初始化状态：x[IB(sat[i],k)] = bias[i]，协方差 P[IB(sat[i],k)] = (opt->std[0])^2 */
            initx(rtk,bias[i],SQR(rtk->opt.std[0]),IB(sat[i],k,&rtk->opt));
            trace(3,"     sat=%3d, F=%d: init phase=%.3f\n",sat[i],k+1,bias[i]);
            /* 非瞬时模式：设置锁定计数为负（-minlock），需重新锁定 */
            if (rtk->opt.modear!=ARMODE_INST) {
                rtk->ssat[sat[i]-1].lock[k]=-rtk->opt.minlock;
            }
        }
        free(bias); /* 释放临时数组 */
    }
}
```
:::
