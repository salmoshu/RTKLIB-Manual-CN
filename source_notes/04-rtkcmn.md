# 4. 基础工具库

本篇介绍将 RTKLIB 最基础的知识，主要包括时间系统、坐标系统、卫星系统、卫星、观测值定义和矩阵运算相关的内容。如果将 RTKLIB 作为第三方库，这些东西将会被频繁使用。除此之外，还可以在 RTKLIB-Manual-CN 的 [E.1](/algorithm/RTKLIB-Manual-CN/09-appendixE-E.1.html) 和 [E.2](/algorithm/RTKLIB-Manual-CN/09-appendixE-E.2.html) 节查看相关内容。

## 4.1 时间系统

<img style="width: 100%; margin: 0 auto; display: block;" src="https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231021090651319.png"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图4.1-1 RTKLIB 时间相关函数</p>

### 4.1.1 基本概念

时间系统通常包括时间起点基准、时间间隔尺度，以下是时间系统中一些常见的概念：

1. **恒星时**（Sidereal Time，**ST**）：以春分点为基本参考点，由春分点周日视运动确定的时间称为恒星时。
2. **真太阳时**：以真太阳作为基本参考点，由其周日视运动确定的时间称为真太阳时。
3. **平太阳时**：（由于真太阳的视运动是不均匀的，所以引入虚拟的在赤道上匀速运行的平太阳，其速度等于真太阳周年运动的平均速度），平太阳连续经过同一子午圈的时间称为一个平太阳日，分为 24 个平太阳时。
4. **历书时**（Ephemeris Time，**ET**）：以地球公转为基准的度量时间的系统。（秒长规定：1900年1月1日12时整回归年长度的 1/31556925.9747；起始历元定在1900年1月1日12时）太阳系质心力学时TDB，地球质心力学时TDT。
5. **原子时**（Atomic Time，**AT**）：是一种以原子谐振信号周期为标准，并对它进行连续计数的时标。
   > UTC、TT、各种GNSS时都是建立在原子时基础上，用各自的原子钟来维持的，只是起点的选择、跳秒与否，已经因为用于维持的原子钟不同而产生的细微差别。
6. **国际原子时**（International Atomic Time，**TAI**）：由各实验室的原子钟维持。取 1958 年 1 月 1 日 0 时 0 分 0 秒世界时(UT)的瞬间作为同年同月同日 0 时 0 分 0 秒TAI。（事后发现，在该瞬间原子时与世界时的时刻之差为 0.0039 秒。这一差值就作为历史事实而保留下来。）
7. **区时**：全球化为 24 个时区，在同一时区采用该时区中央子午线上的平太阳时为区时。
8. **世界时UT**：0 区的区时，即格林尼治起始子午线上的平太阳时为世界时UT
   * UT0：直接天文观测的世界时
   * UT1：经过极移改正的世界时
   * UT2：再经过地球自转速度季节性改正的世界时
9. **协调世界时**（Universal Time of Coordination，**UTC**）：以原子秒长为计量单位，在时刻上与平太阳时之差小于 0.9 秒的时间系统。**UT1 + 跳秒**。（C、C++ 获取的系统时间默认是 UTC）
10. **跳秒**：leap second，在 1980 年 1 月 6 日 0 时 GPS 时与 UTC 时对齐，GPS 时是依靠稳定的原子钟来维持的，也就是说它的单位时间长度是很稳定的；而协调世界时是根据天文确定的，和地球自转有关，但是地球自转速度在不断变慢，也就是说协调世界时的单位时间长度并不是恒定的。按照规定，当两者相差接近1秒时，就让 UTC 跳一秒[31]。可看。
    
    ```c
    // rtkcmn.c 中跳秒的定义
    static double leaps[MAXLEAPS+1][7]={ /* (y,m,d,h,m,s,utc-gpst) */
       {2017,1,1,0,0,0,-18},
       {2015,7,1,0,0,0,-17},
       // ...
       {0}
    };
    ```

11. **GPS 时**（GPS Time，**GPST**）：由 GPS 星载原子钟和地面监控站原子钟组成的一种原子时基准，与国际原子时保持有 `19s` 的常数差，并在 GPS 标准历元 1980 年 1 月 6 日 0 时与 UTC 保持一致。

    > GPS 系统内部所采用的时间系统是 GPS 时，其时间零点定义为1980年1月5日 夜与1980年1月6日晨之间的子夜。GPS时系统在标示时间时所采用的最大时间单位为周（week，604800秒），其标示时间的方法是从 1980 年1月 6日0时开始起算的周数（WN-Week Number）加上被称为周内时间（TOW-Time of Week）的从 每周周六/周日子夜开始起算的秒数。 <br>
    > 例如：“1980 年 1 月 6 日 0 时 0 分 0 秒”用GPS时标示法则为“第 0 周 0 秒”；而 “2004年 5 月 1 日 10 时 5 分 15 秒”用GPS时标示法则为“第 1268 周第 554715 秒”。 在 GPS 卫星所发送的导航电文中，时间信息的标示就是采用这种形式。 <br>

12. **北斗时**（BDS Time，**BDT**） ：同 GPST 一样由原子钟保持基准，在 2006 年 1 月 1 日 0 时与 UTC 保持一致。因为从 1980 年到 2006 年共有 14 次跳秒发生，所以 BDT 和 GPST 相差 14 秒且基本恒定不变。
13. **GLONASS时**（GLONASS Time，**GLST**）：以莫斯科本地协调时 UCTsu 定义，其值与 UTC 存在 3 小时时差。
14. **Galileo时**（Galileo Time，**GST**）：同 GPST 保持一致。
15. **QZSSS时** (QZSS TIME , **QZSST**) ：同 GPST 保持一致。
16. **判断闰年**：年份是 4 的倍数但不是 100 的倍数或者年份是 400 的倍数。
17. **GPS 周**：（GPS Week，**GPSW**） GPS系统内部描述时间的一种方式，它的起点为 1980 年 1 月 5 日夜晚与 1980 年 1 月 6 日凌晨之间 0 点（这天是周日，老外经常周日表示一周的第一天，以`0`表示）。

    ```c
    // rtkcmn.c 中 GPST(GST)、GLST、BDT 的时间基准的的定义
    static const double gpst0[]={1980,1, 6,0,0,0}; /* gps time reference */
    static const double gst0 []={1999,8,22,0,0,0}; /* galileo system time reference */
    static const double bdt0 []={2006,1, 1,0,0,0}; /* beidou time reference */
    ```

18. **儒略日**：（Julian Day，**JD**） 是指由公元前 4713 年 1 月 1 日，协调世界时中午 12 时开始所经过的天数。采用连续的数值来标示时间，适合科学计算，且可以很方便地将采用不同方法所标示的时间联系起来，但无法直接反映季节等信息，故日常生活中不常用。

19. **简化儒列日**：（Modified Julian Day、**MJD**），由于儒略时的计时起点距今较为久远，若将现今时间用儒略时来表示，数值 非常大。1973年国际天文学联合会提出 了约化儒略日的时间标示法，其起点是 1858 年 11 月 17 日世界时 0 时。`MJD = JD - 2400000.5`

20. **年积日**：（Day Of Year，**DOY**） 一年当中的第几天，其取值范围为`[1,365]`，

    > 在 GPS 中的用途：年积日通常用来区分观测时段，常用于 GPS 观测文件的命名。例如，在 RINEX 格式中就规定：在数据文件的 8 字符主文件名中，第 5-7 个字符为观测起始时刻的年积日。

21. **天内秒**：（Second Of Day，**SOD**） 一天中的第几秒，其取值范围为`[1,86400]`

22. **周内秒**：（Second Of Week，**SOW**） 一周中的第几秒，其取值范围为`[1,604800]`

23. **周内分**：(Hour Of Week，**HOW**)  一周中的第几小时，其取值范围为`[1,168]`

### 4.1.2 gtime_t 结构体

**gtime_t结构体**：GNSS 数据处理对时间精度非常敏感，如果用 double 类型的 TOW，相当于距离上只有 0.004m 的精度。出于精度的考虑，RTKLIB 将时间表示为`gtime_t` 结构体：

* 用`time_t` 类型表示1970年以来的整秒数，`time_t` 随机器而定一般是是无符号 `__int64 ` 类型，由于整数长度限制，`gtime_t` 不能表示 1970 年以前和2038 以后的时间。
* 用 double 类型表示不到 1s 的时间。

> gtime_t 只是一种时间表示的形式，来表示 GPST、UCT、BDT、GST 等

```c
typedef struct {        /* time struct */
   time_t time;        /* time (s) expressed by standard time_t */
   double sec;         /* fraction of second under 1 s */
} gtime_t;
```

### 4.1.3 str2num()：字符串转数字

字符串转数字，有个判断，字符中出现 d、D 会被转为 E，因为 RINEX 中有时用 D 表示科学计数法，而计算机只识别的是 E。

```c
extern double str2num(const char *s, int i, int n)
{
    double value;
    char str[256],*p=str;
    
    if (i<0||(int)strlen(s)<i||(int)sizeof(str)-1<n) return 0.0;
    for (s+=i;*s&&--n>=0;s++) *p++=*s=='d'||*s=='D'?'E':*s;
    *p='\0';
    return sscanf(str,"%lf",&value)==1?value:0.0;
}
```

### 4.1.4 str2time()：字符串转 gtime_t

字符串转`gtime_t`，string格式： ("... yyyy mm dd hh mm ss ...") 

```c
extern int str2time(const char *s, int i, int n, gtime_t *t)
{
    double ep[6];   //ep数组表示时间：double类型数组，存年月日时分秒
    char str[256],*p=str;
    
    if (i<0||(int)strlen(s)<i||(int)sizeof(str)-1<i) return -1;
    for (s+=i;*s&&--n>=0;) *p++=*s++;
    *p='\0';
    if (sscanf(str,"%lf %lf %lf %lf %lf %lf",ep,ep+1,ep+2,ep+3,ep+4,ep+5)<6)
        return -1;  //sscanf从字符串中读取年月日时分秒，先存到ep数组中
    if (ep[0]<100.0) ep[0]+=ep[0]<80.0?2000.0:1900.0;
    *t=epoch2time(ep);  //再把ep数组转换成gtime_t类型
    return 0;
}
```

### 4.1.5 epoch2time()：把年月日时分秒数组转为 gtime_t

把年月日时分秒数组转为 gtime_t。

```c
//   格里高利至儒略日转换方法：
//   JD=floor(365.25*(y+4716))+floor(30.6001*(m+1))+D+h/24-1537.5
//   若m≤2,则 y=Y-1，m=M+12；
//   若m＞2,则 y=Y, m=M。
//   式中：JD为儒略日；Y为年：M为月；D为日；h=H+Min/60+s/3600,H,Min和S分别为时，分和秒；floor()为取整函数，有floor(a) ≤a，例如floor(-2.3)=-3。
extern gtime_t epoch2time(const double *ep)
{
    const int doy[]={1,32,60,91,121,152,182,213,244,274,305,335};   //每月第一天的doy
    gtime_t time={0};
    int days,sec,year=(int)ep[0],mon=(int)ep[1],day=(int)ep[2];
    
    if (year<1970||2099<year||mon<1||12<mon) return time;
    
    /* leap year if year%4==0 in 1901-2099 */
    days=(year-1970)*365+(year-1969)/4+doy[mon-1]+day-2+(year%4==0&&mon>=3?1:0);
    sec=(int)floor(ep[5]);
    time.time=(time_t)days*86400+(int)ep[3]*3600+(int)ep[4]*60+sec;
    time.sec=ep[5]-sec;
    return time;
}
  ```

### 4.1.6 time2epoch ()：把 gtime_t 转为年月日时分秒数组

儒略日至格里高利转换方法：

```text
a= floor(julday+0.5);
b=a+1537;
c = floor((b-122.1)/365.25);
d = floor(365.25*c);
e = floor((b-d)/30.6001);
D = floor(b-d- floor(30.6001*e)+ rem(julday+.5,1))； 	%天
H=(rem(julday+.5,1))*24;  								%时
M=e-1-12*floor(e/14); 									%月
Y=c-4715-floor((7+M)/10); 								%年
```

`Rem` 函数是取余函数 `rem(x,y)=x-y*fix(x/y)`，其中 `fix()` 是向 0 取整，例如 `fix(-1.5)=-1`, `fix(1.5)=1`。

注：上面的转换算法仅在 1900 年 3 月 1 日至 2100 年 2 月 28 日期间有效。

```c
extern void time2epoch(gtime_t t, double *ep)
{
    const int mday[]={ /* # of days in a month */
        31,28,31,30,31,30,31,31,30,31,30,31,31,28,31,30,31,30,31,31,30,31,30,31,
        31,29,31,30,31,30,31,31,30,31,30,31,31,28,31,30,31,30,31,31,30,31,30,31
    };
    int days,sec,mon,day;
    
    /* leap year if year%4==0 in 1901-2099 */
    days=(int)(t.time/86400);
    sec=(int)(t.time-(time_t)days*86400);
    for (day=days%1461,mon=0;mon<48;mon++) {
        if (day>=mday[mon]) day-=mday[mon]; else break;
    }
    ep[0]=1970+days/1461*4+mon/12; ep[1]=mon%12+1; ep[2]=day+1;
    ep[3]=sec/3600; ep[4]=sec%3600/60; ep[5]=sec%60+t.sec;
}
```

### 4.1.7 time2str()：gtime_t 转字符串


gtime_t 转字符串，格式：string ("yyyy/mm/dd hh:mm:ss.ssss") ，以传入的字符串指针 s 返回。


```c
// * n：小数位数（0~12），前面的年月日时分一定有，
// * n取0，输出格式：yyyy/mm/dd hh:mm:ss
// * n>0，yyyy/mm/dd hh:mm:ss后面还有 n 位
extern void time2str(gtime_t t, char *s, int n)
{
    double ep[6];
    if (n<0) n=0; else if (n>12) n=12;
    if (1.0-t.sec<0.5/pow(10.0,n)) {t.time++; t.sec=0.0;};
    time2epoch(t,ep);	//先把时间转为epoch时间数组
    sprintf(s,"%04.0f/%02.0f/%02.0f %02.0f:%02.0f:%0*.*f",ep[0],ep[1],ep[2],
            ep[3],ep[4],n<=0?2:n+3,n<=0?0:n,ep[5]);
}
```

### 4.1.8 time_str()：gtime_t 转字符串

gtime_t 转字符串，类**time2str()**，区别是转出的字符串直接做返回值，返回的是**静态内存**的首地址，所以不要乱释放或者使用 。

```c
extern char *time_str(gtime_t t, int n)
{
    static char buff[64];
    time2str(t,buff,n);
    return buff;
}
```

### 4.1.9 time2doy()：gtime_t 转年积日DOY

gtime_t 转年积日DOY。

```c
extern double time2doy(gtime_t t)
{
    double ep[6];
    
    time2epoch(t,ep);
    ep[1]=ep[2]=1.0; ep[3]=ep[4]=ep[5]=0.0;
    return timediff(t,epoch2time(ep))/86400.0+1.0;
}
```

### 4.1.10 timeadd()：给传入的 gtime_t 增加秒数

给传入的 gtime_t 增加秒数。

```c
extern gtime_t timeadd(gtime_t t, double sec)
{
    double tt;
    
    t.sec+=sec; tt=floor(t.sec); t.time+=(int)tt; t.sec-=tt;
    return t;
}
```

### 4.1.11 timediff()：求时间差

求时间差 **t1 - t2**。

```c
extern double timediff(gtime_t t1, gtime_t t2)
{
    return difftime(t1.time,t2.time)+t1.sec-t2.sec;
}
```

### 4.1.12 timeget()：获取系统UTC时间

获取系统**UTC**时间，表示成 gtime_t，要转为北京时间还得加 8 小时。

```c
extern gtime_t timeget(void)
{
    gtime_t time;
    double ep[6]={0};
#ifdef WIN32
    SYSTEMTIME ts;
    
    GetSystemTime(&ts); /* utc */
    ep[0]=ts.wYear; ep[1]=ts.wMonth;  ep[2]=ts.wDay;
    ep[3]=ts.wHour; ep[4]=ts.wMinute; ep[5]=ts.wSecond+ts.wMilliseconds*1E-3;
#else
    struct timeval tv;
    struct tm *tt;
    
    if (!gettimeofday(&tv,NULL)&&(tt=gmtime(&tv.tv_sec))) {
        ep[0]=tt->tm_year+1900; ep[1]=tt->tm_mon+1; ep[2]=tt->tm_mday;
        ep[3]=tt->tm_hour; ep[4]=tt->tm_min; ep[5]=tt->tm_sec+tv.tv_usec*1E-6;
    }
#endif
    time=epoch2time(ep);
    return time;
}
```

### 4.1.13 getimeofday()：获取系统时间

获取到的是自1970-01-01 00:00:00 +0000 (UTC)以来的秒数和微秒数，原型：`int gettimeofday(struct timeval *tv, struct timezone *tz);`。

```c
struct timeval {	//tv
    time_t      tv_sec;     /* seconds */
    suseconds_t tv_usec;    /* microseconds */
};
    
struct timezone {	//tz 是一个过时的设计，填NULL即可
    int tz_minuteswest;     /* minutes west of Greenwich */
    int tz_dsttime;         /* type of DST correction */
};
```

### 4.1.14 timeget()：获取系统UTC时间

获取系统UTC时间。

```c
static double timeoffset_=0.0;        /* time offset (s) */

extern gtime_t timeget(void)
{
    gtime_t time;
    double ep[6]={0};
#ifdef WIN32
    SYSTEMTIME ts;
    
    GetSystemTime(&ts); /* utc */
    ep[0]=ts.wYear; ep[1]=ts.wMonth;  ep[2]=ts.wDay;
    ep[3]=ts.wHour; ep[4]=ts.wMinute; ep[5]=ts.wSecond+ts.wMilliseconds*1E-3;
#else
    struct timeval tv;
    struct tm *tt;
    
    if (!gettimeofday(&tv,NULL)&&(tt=gmtime(&tv.tv_sec))) {
        ep[0]=tt->tm_year+1900; ep[1]=tt->tm_mon+1; ep[2]=tt->tm_mday;
        ep[3]=tt->tm_hour; ep[4]=tt->tm_min; ep[5]=tt->tm_sec+tv.tv_usec*1E-6;
    }
#endif
    time=epoch2time(ep);
    
#ifdef CPUTIME_IN_GPST /* cputime operated in gpst */
    time=gpst2utc(time);
#endif
    return timeadd(time,timeoffset_);
}
```

### 4.1.15 timeset()：设置相对于UTC的时间偏移

设置相对于UTC的时间偏移，只对**timeget()**有影响。

```c
extern void timeset(gtime_t t)
{
    timeoffset_+=timediff(t,timeget());
}
```

### 4.1.16 timereset()：时间偏移重置

时间偏移重置为0.0。

```c
extern void timereset(void)
{
    timeoffset_=0.0;
}
```

### 4.1.17 time2sec()：计算SOD

由 gtime_t 算 SOD 和 0 时 0 分 0 秒的 gtime_t。

```c
static double time2sec(gtime_t time, gtime_t *day)
{
    double ep[6],sec;
    time2epoch(time,ep);    //gtime_t转epoch时间数组
    sec=ep[3]*3600.0+ep[4]*60.0+ep[5];  //根据时分秒算天内秒SOD
    ep[3]=ep[4]=ep[5]=0.0;  //时分秒赋值0
    *day=epoch2time(ep);    //0时0分0秒的gtime_t
    return sec;
}
```

### 4.1.18 utc2gmst()：UTC转格林威治平均恒星时

**utc2gmst()**：UTC转格林威治平均恒星时，**返回弧度**

```c
extern double utc2gmst(gtime_t t, double ut1_utc)
{
    const double ep2000[]={2000,1,1,12,0,0};
    gtime_t tut,tut0;
    double ut,t1,t2,t3,gmst0,gmst;
    
    tut=timeadd(t,ut1_utc);
    ut=time2sec(tut,&tut0);
    t1=timediff(tut0,epoch2time(ep2000))/86400.0/36525.0;
    t2=t1*t1; t3=t2*t1;
    gmst0=24110.54841+8640184.812866*t1+0.093104*t2-6.2E-6*t3;
    gmst=gmst0+1.002737909350795*ut;
    
    return fmod(gmst,86400.0)*PI/43200.0; /* 0 <= gmst <= 2*PI */
}
```

### 4.1.19 adjgpsweek()：用CPU时间调整GPS周

用 CPU 时间调整 GPS 周。

```c
extern int adjgpsweek(int week)
{
    int w;
    (void)time2gpst(utc2gpst(timeget()),&w);	//从CPU获取周数
    if (w<1560) w=1560; /* use 2009/12/1 if time is earlier than 2009/12/1 */
    return week+(w-week+1)/1024*1024;	//转为GPS周，并返回
}
```

### 4.1.20 tickget()：获取系统时间

* **tickget()**：
  * **timeGetTime()** ：返回以 **毫秒** 计的系统时间。该时间为从系统开启算起所经过的时间 

```c
extern uint32_t tickget(void)
{
#ifdef WIN32
    return (uint32_t)timeGetTime();
#else
    struct timespec tp={0};
    struct timeval  tv={0};
    
#ifdef CLOCK_MONOTONIC_RAW
    /* linux kernel > 2.6.28 */
    if (!clock_gettime(CLOCK_MONOTONIC_RAW,&tp)) {
        return tp.tv_sec*1000u+tp.tv_nsec/1000000u;
    }
    else {
        gettimeofday(&tv,NULL);
        return tv.tv_sec*1000u+tv.tv_usec/1000u;
    }
#else
    gettimeofday(&tv,NULL);
    return tv.tv_sec*1000u+tv.tv_usec/1000u;
#endif
#endif /* WIN32 */
}
```

### 4.1.21 sleepms()：线程挂起若干毫秒

线程挂起若干毫秒。

```c
extern void sleepms(int ms)
{
#ifdef WIN32
    if (ms<5) Sleep(1); else Sleep(ms);
#else
    struct timespec ts;
    if (ms<=0) return;
    ts.tv_sec=(time_t)(ms/1000);
    ts.tv_nsec=(long)(ms%1000*1000000);
    nanosleep(&ts,NULL);
#endif
}
```

1. **gtime_t**、**GPST**、**UCT**、**BDT**、**GST**间的转换函数

   > - RTKLIB 内以 GPST 进行计算处理，其它时间系统的数据需转换为 GPST 数据才能进行计算。
   >
   > - gtime_t 只是一种时间表示的形式，来表示 GPST、UCT、BDT、GST

* **gpst2time ()**：GPST 转 gtime_t。

  **time2gpst ()**：gtime_t 转 GPST。

  * **WN**：以第二个参数 int 指针 week 返回，不用周可以传 null 或 nullptr
  * **TOW**：double类型返回值

  ```c
  extern double time2gpst(gtime_t t, int *week)
  {
      gtime_t t0=epoch2time(gpst0);
      time_t sec=t.time-t0.time;
      int w=(int)(sec/(86400*7));
      
      if (week) *week=w;
      return (double)(sec-(double)w*86400*7)+t.sec;
  }
  ```

  ```c
  extern gtime_t gpst2time(int week, double sec)
  {
      gtime_t t=epoch2time(gpst0);
      
      if (sec<-1E9||1E9<sec) sec=0.0;
      t.time+=(time_t)86400*7*week+(int)sec;
      t.sec=sec-(int)sec;
      return t;
  }
  ```

### 4.1.22 gst2time()、time2gst ()：GST 和 gtime_t 互转

**gst2time ()**：GST转gtime_t；**time2gst ()**：gtime_t转GST。
  - **WN**：以第二个参数 int 指针 week 返回
  - **TOW**：double类型返回值

```c
extern double time2gst(gtime_t t, int *week)
{
    gtime_t t0=epoch2time(gst0);
    time_t sec=t.time-t0.time;
    int w=(int)(sec/(86400*7));
    
    if (week) *week=w;
    return (double)(sec-(double)w*86400*7)+t.sec;
}

extern gtime_t gst2time(int week, double sec)
{
    gtime_t t=epoch2time(gst0);
    
    if (sec<-1E9||1E9<sec) sec=0.0;
    t.time+=(time_t)86400*7*week+(int)sec;
    t.sec=sec-(int)sec;
    return t;
}
```

### 4.1.23 bdt2time ()、time2bdt ()：BDT 和 gtime_t 互转

**bdt2time ()**：BDT转gtime_t；**time2bdt ()**：gtime_t转BDT。
* **WN**：以第二个参数int指针week返回
* **TOW**：double类型返回值

```c
extern double time2bdt(gtime_t t, int *week)
{
    gtime_t t0=epoch2time(bdt0);
    time_t sec=t.time-t0.time;
    int w=(int)(sec/(86400*7));
    
    if (week) *week=w;
    return (double)(sec-(double)w*86400*7)+t.sec;
}

extern gtime_t bdt2time(int week, double sec)
{
    gtime_t t=epoch2time(bdt0);
    
    if (sec<-1E9||1E9<sec) sec=0.0;
    t.time+=(time_t)86400*7*week+(int)sec;
    t.sec=sec-(int)sec;
    return t;
}
```

### 4.1.24 gpst2utc()、utc2gpst ()：GPST 和 UTC 互转

**gpst2utc()**：GPST转UTC，跳秒；**utc2gpst ()**：UTC转GPST。

```c
extern gtime_t utc2gpst(gtime_t t)
{
    int i;
    
    for (i=0;leaps[i][0]>0;i++) {
        if (timediff(t,epoch2time(leaps[i]))>=0.0) return timeadd(t,-leaps[i][6]);
    }
    return t;
}

extern gtime_t gpst2utc(gtime_t t)
{
    gtime_t tu;
    int i;
    
    for (i=0;leaps[i][0]>0;i++) {
        tu=timeadd(t,leaps[i][6]);
        if (timediff(tu,epoch2time(leaps[i]))>=0.0) return tu;
    }
    return t;
}
```

## 4.2 坐标系统

<img style="width: 100%; margin: 0 auto; display: block;" src="https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231021091756265.png"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图4.2-1 RTKLIB 坐标系统相关函数</p>

全球定位系统（GPS）核心任务是确定用户在特定坐标系中的位置坐标。位置依赖于参考坐标系，因此需建立合适的坐标系。坐标系由原点、三个坐标轴指向和尺度定义，分为两大类：

- 天球坐标系：基于天球参考。
- 地球坐标系：以地球为参考。

根据时间依赖性，每类坐标系可进一步细分。不同坐标系间通过七参数变换（平移、旋转、尺度）实现相互转换。

注意：RTKLIB内部中，运算使用的是ECEF坐标系统。 

### 4.2.1 基础理论知识

1. **大地水准面与地球椭球**

    * **水准面**：海水在重力作用下，呈静止状态，形成的重力等位面（不规则、可以很多）。
    * **大地水准面**：平均海平面向陆地延伸（不规则，只有一个），不能作为计算和制图的基准面。
    * **地球形状**：两极稍扁、赤道略鼓 ，北极稍凸、南极稍凹的类似梨形的形体，横切面接近一个圆，纵切面接近一个椭圆，近视一个旋转椭球。
    * **地球椭球**：椭圆绕其短半轴旋转一周形成旋转椭球，确定一个子午圈即可确定一个地球椭球。用地球椭球代替地球表面，作为计算和制图的基准面。
      * **总地球椭球**：与全球大地水准面吻合的旋转椭球。
      * **参考椭球**：用局部资料推算出的地球椭球。
      * **椭球定位**：确定大地水准面与椭球面的相对关系，使椭球面与地球密合。
      * **椭球定向**：使旋转椭球短半轴，平行和重合。

2. **天球的基本概念**

    * **天球**：以地球质心为球心，以无穷远为半径形成的球体。 
    * **天轴**：地球自转轴的延伸。 
    * **天极**：天轴与天球表面的交点。 
    * **天球赤道面**：过地球质心与天轴垂直的平面。 
    * **天球赤道**：天球表面与天球赤道面的交线（圈）。
    * **时圈**：过天轴的平面与天球表面的交线（圈）。
    * **黄道**：地球公转平面与天球表面的交线（圈）。 
    * **黄赤交角**：天球（地球）赤道面与黄道面的夹角。 
    * **黄极**：过地球质心垂直于黄道面的直线，与天球表面的交点。
    * **秋分点**：太阳在黄道面上运动（逆时针旋转），由北向南，运动到天球赤道与黄道的交点。

3. **空间直角坐标系与大地坐标系**

    * **空间直角坐标系**XYZ：
      * X纵轴表示南北、Y横轴表示东西，XY互换可使用所有三角公式。
      * z轴与椭球短半轴重合指向北极N
      * x轴为起始大地子午面与椭球赤道面交线
    * **大地坐标系**BLH

4. **天球坐标系与地球坐标系**

    * **天球坐标系**：坐标系指向不变，方便描述卫星运动。
    * **地球坐标系**：坐标系随地球自转，方便描述地球上的位置。

5. **参心坐标系与地心坐标系、站心坐标系**

    * **参心坐标系**：原点在参心（参考椭球）
    * **地心坐标系**：原点在地心（总地球椭球）
    * **站心坐标系**：原点在测站

6. **岁差、章动、极移**

    * **岁差**：由于天球赤道和天球黄道的长期运动而导致的春分点的进动 ，**北天极绕北黄极顺时针转动**，周期为25800年。 

      > 岁差模型：IAU1976（L77）、IAU2000、IAU2006（P03）、B03、fF03

    * **章动**：由于月球、太阳和各大行星与地球间的相对位置存在周期性变化，因此作用在地球赤道隆起部分的力矩也在变化，地月系质心绕日公转的轨道面也存在周期性摄动，因此在岁差的基础上还存在各种大小和周期各不相同的微小周期性变化。 **瞬时北天极围绕瞬时北黄极旋转**，大致成椭圆形状，周期为18.6年。

      > 章动模型：IAU1980、IAU2000 <br>
      > 精确模型 IAU2000A，简化模型 IAU2000B，精确模型精度 0.2mas、简化模型精度1mas，对 GPS 来说 1mas 引起 13cm 的卫星位置误差 <br>
      > 不改正岁差与章动影响：**瞬时真** <br>
      > 改正章动影响：**瞬时平** <br>
      > 改正岁差与章动影响：**协议** <br>
   
   * **极移**：由于地球表面的物质运动（如洋流、海潮等）以及地球内部的物质运动（如地幔的运动），会使极点的位置产生变化 ，**极点的位置产生变化**。 

7. **国际天球坐标参考系ITRS即其参考框架ICRF**：国际天球参考系ICRS由国际地球自转服务IERS所建立的国际天球参考框架ICEF来实现的。ICRF框架中坐标轴是指向由甚长基线干涉VLBI所确定的一组河外射电源（控制），在J2000.0的天球赤道来予以定义和维持的。
   > 根据质心选太阳系质心和地球质心分为质心天球参考系（BCRS）和地心天球参考系（GCRS） 

8. **国际地球坐标参考系ITRS及其参考框架ITRF**：ITRF是由一组IERS测站的坐标，站坐标的年变化率，及相应的地球定向参数EOP实现的。IGS的精密星历就采用这一框架。
   >  WGS-84有时视为一个坐标系统，有时视为一个参考框架，WGS84满足ITRS的规定，理论上说WGG84与ITRF、ITRS一致。WGS-84（BLH）主要用于导航定位、ITRS主要用于大地测量和地球动力学研究。

### 4.2.2 常用坐标系

定位需基于坐标系，坐标是相对参考系的描述。首先需建立坐标系，分为两大类：

- 惯性坐标系：空间静止或匀速直线运动。
- 非惯性坐标系：其他运动状态。

GNSS 定位涉及约五种具体坐标系。

1. **地心惯性坐标系（ECI: Earth Centered Inertial）**

    * 地心惯性坐标系是太阳系内的一个惯性坐标系，不随地球而转动，也不受地球、太阳运行的章动和岁差的影响。
    * 坐标原点位于地心；X轴位于赤道平面内，指向某一特定年(历元时刻)的太阳春分点位置；Z轴指向那一年地球北极的平均位置处；Y轴位于赤道平面内，与X轴垂直，且与X、Z轴构成右手直角坐标系。
    * 由于采用的历元时间不同，可以有各种不同的地心惯性坐标系，目前国际上通用的地心惯性坐标系是 J2000 历元坐标系，它是以公元 2000 年的春分点为基准的历元坐标系。

2. **地心地固直角坐标系（ECEF: Earth Centered Earth Fixed）**

    * 地固坐标系固定在地球上而随地球一起在空间做公转和自转运动，因此地球上任一固定点在地球坐标系的坐标就不会由于地球旋转而变化。
    * 坐标原点位于地心；X轴指向参考子午面与地球赤道的交点；Z轴与地球自转轴重合并指向地球北极；Y轴位于赤道平面内，与X轴垂直，且与X、Z轴构成右手直角坐标系。
    * 因为有极移，所以采用了协议地极，以1900年到1905年间的地极实际位置的平均值作为基准点。 

3. **大地坐标系：也叫经纬高坐标系（LLA: Longitude Latitude Altitude）**

    * 也是地固坐标系。坐标原点位于地心。
    * 基于基准椭球体（基准椭球体是定义的与地球几何最吻合的椭球体）。
    * 大地纬度 $\phi$ 是过该点的基准椭球面法线与赤道面的夹角。纬度值在-90°到+90°之间。北半球为正，南半球为负。
    * 大地经度 $\lambda$ 是过该点的子午面与本初子午面之间的夹角。经度值在-180°到+180°之间。
    * 大地高度 $h$ 是过该点到基准椭球面的法线距离，基准椭球面以内为负，以外为正。

4. **站心坐标系：也叫东北天坐标系（ENU: East North Up）**

    * 是以观测站为原点的坐标系，主要用于了解以观察者为中心的其他物体运动规律。
    * 三个坐标轴分别指向相互垂直的东向、北向和天向，因而又称东北天坐标系。
    * 可用于计算卫星在用户处的观测向量、仰角和方位角。

5. **WGS-84: World Geodetic System-1984 Coordinate System**
    > CGCS2000 坐标系和 WGS84 坐标系的区别参考资料[27]。
    * 是一个地心地固直角坐标系。
    * 坐标原点为地心，Z轴指向国际时间服务机构（BIH）1984年定义的协议地球极（CTP: Conventional Terrestrial Pole）方向，X轴指向本初子午面和CTP赤道的交点，Y轴与Z轴、X轴垂直构成右手坐标系。
    * GPS广播星历是以 WGS-84 坐标系为基准的。

### 4.2.3 dms2deg()、deg2dms()：度分秒与弧度互转

- **dms2deg()**：度分秒 dms —>度 deg
- **deg2dms()**：度 deg —>度分秒 dms

```c
extern void deg2dms(double deg, double *dms, int ndec)
{
    double sign=deg<0.0?-1.0:1.0,a=fabs(deg);
    double unit=pow(0.1,ndec);
    dms[0]=floor(a); a=(a-dms[0])*60.0;
    dms[1]=floor(a); a=(a-dms[1])*60.0;
    dms[2]=floor(a/unit+0.5)*unit;
    if (dms[2]>=60.0) {
        dms[2]=0.0;
        dms[1]+=1.0;
        if (dms[1]>=60.0) {
            dms[1]=0.0;
            dms[0]+=1.0;
        }
    }
    dms[0]*=sign;
}

extern double dms2deg(const double *dms)
{
    double sign=dms[0]<0.0?-1.0:1.0;
    return sign*(fabs(dms[0])+dms[1]/60.0+dms[2]/3600.0);
}
```

### 4.2.4 pos2ecef()、ecef2pos()：地心地固直角坐标系与大地坐标系 LLA 互转

> pos2ecf 和 ecf2pos函数相关理论部分参考资料[28]。

* **pos2ecef()**：大地坐标（lon,lat,h）转地心空间直角坐标（x,y,z） 
  * pos：{lat,lon,h} (rad,m) 
  * r：输出参数：{x,y,z} (m) 
* **ecef2pos()**： 地心空间直角坐标（x,y,z）转大地坐标（lon,lat,h） 


```c
extern void pos2ecef(const double *pos, double *r)
{
    double sinp=sin(pos[0]),cosp=cos(pos[0]),sinl=sin(pos[1]),cosl=cos(pos[1]);
    double e2=FE_WGS84*(2.0-FE_WGS84),v=RE_WGS84/sqrt(1.0-e2*sinp*sinp);
    
    r[0]=(v+pos[2])*cosp*cosl;
    r[1]=(v+pos[2])*cosp*sinl;
    r[2]=(v*(1.0-e2)+pos[2])*sinp;
}

extern void ecef2pos(const double *r, double *pos)
{
    double e2=FE_WGS84*(2.0-FE_WGS84),r2=dot(r,r,2),z,zk,v=RE_WGS84,sinp;
    
    for (z=r[2],zk=0.0;fabs(z-zk)>=1E-4;) {
        zk=z;
        sinp=z/sqrt(r2+z*z);
        v=RE_WGS84/sqrt(1.0-e2*sinp*sinp);
        z=r[2]+v*e2*sinp;
    }
    pos[0]=r2>1E-12?atan(z/sqrt(r2)):(r[2]>0.0?PI/2.0:-PI/2.0);
    pos[1]=r2>1E-12?atan2(r[1],r[0]):0.0;
    pos[2]=sqrt(r2+z*z)-v;
}
```

### 4.2.5 enu2ecef()、ecef2enu()：ECEF地心地固直角坐标系与ENU站心坐标系互转

* **xyz2enu()**：计算将ECEF中的向量转换到站心坐标系中的转换矩阵。
  * pos：{lat,lon} (rad) 
  * E：33转换矩阵
* **enu2ecef()**：地方坐标(N,E,U)转ECEF坐标（x,y,z）。参数中要输入(N,E,U)以及其大地坐标(lon,lat)。 
* **ecef2enu()**：地方坐标(E,N,U) 转ECEF坐标（x,y,z）
* **covecef()**：将地方坐标（N,E,U）转换为ECEF坐标的协方差阵
  * 先调用 xyz2enu() 算转换矩阵 E，再用协方差传播定律 $EP=E*P*E^T$
* **covenu()**：将ECEF坐标的协方差阵，转换为地方坐标（N,E,U）的协方差阵 

```c
extern void xyz2enu(const double *pos, double *E)
{
    double sinp=sin(pos[0]),cosp=cos(pos[0]),sinl=sin(pos[1]),cosl=cos(pos[1]);
    
    E[0]=-sinl;      E[3]=cosl;       E[6]=0.0;
    E[1]=-sinp*cosl; E[4]=-sinp*sinl; E[7]=cosp;
    E[2]=cosp*cosl;  E[5]=cosp*sinl;  E[8]=sinp;
}

extern void enu2ecef(const double *pos, const double *e, double *r)
{
    double E[9];
    xyz2enu(pos,E);
    matmul("TN",3,1,3,1.0,E,e,0.0,r);
}

extern void ecef2enu(const double *pos, const double *r, double *e)
{
    double E[9];
    
    xyz2enu(pos,E);
    matmul("NN",3,1,3,1.0,E,r,0.0,e);
}

extern void covecef(const double *pos, const double *Q, double *P)
{
    double E[9],EQ[9];
    
    xyz2enu(pos,E);
    matmul("TN",3,3,3,1.0,E,Q,0.0,EQ);
    matmul("NN",3,3,3,1.0,EQ,E,0.0,P);
}

extern void covenu(const double *pos, const double *P, double *Q)
{
    double E[9],EP[9];
    
    xyz2enu(pos,E);
    matmul("NN",3,3,3,1.0,E,P,0.0,EP);
    matmul("NT",3,3,3,1.0,EP,E,0.0,Q);
}
```

### 4.2.6 eci2ecef()：ECI地心惯性坐标系与ECEF地心地固直角坐标系转换

> 导航卫星定轨在惯性系 ECI 完成，之后还需要转换到地固系ECEF <br>
> ITRS 与 GCRS 之间的坐标转换参考资料[29] <br>
> IAU1976_1980 及 2000A 岁差章动模型的比较参考资料[30] <br>
> RTKLIB 里用的好像是 1976 岁差模型和 1980 章动模型<br>

* **Rx**、**Ry**、**Rz**坐标旋转矩阵
* **eci2ecef()**：求ECI坐标 (x,y,z) 转ECEF坐标 (x,y,z) 的转换矩阵。
* * fabs()：对浮点数取绝对值
  * fmod()：用于查找除法的余数，它接受两个数字(分子和分母)，并返回四舍五入为零的分子/分母的浮点余数 

```c
#define Rx(t,X) do { \
    (X)[0]=1.0; (X)[1]=(X)[2]=(X)[3]=(X)[6]=0.0; \
    (X)[4]=(X)[8]=cos(t); (X)[7]=sin(t); (X)[5]=-(X)[7]; \
} while (0)

#define Ry(t,X) do { \
    (X)[4]=1.0; (X)[1]=(X)[3]=(X)[5]=(X)[7]=0.0; \
    (X)[0]=(X)[8]=cos(t); (X)[2]=sin(t); (X)[6]=-(X)[2]; \
} while (0)

#define Rz(t,X) do { \
    (X)[8]=1.0; (X)[2]=(X)[5]=(X)[6]=(X)[7]=0.0; \
    (X)[0]=(X)[4]=cos(t); (X)[3]=sin(t); (X)[1]=-(X)[3]; \
} while (0)

extern void eci2ecef(gtime_t tutc, const double *erpv, double *U, double *gmst)
{
    const double ep2000[]={2000,1,1,12,0,0};
    static gtime_t tutc_;       //带_:静态变量，保存上一次调用时的值
    static double U_[9],gmst_;
    gtime_t tgps;
    double eps,ze,th,z,t,t2,t3,dpsi,deps,gast,f[5];
    double R1[9],R2[9],R3[9],R[9],W[9],N[9],P[9],NP[9];
    int i;
    
    trace(4,"eci2ecef: tutc=%s\n",time_str(tutc,3));
    
    if (fabs(timediff(tutc,tutc_))<0.01) { /* read cache */
        for (i=0;i<9;i++) U[i]=U_[i];
        if (gmst) *gmst=gmst_; 
        return;
    }
    tutc_=tutc;
    
    /* terrestrial time */  //地面时间
    tgps=utc2gpst(tutc_);
    t=(timediff(tgps,epoch2time(ep2000))+19.0+32.184)/86400.0/36525.0;
    t2=t*t; t3=t2*t;
    
    /* astronomical arguments */
    ast_args(t,f);
    
    /* iau 1976 precession */   //岁差
    ze=(2306.2181*t+0.30188*t2+0.017998*t3)*AS2R;
    th=(2004.3109*t-0.42665*t2-0.041833*t3)*AS2R;
    z =(2306.2181*t+1.09468*t2+0.018203*t3)*AS2R;
    eps=(84381.448-46.8150*t-0.00059*t2+0.001813*t3)*AS2R;
    Rz(-z,R1); Ry(th,R2); Rz(-ze,R3);
    matmul("NN",3,3,3,1.0,R1,R2,0.0,R);
    matmul("NN",3,3,3,1.0,R, R3,0.0,P); /* P=Rz(-z)*Ry(th)*Rz(-ze) */
    
    /* iau 1980 nutation */     //章动
    nut_iau1980(t,f,&dpsi,&deps);
    Rx(-eps-deps,R1); Rz(-dpsi,R2); Rx(eps,R3);
    matmul("NN",3,3,3,1.0,R1,R2,0.0,R);
    matmul("NN",3,3,3,1.0,R ,R3,0.0,N); /* N=Rx(-eps)*Rz(-dspi)*Rx(eps) */
    
    /* greenwich aparent sidereal time (rad) */ //格林威治视恒星时
    gmst_=utc2gmst(tutc_,erpv[2]);
    gast=gmst_+dpsi*cos(eps);
    gast+=(0.00264*sin(f[4])+0.000063*sin(2.0*f[4]))*AS2R;
    
    /* eci to ecef transformation matrix */
    Ry(-erpv[0],R1); Rx(-erpv[1],R2); Rz(gast,R3);
    matmul("NN",3,3,3,1.0,R1,R2,0.0,W );
    matmul("NN",3,3,3,1.0,W ,R3,0.0,R ); /* W=Ry(-xp)*Rx(-yp) */
    matmul("NN",3,3,3,1.0,N ,P ,0.0,NP);
    matmul("NN",3,3,3,1.0,R ,NP,0.0,U_); /* U=W*Rz(gast)*N*P */
    
    for (i=0;i<9;i++) U[i]=U_[i];
    if (gmst) *gmst=gmst_; 
    
    trace(5,"gmst=%.12f gast=%.12f\n",gmst_,gast);
    trace(5,"P=\n"); tracemat(5,P,3,3,15,12);
    trace(5,"N=\n"); tracemat(5,N,3,3,15,12);
    trace(5,"W=\n"); tracemat(5,,3,3,15,12);
    trace(5,"U=\n"); tracemat(5,U,3,3,15,12);
}
```

## 4.3 矩阵操作

GNSS 处理的数据都是矩阵数据，RTKLIB 的 **rtkcmn.c** 中写了一些矩阵运算的函数。

![image-20231021091639437](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231021091639437.png)

RTKLIB 里的矩阵一般都是 double 类型的一数组，列优先顺序，一般 n 是行、m 是列

### 4.3.1 mat()：创建新矩阵

创建新矩阵，传入行数 n，列数 m，malloc 开辟 n*m 个**double**空间，返回此空间的首地址。

```C++
extern double *mat(int n, int m)
{
    double *p;
    
    if (n<=0||m<=0) return NULL;
    if (!(p=(double *)malloc(sizeof(double)*n*m))) {
        fatalerr("matrix memory allocation error: n=%d,m=%d\n",n,m);
    }
    return p;
}
```

### 4.3.2 imat()：创建整型矩阵

与 mat() 类似，区别在于 iamt() 创建的矩阵是整型的。

```C++
extern int *imat(int n, int m)
{
    int *p;
    
    if (n<=0||m<=0) return NULL;
    if (!(p=(int *)malloc(sizeof(int)*n*m))) {
        fatalerr("integer matrix memory allocation error: n=%d,m=%d\n",n,m);
    }
    return p;
}
```

### 4.3.3 zeros()：创建 double 类型全零矩阵

创建 double 类型全零矩阵。它比mat多了`\#if NOCALLOC if ((p=mat(n,m))) for (n=n*m-1;n>=0;n--) p[n]=0.0;`，如果预编译指令有 NOCALLOC，会把所有位赋值0.0。

```C++
extern double *zeros(int n, int m)
{
    double *p;
    
#if NOCALLOC
    if ((p=mat(n,m))) for (n=n*m-1;n>=0;n--) p[n]=0.0;
#else
    if (n<=0||m<=0) return NULL;
    if (!(p=(double *)calloc(sizeof(double),n*m))) {
        fatalerr("matrix memory allocation error: n=%d,m=%d\n",n,m);
    }
#endif
    return p;
}
```

### 4.3.4 eye()：创建单位矩阵

创建单位矩阵，**对角线元素全赋值1.0**

```C++
extern double *eye(int n)
{
    double *p;
    int i;
    
    if ((p=zeros(n,n))) for (i=0;i<n;i++) p[i+i*n]=1.0;
    return p;
}
```

### 4.3.5 dot()：点乘

点乘、求两个向量的**内积**（对应位元素相乘再相加）

```C++
extern double dot(const double *a, const double *b, int n)
{
    double c=0.0;
    
    while (--n>=0) c+=a[n]*b[n];
    return c;
}
```

### 4.3.6 norm()：求向量的模长

求向量的模长（向量自己的内积再开方 ）

```c
extern double norm(const double *a, int n)
{
    return sqrt(dot(a,a,n));
}
```

### 4.3.7 cross3()：计算两个三维向量的外积

计算两个三维向量的外积。

```c++
extern void cross3(const double *a, const double *b, double *c)
{
    c[0]=a[1]*b[2]-a[2]*b[1];
    c[1]=a[2]*b[0]-a[0]*b[2];
    c[2]=a[0]*b[1]-a[1]*b[0];
}
```

### 4.3.8 normv3()：单位化三维向量

单位化三维向量（各元素除以向量的模长范数），a为传入向量，b为传出向量。

```c
extern int normv3(const double *a, double *b)
{
    double r;
    if ((r=norm(a,3))<=0.0) return 0;
    b[0]=a[0]/r;
    b[1]=a[1]/r;
    b[2]=a[2]/r;
    return 1;
}
```
### 4.3.9 matcpy()：矩阵复制

矩阵复制，将 B 矩阵的所有元素赋值给 A 矩阵 

```C++
extern void matcpy(double *A, const double *B, int n, int m)
{
    memcpy(A,B,sizeof(double)*n*m);
}
```

### 4.3.10 matmul()：矩阵相乘

矩阵相乘、矩阵 A、B 相乘返回 C

- **tr**：两个元素的字符数组，Tr、Tr[1]分别标识A、B的状态，取值N正常、T转置，可直接写TN、NN。
- **n、k、m**：A 为 nm 矩阵， B为mk矩阵、A的列数与B的行数相等，所有只用三个参数。作者之所以把 n 和 k 放在 m 前面，看起来顺序反逻辑，其实是矩阵相乘一共要循环n*k次，从编程的顺序来排的 。
- 总体流程为：`C =alphaAB + beta * C`

```c++
extern void matmul(const char *tr, int n, int k, int m, double alpha,
                  const double *A, const double *B, double beta, double *C)
{
   double d;
   int i,j,x,f=tr[0]=='N'?(tr[1]=='N'?1:2):(tr[1]=='N'?3:4);
   
   for (i=0;i<n;i++) for (j=0;j<k;j++) {
       d=0.0;
       switch (f) {
           case 1: for (x=0;x<m;x++) d+=A[i+x*n]*B[x+j*m]; break;
           case 2: for (x=0;x<m;x++) d+=A[i+x*n]*B[j+x*k]; break;
           case 3: for (x=0;x<m;x++) d+=A[x+i*m]*B[x+j*m]; break;
           case 4: for (x=0;x<m;x++) d+=A[x+i*m]*B[j+x*k]; break;
       }
       if (beta==0.0) C[i+j*n]=alpha*d; else C[i+j*n]=alpha*d+beta*C[i+j*n];
   }
}
```

### 4.3.11 matinv()：矩阵求逆

矩阵求逆。

```C++
extern int matinv(double *A, int n)
{
    double d,*B;
    int i,j,*indx;
    
    indx=imat(n,1); B=mat(n,n); matcpy(B,A,n,n);
    if (ludcmp(B,n,indx,&d)) {free(indx); free(B); return -1;}
    for (j=0;j<n;j++) {
        for (i=0;i<n;i++) A[i+j*n]=0.0;
        A[j+j*n]=1.0;
        lubksb(B,n,indx,A+j*n);
    }
    free(indx); free(B);
    return 0;
}
```

### 4.3.12 solve()：求线性方程组的解

求线性方程组的解 、AX=Y，求 X，A 为 nn，Y为 nm，用的不多，好像只有模糊度固定里涉及到了。

```c++
extern int solve(const char *tr, const double *A, const double *Y, int n,
                 int m, double *X)
{
    double *B=mat(n,n);
    int info;
    
    matcpy(B,A,n,n);
    if (!(info=matinv(B,n))) matmul(tr[0]=='N'?"NN":"TN",n,m,n,1.0,B,Y,0.0,X);
    free(B);
    return info;
}
```

### 4.3.13 ludcmp ()：LU 分解

LU 分解，即把矩阵 A 分解 LU 乘积的形式，U 为单位上三角矩阵和 L 单位下三角矩阵两部分。

```c++
static int ludcmp(double *A, int n, int *indx, double *d)
{
    double big,s,tmp,*vv=mat(n,1);
    int i,imax=0,j,k;
    
    *d=1.0;
    for (i=0;i<n;i++) {
        big=0.0; for (j=0;j<n;j++) if ((tmp=fabs(A[i+j*n]))>big) big=tmp;
        if (big>0.0) vv[i]=1.0/big; else {free(vv); return -1;}
    }
    for (j=0;j<n;j++) {
        for (i=0;i<j;i++) {
            s=A[i+j*n]; for (k=0;k<i;k++) s-=A[i+k*n]*A[k+j*n]; A[i+j*n]=s;
        }
        big=0.0;
        for (i=j;i<n;i++) {
            s=A[i+j*n]; for (k=0;k<j;k++) s-=A[i+k*n]*A[k+j*n]; A[i+j*n]=s;
            if ((tmp=vv[i]*fabs(s))>=big) {big=tmp; imax=i;}
        }
        if (j!=imax) {
            for (k=0;k<n;k++) {
                tmp=A[imax+k*n]; A[imax+k*n]=A[j+k*n]; A[j+k*n]=tmp;
            }
            *d=-(*d); vv[imax]=vv[j];
        }
        indx[j]=imax;
        if (A[j+j*n]==0.0) {free(vv); return -1;}
        if (j!=n-1) {
            tmp=1.0/A[j+j*n]; for (i=j+1;i<n;i++) A[i+j*n]*=tmp;
        }
    }
    free(vv);
    return 0;
}
```

### 4.3.14 lubksb ()：LU 回代

LU 回代，即把单位上三角矩阵 U 和单位下三角矩阵 L 矩阵回代为一个整体矩阵。

```c++
static void lubksb(const double *A, int n, const int *indx, double *b)
{
    double s;
    int i,ii=-1,ip,j;
    
    for (i=0;i<n;i++) {
        ip=indx[i]; s=b[ip]; b[ip]=b[i];
        if (ii>=0) for (j=ii;j<i;j++) s-=A[i+j*n]*b[j]; else if (s) ii=i;
        b[i]=s;
    }
    for (i=n-1;i>=0;i--) {
        s=b[i]; for (j=i+1;j<n;j++) s-=A[i+j*n]*b[j]; b[i]=s/A[i+i*n];
    }
}
```

### 4.3.15 matprint()、matfprint()：打印矩阵

**matprint()：**打印矩阵到终端、**matfprint()：**打印矩阵到文件中。这两个函数必须熟练掌握，调试程序的时候想看某个矩阵的值都是把值输出到终端，断点调试看变量比较困难；有的时候能看到别的基于 RTKLIB 二次开发的程序有很多行被注释了的 `matprint()`，那就是为了调试用的。

> 为了调试更方便，可以拓展 `matfprint()`，比如控制输出小数位数、转置再输出、输出之前加一行标志等。

```c
extern void matfprint(const double A[], int n, int m, int p, int q, FILE *fp)
{
    int i,j;
    
    for (i=0;i<n;i++) {
        for (j=0;j<m;j++) fprintf(fp," %*.*f",p,q,A[i+j*n]);
        fprintf(fp,"\n");
    }
}

extern void matprint(const double A[], int n, int m, int p, int q)
{
    matfprint(A,n,m,p,q,stdout);
}
```


## 4.4 LSQ 与 KF

### 4.4.1 lsq()：最小二乘估计

最小二乘估计。

- **A**：nm 阶设计矩阵的转置，m<n则无法计算。
- **y**：m 阶观测残差，**y=v=l-HX** 。
- **X**：传出参数、待估计的n阶参数向量的增量。
- **Q**：传出参数、nn协方差阵。

```c++
extern int lsq(const double *A, const double *y, int n, int m, double *x,
              double *Q)
{
   double *Ay;
   int info;
   
   if (m<n) return -1;
   Ay=mat(n,1);
   matmul("NN",n,1,m,1.0,A,y,0.0,Ay); /* Ay=A*y */
   matmul("NT",n,n,m,1.0,A,A,0.0,Q);  /* Q=A*A' */
   if (!(info=matinv(Q,n))) matmul("NN",n,1,n,1.0,Q,Ay,0.0,x); /* x=Q^-1*Ay */
   free(Ay);
   return info;
}
```

### 4.4.2 filter_()、filter()：卡尔曼滤波器

**filter_()、filter()**：卡尔曼滤波器

- X：n阶参数向量，就是参数，不再是最小二乘中的参数的增量 
- P：nn阶协方差阵。X、P在filter()函数中既是传入参数，也是传出参数，迭代。
- H：nm阶设计矩阵的转置
- V：m阶新息向量
- R：mm阶量测噪声协方差阵
- Xp、Pp：预测参数和预测协方差

```c++
extern  int filter_(const double *x, const double *P, const double *H,
                  const double *v, const double *R, int n, int m,
                  double *xp, double *Pp)
{
   double *F=mat(n,m),*Q=mat(m,m),*K=mat(n,m),*I=eye(n);
   int info;
   
   matcpy(Q,R,m,m);
   matcpy(xp,x,n,1);
   matmul("NN",n,m,n,1.0,P,H,0.0,F);       /* Q=H'*P*H+R */
   matmul("TN",m,m,n,1.0,H,F,1.0,Q);
   if (!(info=matinv(Q,m))) {
       matmul("NN",n,m,m,1.0,F,Q,0.0,K);   /* K=P*H*Q^-1 */
       matmul("NN",n,1,m,1.0,K,v,1.0,xp);  /* xp=x+K*v */
       matmul("NT",n,n,m,-1.0,K,H,1.0,I);  /* Pp=(I-K*H')*P */
       matmul("NN",n,n,n,1.0,I,P,0.0,Pp);
   }
   free(F); free(Q); free(K); free(I);
   return info;
}

extern int filter(double *x, double *P, const double *H, const double *v,
                 const double *R, int n, int m)
{
   double *x_,*xp_,*P_,*Pp_,*H_;
   int i,j,k,info,*ix;
   
   ix=imat(n,1); for (i=k=0;i<n;i++) if (x[i]!=0.0&&P[i+i*n]>0.0) ix[k++]=i;
   x_=mat(k,1); xp_=mat(k,1); P_=mat(k,k); Pp_=mat(k,k); H_=mat(k,m);
   for (i=0;i<k;i++) {
       x_[i]=x[ix[i]];
       for (j=0;j<k;j++) P_[i+j*k]=P[ix[i]+ix[j]*n];
       for (j=0;j<m;j++) H_[i+j*k]=H[ix[i]+j*n];
   }
   info=filter_(x_,P_,H_,v,R,k,m,xp_,Pp_);
   for (i=0;i<k;i++) {
       x[ix[i]]=xp_[i];
       for (j=0;j<k;j++) P[ix[i]+ix[j]*n]=Pp_[i+j*k];
   }
   free(ix); free(x_); free(xp_); free(P_); free(Pp_); free(H_);
   return info;
}
```

### 4.4.3 smoother()：卡尔曼滤波平滑（组合滤波）

卡尔曼滤波平滑、加权平均结合前向滤波和后向滤波结果。

- xf、Qf：前向滤波n阶结果和nn阶协方差
- xb、Qb：后向滤波n阶结果和nn阶协方差
- xs、Qs：平滑n阶结果和nn阶协方差

```c++
extern int smoother(const double *xf, const double *Qf, const double *xb,
                   const double *Qb, int n, double *xs, double *Qs)
{
   double *invQf=mat(n,n),*invQb=mat(n,n),*xx=mat(n,1);
   int i,info=-1;
   
   matcpy(invQf,Qf,n,n);
   matcpy(invQb,Qb,n,n);
   if (!matinv(invQf,n)&&!matinv(invQb,n)) {
       for (i=0;i<n*n;i++) Qs[i]=invQf[i]+invQb[i];
       if (!(info=matinv(Qs,n))) {
           matmul("NN",n,1,n,1.0,invQf,xf,0.0,xx);
           matmul("NN",n,1,n,1.0,invQb,xb,1.0,xx);
           matmul("NN",n,1,n,1.0,Qs,xx,0.0,xs);
       }
   }
   free(invQf); free(invQb); free(xx);
   return info;
}
```


## 4.5 卫星系统与观测值表示

<img style="width: 100%; margin: 0 auto; display: block;" src="https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231024191221181.png"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图4.5-1 RTKLIB 时间相关函数</p>

### 4.5.1 卫星系统

* 表示卫星系统的字母：G：GPS、R：GLONASS、E：GALILEO、C：BDS、J：QZSS，I：IRNSS、S：SBAS

* 7位二进制码表示，对应位写1表示有对应的系统，做与运算可加系统。

```c
static const int navsys[]={             /* satellite systems */
    SYS_GPS,SYS_GLO,SYS_GAL,SYS_QZS,SYS_SBS,SYS_CMP,SYS_IRN,0
};

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

### 4.5.2 卫星信息查询

可以表示为各系统的卫星ID（系统缩写+PRN）：B02、C21，也可表示为连续的satellite number ，各种转换函数如下：

* **satno()**：传入卫星系统（SYS_GPS,SYS_GLO,...） ，和PRN码，转换为连续的satellite number。
* **satsys()**：传入satellite number ，返回卫星系统（SYS_GPS,SYS_GLO,...），通过传入的指针prn传出PRN值。
* **satid2no()**：传入卫星ID，返回satellite number。
* **satno2id()**：传入卫星系统，和PRN，返回卫星 ID(Gxx,Cxx）
* **sat2code()**：传入satellite number，返回卫星 ID(Gxx,Cxx)
* **code2sys()**：传入卫星系统缩写，返回系统二进制码SYS_XXX。
* **satexclude()**：检测某颗卫星在定位时是否需要将其排除

### 4.5.3 观测值类型

- 观测类型符号标识：
  - C：伪距
  - D：多普勒
  - L：载波相位
  - S：载噪比
- `sigind_t`：表示每种卫星系统的载波类型和观测值类型 ，每种类型的系统其实对应的就是一个 `sigind_t` 结构体，也就是说只需要建立七个结构体就够了。 
- **CODE_XXX**：观测值类型定义，用一串连续的数字表示。
- **code2obs**()：传入obs code (CODE_???) ，返回code string ("1C","1P","1Y",...)
- **obs2code**()：传入code string ("1C","1P","1Y",...)，返回obs code (CODE_???) 
- **code2idx**()：传入obs code (`CODE_XXX`) 和卫星系统(`SYS_XXX`) ，返回载波频率的下标
- **code2freq**()：传入obs code (CODE???))和卫星系统(SYS_???)，以及GLONASS的信道，调用code2freq_GPS()、code2freq_GLO()、code2freq_GAL()、code2freq_QZS()、code2freq_SBS()、code2freq_BDS()、code2freq_IRN()，返回对应的载波频率(Hz) 
- **sat2freq()**：传入 satellite number 和 obs code ，返回对应的载波频率(Hz) 
- **setcodepri()**、**getcodepri()**：设置和获取信号优先级。如果输入的观测数据在同一频率内包含多个信号，RTKLIB将按照以下默认优先级选择一个信号进行处理。 

<GiscusTalk />
