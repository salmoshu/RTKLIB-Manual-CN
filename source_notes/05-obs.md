# 5. 原始观测数据

## 5.1 基本原理

### 5.1.1 伪距

伪距 $\rho \left ( t \right )$ 的定义：信号接收时间 $t_u\left ( t \right )$ 与信号的发送时间 $t^{(S)}\left ( t-\tau  \right )$ 的差乘以光在真空中的速度 c，即： 
$$
\rho \left ( t \right ) =c( t_u\left ( t \right ) - t^{(S)}\left ( t-\tau  \right ) )
$$
表示接收机天线到卫星天线的距离，包含了接收机钟差、卫星钟差和其它偏差（如：大气延迟atmospheric delays）的影响，所以称之为“伪距”，伪距观测方程： 
$$
\rho =r+c\left ( \delta t_u-\delta t^{s} \right ) +cI+cT+\varepsilon _\rho
$$
其中 $r$ 表示接收机卫星间的几何距离，$\delta t_u$ 表示接收机钟差， $\delta t^{s}$ 表示卫星钟差， $I$ 表示电离层延迟， $T$ 表示对流层延迟，$\varepsilon _\rho$ 表示伪距测量噪声。$\delta t^{s}$ 、 $I$ 、 $T$ 都可通过模型消除，视为已知量，因此我们定义校正后的伪距测量值 $\rho_c$ ：
$$
\rho _c=\rho +\delta t^{(s)}-I-T
$$

### 5.1.2 时间

信号发送时间 $t^{(s)}$ 的获取：

* 接收机直接测量的不是信号的发射时间 $t^{(s)}$ 是码相位（CP），信号发送时间 $t^{(s)}$ 和伪距 $\rho$ 是在码相位测量值的基础上组装起来的。

* 码相位通过接收机内部码跟踪环路上的 C/A 码发生器和 C/A 码相关器获得的，码相位反应最新接收到C/A码在一整个周期 C/A 码中的位置，其值在 0—102 3间，且通常不是一个整数。

* 只有当接收机接收到卫星信号并进入子帧同步状态后，才能将 $t^{(s)}$ 构筑出来，构筑公式： 
    $$
    t^{(s)}=TOW +\left ( 30w+b \right )\times 0.020+\left ( c+\frac{CP}{1023}  \right )  \times 0.001
    $$
    其中 TOW 是周内秒子帧起始时间，每个字 30 比特，w 个字，b 个比特，每个比特 0.02s，c 个周期，CP 码相位，C/A 码周期 0.001s。

### 5.1.3 载波相位

**载体相位差**是实际上是对卫星信号的接收载波和接收机产生的参考频率之间的差频的测量。若能测得信号传播过程中两点的载波相位差，就可推断出两点的距离。在接收机采样时刻$t_u$ ，接收机内部复制的载波相位为$\phi_u$,接收机接收到的卫星载波信号相位为$\phi^{(s)}$，则载波相位测量值： 
$$
\phi=\phi_u-\phi^{(s)}
$$
以周为单位，一周360度，距离上对应一个载波波长。乘以波长就转化为距离： 
$$
\phi=\lambda^{-1}r+N
$$
其中， $r$ 为卫星到接收机的几何距离，$N$为整周模糊度。如果能确定$N$的值，就可反推出几何距离 $r$ 。考虑到误差可将载波相位观测方程式写为： 
$$
\phi=\lambda^{-1}(r+\delta t_u+\delta t^(s)-I+T)+N+\varepsilon_\phi
$$

### 5.1.4 多普勒频移

**多普勒频移**：一个静止不动的信号发射频率为$f$的信号，而接收机以速度$v$运行，那接收机收到的信号频率$f_r$不是信号发设频率频率$f$，而是$f+f_d$，我们将这种信号接收频率随发射源和接收机间相对运动而变化的现象称为多普勒频移，将$f_d$称为多普勒频移。这样多普勒频移$f_d$等于信号接收频率与信号发送频率的差，即： 
$$
f_d=f_r-f
$$
从电磁波传播的基本理论出发，我们可以严格推导出以下多普勒频移值的计算公式： 
$$
f_d=\frac{v}{\lambda } \cos \beta =\frac{v}{c}f\cos \beta
$$
其中，$\lambda$是信号发送频率对应的波长，$c$为光速，$\beta$为入射角。推广到移动型信号发射源：
$$
f_d=\frac{(v-v^{(s)})I^{(s)}}{\lambda } =-\frac{(v^{(s)}-v)I^{(s)}}{\lambda }=\frac{-\dot{r} }{\lambda } 
$$
对于静态信号发射源， $\beta$ 指接收机运动方向到信号入射方向的夹角。接收机朝信号方向运动时， $\beta$ 小于$90^\circ$ ， $f_d$ 大于0，相同的时间里接受到的载波周数更多。由此我们可以总结：多普勒频移反应信号发射源与接收机之间连线距离的变化快慢，与接收机运行速度在连线方向上的投影成正比。

为测量接收到的卫星信号的载波相位，接收机内部复制的不是频率始终为$f$的载波，而是通过内部的跟踪环路尽可能复制每时每刻的载波，并让复制出的载波相位与接收到卫星信号的载波一致。根据锁定的频率、相位的不同，载波跟踪环路存在**频率锁定环路**（FLL）和**相位锁定环路**（PLL）两种形式；**FLL直接输出多普勒频移测量值**，**而PLL输出的是积分多普勒测量值**。

### 5.1.5 积分多普勒

**积分多普勒**：$d\phi$是多普勒频移$f_d$对时间的积分，即： 
$$
d\phi _k=d\phi (t_k)=-\int_{t_0}^{t_k}f_d(t)dt
$$
其中， $d\phi _k$代表接收机在历元$k$时输出的积分多普勒测量值。在载波跟踪环路刚锁定或重锁定载波信号的一刻，接收机一般将积分多普勒值重置为0，也就是说

- **$k$ 时刻的积分多普勒值等于 0~k 时间的载波相位测量值的变化量**。
- 积分多普勒 $d\phi _k$ 乘以波长 $\lambda$  后的值等于这段时间内卫星与接收机间的距离变化量，所以积分多普勒也称为**积分距离差**（ADR）。
- 由式也可以看出：积分多普勒 $d\phi _k$ 对时间的导数等于多普勒频移 $f_d$ 乘以-1。

积分多普勒与多普勒频移的**区别**：
- 多普勒频移值是一个**瞬时值**，体现用户接收机在测量时刻相对于卫星的瞬时运动速度。
- 积分多普勒是一个**平均值**，两时刻间的积分多普勒反应的是这也是断用户相对于卫星的总位移，运动位移反应的是平均速度。

## 5.2 RINEX 格式

### 5.2.1 RINEX 概述

- 卫星导航定位都是通过对卫星观测以获得卫星所需的观测量来实现的，卫星发射的信号由**载波**、**测距码**、**导航电文**三部分组成，接收机通过接收处理卫星信号生成定位所需的观测量。

- RINEX 是 **Receiver Independent Exchange Format** 的缩写。采用文本文件**ASCII码**存储数据。数据记录格式与接收机的制造厂商和具体型号无关，这样可以方便数据的传递，使数据尽可能多被使用 。

- RINEX 文件分三种：**观测数据文件**（**O**bservation Data File）、**导航信息文件**（**N**avigation Message File）、**气象数据文件**（**M**eteorological Data File），本文主要介绍OBS和NAV文件的读取。

- 文件结构都以节、记录、字段和列为单位逐级组织。都由文件头和数据记录两节组成，每一节含若干记录，每条记录通常为一行，字段在行中所处的位置固定，不能错位。

- 文件头：包含整个文件的全局信息，由一行行组成，每一行由文件头标签和数据组成。

  1-60 列为数据，60-80 列为文件头标签，文件头标签是必须的，且只能是格式描述的那几种选择。

### 5.2.2 观测值类型

C：伪距、D：多普勒、L：载波相位、S：载噪比

```c
 static const char obscodes[]="CLDS";    /* observation type codes */
```

### 5.2.3 sigind_t 结构体

sigind_t：表示每种卫星系统的载波类型和观测值类型 ，每种类型的系统其实对应的就是一个sigind_t结构体，也就是说只需要建立七个结构体就够了。 

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

### 5.2.3 CODE_XXX 观测码

**CODE_XXX**：观测值类型定义，用一串连续的数字表示。

```c
#define CODE_NONE   0                   /* obs code: none or unknown */
#define CODE_L1C    1                   /* obs code: L1C/A,G1C/A,E1C (GPS,GLO,GAL,QZS,SBS) */
#define CODE_L1P    2                   /* obs code: L1P,G1P,B1P (GPS,GLO,BDS) */
......
#define CODE_L4B    67                  /* obs code: G1aL1OCd   (GLO) */
#define CODE_L4X    68                  /* obs code: G1al1OCd+p (GLO) */
#define MAXCODE     68                  /* max number of obs code */
```

### 5.2.4 码处理函数

* **code2obs**()：传入obs code (CODE_???) ，返回code string ("1C","1P","1Y",...)
* **obs2code**()：传入code string ("1C","1P","1Y",...)，返回obs code (CODE_???) 
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
* **code2idx**()：传入obs code (CODE_???) 和卫星系统(SYS_???) ，返回载波频率的下标
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
* **code2freq**()：传入obs code (CODE???))和卫星系统(SYS_???)，以及GLONASS的信道，调用code2freq_GPS()、code2freq_GLO()、code2freq_GAL()、code2freq_QZS()、code2freq_SBS()、code2freq_BDS()、code2freq_IRN()，返回对应的载波频率(Hz) 
* **sat2freq()**：传入satellite number和obs code ，返回对应的载波频率(Hz) 
* **setcodepri()**、**getcodepri()**：设置和获取信号优先级。如果输入的观测数据在同一频率内包含多个信号，RTKLIB将按照以下默认优先级选择一个信号进行处理。 
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

### 5.2.5 RINEX 辅助函数

* setstr()：去除结尾空格，传入字符串`src`，返回去除结尾空格的字符串`dst`。
* adjweek()：调整时间差到半周内，传入两个时间，算时间差，通过加减整周秒数，将时间差调整到半周内。
* adjday()：调整时间差到半天内， 传入两个时间，算时间差，通过加减整天秒数，将时间差调整到半天内。
* timestr_rnx()：获取系统 UTC 时间，字符串形式输出：(yyyymmdd hhmmss UTC) 。
* init_sta()：初始化测站信息，将结构体`sta_t`中的字段都设为 0。
* uravalue()：用户距离精度 URA 到名义值，用于计算卫星位置的时候标定方差。
* uraindex()：URA 值到 URA 下标，
* sisa_value()：Galileo卫星系统完备性SISA下标到值
* sisa_index()：Galileo卫星系统完备性SISA值到下标

## 5.3 源码解析

<img style="width: 100%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250717-014830.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图10.1-1 RINEX 相关函数调用流程</p>

### 5.3.1 readobsnav()：RINEX 文件读取主入口函数

**1. 传入参数**：

```c
gtime_t ts                  解算开始时间
gtime_t te                  解算结束时间
double ti                   解算时间间隔
char **infile               传入文件路径数组
const int *index            对应文件下标
int n                       infile[]元素个数
const prcopt_t *prcopt      处理选项
obs_t *obs                  存观测数据OBS
nav_t *nav                  存导航电文数据NAV
sta_t *sta                  测站结构体，存obs文件头读取到的一部分的信息
```

* **index[]的作用**：会传给`execses_b()`，再传给`execses_r()`，再传给`execses()`，再传给`readobsnav() `。如果不需要根据 tu 分时间段解算，index 存的就是 0~n，如果需要分时间段解算，index 存的是对应时间段内文件的下标。

**2. 执行流程**：

* 初始化对所有数据指针，赋空
* 遍历 `infile[]`，调用`readrnxt()`读取文件 ,rcv=1:流动站,2:基准站
* 如果下标和上一次循环的不同，记录当前`index[i]`值到`ind `
* 检测观测数据数`obs->n`、星历数据数`nav->n` 是否正常 
* 调用`sortobs()`,根据 time, rcv, sat ，对`obs->data`的元素进行排序、去重，得到历元数`nepoch`
* 调用`uniqnav()`,进行星历数据的排序去重

**3. 调用函数**

* **sortobs()**：根据time, rcv, sat ，对`obs->data`的元素进行排序、去重，返回历元数。
* **uniqnav()**：调用`uniqeph()`、`uniqgeph()`、`uniqseph()`进行星历数据的排序去重。
  **uniqeph()**：根据`ttr`、`toe`对`nav->eph`中的元素排序，根据`sat`和`iode`去重，根据新的星历数据`eph_t`的数量，`realloc`重新分配内存空间。

**4. 源码注释**

::: details 点击查看代码
```c
static int readobsnav(gtime_t ts, gtime_t te, double ti, char **infile,
                     const int *index, int n, const prcopt_t *prcopt,
                     obs_t *obs, nav_t *nav, sta_t *sta)
{
   int i,j,
   nd=0,
   ind=0
   nobs=0,
   rcv=1;
   
   trace(3,"readobsnav: ts=%s n=%d\n",time_str(ts,0),n);
   
   //初始化对所有数据指针
   obs->data=NULL; obs->n =obs->nmax =0;
   nav->eph =NULL; nav->n =nav->nmax =0;
   nav->geph=NULL; nav->ng=nav->ngmax=0;
   nav->seph=NULL; nav->ns=nav->nsmax=0;
   nepoch=0;
   
   //遍历infile[]，调用readrnxt()读取文件
   for (i=0;i<n;i++) {
       if (checkbrk("")) return 0;
       
       if (index[i]!=ind) {            //如果下标和上一次循环的不同
           if (obs->n>nobs) rcv++;     //rcv=1:rover,2:reference
           ind=index[i];               //记录当前index[i]值到ind
           nobs=obs->n;                //
       }
       /* read rinex obs and nav file */
       //最后一个参数为地址sta_t *sta：如果rcv=0，传null；rcv不为0，根据rcv值，rcv=1就传sta的地址，rcv=1就传sta+1
       if (readrnxt(infile[i],rcv,ts,te,ti,prcopt->rnxopt[rcv<=1?0:1],obs,nav,
                    rcv<=2?sta+rcv-1:NULL)<0) {
           checkbrk("error : insufficient memory");
           trace(1,"insufficient memory\n");
           return 0;
       }
   }

   //检测观测数是否正常
   if (obs->n<=0) {
       checkbrk("error : no obs data");
       trace(1,"\n");
       return 0;
   }
   //检测星历数是否正常
   if (nav->n<=0&&nav->ng<=0&&nav->ns<=0) {
       checkbrk("error : no nav data");
       trace(1,"\n");
       return 0;
   }

   //调用sortobs(),根据time, rcv, sat ，对obs->data的元素进行排序、去重，得到历元数nepoch
   /* sort observation data */ 
   nepoch=sortobs(obs);
   
   //调用uniqnav(),进行星历数据的排序去重。
   /* delete duplicated ephemeris */
   uniqnav(nav);
   
   /* set time span for progress display */
   if (ts.time==0||te.time==0) {
       for (i=0;   i<obs->n;i++) if (obs->data[i].rcv==1) break;
       for (j=obs->n-1;j>=0;j--) if (obs->data[j].rcv==1) break;
       if (i<j) {
           if (ts.time==0) ts=obs->data[i].time;
           if (te.time==0) te=obs->data[j].time;
           settspan(ts,te);
       }
   }
   return 1;
}
```
:::

### 5.3.2 readrnxt()：处理文件路径、赋值测站名**

**1. 执行流程**

* 如果传入 `file` 为空，调用 `readrnxfp()` 从标准输入读取 
* 展开 `file` 路径中的通配符`*`到 `files[] `
* 调用 `readrnxfile`，循环解压读取 `files[]` 
* 如果测站名字为空，就给依据头文件自动赋 4 个字符的名字 

**2. 源码注释**

::: details 点击查看代码
```c
extern int readrnxt(const char *file, int rcv, gtime_t ts, gtime_t te,
                    double tint, const char *opt, obs_t *obs, nav_t *nav,
                    sta_t *sta)
{
    int i,n,stat=0;
    const char *p;
    char type=' ',*files[MAXEXFILE]={0};
    

    trace(3,"readrnxt: file=%s rcv=%d\n",file,rcv);
    
    if (!*file) {   //如果传入file为空，调用readrnxfp()从标准输入读取？
        return readrnxfp(stdin,ts,te,tint,opt,0,1,&type,obs,nav,sta);
    }
    for (i=0;i<MAXEXFILE;i++) {     //为files[]开辟空间
        if (!(files[i]=(char *)malloc(1024))) {
            for (i--;i>=0;i--) free(files[i]);
            return -1;
        }
    }
    /* expand wild-card */  //展开file路径中的通配符*到files[]
    if ((n=expath(file,files,MAXEXFILE))<=0) {
        for (i=0;i<MAXEXFILE;i++) free(files[i]);
        return 0;
    }
    /* read rinex files */  //调用readrnxfile，循环解压读取files[]
    for (i=0;i<n&&stat>=0;i++) {
        stat=readrnxfile(files[i],ts,te,tint,opt,0,rcv,&type,obs,nav,sta);
    }
    /* if station name empty, set 4-char name from file head */ //如果测站名字为空，就给依据头文件自动赋4个字符的名字
    if (type=='O'&&sta) {
        if (!(p=strrchr(file,FILEPATHSEP))) p=file-1;
        if (!*sta->name) setstr(sta->name,p+1,4);
    }
    for (i=0;i<MAXEXFILE;i++) free(files[i]);
    
    return stat;
}
```
:::

### 5.3.3 readrnxfile()：解压、打开文件**

**1. 执行流程**

* 调用 `rtk_uncompress()` 解压文件`file`到`tmpfile `,如果不需要解压`cstat`值会为0，后面`fopen`会根据` cstat` 的值决定要读取的文件
* 以读的方式打开解压后的文件 
* 调用 `readrnxfp()` ，从文件描述符fp中读取文件 
* 删除 `tmpfile` 

**2. 源码注释**

::: details 点击查看代码
```c
static int readrnxfile(const char *file, gtime_t ts, gtime_t te, double tint,
                       const char *opt, int flag, int index, char *type,
                       obs_t *obs, nav_t *nav, sta_t *sta)
{
    FILE *fp;
    int cstat,stat;
    char tmpfile[1024];
    
    trace(3,"readrnxfile: file=%s flag=%d index=%d\n",file,flag,index);
    
    if (sta) init_sta(sta);
    
    /* uncompress file */   //解压文件file到tmpfile
    if ((cstat=rtk_uncompress(file,tmpfile))<0) {
        trace(2,"rinex file uncompact error: %s\n",file);
        return 0;
    }
    //以读的方式打开解压后的文件
    if (!(fp=fopen(cstat?tmpfile:file,"r"))) {
        trace(2,"rinex file open error: %s\n",cstat?tmpfile:file);
        return 0;
    }
    //调用readrnxfp，从文件描述符fp中读取文件
    /* read RINEX file */
    stat=readrnxfp(fp,ts,te,tint,opt,flag,index,type,obs,nav,sta);
    
    fclose(fp);
    
    /* delete temporary file */ //删除tmpfile
    if (cstat) remove(tmpfile); 
    
    return stat;
}
```
:::

### 5.3.4 readrnxfp()：根据文件类型调用对应的读取函数**

**1. 执行流程**

* 调用`readrnxh()`读取头文件。并获取文件类型`type`
* 根据type调用对应的函数进行分类读取，`readrnxobs()`读OBS文件，`readrnxnav()`读NAV文件，调用`readrnxnav()`读clock文件。

**2. 源码注释**

::: details 点击查看代码
```c
static int readrnxfp(FILE *fp, gtime_t ts, gtime_t te, double tint,
                     const char *opt, int flag, int index, char *type,
                     obs_t *obs, nav_t *nav, sta_t *sta)
{
    double ver;
    int sys,tsys=TSYS_GPS;
    char tobs[NUMSYS][MAXOBSTYPE][4]={{""}};
    
    trace(3,"readrnxfp: flag=%d index=%d\n",flag,index);
    

    /* read RINEX file header */    //读取rinex头文件
    if (!readrnxh(fp,&ver,type,&sys,&tsys,tobs,nav,sta)) return 0;
    
    //type:通过readrnxh()读取文件头第一行获得
    /* flag=0:except for clock,1:clock */
    if ((!flag&&*type=='C')||(flag&&*type!='C')) return 0;
    
    /* read RINEX file body */  //读取观测文件体
    switch (*type) {    //通过判断头文件识别的type进行分类读取
        case 'O': return readrnxobs(fp,ts,te,tint,opt,index,ver,&tsys,tobs,obs,
                                    sta);
        case 'N': return readrnxnav(fp,opt,ver,sys    ,nav);
        case 'G': return readrnxnav(fp,opt,ver,SYS_GLO,nav);
        case 'H': return readrnxnav(fp,opt,ver,SYS_SBS,nav);
        case 'J': return readrnxnav(fp,opt,ver,SYS_QZS,nav); /* extension */
        case 'L': return readrnxnav(fp,opt,ver,SYS_GAL,nav); /* extension */
        case 'C': return readrnxclk(fp,opt,index,nav);
    }
    trace(2,"unsupported rinex type ver=%.2f type=%c\n",ver,*type);
    return 0;
}
```
:::

### 5.3.5 readrnxh()：读取文件头**

**1. 执行流程**

* 函数的主体在一个while大循环中，循环读取每一行，直到出现"END OF HEADER" 

* 首先进行第一行版本号读取，记录版本号以及观测文件类型。
* PGM / RUN BY / DATE" 、END OF HEADER 直接跳过不读取。
* 通过判断文件类型分配不同函数读取文件头，decode_obsh()、decode_navh()、decode_gnavh()、decode_hnavh()、decode_navh() ，

  ![](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/22c34202b8934f0c97a3a9b5dd8fb2b1.png)

::: details 点击查看代码
```c
static int readrnxh(FILE *fp, double *ver, char *type, int *sys, int *tsys,
                    char tobs[][MAXOBSTYPE][4], nav_t *nav, sta_t *sta)
{
    char buff[MAXRNXLEN],*label=buff+60;
    int i=0;
    
    trace(3,"readrnxh:\n");
    
    *ver=2.10; *type=' '; *sys=SYS_GPS;
    
    while (fgets(buff,MAXRNXLEN,fp)) {
        //判定观测文件头部分所有字符总长度是否正常
        if (strlen(buff)<=60) {
            continue;
        }
        //首先进行第一行版本号读取，记录版本号以及观测文件类型
        else if (strstr(label,"RINEX VERSION / TYPE")) {
            *ver=str2num(buff,0,9);
            *type=*(buff+20);   
            //通过定位字符位置读取字符，判断系统类型并记录
            /* satellite system */
            switch (*(buff+40)) {
                case ' ':
                case 'G': *sys=SYS_GPS;  *tsys=TSYS_GPS; break;
                case 'R': *sys=SYS_GLO;  *tsys=TSYS_UTC; break;
                case 'E': *sys=SYS_GAL;  *tsys=TSYS_GAL; break; /* v.2.12 */
                case 'S': *sys=SYS_SBS;  *tsys=TSYS_GPS; break;
                case 'J': *sys=SYS_QZS;  *tsys=TSYS_QZS; break; /* v.3.02 */
                case 'C': *sys=SYS_CMP;  *tsys=TSYS_CMP; break; /* v.2.12 */
                case 'I': *sys=SYS_IRN;  *tsys=TSYS_IRN; break; /* v.3.03 */
                case 'M': *sys=SYS_NONE; *tsys=TSYS_GPS; break; /* mixed */
                default :
                    trace(2,"not supported satellite system: %c\n",*(buff+40));
                    break;
            }
            continue;
        }
        else if (strstr(label,"PGM / RUN BY / DATE")) {
            continue;
        }
        else if (strstr(label,"COMMENT")) {
            continue;
        }
        //通过判断文件类型分配不同函数读取文件头
        switch (*type) { /* file type */
            case 'O': decode_obsh(fp,buff,*ver,tsys,tobs,nav,sta); break;
            case 'N': decode_navh (buff,nav); break;
            case 'G': decode_gnavh(buff,nav); break;
            case 'H': decode_hnavh(buff,nav); break;
            case 'J': decode_navh (buff,nav); break; /* extension */
            case 'L': decode_navh (buff,nav); break; /* extension */
        }
        if (strstr(label,"END OF HEADER")) return 1;
        
        if (++i>=MAXPOSHEAD&&*type==' ') break; /* no RINEX file */
    }
    return 0;
}
```
:::

### 5.3.6 decode_obsh()：解析观测数据文件头

decode_obsh()、readrnxobs()->readrnxobsb()->decode_obsepoch()、decode_obsdata()

**1. 执行流程**

最关键的是解析观测值类型如下图，存到tobs三维数组中，【星座类型】【观测类型】【字符串数4】 

![在这里插入图片描述](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/9a77a8b017d54244a016a9debac3193a.png)

> 核心代码：先读取开头的卫星系统`i`，再读该卫星系统的观测值类型数`n`，之后循环读取n个观测值类型存到`tobs[i][nt++]`中，一行读完观测值类型数不到`n`就再`fgets()`读下一行
>
> ```c
> else if (strstr(label,"SYS / # / OBS TYPES" )) { /* ver.3 */
>   //如果卫星系统不在static const char syscodes[]="GREJSCI"; 中输出错误消息
>   if (!(p=strchr(syscodes,buff[0]))) {
>       trace(2,"invalid system code: sys=%c\n",buff[0]);
>       return;
>   }   
>   i=(int)(p-syscodes);        //i为系统在syscodes[]="GREJSCI"的下标
>   n=(int)str2num(buff,3,3);   //一个系统下的观测值类型数量
>   for (j=nt=0,k=7;j<n;j++,k+=4) { //读取具体观测值类型，每行第7位开始，每次读4位
>       if (k>58) {     //读完一行
>           if (!fgets(buff,MAXRNXLEN,fp)) break;   //再读取一行
>           k=7;
>       }
>       if (nt<MAXOBSTYPE-1) setstr(tobs[i][nt++],buff+k,3);
>   }
>   *tobs[i][nt]='\0';  //存储观测类型，分星座类型用三维数组进行存储【星座类型】【观测类型】【字符串数4】
> ```

**2. 源码注释**

::: details 点击查看代码
```c
static void decode_obsh(FILE *fp, char *buff, double ver, int *tsys,
                        char tobs[][MAXOBSTYPE][4], nav_t *nav, sta_t *sta)
{
    /* default codes for unknown code */
    const char frqcodes[]="1256789";    //定义频率数组
    const char *defcodes[]={    //定义一个指针数组，不知道的编码直接采取默认字符串加空格的形式存储
        "CWX    ",  /* GPS: L125____ */
        "CCXX X ",  /* GLO: L1234_6_ */
        "C XXXX ",  /* GAL: L1_5678_ */
        "CXXX   ",  /* QZS: L1256___ */
        "C X    ",  /* SBS: L1_5____ */
        "XIXIIX ",  /* BDS: L125678_ */
        "  A   A"   /* IRN: L__5___9 */
    };
    double del[3];
    int i,j,k,n,nt,prn,fcn;
    const char *p;
    char *label=buff+60,    //读文件头标签，从60开始
            str[4];
    
    trace(4,"decode_obsh: ver=%.2f\n",ver);
    
    if      (strstr(label,"MARKER NAME"         )) {
        if (sta) setstr(sta->name,buff,60);     //测站名，存到测站结构体sta_t
    }
    else if (strstr(label,"MARKER NUMBER"       )) { /* opt */
        if (sta) setstr(sta->marker,buff,20);
    }
    else if (strstr(label,"MARKER TYPE"         )) ; /* ver.3 */
    else if (strstr(label,"OBSERVER / AGENCY"   )) ;    //有很多else if后不做处理，空语句
    else if (strstr(label,"REC # / TYPE / VERS" )) {
        if (sta) {  //在文件中各个信息占20个字符，因此每20赋值一次
            setstr(sta->recsno, buff,   20);
            setstr(sta->rectype,buff+20,20);
            setstr(sta->recver, buff+40,20);
        }
    }   
    else if (strstr(label,"ANT # / TYPE"        )) {
        if (sta) {
            setstr(sta->antsno,buff   ,20);
            setstr(sta->antdes,buff+20,20);
        }
    }
    else if (strstr(label,"APPROX POSITION XYZ" )) {
        if (sta) {  //这里的循环语句与上面类同，每14字符赋值一次
            for (i=0,j=0;i<3;i++,j+=14) sta->pos[i]=str2num(buff,j,14);
        }
    }
    else if (strstr(label,"ANTENNA: DELTA H/E/N")) {    //记录天线各方向延迟
        if (sta) {
            for (i=0,j=0;i<3;i++,j+=14) del[i]=str2num(buff,j,14);
            sta->del[2]=del[0]; /* h */
            sta->del[0]=del[1]; /* e */
            sta->del[1]=del[2]; /* n */
        }
    }
    else if (strstr(label,"ANTENNA: DELTA X/Y/Z")) ; /* opt ver.3 */
    else if (strstr(label,"ANTENNA: PHASECENTER")) ; /* opt ver.3 */
    else if (strstr(label,"ANTENNA: B.SIGHT XYZ")) ; /* opt ver.3 */
    else if (strstr(label,"ANTENNA: ZERODIR AZI")) ; /* opt ver.3 */
    else if (strstr(label,"ANTENNA: ZERODIR XYZ")) ; /* opt ver.3 */
    else if (strstr(label,"CENTER OF MASS: XYZ" )) ; /* opt ver.3 */

    //观测数据类型读取
    else if (strstr(label,"SYS / # / OBS TYPES" )) { /* ver.3 */
        //如果卫星系统不在static const char syscodes[]="GREJSCI"; 中输出错误消息
        if (!(p=strchr(syscodes,buff[0]))) {
            trace(2,"invalid system code: sys=%c\n",buff[0]);
            return;
        }   
        i=(int)(p-syscodes);        //i为系统在syscodes[]="GREJSCI"的下标
        n=(int)str2num(buff,3,3);   //一个系统下的观测值类型数量
        for (j=nt=0,k=7;j<n;j++,k+=4) { //读取具体观测值类型，每行第7位开始，每次读4位
            if (k>58) {     //读完一行
                if (!fgets(buff,MAXRNXLEN,fp)) break;   //再读取一行
                k=7;
            }
            if (nt<MAXOBSTYPE-1) setstr(tobs[i][nt++],buff+k,3);
        }
        *tobs[i][nt]='\0';  //存储观测类型，分星座类型用三维数组进行存储【星座类型】【观测类型】【字符串数4】
        
        //北斗数据类型转换
        /* change BDS B1 code: 3.02 */
        if (i==5&&fabs(ver-3.02)<1e-3) {
            for (j=0;j<nt;j++) if (tobs[i][j][1]=='1') tobs[i][j][1]='2';
        }
        /* if unknown code in ver.3, set default code */
        for (j=0;j<nt;j++) {
            if (tobs[i][j][2]) continue;
            if (!(p=strchr(frqcodes,tobs[i][j][1]))) continue;
            tobs[i][j][2]=defcodes[i][(int)(p-frqcodes)];
            trace(2,"set default for unknown code: sys=%c code=%s\n",buff[0],
                  tobs[i][j]);  //如果没有该观测类型，就进行错误提示
        }
    }
    else if (strstr(label,"WAVELENGTH FACT L1/2")) ; /* opt ver.2 */
    
    //rinex2版本的观测类型存储
    else if (strstr(label,"# / TYPES OF OBSERV" )) { /* ver.2 */
        n=(int)str2num(buff,0,6);
        for (i=nt=0,j=10;i<n;i++,j+=6) {
            if (j>58) {
                if (!fgets(buff,MAXRNXLEN,fp)) break;
                j=10;
            }
            if (nt>=MAXOBSTYPE-1) continue;
            //将rnx3以下的版本的观测类型符号转化为rnx3的观测类型符号
            if (ver<=2.99) {
                setstr(str,buff+j,2);
                convcode(ver,SYS_GPS,str,tobs[0][nt]);
                convcode(ver,SYS_GLO,str,tobs[1][nt]);
                convcode(ver,SYS_GAL,str,tobs[2][nt]);
                convcode(ver,SYS_QZS,str,tobs[3][nt]);
                convcode(ver,SYS_SBS,str,tobs[4][nt]);
                convcode(ver,SYS_CMP,str,tobs[5][nt]);
            }
            nt++;
        }
        *tobs[0][nt]='\0';
    }
    else if (strstr(label,"SIGNAL STRENGTH UNIT")) ; /* opt ver.3 */
    else if (strstr(label,"INTERVAL"            )) ; /* opt */
    //根据历元开始时间第49-51列判断星座类型
    else if (strstr(label,"TIME OF FIRST OBS"   )) {
        if      (!strncmp(buff+48,"GPS",3)) *tsys=TSYS_GPS;
        else if (!strncmp(buff+48,"GLO",3)) *tsys=TSYS_UTC;
        else if (!strncmp(buff+48,"GAL",3)) *tsys=TSYS_GAL;
        else if (!strncmp(buff+48,"QZS",3)) *tsys=TSYS_QZS; /* ver.3.02 */
        else if (!strncmp(buff+48,"BDT",3)) *tsys=TSYS_CMP; /* ver.3.02 */
        else if (!strncmp(buff+48,"IRN",3)) *tsys=TSYS_IRN; /* ver.3.03 */
    }
    else if (strstr(label,"TIME OF LAST OBS"    )) ; /* opt */
    else if (strstr(label,"RCV CLOCK OFFS APPL" )) ; /* opt */
    else if (strstr(label,"SYS / DCBS APPLIED"  )) ; /* opt ver.3 */
    else if (strstr(label,"SYS / PCVS APPLIED"  )) ; /* opt ver.3 */
    else if (strstr(label,"SYS / SCALE FACTOR"  )) ; /* opt ver.3 */
    else if (strstr(label,"SYS / PHASE SHIFTS"  )) ; /* ver.3.01 */
    else if (strstr(label,"GLONASS SLOT / FRQ #")) { /* ver.3.02 */
        for (i=0;i<8;i++) {
            if (buff[4+i*7]!='R') continue;
            prn=(int)str2num(buff,5+i*7,2);
            fcn=(int)str2num(buff,8+i*7,2);
            if (prn<1||prn>MAXPRNGLO||fcn<-7||fcn>6) continue;
            if (nav) nav->glo_fcn[prn-1]=fcn+8;
        }
    }
    else if (strstr(label,"GLONASS COD/PHS/BIS" )) { /* ver.3.02 */
        if (sta) {
            sta->glo_cp_bias[0]=str2num(buff, 5,8);
            sta->glo_cp_bias[1]=str2num(buff,18,8);
            sta->glo_cp_bias[2]=str2num(buff,31,8);
            sta->glo_cp_bias[3]=str2num(buff,44,8);
        }
    }

    //记录跳秒，GLONASS观测文件中独有
    else if (strstr(label,"LEAP SECONDS"        )) { /* opt */
        if (nav) {
            nav->utc_gps[4]=str2num(buff, 0,6);
            nav->utc_gps[7]=str2num(buff, 6,6);
            nav->utc_gps[5]=str2num(buff,12,6);
            nav->utc_gps[6]=str2num(buff,18,6);
        }
    }
    else if (strstr(label,"# OF SALTELLITES"    )) { /* opt */
        /* skip */ ;
    }
    else if (strstr(label,"PRN / # OF OBS"      )) { /* opt */
        /* skip */ ;
    }
}
```
:::

### 5.3.7 readrnxobs()：读取o文件中全部观测值数据

重复调用`readrnxobsb()`函数，直到所有的观测值全被读完，或者是出现了某个历元没有卫星的情况为止 

**1. 传入参数**：

```c
FILE *fp                    I   传入的RINEX文件指针
gtime_t ts                  I   开始时间
gtime_t te                  I   结束时间
double tint                 I   时间间隔
const char *opt             I   选项
int rcv                     I   接收机号
double ver                  I   RINEX文件版本
int *tsys                   I   时间系统
char tobs[][MAXOBSTYPE][4]  I   观测值类型数组
obs_t *obs                  O   obsd_t类型的观测值数组
sta_t *sta                  O   卫星数组
```

**2. 执行流程**

* 为`data[]` 开辟空间
* while大循环调用`readrnxobsb()`每次读取一个历元的观测数据，获取观测值数n
* 遍历`data[]`，如果时间系统为UTC，转为GPST，调用`saveslips()`
* 调用`screent()`，判断传入的时间是否符合起始时间 ts，结束时间 te，时间间隔 tint
* 遍历`data[]` ，调用`restslips()`，`addobsdata()`将`data[]`信息存到`obs`中 

**3. 调用函数**
* **saveslips()**：`data->LLI[i]` 到 `slips[]`
* **restslips()**：`slips[] `到`data->LLI[i]`
* **screent()**：判断传入的时间time是否符合起始时间ts，结束时间te，时间间隔tint，符合返回1，不符合返回0 

### 5.3.8 readrnxobsb()：读取一个观测历元的观测数据

**1. 参数列表**

```c
FILE *fp    I               I   传入的RINEX文件指针
const char *opt             I   选项
double ver                  I   RINEX文件版本
int *tsys                   I   时间系统
char tobs[][MAXOBSTYPE][4]  I   观测值类型数组
int *flag                   I   历元信息状态
obsd_t *data                O   obsd_t类型的观测值数组
sta_t *sta                  O   卫星数组
```

**2. 执行流程**

* 调用`set_sysmask()`获取卫星系统掩码mask，mask在之后`decode_obsdata()`中会用到，mask中没有的卫星系统不用。
* 调用set_index()，将将tobs数组中存的观测值类型信息存到sigind_t类型的index[]结构体数组中，此时传入的tobs数组是二维数组，每个传入的tobs都存了一个卫星系统的观测值类型，同理index[]的一个元素就存一个卫星系统的所有观测值类型。
* while大循环，fgets()存一行的数据
  * 如果是第一行，则调用`decode_obsepoch()`函数解码首行数据（包括历元时刻、卫星数、卫星编号、历元状态等信息），并将信息保存 ，获取的卫星数量nsat是判断循环次数的关键。
  * 如果不是第一行则调用`decode_obsdata()`函数对该行观测数据进行数据解码，读取一个历元内一颗卫星的观测值 ，到data[n]
  * 知道读取数量 i 等于`decode_obsepoch()`获取的卫星数量`nsat`，结束循环，返回读取的观测值数（卫星数）

**3. 调用函数**

* **set_index()**：将`tobs`数组中存的观测值类型信息存到`sigind_t`类型的`ind`结构体中

**4. 源码注释**

::: details 点击查看代码
```c
static int readrnxobsb(FILE *fp, const char *opt, double ver, int *tsys,
                      char tobs[][MAXOBSTYPE][4], int *flag, obsd_t *data,
                      sta_t *sta)
{
   gtime_t time={0};
   sigind_t index[NUMSYS]={{0}};
   char buff[MAXRNXLEN];
   int i=0,n=0,nsat=0,nsys=NUMSYS,sats[MAXOBS]={0},mask;
   
   /* set system mask */
   mask=set_sysmask(opt);

   //建立索引。将三维观测值类型数组退化成二维数组，建立一个索引数组
   //通过判断nsys值对set_index进行传参，然后记录在sigind_t结构体中
   /* set signal index */
   if (nsys>=1) set_index(ver,SYS_GPS,opt,tobs[0],index  );
   if (nsys>=2) set_index(ver,SYS_GLO,opt,tobs[1],index+1);
   if (nsys>=3) set_index(ver,SYS_GAL,opt,tobs[2],index+2);
   if (nsys>=4) set_index(ver,SYS_QZS,opt,tobs[3],index+3);
   if (nsys>=5) set_index(ver,SYS_SBS,opt,tobs[4],index+4);
   if (nsys>=6) set_index(ver,SYS_CMP,opt,tobs[5],index+5);
   if (nsys>=7) set_index(ver,SYS_IRN,opt,tobs[6],index+6);

   /* read record */
   while (fgets(buff,MAXRNXLEN,fp)) {  //利用fgets()函数缓存一行数据
       //记录一个观测历元的有效性、时间和卫星数
       /* decode observation epoch */
       
       //如果是第一行，则调用decode_obsepoch()函数解码首行数据（包括历元时刻、卫星数、卫星编号、历元状态等信息），并将信息保存
       if (i==0) {     
           if ((nsat=decode_obsepoch(fp,buff,ver,&time,flag,sats))<=0) {
               continue;
           }
       }
      
       //如果不是第一行则调用decode_obsdata()函数对该行观测数据进行数据解码
       else if ((*flag<=2||*flag==6)&&n<MAXOBS) {
           data[n].time=time;
           data[n].sat=(uint8_t)sats[i-1];
           //记录历元的数据部分
           /* decode RINEX observation data */
           if (decode_obsdata(fp,buff,ver,mask,index,data+n)) n++;
       }
       else if (*flag==3||*flag==4) { /* new site or header info follows */
           //读取文件头，例如可以将不同系统的星历放在一个文件当中当作混合星历来使用
           /* decode RINEX observation data file header */
           decode_obsh(fp,buff,ver,tsys,tobs,NULL,sta);
       }
       if (++i>nsat) return n;
   }
   return -1;
}
```
:::

### 5.3.9 decode_obsepoch()：解码历元首行数据

**1. 参数列表**：

```c
FILE *fp        I   传入的RINEX文件指针
char *buff      I   fgets()读取到一行数据的首地址
double ver      I   RINEX文件版本
gtime_t *time   O   历元时间
int *flag       O   epoh flag (o:ok,3:new site,4:header info,5:external event)
int *sats       O   历元卫星信息，2版本才有
```

**2. 执行流程**：

* 2版本：
  * 读取卫星数到 `n` 
  * 读取`epoh flag` 到 `flag`
  * 读取历元时间 `time`
  * 循环读取卫星ID(G10、G32、G26) ，读到68列，还没把卫星读完，就fgets()读取新的一行 
  * 将卫星ID转为`satellite number`，存到`sats[]`数组中 
* 3版本：
  * 读取卫星数量 `n`
  * 读取`epoh flag` 到 `flag`
  * 读取历元时间 `time`

**3. 源码注释**

::: details 点击查看代码
```c
static int decode_obsepoch(FILE *fp, char *buff, double ver, gtime_t *time,
                          int *flag, int *sats)
{
   int i,j,n;
   char satid[8]="";
   
   trace(4,"decode_obsepoch: ver=%.2f\n",ver);
   
   if (ver<=2.99) { /* ver.2 */
       if ((n=(int)str2num(buff,29,3))<=0) return 0;   //读取卫星数到n
       
       /* epoch flag: 3:new site,4:header info,5:external event */
       *flag=(int)str2num(buff,28,1);  //读取epoh flag
       
       if (3<=*flag&&*flag<=5) return n;
       
       if (str2time(buff,0,26,time)) {     //读取历元时间
           trace(2,"rinex obs invalid epoch: epoch=%26.26s\n",buff);
           return 0;
       }
       for (i=0,j=32;i<n;i++,j+=3) {   //循环读取卫星ID(G10、G32、G26)
           if (j>=68) {                //读到68列，还没把卫星读完，就fgets()读取新的一行
               if (!fgets(buff,MAXRNXLEN,fp)) break;
               j=32;
           }
           if (i<MAXOBS) {
               strncpy(satid,buff+j,3);
               sats[i]=satid2no(satid);    //将卫星ID转为satellite number，存到sats[]数组中
           }
       }
   }
   else { /* ver.3 */
       if ((n=(int)str2num(buff,32,3))<=0) return 0;   //读取卫星数量
       
       *flag=(int)str2num(buff,31,1);      //读取epoh flag
       
       if (3<=*flag&&*flag<=5) return n;

       //识别历元第一个字符是否匹配以及历元时间是否可以正常转换
       if (buff[0]!='>'||str2time(buff,1,28,time)) {   //读取历元时间，
           trace(2,"rinex obs invalid epoch: epoch=%29.29s\n",buff);
           return 0;
       }
   }
   trace(4,"decode_obsepoch: time=%s flag=%d\n",time_str(*time,3),*flag);
   return n;
}
```
:::

### 5.3.10 decode_obsdata()：读取一个历元内一颗卫星的观测值

**1. 传入参数**：

```c
FILE *fp        I   传入的RINEX文件指针
char *buff      I   fgets()读取到一行数据的首地址
double ver      I   RINEX文件版本
int mask        I   卫星系统掩码
sigind_t *index I   观测数据类型索引
obsd_t *obs     O   观测数据OBS   
```

**2. 执行流程**：

* 3版本，读取卫星`satellite number`存到`obs->sat `
* 星系统和`mask`做与运算，判断卫星系统是否启用 
* 根据卫星系统分配索引 `ind` 
* 根据索引`ind`中的观测值类型，循环读取观测值，读取一个历元内，一颗卫星的观测值，记录有效的观测值到 `val[i]` ，记录记录信号失锁到 `lli[i]`
* 初始化`obs`各观测值数组，赋空 
* 遍历观测值类型，同频率的观测码，下标分别存到`k[]`，`l[]`中 ，`p[]` 存频率索引，后面`obs->P[0]`就是利用L1载波观测到的伪距，`obs->P[1]`就是利用L2载波观测到的伪距 
* 同一个频率有不同的观测码，取优先级高的 
* 根据索引`ind`中的观测值类型，遍历观测值，`val[i]`、`lli[i]`存入`obs`中 

**3. 源码注释**

::: details 点击查看代码
```c
static int decode_obsdata(FILE *fp, char *buff, double ver, int mask,
                         sigind_t *index, obsd_t *obs)
{
   sigind_t *ind;
   double val[MAXOBSTYPE]={0};
   uint8_t lli[MAXOBSTYPE]={0};
   char satid[8]="";
   int i,j,n,m,stat=1,
       p[MAXOBSTYPE],
       k[16],
       l[16];
   
   trace(4,"decode_obsdata: ver=%.2f\n",ver);
   
   if (ver>2.99) { /* ver.3 */ 
       sprintf(satid,"%.3s",buff); 
       obs->sat=(uint8_t)satid2no(satid);  //3版本，读取卫星satellite number存到obs->sat
   }
   if (!obs->sat) {
       trace(4,"decode_obsdata: unsupported sat sat=%s\n",satid);
       stat=0;
   }
   else if (!(satsys(obs->sat,NULL)&mask)) {   //卫星系统和mask做与运算，判断卫星系统是否启用
       stat=0;
   }

   /* read observation data fields */  //根据卫星系统分配索引
   switch (satsys(obs->sat,NULL)) {
       case SYS_GLO: ind=index+1; break;
       case SYS_GAL: ind=index+2; break;
       case SYS_QZS: ind=index+3; break;
       case SYS_SBS: ind=index+4; break;
       case SYS_CMP: ind=index+5; break;
       case SYS_IRN: ind=index+6; break;
       default:      ind=index  ; break;
   }

   //根据索引ind中的观测值类型，循环读取观测值，读取一个历元内，一颗卫星的观测值
   //2版本从0开始，3版本从3开始，一次读取16个字符(每一个卫星的观测数据）
   for (i=0,j=ver<=2.99?0:3;i<ind->n;i++,j+=16) {
       
       if (ver<=2.99&&j>=80) { /* ver.2 */     //2版本，一行读不完就fgets读下一行
           if (!fgets(buff,MAXRNXLEN,fp)) break;
           j=0;
       }
       if (stat) {
           val[i]=str2num(buff,j,14)+ind->shift[i];    //记录有效的观测值
           lli[i]=(uint8_t)str2num(buff,j+14,1)&3;     //记录信号失锁，判定周跳？
       }
   }
   if (!stat) return 0;
   
   //初始化obs各观测值数组，赋空
   for (i=0;i<NFREQ+NEXOBS;i++) {  
       obs->P[i]=obs->L[i]=0.0; obs->D[i]=0.0f;
       obs->SNR[i]=obs->LLI[i]=obs->code[i]=0;
   }

   //遍历观测值类型，同频率的观测码，下标分别存到k[]，l[]中
   /* assign position in observation data */
   for (i=n=m=0;i<ind->n;i++) {
       
       p[i]=(ver<=2.11)?ind->idx[i]:ind->pos[i];
       
       if (ind->type[i]==0&&p[i]==0) k[n++]=i; /* C1? index */
       if (ind->type[i]==0&&p[i]==1) l[m++]=i; /* C2? index */
   }

   if (ver<=2.11) {
       
       //同一个频率有不同的观测码，取优先级高的
       /* if multiple codes (C1/P1,C2/P2), select higher priority */
       if (n>=2) {
           if (val[k[0]]==0.0&&val[k[1]]==0.0) {
               p[k[0]]=-1; p[k[1]]=-1;
           }
           else if (val[k[0]]!=0.0&&val[k[1]]==0.0) {
               p[k[0]]=0; p[k[1]]=-1;
           }
           else if (val[k[0]]==0.0&&val[k[1]]!=0.0) {
               p[k[0]]=-1; p[k[1]]=0;
           }
           else if (ind->pri[k[1]]>ind->pri[k[0]]) {
               p[k[1]]=0; p[k[0]]=NEXOBS<1?-1:NFREQ;
           }
           else {
               p[k[0]]=0; p[k[1]]=NEXOBS<1?-1:NFREQ;
           }
       }
       if (m>=2) {
           if (val[l[0]]==0.0&&val[l[1]]==0.0) {
               p[l[0]]=-1; p[l[1]]=-1;
           }
           else if (val[l[0]]!=0.0&&val[l[1]]==0.0) {
               p[l[0]]=1; p[l[1]]=-1;
           }
           else if (val[l[0]]==0.0&&val[l[1]]!=0.0) {
               p[l[0]]=-1; p[l[1]]=1; 
           }
           else if (ind->pri[l[1]]>ind->pri[l[0]]) {
               p[l[1]]=1; p[l[0]]=NEXOBS<2?-1:NFREQ+1;
           }
           else {
               p[l[0]]=1; p[l[1]]=NEXOBS<2?-1:NFREQ+1;
           }
       }
   }
   // obs->P 代表着这个观测值结构体中的伪距观测值。不管是伪距观测值还是载波相位观测值和多普勒观测值，都是利用各种载波得到的
   //obs->P[0]就是利用L1载波观测到的伪距，obs->P[1]就是利用L2载波观测到的伪距…
   /* save observation data */
   //保存数据部分，每一个观测类型的组成包括：
   //观测值（保留三位小数） + LLI + 信号强度，所以obs指向的三个可能代表的就是这三个
   //遍历观测值，存入obs中
   for (i=0;i<ind->n;i++) {
       if (p[i]<0||val[i]==0.0) continue;
       switch (ind->type[i]) {
           case 0: obs->P[p[i]]=val[i]; obs->code[p[i]]=ind->code[i]; break;
           case 1: obs->L[p[i]]=val[i]; obs->LLI [p[i]]=lli[i];    break;
           case 2: obs->D[p[i]]=(float)val[i];                     break;
           case 3: obs->SNR[p[i]]=(uint16_t)(val[i]/SNR_UNIT+0.5); break;
       }
   }
   trace(4,"decode_obsdata: time=%s sat=%2d\n",time_str(obs->time,0),obs->sat);
   return 1;
}
```
:::

<GiscusTalk />
