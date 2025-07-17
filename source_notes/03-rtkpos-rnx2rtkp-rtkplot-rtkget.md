# 3. 后处理程序

## 3.1 基本使用

推荐 B 站赵老师的视频讲解[24]，看视频学软件操作更直观，后文主要参考资料[1]中的相关笔记。

### 3.1.1 RTKGET 数据下载

GNSS 观测数据及各种产品下载网址可以参考资料[25]，有关 RTKGET 的更多内容请查看 [RTKLIB-Manual-CN 3.9 节](/algorithm/RTKLIB-Manual-CN/03-instructions-3.9.html)。

**1. 数据下载**

![bf7b9a35f8b2c5f149215023bfd25831](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/bf7b9a35f8b2c5f149215023bfd25831.png)

* **设置时间范围**：选择下载数据的起止时间；
* **配置 URL_LIST**：
  - 加载文件：在 RTKLIB 的 options 中加载 `URL_LIST.txt`（位于源码版 data 文件夹，建议使用源码版）。
  - 效果：列表文件加载后，界面左侧显示数据源列表。
* **数据类型**：
  - **OBS**：原始观测数据（伪距、载波相位）。
  - **NAV**：广播轨道信息。
  - **EPH**：精密轨道数据。
  - **CLK**：精密钟差数据。
  - **ATX**：天线相位中心校正。
- **相对定位数据下载**：
  - **选择分析中心**：如 IGS、MGEX。
  - **添加测站**：点击 “...” 选择测站（如 ALIC、KARR）。
  - **下载**：
    - 选中测站，点击 “Download”，数据存至指定目录（速度可能较慢）。
    - 或复制 FTP 路径，通过浏览器直接下载。
  - **批量处理**：Linux 环境下可用脚本自动化下载。

**2. 用 RTKGET 做时间转换**

![image-20231025204151284](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025204151284.png)

* **输入年月日时分秒**：
  * 点击“?”查看支持的时间格式，下载观测值需要提供年积日（DOY）。修改链接中的日期，即可下载对应的观测值文件（OBS）。

**3. 数据命名格式**

- **命名格式**：`测站名（4位）+机构信息+年+年积日（DOY）+采样间隔`。
- **压缩格式**：
  - `.crx`：压缩观测数据。
  - `.gz`：压缩格式。
  - `.o.z`：单次压缩的观测文件。

**4. 星历下载**

- **工具**：使用RTKLIB的rtkget或FTP下载。
- **文件类型**：
  - 广播星历（NAV）：卫星广播轨道信息。
  - 精密星历（EPH）：高精度卫星轨道数据。


### 3.1.2 RTKCONV 数据转换

![19792c3d48403c733efcf69677e4863b](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/19792c3d48403c733efcf69677e4863b.png)

**1. RTKCONV使用步骤**

有关 RTKCONV 的更多内容，请查看 [RTKLIB-Manual-CN 3.6节](/algorithm/RTKLIB-Manual-CN/03-instructions-3.6.html)。

- **设置参数**：
  - **时间范围**：选择数据的起止时间。
  - **采样率**：设置 Interval（如1秒、5秒）。
  - **输入文件**：指定原始数据路径。
  - **格式选择**：支持 u-blox、RINEX、RTCM3 等，未知格式可选“Auto”。
  - **输出数据**：勾选所需文件，常用OBS（观测数据），如需导航电文（NAV），需勾选。
- **配置信息**：
  - **RINEX版本**：选择版本号（如 3.03）。
  - **测站信息**：测站 ID、RunBy（可填个人/机构）、天线/接收机类型（可选）、近似坐标、改正信息。
  - **输出选项**：选择 GNSS 系统（如 GPS、GLONASS）、观测值类型（全选推荐）、观测频率、信号通道。
- **执行转换**：点击 “Convert” 生成 RINEX 文件。

**2. 数据质量可视化**

点击“Plot”可以查看数据质量，有关该部分的更多内容请查看 [RTKLIB-Manual-CN 3.8 节](/algorithm/RTKLIB-Manual-CN/03-instructions-3.8.html)。

- **Sat Vis**：卫星可见性，显示频率，颜色表示信噪比（SNR）。
- **Skyplot**：站心地平坐标系下的卫星天空视图，反映低高度角卫星信号质量。
- **DOP**：上部显示可见卫星数，下部显示DOP值（几何精度因子）。
- **SNR**：载噪比及多路径分析，可选特定卫星/频率，横坐标支持时间或高度角。

### 3.1.3 RTKPOST 数据后处理

![image-20231026163551141](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231026163551141.png)

**1. 主界面操作**

RTKPOST 一定程度上可以参考 RTKNAVI 的使用，这部分可以参考 [2.1 RTKNAVI 使用示例](/algorithm/RTKLIB-Source-Notes/02-rtknavi-rtkrcv.html#_2-1-rtknavi-使用示例)。不过 RTKPOST 解算会更快，并且其中包含了只有后处理才能拥有的一些手段。参数部分使用可以参考 [附录 F 配置文件](/algorithm/RTKLIB-Manual-CN/10-appendixF.html)。

- **设置解算参数**：
  - **起止时间**：指定解算时间范围。
  - **解算间隔**：设置采样率。
- **加载数据**：
  - **RINEX OBS**：加载流动站（Rover）和基准站（Base）观测数据。点击右上角天空图标，打开 RTKPLOT 查看数据状态。基准站数据量大，可截取与流动站对应的时间段。
  - **其他数据**：加载NAV（导航电文）、CLK（精密钟差）、SP3（精密轨道）等。接收机NAV仅包含其观测卫星星历，可从网上下载全系统导航电文。
- **输出路径**：默认在流动站文件路径，输出文件后缀为.pos。

**2. 结果可视化**

* **Plot**：对解算结果进行可视化分析，查看结果的状态（固定或浮点解）
* **view**：查看解算结果的文本信息，类似记事本
* **KML**：转为 GoogleKML 可把结果展示到地图上

**3. PPP 数据处理**

* 实时PPP：IGS/MGEX 分析中心播发的实时卫星轨道和钟差产品，结合广播星历
* 事后或近实时：下载精密星历、钟差产品，结合其它精密改正信息实现定位
* RTKLIB 使用必须给广播星历，因为解算前都会先进行一次伪距单点定位

## 3.2 源码解析

`rtkpos()` 算法核心函数将在后续相对定位章节进行解析，这里仅分析算法以外的业务函数。

### 3.2.1 rnx2rtkp()：主函数

**1. 执行流程**

* 读取配置文件过程：
  * 循环判断参数是否有 `-k`，如果有就代表传入了配置文件，需要读取进来
  * 创建 `porcopt_t`、`solopt_t`、`filopt_t` 变量用于接受读取到的配置
  * 调用 `resetsysopts()` ，重置配置为默认值。
  * 调用 `loadopts()`，从文件中读取配置，存到 `opt_t` 类型的 `sysopt` 中。
  * 调用 `getsysopts()`，把 `opt_t` 类型的 `sysopt` 转到 `porcopt_t`、`solopt_t`、`filopt_t` 变量中，会调用 `buff2sysopts()`。
* 读其它参数：
  * 循环内，`if-else if`，判断参数，根据参数赋值
  * 若都不是参数，最后一个到 `else if`，认为是文件路径，用 `infile` 数组接收
* 最后调用 `postpos()` 后处理解算

**2. 源码注释**

::: details 点击查看代码
```c
int main(int argc, char **argv)
{
    prcopt_t prcopt=prcopt_default;     // 定位处理模式
    solopt_t solopt=solopt_default;     // 结果输出形式
    filopt_t filopt={""};               // 文件路径选项
    gtime_t ts={0},te={0};              // ts开始时间、te结束时间
    double tint=0.0,
            es[]={2000,1,1,0,0,0},
            ee[]={2000,12,31,23,59,59},
            pos[3];
    int i,      // for循环的计数
        j,      // 嵌套的for循环计数
        n,      // 记录读入文件数
        ret;    // 接受postpos的返回值
    char *infile[MAXFILE],  // 读入文件，默认最多16个，可改MAXFILE定义
         *outfile="",       // 输出文件
         *p;                // 指向字符串的指针，用于循环指向各main函数参数
    
    prcopt.mode  =PMODE_KINEMA;     // 定位模式默认动态相对定位Kinematic
    prcopt.navsys=0;                // 卫星系统，先设置无
    prcopt.refpos=1;                // 基准站坐标，先设为由SPP平均解得到
    prcopt.glomodear=1;             // GLONASS AR mode,先设on
    solopt.timef=0;                 // 输出时间格式，先设sssss.s
    sprintf(solopt.prog ,"%s ver.%s %s",PROGNAME,VER_RTKLIB,PATCH_LEVEL);   // 项目名称
    sprintf(filopt.trace,"%s.trace",PROGNAME);
    
    /* load options from configuration file */
    for (i=1;i<argc;i++) {
        if (!strcmp(argv[i],"-k")&&i+1<argc) {          // 如果有-k和配置文件输入
            resetsysopts();                             // 先重置所有配置
            if (!loadopts(argv[++i],sysopts)) return -1;// 再读取配置文件内容，存入opt_t的sysopt中
            getsysopts(&prcopt,&solopt,&filopt);        // opt_t转到porcopt_t/solopt_t/filopt_t，
        }   
    }          
    // for 循环判断 main 函数参数
    for (i=1,n=0;i<argc;i++) {  
        if      (!strcmp(argv[i],"-o")&&i+1<argc) outfile=argv[++i];//读取输出文件路径，赋值给outfile
        else if (!strcmp(argv[i],"-ts")&&i+2<argc) {    // 读取开始解算时间   
            sscanf(argv[++i],"%lf/%lf/%lf",es,es+1,es+2);
            sscanf(argv[++i],"%lf:%lf:%lf",es+3,es+4,es+5);
            ts=epoch2time(es);      // 转为gtime_t
        }
        else if (!strcmp(argv[i],"-te")&&i+2<argc) {    // 读取结束解算时间
            sscanf(argv[++i],"%lf/%lf/%lf",ee,ee+1,ee+2);   
            sscanf(argv[++i],"%lf:%lf:%lf",ee+3,ee+4,ee+5);
            te=epoch2time(ee);  // 转为gtime_t
        }
        else if (!strcmp(argv[i],"-ti")&&i+1<argc) tint=atof(argv[++i]);        // 读取解算时间间隔频率
        else if (!strcmp(argv[i],"-k")&&i+1<argc) {++i; continue;}              // 有-k，跳过
        else if (!strcmp(argv[i],"-p")&&i+1<argc) prcopt.mode=atoi(argv[++i]);  // 读取解算模式
        else if (!strcmp(argv[i],"-f")&&i+1<argc) prcopt.nf=atoi(argv[++i]);    // 读取用于计算的频率
        else if (!strcmp(argv[i],"-sys")&&i+1<argc) {       // 读取用于计算的导航系统
            for (p=argv[++i];*p;p++) {      
                switch (*p) {                           //有对应导航系统，就把它的码做与运算加上                 
                    case 'G': prcopt.navsys|=SYS_GPS;
                    case 'R': prcopt.navsys|=SYS_GLO;
                    case 'E': prcopt.navsys|=SYS_GAL;
                    case 'J': prcopt.navsys|=SYS_QZS;
                    case 'C': prcopt.navsys|=SYS_CMP;
                    case 'I': prcopt.navsys|=SYS_IRN;
                }
                if (!(p=strchr(p,','))) break;  
            }
        }
        else if (!strcmp(argv[i],"-m")&&i+1<argc) prcopt.elmin=atof(argv[++i])*D2R;     // 设置截止高度角     
        else if (!strcmp(argv[i],"-v")&&i+1<argc) prcopt.thresar[0]=atof(argv[++i]);    // 设置整周模糊度Ratio值
        else if (!strcmp(argv[i],"-s")&&i+1<argc) strcpy(solopt.sep,argv[++i]);         // 设置文件路径分隔符
        else if (!strcmp(argv[i],"-d")&&i+1<argc) solopt.timeu=atoi(argv[++i]);         // 设置时间小数位数
        else if (!strcmp(argv[i],"-b")) prcopt.soltype=1;           // 后向滤波
        else if (!strcmp(argv[i],"-c")) prcopt.soltype=2;           // 前后向滤波组合
        else if (!strcmp(argv[i],"-i")) prcopt.modear=2;            // 单历元模糊度固定
        else if (!strcmp(argv[i],"-h")) prcopt.modear=3;            // fix and hold 模糊度固定
        else if (!strcmp(argv[i],"-t")) solopt.timef=1;             // 输出时间格式为 yyyy/mm/dd hh:mm:ss.ss
        else if (!strcmp(argv[i],"-u")) solopt.times=TIMES_UTC;     // 输出为 UTC 时间
        else if (!strcmp(argv[i],"-e")) solopt.posf=SOLF_XYZ;       // 输出 XYZ-ecef 坐标
        else if (!strcmp(argv[i],"-a")) solopt.posf=SOLF_ENU;       // 输出 ENU-baseline
        else if (!strcmp(argv[i],"-n")) solopt.posf=SOLF_NMEA;      // 输出 NMEA-0183 GGA
        else if (!strcmp(argv[i],"-g")) solopt.degf=1;              // 输出经纬度格式为 ddd mm ss.ss
        else if (!strcmp(argv[i],"-r")&&i+3<argc) {                 // 基站位置E CEF-XYZ (m)              
            prcopt.refpos=prcopt.rovpos=0;                  // 基准站和流动站位置都先设0
            for (j=0;j<3;j++) prcopt.rb[j]=atof(argv[++i]); // 循环存入基准站坐标
            matcpy(prcopt.ru,prcopt.rb,3,1);    
        }
        else if (!strcmp(argv[i],"-l")&&i+3<argc) {     // 循环存入基站位置基站位置LLH (deg/m)
            prcopt.refpos=prcopt.rovpos=0;              // 基准站和流动站位置都先设0
            for (j=0;j<3;j++) pos[j]=atof(argv[++i]);   
            for (j=0;j<2;j++) pos[j]*=D2R;              // 角度转弧度   
            pos2ecef(pos,prcopt.rb);                    // LLH 转 XYZ
            matcpy(prcopt.ru,prcopt.rb,3,1);
        }
        else if (!strcmp(argv[i],"-y")&&i+1<argc) solopt.sstat=atoi(argv[++i]); //输出结果信息
        else if (!strcmp(argv[i],"-x")&&i+1<argc) solopt.trace=atoi(argv[++i]); //输出debug trace等级
        else if (*argv[i]=='-') printhelp();        //输入-，打印帮助
        else if (n<MAXFILE) infile[n++]=argv[i];    //循环判断完一遍参数之后，认为参数是文件路径，用infile数组接收
    }
    if (!prcopt.navsys) {               //如果没设卫星系统，默认为GPS、GLONASS
        prcopt.navsys=SYS_GPS|SYS_GLO;
    }
    if (n<=0) {         //如果读入文件数为0,报错，-2退出
        showmsg("error : no input file");
        return -2;
    }

    
    //   gtime_t ts       I   processing start time (ts.time==0: no limit)
    //   gtime_t te       I   processing end time   (te.time==0: no limit)
    //   double ti        I   processing interval  (s) (0:all)
    //   double tu        I   processing unit time (s) (0:all)
    //   prcopt_t *popt   I   processing options
    //   solopt_t *sopt   I   solution options
    //   filopt_t *fopt   I   file options
    //   char   **infile  I   input files (see below)
    //   int    n         I   number of input files
    //   char   *outfile  I   output file ("":stdout, see below)
    //   char   *rov      I   rover id list        (separated by " ")
    //   char   *base     I   base station id list (separated by " ")
    //后处理定位解算
    ret=postpos(ts,te,tint,0.0,&prcopt,&solopt,&filopt,infile,n,outfile,"",""); 
    
    if (!ret) fprintf(stderr,"%40s\r","");
    return ret;
}
```
:::

### 3.2.2 postpos()：后处理定位解算

**1. 概述**

![image-20241006135819519](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20241006135819519.png)

后处理定位的主入口函数，根据 tu 分计算时间段，调用调用`execses_b()`进行下一步解算，输入文件包括观测文件、导航文件、精密星历文件等，postpos 在处理输入文件时有两种方法：

* 一种是输入文件可以只包含替换，然后通过函数`reppath()`处理，将关键词用时间、基准站编号、流动站编号等代替；
* 另一种是直接调用输入文件的文件名，

**2. 参数列表**

```c
/* args */
gtime_t   ts       I   处理的起始时间，写 0 表示不限制
gtime_t   te       I   处理的起始时间，写 0 表示不限制
double    ti       I   处理的间隔时间（s），写 0 表示不限制，全处理
double    tu       I   处理的单元时间（s），写 0 表示全部做一个单元处理
prcopt_t *popt     I   处理选项结构体
solopt_t *sopt     I   结果选项结构体
filopt_t *fopt     I   文件选项结构体
char    **infile   I   传入文件路径数组首地址
int       n        I   传入文件数量
char     *outfile  I   输出文件的路径，写 0 表示 stdout 终端
char     *rov      I   流动站 ID 列表，空格隔开
char     *base     I   基准站 ID 列表，空格隔开
/* return */
int       ret      -   0：成功，0>：失败，1：中断
```

**3. 执行流程**

- 变量定义，`stat`默认为0，`flag`默认为1。
- 调用`openses()`，开始解算进程，读取天线、大地水准面文件。
- 判断起始解算时间`ts`、结束解算时间`te`、解算时间单元`tu`，有三种情况：
  > * 为何要判断：拆分时间段解算需要tu值有效、调用reppath需要ts有效，调用reppaths需要ts和te有效。
  > * ifile[]、ofile[]作用：infile[]、ofile[]里的路径替换处理后存到ifile[]、ofile[]，传入`execses_b()`进行之后的解算。
  > * index[]的作用：会传给`execses_b()`，再传给`execses_r()`，再传给`execses()`，再传给`readobsnav() `。如果不需要根据tu分时间段解算，index存的就是0~n，如果需要分时间段解算，index存的是对应时间段内文件的下标。

  - **情况1：若`ts`、`te`不为0，`tu`大于等于0**：
    * 判断`te`早于`ts`，return
    * 为`ifile[]`数组空间
    * 处理解算时间单元`tu`，0或者时间大于100天，设为100天 
    * 循环处理每个时间单元`tts`到`tte`：
     * 计算解算时间单元的开始`tts`、结束`tte`，判断`tts<ts`则设为`ts`，`tte>te`设为`te`
       * 流动站、基准站名赋空值								
     * 遍历遍历infile[]，`strrchr`找文件后缀名，`strcmp`判断后缀名 ：
       * rtcm3：直接把`infile[j]`中路径赋值到`ifile[]`中
       * 星历文件：精密星历`ttte=tte+一小时`、广播星历`ttte=tte+两小时`，根据`tts`、`ttte`调用`reppaths()`将infile[j]中路径展开到`ifile[nf]`中。
       之后把`infile[]`的下标`j`存到`index[]`中。
     * 调用`reppath()`替换`outfile`的替换符，存到ofile中。
     * 调用`execses_b()`进行下一步解算。

  - **情况2：若`ts`不为0，`tu`为0或小于0** ：就不考虑`te`、和`tu`
    * 为`ifile[]`开辟空间，循环替换`infile[i]`的替换符到`ifile[i]`中。
    * 调用`reppath`替换outfile的替换符，存到ofile中。
    * 调用`execses_b()`进行下一步解算。

  - **情况3：若`ts`为0**：
    - 直接把把`infile[]`的下标`j`存到`index[]`中，调用`execses_b` 进行下一步解算

**4. 注意事项**

- `postpos()` 冗长的代码段主要是来判断是哪一种输入方式，然后调用相应函数进行处理，一般不用看太仔细的去看。
- `postpos()` 调用了一些小函数，这样将不会详细展开：
  * **`openses()`**：开始解算进程，读取天线、大地水准面文件
    * `readpcv()`：读取天线文件，会调用 `readantex()`、`readngspcv()`
    * `opengeoid()`：读取 geoid 文件，会调用 `closegeoid()`
  * **`closeses()`**：结束解算程序，释放天线、`geoid`、`erp`、`trace`、`fp_stat` 。会调用 `closegeoid()`、`rtkclosestat()`、`traceclose()`。
  * **`reppaths()`**：根据 `ts` 和 `te`分时间段，循环调用 `reppath()`，替换 `path[]` 中的替换符，存到 `repath[]` 中，返回文件数量
    * **`reppath()`**：如果输入文件（file）中，含有替换符，则 `reppath()` 函数的目的就是将文件名中的替换符调用 `repstr()` 进行替换，保存到 `rpath` 中。替换符如下：
    > `reppaths()` 需要 `ts` 和 `te`、而 `reppath()` 只用 `ts`。

**5. 源码注释**

::: details 点击查看代码
```c
extern int postpos(gtime_t ts, gtime_t te, double ti, double tu,
                   const prcopt_t *popt, const solopt_t *sopt,
                   const filopt_t *fopt, char **infile, int n, char *outfile,
                   const char *rov, const char *base)
{
    gtime_t tts,    //解算单元的开始时间
            tte,    //解算单元的结束时间
            ttte;   //读取星历文件的结束时间
    double tunit,   //
           tss;     //
    int i,j,k,      //循环和数组下标控制
            nf,     //文件路径数组下标控制
        stat=0,     //接收返回状态值，为1
        week,       //用于存GPST的周
        flag=1,
        index[MAXINFILE]={0};
    char *ifile[MAXINFILE],
          ofile[1024],
          *ext;
    
    trace(3,"postpos : ti=%.0f tu=%.0f n=%d outfile=%s\n",ti,tu,n,outfile);
    
    /* open processing session */   //开始处理,文件读取，赋值navs、pcvs、pcvsr
    if (!openses(popt,sopt,fopt,&navs,&pcvss,&pcvsr)) return -1;
    
    if (ts.time!=0&&te.time!=0&&tu>=0.0) {  //判断起始时间ts、te、处理单位时间是否大于0
        if (timediff(te,ts)<0.0) {  //结束时间早于开始时间
            showmsg("error : no period");
            closeses(&navs,&pcvss,&pcvsr);  //不合理则关闭处理，释放navs、pcvs、pcvsr
            return 0;
        }
        for (i=0;i<MAXINFILE;i++) {
            if (!(ifile[i]=(char *)malloc(1024))) { //为infile数组malloc开辟空间
                for (;i>=0;i--) free(ifile[i]);     //开辟失败则释放已开辟的空间，关闭处理释放navs、pcvs、pcvsr
                closeses(&navs,&pcvss,&pcvsr);
                return -1;
            }
        }
        if (tu==0.0||tu>86400.0*MAXPRCDAYS) tu=86400.0*MAXPRCDAYS;  //解算处理时间单元处理，0或者时间大于100天，设为100天
        settspan(ts,te);    //设置时间跨度，好像是空函数，需要自己实现
        
        tunit=tu<86400.0?tu:86400.0;    //tunit：如果tu小于一天就为tu；否则为一天
        tss=tunit*(int)floor(time2gpst(ts,&week)/tunit);   //
        
        //根据解算时间单元，分时间段循环处理，算出来tts>te或过程有错误，结束循环
        //很多时候解算单元时间直接设0.0，只循环一次，tts=ts，tte=te
        for (i=0;;i++) { /* for each periods */
            tts=gpst2time(week,tss+i*tu);       //解算单元开始时间，每次循环加上一个i个tu？
            tte=timeadd(tts,tu-DTTOL);          //解算结束时间tte=tu-DTTOL
            if (timediff(tts,te)>0.0) break;   //算出来tts>te结束循环
            if (timediff(tts,ts)<0.0) tts=ts;   //分时间段后tts若早于ts，设为ts
            if (timediff(tte,te)>0.0) tte=te;   //分时间段后tte若早于te，设为te
            
            strcpy(proc_rov ,"");   //流动站、基准站值赋空
            strcpy(proc_base,"");   
            if (checkbrk("reading    : %s",time_str(tts,0))) {
                stat=1;
                break;
            }
            for (j=k=nf=0;j<n;j++) {    //遍历infile[]，根据后缀名
                
                ext=strrchr(infile[j],'.'); //ext：文件路径中.后缀开始的位置
                
                if (ext&&(!strcmp(ext,".rtcm3")||!strcmp(ext,".RTCM3"))) {  //rtcm3文件
                    strcpy(ifile[nf++],infile[j]);
                }
                else {      //星历文件，包括精密星历和广播星历
                    /* include next day precise ephemeris or rinex brdc nav */
                    ttte=tte;
                    if (ext&&(!strcmp(ext,".sp3")||!strcmp(ext,".SP3")||
                              !strcmp(ext,".eph")||!strcmp(ext,".EPH"))) {
                        ttte=timeadd(ttte,3600.0);  //精密星历加一小时
                    }
                    else if (strstr(infile[j],"brdc")) {
                        ttte=timeadd(ttte,7200.0);  //广播星历加两小时
                    }
                    nf+=reppaths(infile[j],ifile+nf,MAXINFILE-nf,tts,ttte,"","");
                }
                while (k<nf) index[k++]=j;
                
                if (nf>=MAXINFILE) {
                    trace(2,"too many input files. trancated\n");
                    break;
                }
            }
            if (!reppath(outfile,ofile,tts,"","")&&i>0) flag=0;
            
            /* execute processing session */
            stat=execses_b(tts,tte,ti,popt,sopt,fopt,flag,ifile,index,nf,ofile,
                           rov,base);
            
            if (stat==1) break;
        }

        for (i=0;i<MAXINFILE;i++) free(ifile[i]);
    }
    else if (ts.time!=0) {  //如果起始时间不为0，结束时间为0或处理单元时间小于0
        for (i=0;i<n&&i<MAXINFILE;i++) {
            if (!(ifile[i]=(char *)malloc(1024))) {
                for (;i>=0;i--) free(ifile[i]);
                return -1;
            }
            reppath(infile[i],ifile[i],ts,"","");
            index[i]=i;
        }
        reppath(outfile,ofile,ts,"","");
        
        /* execute processing session */
        stat=execses_b(ts,te,ti,popt,sopt,fopt,1,ifile,index,n,ofile,rov,
                       base);
        
        for (i=0;i<n&&i<MAXINFILE;i++) free(ifile[i]);
    }
    else {  //如果起始时间为0
        for (i=0;i<n;i++) index[i]=i;
        
        /* execute processing session */
        stat=execses_b(ts,te,ti,popt,sopt,fopt,1,infile,index,n,outfile,rov,
                       base);
    }
    /* close processing session */
    closeses(&navs,&pcvss,&pcvsr);
    
    return stat;
}
```
:::

### 3.2.3 execses_*()：流动站和基准站处理

**1. 功能**

![image-20241006135943200](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20241006135943200.png)

execses_b() 和 execses_r() 函数非常类似，execsec_b()会调用调用`readpreceph()`读取精密星历和SBAS数据，把传入`infile[]`文件中基准站替换符进行替换，之后调用`execses_r()`。`execses_r()`把传入`infile[]`文件中流动站站替换符进行替换，再调用`execses()`

**2. 参数列表**

```c
/* args */
gtime_t ts              I   处理的起始时间，写0表示不限制
gtime_t te              I   处理的起始时间，写0表示不限制
double ti               I   处理的间隔时间 (s)，写0表示不限制，全处理
const prcopt_t *popt    I   处理选项结构体
const solopt_t *sopt    I   结果选项结构体
const filopt_t *fopt    I   文件选项结构体
int flag                I   用于控制输出
char **infile           I   传入文件路径数组首地址
const int *index        I   传入文件路径数组首地址
int n                   I   传入文件数量
char *outfile           I   输出文件的路径，写0表示stdout终端
const char *rov         I   流动站ID列表，空格隔开
const char *base        I   基准站ID列表，空格隔开
```

* 参数flag：
  * 传入execses_r()，再传入execses()，用于控制输出，如果值为0,很多不输出；
  * 在postpos函数中赋值传入，替换输出文件替换符出错的时候设为0，其它情况为1

**3. 执行流程**

- 调用 `readpreceph()` 读取精密星历和 SBAS 数据。
- 遍历 `infile[]`，寻找基准站替换符 %b：
  * 找不到基准站 ID 的替换符，直接调用 `execses_r()` 进行下一步解算 。
  * 找到了 `infile[i]` 含有基准站 ID 的替换符，遍历基准站：
   * 将基准站 ID 赋值给 `proc_base`。
   * 循环替换 `infile[i]` 里的基准站ID的替换符到 `ifile[i]`。
   * 替换 `outfile` 里的基准站ID替换符到 `ofile`。
   * 调用 `execses_r()` 进行下一步解算 。
- 调用 `freepreceph()`，释放 `readpreceph()` 开辟的空间。

**4. 注意事项**

* **readpreceph()**：遍历infile[]，判断，调用readsp3()读取精密星历、调用readrnxc() 读取精密钟差，调用sbsreadmsg()读取sbas文件，将RCTM的路径赋值给rtcm_file，调用init_rtcm()初始化rtcm控制结构体。

**5. 源码注释**

::: details 点击查看代码：execses_b()
```c
static int execses_b(gtime_t ts, gtime_t te, double ti, const prcopt_t *popt,
                     const solopt_t *sopt, const filopt_t *fopt, int flag,
                     char **infile, const int *index, int n, char *outfile,
                     const char *rov, const char *base)
{
    gtime_t t0={0};
    int i,stat=0;
    char *ifile[MAXINFILE],ofile[1024], *base_,*p,*q,s[64];
    
    trace(3,"execses_b: n=%d outfile=%s\n",n,outfile);
    
    /* read prec ephemeris and sbas data */
    readpreceph(infile,n,popt,&navs,&sbss); //读取精密星历和SBAS数据
    
    //%b：基准站ID的替换符
    for (i=0;i<n;i++) if (strstr(infile[i],"%b")) break;
    //如果某个infile[i]含有基准站ID的替换符
    if (i<n) { /* include base station keywords */
        //为base_开辟空间，将base赋值给base_
        if (!(base_=(char *)malloc(strlen(base)+1))) {  
            freepreceph(&navs,&sbss);
            return 0;
        }
        strcpy(base_,base); 
        
        for (i=0;i<n;i++) {     //为ifile[]开辟空间
            if (!(ifile[i]=(char *)malloc(1024))) { 
                free(base_); for (;i>=0;i--) free(ifile[i]);
                freepreceph(&navs,&sbss);
                return 0;
            }
        }
        //遍历base_基准站字符串
        for (p=base_;;p=q+1) { /* for each base station */
            if ((q=strchr(p,' '))) *q='\0'; //拆出一个基准站
            
            if (*p) {   
                strcpy(proc_base,p);    //把基准站名赋值给proc_base
                if (ts.time) time2str(ts,s,0); else *s='\0';
                if (checkbrk("reading    : %s",s)) {
                    stat=1;
                    break;
                }
                //循环替换infile[i]里的基准站ID的替换符到ifile[i]
                for (i=0;i<n;i++) reppath(infile[i],ifile[i],t0,"",p);
                //替换outfile里的基准站ID替换符到ofile
                reppath(outfile,ofile,t0,"",p); 
                //调用execses_r()进行下一步解算
                stat=execses_r(ts,te,ti,popt,sopt,fopt,flag,ifile,index,n,ofile,rov);
            }
            if (stat==1||!q) break;
        }
        free(base_); for (i=0;i<n;i++) free(ifile[i]);
    }
    else {  //infile[i]都没有有基准站ID的替换符，直接调用execses_r()进行下一步解算
        stat=execses_r(ts,te,ti,popt,sopt,fopt,flag,infile,index,n,outfile,rov);
    }
    /* free prec ephemeris and sbas data */
    freepreceph(&navs,&sbss);
    
    return stat;
}
```
:::

::: details 点击查看代码：execses_r()
```c
static int execses_r(gtime_t ts, gtime_t te, double ti, const prcopt_t *popt,
                     const solopt_t *sopt, const filopt_t *fopt, int flag,
                     char **infile, const int *index, int n, char *outfile,
                     const char *rov)
{
    gtime_t t0={0};
    int i,stat=0;
    char *ifile[MAXINFILE],ofile[1024],*rov_,*p,*q,s[64]="";
    
    trace(3,"execses_r: n=%d outfile=%s\n",n,outfile);
    
    for (i=0;i<n;i++) if (strstr(infile[i],"%r")) break;
    
    //如果某个infile[i]含有基准站ID的替换符
    if (i<n) { /* include rover keywords */
        if (!(rov_=(char *)malloc(strlen(rov)+1))) return 0;
        strcpy(rov_,rov);
        
        for (i=0;i<n;i++) {
            if (!(ifile[i]=(char *)malloc(1024))) {
                free(rov_); for (;i>=0;i--) free(ifile[i]);
                return 0;
            }
        }
        for (p=rov_;;p=q+1) { /* for each rover */
            if ((q=strchr(p,' '))) *q='\0';
            
            if (*p) {
                strcpy(proc_rov,p);
                if (ts.time) time2str(ts,s,0); else *s='\0';
                if (checkbrk("reading    : %s",s)) {
                    stat=1;
                    break;
                }
                for (i=0;i<n;i++) reppath(infile[i],ifile[i],t0,p,"");
                reppath(outfile,ofile,t0,p,"");
                
                /* execute processing session */
                stat=execses(ts,te,ti,popt,sopt,fopt,flag,ifile,index,n,ofile);
            }
            if (stat==1||!q) break;
        }
        free(rov_); for (i=0;i<n;i++) free(ifile[i]);
    }
    else {
        /* execute processing session */
        stat=execses(ts,te,ti,popt,sopt,fopt,flag,infile,index,n,outfile);
    }
    return stat;
}
```
:::

### 3.2.4 execses()：定位解算处理

**1. 功能**

![image-20241006140111009](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20241006140111009.png)

读取各种文件，并将文件中的内容赋值到程序的结构体内，获取基准站的位置，根据滤波方向调用 `procpos()` 进行下一步解算。.trace文件的生成、文件读取相关trace文件内容的生成，均在 execses() 中 。

**2. 输入参数**

```c
gtime_t ts              I   处理的起始时间，写0表示不限制
gtime_t te              I   处理的起始时间，写0表示不限制
double ti               I   处理的间隔时间 (s)，写0表示不限制，全处理
const prcopt_t *popt    I   处理选项结构体
const solopt_t *sopt    I   结果选项结构体
const filopt_t *fopt    I   文件选项结构体
int flag                I   用于控制输出
char **infile           I   传入文件路径数组首地址
const int *index        I   传入文件路径数组首地址
int n                   I   传入文件数量
char *outfile           I   输出文件的路径，写0表示stdout终端
const char *rov         I   流动站ID列表，空格隔开
const char *base        I   基准站ID列表，空格隔开
```

**3. 执行流程**

* 调用`traceclose() `、`traceopen()` 、`tracelevel()`，先关闭原有trace，打开trace文件，并设置trace等级。
* 调用`readtec()` ，读取电离层TEC文件，TEC:Total electronic content 总电子含量 。
* 调用`readerp()`，读取地球自转参数ERP文件。
* 调用`readobsnav() `，读取OBS和NAV文件 。
* 调用`readdcb()`，读取差分码偏差DCB参数，一种硬件误差 。
* 调用`setpcv()`，读取天线参数，PCV：天线相位中心变化 。
* 调用`readotl()`，读取潮汐参数 。
* FIXED模式，调用`antpos()`得到流动站坐标 。
* DGPS、KINEMA、STATIC模式，调用`antpos()`得到基准站坐标 。
* 调用`rtkclosestat() `、`rtkopenstat()`，打开结果统计文件 。
* 调用`outhead()`，写输出结果文件的文件头 。结果文件的文件尾在`procpos()`内调用`outsol()`输出。
* 判断滤波类型，用不同的方式调用`procpos()`进行下一步解算：
    > 前向滤波和后向滤波调用procpos函数传参相同，两者区别在于procpos函数内会调用inputobs函数，针对不同的滤波解算类型，inputobs函数内读取文件数据的顺序不同。<br>
    > revs：0：forward；1：backward<br>
    > iobsu：当前流动站观测数据下标<br>
    > iobsr：当前参考站观测数据下标<br>
    > isbs：当前sbas数据下标<br>

    * forward 前向滤波：iobsu=iobsr=isbs=revs 0，直接调用`procpos()`。
    * backward 后向滤波：res=1，iobsu=iobsr=obss.n-1 ，isbs=sbss.n-1 ，再调用`procpos()`。
    * combined ：先算前向滤波的结果，设置revs、iobsu、iobsr、isbs值之后再算后向滤波的结果，最后调用combress()结合。
* 调用`freeobsnav()`释放obs->data 、nav->eph 、nav->geph 、nav->seph

**4. 注意事项**

* **antpos()**：得到坐标，参2`rcvno`传1得到流动站坐标，传0得到基准站坐标
  * postype=POSOPT_SINGLE ：调用`avepos()`利用基准站的观测文件计算其SPP定位结果作为基准站的坐标 。
  * postype=POSOPT_FILE ：调用`getstapos()`从pos文件读取基准站坐标 。
  * postype=POSOPT_RINEX ：从rinex头文件中获取测站经过相位中心改正的位置数据。头文件中的测站数据经过读取后已存到stas中。
* **avepos()**：通过nav和多个obs单点定位计算位置，存到ra[]中 
* **getstapos()**：从pos文件读取基准站坐标
* **outhead()**：创建输出结果文件，写入文件头
* **openfile()**：以追加的方式打开结果文件，返回文件描述符
* **combres()**：调用smoother()结合前后向滤波的结果
* **valcomb()**：判断前后向滤波合并后结果的有效性（ $4 \sigma$ 以内有效）

**4. 源码注释**

::: details 点击查看代码
```c
static int execses(gtime_t ts, gtime_t te, double ti, const prcopt_t *popt,
                   const solopt_t *sopt, const filopt_t *fopt, int flag,
                   char **infile, const int *index, int n, char *outfile)
{
    FILE *fp;
    prcopt_t popt_=*popt;
    char tracefile[1024],statfile[1024],path[1024],*ext;
    
    trace(3,"execses : n=%d outfile=%s\n",n,outfile);
    
    /* open debug trace */  //打开trace文件，并设置trace等级
    if (flag&&sopt->trace>0) {
        if (*outfile) {
            strcpy(tracefile,outfile);
            strcat(tracefile,".trace");
        }
        else {
            strcpy(tracefile,fopt->trace);
        }
        traceclose();
        traceopen(tracefile);
        tracelevel(sopt->trace);
    }
    /* read ionosphere data file */ //读取电离层TEC文件
    if (*fopt->iono&&(ext=strrchr(fopt->iono,'.'))) {
        if (strlen(ext)==4&&(ext[3]=='i'||ext[3]=='I')) {
            reppath(fopt->iono,path,ts,"","");
            readtec(path,&navs,1);  //TEC:Total electronic content 总电子含量
        }
    }
    /* read erp data */ //读取地球自转参数ERP文件
    if (*fopt->eop) {
        free(navs.erp.data); navs.erp.data=NULL; navs.erp.n=navs.erp.nmax=0;
        reppath(fopt->eop,path,ts,"","");
        if (!readerp(path,&navs.erp)) {
            showmsg("error : no erp data %s",path);
            trace(2,"no erp data %s\n",path);
        }
    }
    /* read obs and nav data */ //读取OBS和NAV文件
    if (!readobsnav(ts,te,ti,infile,index,n,&popt_,&obss,&navs,stas)) return 0;
    
    /* read dcb parameters */   //读取差分码偏差DCB参数，一种硬件误差
    if (*fopt->dcb) {
        reppath(fopt->dcb,path,ts,"","");
        readdcb(path,&navs,stas);
    }
    /* set antenna paramters */ //读取天线参数，PCV：天线相位中心变化
    if (popt_.mode!=PMODE_SINGLE) {
        setpcv(obss.n>0?obss.data[0].time:timeget(),&popt_,&navs,&pcvss,&pcvsr,
               stas);
    }
    /* read ocean tide loading parameters */    //读取潮汐参数
    if (popt_.mode>PMODE_SINGLE&&*fopt->blq) {
        readotl(&popt_,fopt->blq,stas);
    }
    /* rover/reference fixed position */    //FIXED模式，调用antpos()得到流动站坐标
    if (popt_.mode==PMODE_FIXED) {
        if (!antpos(&popt_,1,&obss,&navs,stas,fopt->stapos)) {
            freeobsnav(&obss,&navs);
            return 0;
        }
    }
    else if (PMODE_DGPS<=popt_.mode&&popt_.mode<=PMODE_STATIC) {    //DGPS、KINEMA、STATIC模式，调用antpos()得到基准站坐标
        if (!antpos(&popt_,2,&obss,&navs,stas,fopt->stapos)) {
            freeobsnav(&obss,&navs);
            return 0;
        }
    }
    /* open solution statistics */  //打开结果统计文件
    if (flag&&sopt->sstat>0) {
        strcpy(statfile,outfile);
        strcat(statfile,".stat");
        rtkclosestat();
        rtkopenstat(statfile,sopt->sstat);
    }
    /* write header to output file */   //写输出结果文件的文件头
    if (flag&&!outhead(outfile,infile,n,&popt_,sopt)) {
        freeobsnav(&obss,&navs);
        return 0;
    }
    iobsu=iobsr=isbs=revs=aborts=0;
    
    if (popt_.mode==PMODE_SINGLE||popt_.soltype==0) {
        if ((fp=openfile(outfile))) {
            procpos(fp,&popt_,sopt,0); /* forward */    //前向滤波
            fclose(fp);
        }
    }
    else if (popt_.soltype==1) {
        if ((fp=openfile(outfile))) {
            revs=1; iobsu=iobsr=obss.n-1; isbs=sbss.n-1;
            procpos(fp,&popt_,sopt,0); /* backward */   //后向滤波
            fclose(fp);
        }
    }
    else { /* combined */
        //开辟内存空间
        solf=(sol_t *)malloc(sizeof(sol_t)*nepoch);     //前向结果
        solb=(sol_t *)malloc(sizeof(sol_t)*nepoch);     //后向结果
        rbf=(double *)malloc(sizeof(double)*nepoch*3);  //前向基准站坐标
        rbb=(double *)malloc(sizeof(double)*nepoch*3);  //后向基准站坐标
        
        if (solf&&solb) {   //判断内存开辟成功
            isolf=isolb=0;
            procpos(NULL,&popt_,sopt,1); /* forward */      //前向滤波
            revs=1; iobsu=iobsr=obss.n-1; isbs=sbss.n-1;
            procpos(NULL,&popt_,sopt,1); /* backward */     //后向滤波

            //虽然前向滤波和后向滤波调用procpos函数的源代码相同（如下所示），
            //但是两者最主要的一个区别就是由于procpos函数内会调用inputobs函数，
            //然而针对不同的滤波解算类型，inputobs函数内读取文件数据的顺序不同
            /* combine forward/backward solutions */
            if (!aborts&&(fp=openfile(outfile))) {
                combres(fp,&popt_,sopt);
                fclose(fp);
            }
        }
        else showmsg("error : memory allocation");
        free(solf);
        free(solb);
        free(rbf);
        free(rbb);
    }
    /* free obs and nav data */
    freeobsnav(&obss,&navs);
    
    return aborts?1:0;
}
```
:::

### 3.2.4 procpos()：定位算法处理

![image-20241006140151333](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20241006140151333.png)

**1. 功能**

从这个函数开始正式整个流动站和基准站逐历元处理。每次循环都通过 `inputobs` 函数读取一个历元的数据，并调用 `rtkpos` 函数对该历元的数据进行解算。 

**2. 参数列表**

```c
FILE *fp	   		   I/O 输出结果文件指针  
const prcopt_t *popt    I   处理选项结构体
const solopt_t *sopt    I   结果选项结构体
const filopt_t *fopt    I   文件选项结构体
int mode			   I   0：forward/backward、1：combined
```

**3. 执行流程**

* 判断结果是否为静态，处理选项和结果选项都为静态才算静态；
* 调用`rtkinit()` 初始化`rtk_t `，将 popt 结构体赋值给 rtk 的部分成员 ；
* while 大循环，调用`inputobs()`，每次取一个历元的观测数据`obs[]`；
* 排除禁用卫星的观测值；
* PPP 中如果需要，调用`corr_phase_bias_ssr()`相位的小数偏差改正（不完善）；
* 调用 `rtkpos()` 对当前历元进行解算 
* 根据模式，输出结果，记录当前历元时间

**4. 注意事项**

* **inputobs()**：取一个历元基准站、流动站的观测数据到OBS数组中；如果需要，调用sbsupdatecorr()、update_rtcm_ssr()进行改正。
  * **nextobsf()**：在obs中正向指定接收机查找下一个历元观测数据的下标 ，下标从 i 开始，连续 n 个，之间卫星不同。
  * **nextobsb()**：在obs反向查找指定接收机下一个历元观测数据的下标 
* **rtkinit()**：初始化rtk_t结构体

**5. 源码注释**

::: details 点击查看代码
```c
static void procpos(FILE *fp, const prcopt_t *popt, const solopt_t *sopt,
                    int mode)
{
    gtime_t time={0};
    sol_t sol={{0}};
    rtk_t rtk;
    obsd_t obs[MAXOBS*2]; /* for rover and base */
    double rb[3]={0};
    int i,
    nobs,
    n,
    solstatic,
    pri[]={6,1,2,3,4,5,1,6};
    
    trace(3,"procpos : mode=%d\n",mode);
    
    solstatic=sopt->solstatic&&     //先判断结果是否为静态,处理选项和结果选项都为静态才算静态
              (popt->mode==PMODE_STATIC||popt->mode==PMODE_PPP_STATIC); 
    
    rtkinit(&rtk,popt);    //初始化rtk_t，主要将popt结构体赋值给rtk的部分成员
    rtcm_path[0]='\0';
    //对每一个历元进行遍历求解和输出
    //获取当前历元观测值数nobs以及当前历元各观测记录obs[MAXOBS*2]
    while ((nobs=inputobs(obs,rtk.sol.stat,popt))>=0) { 
        /* exclude satellites */
        for (i=n=0;i<nobs;i++) {
            //satsys:传入satellite number，返回卫星系统(SYS_GPS,SYS_GLO,...) ，通过传入的指针prn传出PRN码。
            if ((satsys(obs[i].sat,NULL)&popt->navsys)&&
                popt->exsats[obs[i].sat-1]!=1) obs[n++]=obs[i]; //排除禁用卫星的观测值
        }
        if (n<=0) continue;
        //如果ppp模式设置了fractional cycle bias相位的小数轴偏差
        /* carrier-phase bias correction */
        if (!strstr(popt->pppopt,"-ENA_FCB")) {     
            corr_phase_bias_ssr(obs,n,&navs);   
        }
        //调用rtkpos()进行解算
        if (!rtkpos(&rtk,obs,n,&navs)) continue;
        
        //单forward/backward模式
        if (mode==0) { /* forward/backward */
            if (!solstatic) {   //不是静态模式就直接输出结果
                outsol(fp,&rtk.sol,rtk.rb,sopt);
            }
            else if (time.time==0||pri[rtk.sol.stat]<=pri[sol.stat]) {
                sol=rtk.sol;    
                for (i=0;i<3;i++) rb[i]=rtk.rb[i];
                if (time.time==0||timediff(rtk.sol.time,time)<0.0) {
                    time=rtk.sol.time;      //记录上一历元的时间
                }
            }
        }
        else if (!revs) { /* combined-forward */
            if (isolf>=nepoch) return;
            solf[isolf]=rtk.sol;
            for (i=0;i<3;i++) rbf[i+isolf*3]=rtk.rb[i]; 
            isolf++;
        }
        else { /* combined-backward */
            if (isolb>=nepoch) return;
            solb[isolb]=rtk.sol;
            for (i=0;i<3;i++) rbb[i+isolb*3]=rtk.rb[i];
            isolb++;
        }
    }
    if (mode==0&&solstatic&&time.time!=0.0) {
        sol.time=time;
        outsol(fp,&sol,rb,sopt);
    }
    rtkfree(&rtk);
}
```
:::

<GiscusTalk />
