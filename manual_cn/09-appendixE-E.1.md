---
sidebarDepth: 2
---

# 附录E. 模型与算法

## E.1 时间系统

RTKLIB内部采用GPS时间进行GNSS数据处理和定位。UTC时间（协调世界时）是一个不连续时间，简而言之，它和GPS时间之间存在一个跳秒（或闰秒）的关系。GPS时间通常采用周数和周内秒来表示，其起始历元为1980年1月6日00:00:00 UTC。但是对于double类型变量来说，其距离精度只有0.04米（参考[E.1.6/Q1](/algorithm/RTKLIB-Manual-CN/09-appendixE-E.1.html#e-1-6-常见问题)）。因此，在实际运算时，RTKLIB调用C标准库中的时间结构体并定义了`gtime_t`类型：

```c
typedef struct {
    time_t time; /* time (s) expressed by standard time_t */
    double sec;  /* fraction of second under 1 s */
} gtime_t;
```

`time_t` 通常通过一个无符号的32位整数来实现，表示从1970年1月1日00:00:00开始的总秒数。由于整数位的长度限制，`gtime_t` 无法处理1970年1月1日之前或2038年1月19日之后的时间。

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
    <td class="tg-0pky">BDT比GPST滞后14秒</td>
    <td class="tg-0pky">t<sub>BDT</sub>=t<sub>GPST</sub>-14</td>
  </tr>
</tbody></table>

### E.1.1 GPST（GPS时间）和UTC（协调世界时）

现在的UTC时间比GPST慢，因为它当前闰掉的时间为负数。GPST与UTC（协调世界时）之间的粗略转换可以简单地表示为：  

$\begin{equation}
t_{UTC}=t_{GPST}-\Delta{t}_{LS} \tag{E.1.1}
\end{equation}$
$\begin{equation}
t_{GPST}=t_{UTC}+\Delta{t}_{LS} \tag{E.1.2}
\end{equation}$
 
其中，$t_{UTC}$和$t_{GPST}$分别是以秒为单位的UTC时间和GPS时间。$\Delta{t}_{LS}$是由于自1980年1月6日以来累积的闰秒（或跳秒）导致的UTC与GPST之间的时间差（秒）。$\Delta{t}_{LS}$的值如表E.1-1所示。

<p style="text-align: center;">表E.1-2 GPST-UTC值（截至2025年2月，未来可能更新）</p>

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

### E.1.2 GLONASST (GLONASS时间)

GLONASST（GLONASS时间）基于UTC（SU），因此它同样包含了闰秒。GLONASST还与当地时间对齐。因此，大致上，GLONASST中的时间$t_{GLONASST} (s)$可以转换为UTC中的时间$t_{UTC} (s)$。

$\begin{equation}
t_{UTC}=t_{GLONASST}-10800 \tag{E.1.4}
\end{equation}$

更准确地说，GLONASS导航信息中的GLONASST的UTC参数应类似于GPST（GPS时间）和UTC转换的方式使用。忽略闰秒和3小时的时差，GLONASST与GPST的偏差通常在毫秒级，具体通过导航电文校正（GLONASS时间同步精度低于GPS）。

### E.1.3 GST (Galileo时间)

伽利略系统时间（Galileo System Time，GST）与GPST类似，同样有周与周内秒组成。GST的起始历元为1999年8月22日00:00:00 UTC（这个时间刚好和GPS的起始时间1980年1月6日00:00:00 UTC相隔1024周）。在起始历元时，GST比UTC快13秒。GST是连续时间，不插入或删除闰秒。因此，GST与GPS时间（GPST）对齐，除了时间系统原点的1024周差异和一个小的时间偏移（Galileo/GPS时间差异，GGTO）。根据RINEX约定，伽利略周数与GPS周数相等。

$\begin{equation}
t_{GST} = t_{GPST} + GGTO + 1024×604800 \tag{E.1.5}
\end{equation}$

上述GGTO由伽利略导航电文提供。

### E.1.4 QZSST (QZSS时间)

QZSST（QZSS时间）与GPST（GPS时间）对齐。它与GPST具有相同的起始点和相同的秒定义，QZSST可以像GPST一样处理。

### E.1.5 BDT (北斗时间)

北斗时间（BDT）也是一个连续时间系统（没有闰秒的插入或删除）。BDT的起始历元为2006年1月1日00:00:00 UTC。BDT与UTC之间的时间差被控制在100纳秒以内（模1秒）。因此，GPS时间（GPST）中的时间$𝑡_{𝐺𝑃𝑆}(s)$可以大致通过以下公式转换为BDT中的时间$𝑡_{BDT}(s)$，精度约为200纳秒：

$\begin{equation}
𝑡_{𝐵𝐷𝑇} = 𝑡_{𝐺𝑃𝑆} − 14 \tag{E.1.6}
\end{equation}$

BDT同样需要使用北斗导航信息中的UTC参数，然后进行类似GPST与UTC之间的转换。

### E.1.6 常见问题

::: info Q1：为什么使用double类型表示GPS时间，距离精度是0.04米？
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

<GiscusTalk />
