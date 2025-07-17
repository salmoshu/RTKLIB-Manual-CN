
# 1. 相对定位

### 11.1.9 卫星系统的表示

- 表示卫星系统的字母：G：GPS、R：GLONASS、E：GALILEO、C：BDS、J：QZSS，I：IRNSS、S：SBAS

- 7位二进制码表示，对应位写1表示有对应的系统，做与或算可加系统。

  ```c
  static const int navsys[]={             /* satellite systems */
      SYS_GPS,SYS_GLO,SYS_GAL,SYS_QZS,SYS_SBS,SYS_CMP,SYS_IRN,0
  };
  ```

  ```
  #define SYS_NONE    0x00                /* navigation system: none */
  #define SYS_GPS     0x01                /* navigation system: GPS */
  #define SYS_SBS     0x02                /* navigation system: SBAS */
  #define SYS_GLO     0x04                /* navigation system: GLONASS */
  #define SYS_GAL     0x08                /* navigation system: Galileo */
  #define SYS_QZS     0x10                /* navigation system: QZSS */
  #define SYS_CMP     0x20                /* navigation system: BeiDou */
  #define SYS_IRN     0x40                /* navigation system: IRNS */
  #define SYS_LEO     0x80                /* navigation system: LEO */
  #define SYS_ALL     0xFF                /* navigation system: all */
  ```

### 11.1.11 卫星的表示

可以表示为各系统的卫星ID（系统缩写+PRN）：B02、C21，也可表示为连续的satellite number ，各种转换函数如下：

- **satno()**：传入卫星系统(SYS_GPS,SYS_GLO,...) ，和PRN码，转换为连续的satellite number。
- **satsys()**：传入satellite number ，返回卫星系统(SYS_GPS,SYS_GLO,...) ，通过传入的指针prn传出PRN值。
- **satid2no()**：传入卫星ID，返回satellite number。
- **satno2id()**：传入卫星系统，和PRN，返回卫星ID(Gxx,Cxx)
- **sat2code()**：传入satellite number，返回卫星ID(Gxx,Cxx)
- **code2sys()**：传入卫星系统缩写，返回系统二进制码SYS_XXX。
- **satexclude()**：检测某颗卫星在定位时是否需要将其排除

### 11.1.12 观测值类型的表示

- C：伪距、D：多普勒、L：载波相位、S：载噪比

```c
  static const char obscodes[]="CLDS";    /* observation type codes */
```

- sigind_t：表示每种卫星系统的载波类型和观测值类型 ，每种类型的系统其实对应的就是一个sigind_t结构体，也就是说只需要建立七个结构体就够了。 

  ```c
  typedef struct {                        /* signal index type */
      int n;                              /* number of index */   //n代表这个卫星系统总的观测值类型，对应的卫星系统标识符后面的数字
      int idx[MAXOBSTYPE];                /* signal freq-index */
      int pos[MAXOBSTYPE];                /* signal index in obs data (-1:no) */
      uint8_t pri [MAXOBSTYPE];           /* signal priority (15-0) */
      uint8_t type[MAXOBSTYPE];           /* type (0:C,1:L,2:D,3:S) */
      uint8_t code[MAXOBSTYPE];           /* obs-code (CODE_L??) */
      double shift[MAXOBSTYPE];           /* phase shift (cycle) */
  } sigind_t;
  
  ```

- **CODE_XXX**：观测值类型定义，用一串连续的数字表示。

  ```c
  #define CODE_NONE   0                   /* obs code: none or unknown */
  #define CODE_L1C    1                   /* obs code: L1C/A,G1C/A,E1C (GPS,GLO,GAL,QZS,SBS) */
  #define CODE_L1P    2                   /* obs code: L1P,G1P,B1P (GPS,GLO,BDS) */
  ......
  #define CODE_L4B    67                  /* obs code: G1aL1OCd   (GLO) */
  #define CODE_L4X    68                  /* obs code: G1al1OCd+p (GLO) */
  #define MAXCODE     68                  /* max number of obs code */
  ```

- **code2obs**()：传入obs code (CODE_???) ，返回code string ("1C","1P","1Y",...)

- **obs2code**()：传入code string ("1C","1P","1Y",...)，返回obs code (CODE_???) 

  ```c
  static char *obscodes[]={       /* observation code strings */
      
      ""  ,"1C","1P","1W","1Y", "1M","1N","1S","1L","1E", /*  0- 9 */
      "1A","1B","1X","1Z","2C", "2D","2S","2L","2X","2P", /* 10-19 */
      "2W","2Y","2M","2N","5I", "5Q","5X","7I","7Q","7X", /* 20-29 */
      "6A","6B","6C","6X","6Z", "6S","6L","8L","8Q","8X", /* 30-39 */
      "2I","2Q","6I","6Q","3I", "3Q","3X","1I","1Q","5A", /* 40-49 */
      "5B","5C","9A","9B","9C", "9X","1D","5D","5P","5Z", /* 50-59 */
      "6E","7D","7P","7Z","8D", "8P","4A","4B","4X",""    /* 60-69 */
  };
  ```

- **code2idx**()：传入obs code (CODE_???) 和卫星系统(SYS_???) ，返回载波频率的下标

  ```c
  frequency index (-1: error)
                        0     1     2     3     4 
             --------------------------------------
              GPS       L1    L2    L5     -     - 
              GLONASS   G1    G2    G3     -     -  (G1=G1,G1a,G2=G2,G2a)
              Galileo   E1    E5b   E5a   E6   E5ab
              QZSS      L1    L2    L5    L6     - 
              SBAS      L1     -    L5     -     -
              BDS       B1    B2    B2a   B3   B2ab (B1=B1I,B1C,B2=B2I,B2b)
              NavIC     L5     S     -     -     - 
  ```

- **code2freq**()：传入obs code (CODE???))和卫星系统(SYS_???)，以及GLONASS的信道，调用code2freq_GPS()、code2freq_GLO()、code2freq_GAL()、code2freq_QZS()、code2freq_SBS()、code2freq_BDS()、code2freq_IRN()，返回对应的载波频率(Hz) 

- **sat2freq()**：传入satellite number和obs code ，返回对应的载波频率(Hz) 

- **setcodepri()**、**getcodepri()**：设置和获取信号优先级。如果输入的观测数据在同一频率内包含多个信号，RTKLIB将按照以下默认优先级选择一个信号进行处理。 

  ```c
  static char codepris[7][MAXFREQ][16]={  /* code priority for each freq-index */
     /*    0         1          2          3         4         5     */
      {"CPYWMNSL","PYWCMNDLSX","IQX"     ,""       ,""       ,""      ,""}, /* GPS */
      {"CPABX"   ,"PCABX"     ,"IQX"     ,""       ,""       ,""      ,""}, /* GLO */
      {"CABXZ"   ,"IQX"       ,"IQX"     ,"ABCXZ"  ,"IQX"    ,""      ,""}, /* GAL */
      {"CLSXZ"   ,"LSX"       ,"IQXDPZ"  ,"LSXEZ"  ,""       ,""      ,""}, /* QZS */
      {"C"       ,"IQX"       ,""        ,""       ,""       ,""      ,""}, /* SBS */
      {"IQXDPAN" ,"IQXDPZ"    ,"DPX"     ,"IQXA"   ,"DPX"    ,""      ,""}, /* BDS */
      {"ABCX"    ,"ABCX"      ,""        ,""       ,""       ,""      ,""}  /* IRN */
  };
  ```

### 后处理处理流程

![](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/ce409d244fbd4dc0a3da7c4cb5c93c85.png)

**c. LAMBDA 算法核心**

- 基于整数变换的模糊度**降相关**——为了保证LAMBDA捜索的快捷性和准确性，需要首先对原始模糊度的协因数矩阵加以调整，使得原始模糊度间的相关性降低。
- 基于**序贯条件最小二乘**估计的整周捜索LAMBDA搜索是整个过程的关键和主体，其主要目的是通过连续的迭代找出最接近真的的模糊度整数值。

<img src="https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/52c78060560c477689d8c4aaff88cfb6.png" style="zoom: 67%;" />

### 1.6.5 算法学习顺序

算法学习还是建议**以问题为导向**，寻找到自己的关注点，而非全盘通吃。不过如果一定要系统性地研究可以按照如下顺序：

* **矩阵运算**：矩阵都是用一维 double 数组表示、列优先，要熟练掌握矩阵的加减乘除转置求逆，还要会 `matprint()` 输出矩阵用于调试，比如你想看程序运行过程中某个矩阵的值，断点调试直接看肯定不行，矩阵都是指针，得用  `matprint()`  输出；
* **参数估计**：把最小二乘、卡尔曼滤波的四个函数看明白；后处理的时候有前向滤波、反向滤波、正反向结合三种滤波方式，体现在代码上就是有个标记标志前后，取数据的顺序不同；
* **时间系统**：知道基本概念（GPS 时、UTC、周内秒、跳秒、儒略日），理解 `gtime_t` 类型，会用操作 `gtime_t` 的函数，比如算时间差、比较时间先后、输出时间字符串、输出当前北京时间字符串、转周内秒；
* **坐标系统**：矩阵用三维向量表示，要了解 ECEF（XYZ）、LLH（纬经高）、ENU（东北天）的用途、转换函数（包括坐标转换、协方差转换），其中：
  * ECEF 是直角坐标系，列观测方程计算方便，在 RTKLIB 中一般用 r 表示；
  * LLH 反映了测站在地球椭球上的位置，在 RTKLIB 中一般用 pos 表示；
  * ENU 是站心坐标系，是以测站为原点建立的直角坐标系，方便表示相对关系（卫星相对接收机、流动站相对于基准站），比如计算方位角高度角，视线向量；ENU 表示东北天，生活中常用，比如导航软件告诉你“向东行驶200米左转”；ENU 坐标系都是以某一个 LLH，这个原点 LLH 必须存下来，ENU 才有意义，ENU 转 ECEF、LLH 的时候需要有坐标原点的 LLH。
* **卫星系统定义**：算的时候得知道观测值是哪个卫星系统的，有两套表示方法：
  * 表示卫星系统的字母：GRECJIS；
  * 或者 7 位二进制码 SYS_xxx，对应位写 1 表示有对应的系统，做或算可加系统，做与运算判断有无系统；
* **卫星定义**：解算的时候需要知道观测值是哪颗卫星的，也有两套表示方法：
  * 可以表示为各系统的卫星 ID（系统缩写+PRN）：B02、C21；直观且含义明确，但不好处理；
  * 也可表示为连续的整型数字 satellite number，好处理，方便遍历；
* **观测量定义**：**C**：伪距、**D**：多普勒、**L**：载波相位、**S**：载噪比；`CODE_XXX`：观测值类型定义，用一串连续的数字表示；
* **配置选项**：主要是三个结构体：`prcopt_t` 存处理选项、`filopt_t` 存文件路径、`solopt_t` 存结果输出格式；默认处理选项、结果选项要理解，常用的处理选项要记住；
* **后处理解算大致流程**：结合流程图把 rnx2rtkp、postpos、procpos、rtkpos 看明白，知道配置存到哪、数据存到哪、结果存到哪、哪个函数把数据读进来、SPP/RTK/PPP 分别在哪些函数进行、前向滤波/后向滤波区别；
* **RINEX读取**：不用太细看，对数据格式有个基本的认识，知道读进来的数据以什么形式，存到什么变量里就行；
* **Trace 输出**：知道怎么打开和关闭 Trace 输出、设置 Trace 等级，出了问题能根据 Trace 输出定位到出错位置、看明白出错原因；
* **结果输出**：有两套，一套是输出定位结果，包括位置速度钟差以及它们的协方差等，存在 `sol_t`、`solbuf_t` 中，由 `outsol()` 函数输出；一套输出解算中间结果，包括高度角方位角残差等，存在 `solstat_t`、`solstatbuf_t` 中，由 `outsolstat()` 输出；
* **卫星位置计算**：精密星历和广播星历都是读文件套公式计算，对照着代码看一遍文件格式和公式，有点点印象，知道 BDS、GLONASS 和其它系统计算的区别就可以；
* **卫星钟差计算**：用广播星历里的 $a_0,a_1,a_2$ 二次函数拟合系数算，迭代三次，要做群波延迟校正、相对论效应改正；
* **电离层改正**：当信号通过电离层时，传播速度和传播路径会发生改变，带来电离层延迟；大小与电子密度成正比，对载波和伪距影响相反，不同信号频率延迟不同，影响可达数十米；
  * **克罗布歇模型电离层改正**；
  * **电离层 INOEX 文件**；
  * **估计电离层 STEC**；
* **对流层改正**：信号穿过对流层时，由于传播介质密度的增加，信号传播路径和传播速度会发生改变，带来对流层延迟。与频率无关，对载波和伪距影响相同。对流层延迟一般可分为干延迟和湿延迟，对于载波相位和伪距完全相同，一般在米级大小；
  * **Saastamoninen 模型**：对流层分为两层进行积分，一层温度视为常数、一层温度有变化，然后按照天顶距三角函数展开逐项进行积分, 并把对流层天顶延迟分为对流层干延迟和湿延迟两个分量之和；
  * **标准大气模型**：根据经验模型计算求大气压 P、温度 T、大气水汽压力 e；
  * **GPT 模型**：GPT 模型的气压温度算的准一点，利用欧洲中尺度天气预报中心 长期的再分析气象资料建立的全球气象参数经验模型, 仅需知道测站地理位置信息与年积日便可以获得地表温度、大气压力和水汽压等气象参数；
  * **估计对流层 ZTD**；
* **天线相位改正**：包括卫星端和接收机端、PCO 和 PCV，GNSS 观测量是卫星和接收机天线相位之间的，而不是几何中心，需要转到几何中心，常通过 igs14.atx 文件来改正。不研究这个方向，就不用太细看；
* **天线相位缠绕**；
* **地球自转改正**：也称 Sagnac 效应改正，卫星信号到达地球时 ECEF 坐标系会绕地球时转动 $\omega r$，计算卫星与接收机间的几何距离时需要套公式改正；
* **潮汐改正**：地球并非刚体，会在日月引力、地球负荷作用下产生周期性形变，分为固体潮、极潮、海洋潮，改正的时候先算日月坐标，然后套公式计算；
* **观测值排除**：星历缺失、高度角、信噪比、人为排除卫星、URA；
* **差分码偏差 DCB**：GPS 广播星历是相对 P 码而言，而我们普通用户定位解算的时候用 C/A 码，需要通过 DCB 文件中的参数或者广播星历中的 TGD 来把测量的伪距归化到 P 码。BDS、GLONASS、Galileo 也类似；
* **单频单系统伪距单点定位**：高度角方位角、卫地视线向量、近似距离计算，设计矩阵 H、新息向量 V 的构建，量测协方差阵 var；
* **多系统**：多系统涉及到系统间偏差 ISB，以系统间时间偏差为主，还包括硬件延迟，每多一个系统，就要多估计一个相对于 GPS 的 ISB，增广参数向量和设计矩阵；
* **DOP 值计算**：反映卫星的几何分布；
* **RAIM-FDE**：定位解算迭代若干次之后，残差仍然过大，认为定位解算发射，剔除残差最大的卫星观测值再进行解算，不断重复这个过程，知道解算成功，或者卫星数量过少不足以解算；
* **多频**：多频涉及到频间偏差 IFB；由于 GLONASS  信号频分多址调制，同频还存在频间偏差；
* **周跳检测**：RTKLIB 实现了两套周跳检测 LLI 和 GF，检测到周跳要重置模糊度估计参数，没做周跳修复；
* **差分定位**；
* **模糊度固定**；
* **浮点解 PPP**：理清楚改正了哪些误差，用了哪些文件，估计了哪些参数，参数的排列顺序，每种参数建立什么随机模型，初始噪声过程噪声怎么设置，出现什么情况要重置参数；
* **实时解算流程**：顺着 rtkrcv 的主函数往下看，算法和后处理没啥区别，数据读取；
* **数据流**：包括串口、文件、Ntrip、TCP、UDP 等；
* **RTCM、RAW 读取**：简单了解数据格式，知道每种语句都有什么数据，用什么函数能读取到什么类型的哪个变量中；
* **SBAS 改正**；
* **SSR 改正**；

### 1.6.6 总结

* 矩阵运算、参数估计、时间系统、坐标系统、卫星和观测值的表示，是基础，要熟练掌握。
* 结果输出、Trace 输出、Rinex 和各种其它文件的读取，知道文件格式，知道大概哪个函数就行，基本不需要细致了解。
* 后处理流程要有印象，重点关注定位方程，H、V、R 矩阵的构建。了解模型改正原理，比如对流层、电离层、天线、潮汐、地球自转、引力延迟。
* 偏差的处理也算是重点，DCB、FCB、ISB、IFB，要算多系统多频，肯定得考虑。