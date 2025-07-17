## 1.4 核心代码库介绍

本章节仅作为 RTKLIB 内容的概览，不建议花费过多时间研究，详细内容请参考 RTKLIB 源码以及后续章节所对应的解析上。

代码库（src中包含的程序）是 RTKLIB 的核心，该代码库以方便调用的方式提供给用户，提供了相当全面的GNSS功能服务，也因此RTKLIB中包含了"LIB"的名称。RTKLIB提供的库函数和API（应用程序接口）如下所示：
   
- **基础工具：** 卫星和导航系统函数、矩阵运算函数、时间与字符串函数、坐标转换、平台相关函数、选项配置
- **数学模型：** 标准定位、精密定位、整周模糊度固定、星历与时钟函数、大气模型、天线模型、潮汐模型、大地水准面模型、大地基准转换、SBAS
- **数据处理：** 输入和输出函数、接收机原始数据处理、解函数（定位结果处理）、KML转换器
- **协议支持：** RINEX、RTCM、NMEA
- **数据传输：** 数据流服务器、数据下载
- **程序调试：** 调试与跟踪

rtklib.h 管理了核心代码库的代码，并对一些接口函数进行了导出。 rtklib.h 主要有四大部分：**宏定义**、**结构体定义**、**全局变量**、**函数定义**

>  注意只有加了 EXPORT 前缀的函数才可以被其他源文件调用。源码中包含了许多以 static 开头的函数，它们在 rtklib.h 没声明，因此无法被外部调用。想调用它们的话，只需要把前缀改成 EXPORT，然后在 rtklib.h 中加上声明，这也是比较常见的需求。

---

### 1.4.1 宏定义

<img style="width: 100%; margin: 0 auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-120043.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-1 rtklib.h 宏定义[1]</p>

当前包含的各种 ifdef：

* **WIN32、WIN_DLL**：用 Windows 下的代码；
* **TRACE**：用于调试信息输出；
* **ENAGLO、ENAGAL、ENAQZS、ENACMP、ENAIRN、ENALEO**：启用除 GPS 外的卫星系统，
* **OBS_100HZ**：判定时间重合的阈值 DTTOL
* **MKL**：使用英特尔数学核心函数库（Intel MKL）
* **LAPACK**：使用线性代数包（LAPACK）/基础线性代数子程序（BLAS）
* **IERS_MODEL**：使用国际地球自转服务（IERS）潮汐模型
* **CPUTIME_IN_GPST**：以全球定位系统时间（GPST）进行CPU时间操作；
* **CLOCK_MONOTONIC_RAW**：Linux中用于时间测量的一个时钟源选项；
* **RRCENA**：启用循环冗余校验（RRC）校正；
* **SVR_REUSEADDR**：重用TCP服务器地址。

其中 TRACE、ENAGLO、ENAGAL、ENAQZS、ENACMP、ENAIRN、ENALEO 最为常用。

---

### 1.4.2 结构体定义

<img style="width: 100%; margin: 0 auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-120814.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-2 rtklib.h 结构体[1]</p>

---

### 1.4.3 全局变量

* **卡方检验表**：`extern const double chisqr[];`
* **默认处理选项**：`extern const prcopt_t prcopt_default;`
* **默认结果选项**：`extern const solopt_t solopt_default;`
* **SBAS IGP 波段 0-8**：`extern const sbsigpband_t igpband1[9][8];`
* **SBAS IGP 波段 9-10**：`extern const sbsigpband_t igpband2[2][5];`
* **数据流格式字符串**：`extern const char *formatstrs[];`
* **系统选项表**：`extern opt_t sysopts[];`

---

### 1.4.4 基础函数定义

**1. 矩阵、向量、最小二乘、卡尔曼滤波**

![image-20231021091639437](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231021091639437.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-3 rtklib.h 矩阵、向量、最小二乘、卡尔曼滤波[1]</p>

* RTKLIB 中用 double 类型一维数组表示矩阵，不能自动识别矩阵的行列数，每次传矩阵的时候都要传入行数 n、列数 m。
* 用矩阵的时候要先 malloc 开辟空间，用完记得 free 释放空间。
* 要能熟练计算矩阵加减乘除转置求逆。
* RTKLIB 没有实现矩阵加减的函数，用的时候直接写 for 循环，比如把三维向量 dx 加到 X 上：
  ```c
  for (i=0;i<3;i++) X += dx;
  ```
* 矩阵求逆用的 LU 分解法，时间复杂度 $O^3$ ，对于大规模的矩阵，如果利用矩阵的稀疏性和对称性等特性，而且当使用不完全分解方法（例如，只计算到一定程度或使用截断技术）时，LU 分解的效率会更高。
* matprint() 很常用，调试的时候很难直接看的矩阵元素的值（都是指针），得输出到终端或者文件再看。

---

**2. 时间和字符串**

![image-20231021090651319](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231021090651319.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-4 rtklib.h 时间和字符串[1]</p>

* RTKLIB 中时间一般都以 `gtime_t` 类型存储，为了提高时间表示的精度，分开存 GPST 时间的整秒数和不足一秒的部分。
* 经常需要做年月日时分秒、周+周内秒、GPST 三种时间之间的转换；输出北京时间要在 UTC 基础上加 8 小时。
* BDT、GLONASST 不用于计算，读完文件就转为 GPS 时间了。

---

**3. 坐标转换**

![image-20231021091756265](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231021091756265.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-5 rtklib.h 坐标转换[1]</p>

* ECI 用的很少，只在 `sunmoonpos()` 函数中计算日月坐标时候用到了，不用怎么关注。
* ENU、ECEF、LLH 三套坐标系都频繁使用，要熟练掌握他们之间的转换，包括协方差的转换
* ENU 是局部相对坐标系，以某一个 LLH 坐标为原点，坐标转换的时候要传入这个 LLH 坐标。
* ENU 常用 `e`表示、ECEF 常用 `r` 表示、LLH 常用 `pos` 表示。

---

**4. 卫星系统、观测值**

* **卫星系统表示**：
  * 表示卫星系统的字母：GRECJIS。
  * 7 位二进制码表示，对应位写 1 表示有对应的系统，做与运算可加系统。
* **卫星的表示**：
  * 可以表示为各系统的卫星 ID（系统缩写+PRN）：B02、C21。
  * 也可表示为连续的卫星编号 satellite number，断点调试或者看 Trace 文件的时候，经常只能看到卫星编号。
* **观测值类型**：
  * **C**：伪距、**D**：多普勒、**L**：载波相位、**S**：载噪比。
  * `CODE_XXX`：观测值类型定义，用一串连续的数字表示。
  * `sigind_t`：表示每种卫星系统的载波类型和观测值类型 ，每种类型的系统其实对应的就是一个 `sigind_t` 结构体。
* **观测值优先级**：
  ![](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231024191221181.png)
  <p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-6 rtklib.h 观测值优先级[1]</p>

---

### 1.4.5 选项调试输出

**1. 配置选项读取**

![image-20231025205106288](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025205106288.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-7 配置选项读取</p>

* 选择主要存在 `prcopt_t`、`solopt_t`、`filopt_t` 三个结构体中。
* 后处理解算程序 rnx2rtkp 和实时解算程序 rtksvr 读取结果文件流程是一样的：
  * 先调用 `resetsysopts()` 重置所有配置为默认。
  * 调用 `loadopts()` 读取配置文件内容，存入 `opt_t` 的 `sysopt` 中。
  * 最后调用 `getsysopts()` 将 `opt_t` 转到 `porcopt_t`/`solopt_t`/`filopt_t`。

---

**2. Trace 调试**

![image-20231025205158762](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025205158762.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-8 Trace 调试</p>

* 在 rtklib.h 中加入 #define TRACE，启用 trace ，不定义则将 trace 函数全赋空值。
* Trace 信息分五个等级，从 1-5 重要性逐渐降低，通过 tracelevel() 函数可以设置输出的最高等级，设置 2 级意味着只输出 1/2 级信息。
  * **一级 Trace 是致命错误**，出现一级错误基本上意味着程序无法继续执行，比如观测星历文件读取错误、内存分配错误。
  * **二级 Trace 是警告**，出现二级警告程序可能依然能继续执行，但也可能无法进行解算，比如改正文件读取失败，数据解析出错，二进制数据校验出错，某一历元解算失败，缺失解算所需的星历或改正参数等。
  * **三级  Trace 是程序主要执行流程**，主要在函数的开头，告诉我们执行到了这个函数。
  * **四级 Trace 是比三级更深入的程序执行流程**，主要在三级  Trace 函数的中间或者调用的子函数开头，告诉我们执行到了这个操作。
  * **五级 Trace 是解算的中间过程**，具体到每颗卫星，每个频点，每次循环。
* 看 Trace 文件可以辅助断点调试，甚至替代断点调试。程序执行出错，开 2/3 级 Trace，看 Trace 文件里的 error、warring 就能知道大致出了啥问题，定位出问题的函数，断点调试的时候你就知道该在哪设置断点了。

---

**3. 结果输入输出、NMEA**

输出包含了解算结果与过程状态数据，而根据输出的定义，除了NMEA，还有 RTKLIB 中自定义的数据格式（笔者喜欢称其为 POS 格式）。更多内容请参考 [RTKLIB-Manual-CN 附录 B](/algorithm/RTKLIB-Manual-CN/06-appendixB.html)。

![image-20231025205301277](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025205301277.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-9 结果输入输出、NMEA</p>

---

### 1.4.6 导航数据

**1. 导航数据输入**

![image-20231025205602582](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025205602582.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-10 导航数据输入</p>

---

**2. RINEX 文件读写**

![image-20231025205701553](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025205701553.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-11 RINEX 文件读写</p>

---

**3. 二进制数据读写**

* 用于数据流解析。

![image-20231025205800413](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025205800413.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-12 二进制数据读写</p>

---

**4. 星历数据解析**

整体上与观测数据的解析类似，只是会按照星历的数据格式填充相应的结构体。

---

**5. RTCM 读写**

想进行定位解释至少要有星历，要有观测数据，常用 NAV 配 MSM4（伪距载波信噪比）、MSM7（伪距载波多普勒信噪比）

![image-20231025205952192](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025205952192.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-13 RTCM 读写[1]</p>

---

**6. 接收机自定义格式读写**

这里暂未涉及。

---

### 1.4.7 解算相关

**1. 定位解算入口**

![image-20231025210531296](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025210531296.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-14 定位解算入口[1]</p>

---

**2. 实时解算**

![image-20231025210624595](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025210624595.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-15 实时解算[1]</p>

---

### 1.4.8 数据流相关

**1. 数据流**

* 数据流函数用的大部分在 Window 和 Linux 各有一套，涉及到很多系统库，好在现在 AI 发达，可以用来辅助理解。
* 每种数据流关注四个函数：打开、关闭、写数据、读数据。

![image-20231025210846191](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025210846191.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-16 数据流[1]</p>

---

**2. 数据流线程管理**

![image-20231025210938225](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025210938225.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-17 数据流线程管理[1]</p>

---

### 1.4.9 模型改正

**1. 星历、钟差、DCB、FCB**

![image-20231025223050670](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025223050670.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-18 星历、钟差、DCB、FCB[1]</p>

---

**2. SBAS**

![image-20231025223134482](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025223134482.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-19 SBAS[1]</p>

---

**3. 定位模型：计算高度角、方位角、卫地距、DOP 值**

![image-20231025223220415](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025223220415.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-20 定位模型：计算高度角、方位角、卫地距、DOP 值[1]</p>

---

**4. 对流层、电离层模型**

![image-20231025223257219](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025223257219.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-21 对流层、电离层模型[1]</p>

---

**5. 天线改正：读取天线文件**

![image-20231025223330203](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025223330203.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-22 天线改正：读取天线文件[1]</p>

---

**6. 潮汐改正**

![image-20231025223344099](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025223344099.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-23 潮汐改正[1]</p>

---

**7. 水准面模型**

![image-20231025223357868](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025223357868.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-24 水准面模型[1]</p>

---

**8. 高程转换**

都是转日本的高程系统，咱们用不到。

![image-20231025223409859](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025223409859.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-25 高程转换[1]</p>

---

### 1.4.10 杂项函数

**1. 结果格式转换**

把定位结果转为 KML、GPX 格式：

![image-20231025212144253](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025212144253.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-26 结果格式转换[1]</p>

---

**2. GIS 数据读取**

可以读取 shapfile 矢量数据。

![image-20231025212339125](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025212339125.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-27 GIS 数据读取[1]</p>

---

**3. 平台相关函数**

在 Windows 和 Linux 有完全不同的两套实现。

![image-20231025212504258](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025212504258.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.4-28 平台相关函数[1]</p>

<GiscusTalk />
