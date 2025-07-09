# 附录C: 配置参数解析

> 本章节参考：
> 
> [RTKLIB-Manual-CN 3.5节-配置参数](/algorithm/RTKLIB-Manual-CN/03-instructions-3.5.html)。
> 
> [RTKLIB-Manual-CN 附录F-配置文件](/algorithm/RTKLIB-Manual-CN/10-appendixF.html)。

我主要对一些重要的或我感兴趣的参数进行了分析，基于的代码版本是RTKLIB-demo5-b34K。配置参数与源码中的变量对应关系，可以查看sysopts数组（options.c）。

## C.1 Setting1

---

### C.1.1 pos1-posmode (static-start适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-144713.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.1-1 定位模式</p>

**1. 参数解读**

由于不会用到PPP，因此我在研究时会将关注点放在标准定位和基于RTK的精密定位上，以下是几种RTK模式的解读：

- **static**：如果 rover 是静止的，使用static；
- **kinematic**：如果它是移动的，使用kinematic（或static-start）；
- **static-start**：static-start假设 rover 在第一次 Fix 完成之前是静止的，然后切换到动态模式，程序允许滤波器利用 rover 最开始是静止的先验信息；
- **movingbase**：如果 base 和 rover 都在移动，那么可以使用movingbase模式（通常用来定姿）。movingbase模式与 dynamics 模式不兼容，请不要同时启用两者。如果 base 与 rover 保持固定距离，在 movingbase 模式下设置pos2-baselen和pos2-basesig；
- **fixed**：如果知道 rover 的确切位置，且只对分析残差感兴趣，可以使用fixed。

**2. 源码解析**

**a. static和static-start**

```c
/* udpos (rtkpos.c): temporal update */
if (rtk->opt.mode==PMODE_STATIC||rtk->opt.mode==PMODE_STATIC_START) return;
```

如果使用了static或static-start，那么在时间更新udpos的时候，不会进行状态预测操作，函数会在预测的代码前返回，这样将加速滤波器的收敛。

**b. static-start的状态切换**

```c
/* relpos (rtkpos.c): relative positioning */
/* switch to kinematic after qualify for hold if in static-start mode */
if (rtk->opt.mode==PMODE_STATIC_START) {
    rtk->opt.mode=PMODE_KINEMA;
    trace(3,"Fix and hold complete: switch to kinematic mode\n");
}
```

执行relpos时，在判断首次固定后，static-start模式会在内部切换为kinematic模式。

**c. movingbase**

* 定位精度

  ```c
  /* rtkpos (rtkpos.c): precise positioning */
  if (!pntpos(obs+nu,nr,nav,&rtk->opt,&solb,NULL,NULL,msg)) {
      errmsg(rtk,"base station position error (%s)\n",msg);
      return 0;
  }
  /* if base position uninitialized, use full position */
  if (fabs(rtk->rb[0])<0.1)
      for (i=0;i<3;i++) rtk->rb[i]=solb.rr[i];
  /* else filter base position to reduce noise from single precision solution */
  else
      for (i=0;i<3;i++) {
          rtk->rb[i]=0.95*rtk->rb[i]+0.05*solb.rr[i];
          rtk->rb[i+3]=0; /* set velocity to zero */
      }
  ```

  使用movingbase模式时，将不会得到精准的定位结果，每个历元基站的位置为其单点定位的位置。此时的定位结果的精度水平将主要取决于单点定位pntpos。这里的结果使用了简单的平滑技术。

* 时间同步

  ```c
  /* relpos (rtkpos.c): relative positioning */
  if (opt->mode!=PMODE_MOVEB) {
      /* check if exceeded max age of differential */
      rtk->sol.age=dt;
      if (fabs(rtk->sol.age)>opt->maxtdiff) {
          errmsg(rtk,"age of differential error (age=%.1f)\n",rtk->sol.age);
          free(rs); free(dts); free(var); free(y); free(e); free(azel); free(freq);
          return 1;
      }
  }
  …
  /* rtkpos (rtkpos.c): precise positioning */
  if (fabs(rtk->sol.age)>MIN(TTOL_MOVEB,opt->maxtdiff)) {
      errmsg(rtk,"time sync error for moving-base (age=%.1f)\n",rtk->sol.age);
      return 0;
  }
  ```

  movingbase模式下，基站和流动站通常在一个载体上，RTKLIB在设计movingbase模式时，假设了基站和流动站的观测数据是实时或近实时传输的，因此对时间同步会有更严格的要求，最大的时间差小于1.05s（TTOL_MOVEB），此时pos2-maxage的配置不起作用。

* 基线约束

  ```c
  /* ddres (rtkpos.c): double-differenced residuals and partial derivatives */
  /* baseline length constraint, for fixed distance between base and rover */
  if (rtk->opt.baseline[0]>0.0&&constbl(rtk,x,P,v,H,Ri,Rj,nv)) {
      vflg[nv++]=3<<4;
      nb[b++]++;
  }
  …
  /* constbl (rtkpos.c): constraint to baseline length */
  v[index]=rtk->opt.baseline[0]-bb;
  if (H) {
      for (i=0;i<3;i++) H[i+index*rtk->nx]=b[i]/bb;
  }
  ```

  constbl函数只有在启用movingbase模式时才会生效。如果使用基线约束，那么会多一个观测方程（nv++），约束项通过残差v和H作用到滤波器中。基线长度 bb = sqrt(b[0]^2 + b[1]^2 + b[2]^2)，因此对状态变量 x[i] 的偏导为 b[i]/bb。

* 计算航向

  ```c
  /* outnmea_rmc (solution.c): output solution in the form of NMEA RMC sentence */
  if (vel>=1.0) {
      dir=atan2(enuv[0],enuv[1])*R2D;
      if (dir<0.0) dir+=360.0;
      dirp=dir;
  }
  ```

  虽然我预期的movingbase是用来定姿的，但是我发现输出结果中唯一和姿态相关的物理量只有NMEA RMC语句中的航向信息，而该航向信息来自于速度。
  不过RTKLIB提供了E/N/U-baseline的输出格式，此时得到的便是基线向量，可以通过E和N再自行计算一个航向出来。
  movingbase的更多思考可以查看附录[A.7 RTKLIB中moving-base模式和基线约束](/algorithm/RTKLIB-Source-Notes/14-appendixA.html#a-7-moving-base模式和基线约束)。

**4. kinematic**

至于其余的逻辑均为kinematic。

---

### C.1.2 pos1-frequency

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-145058.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.1-2 频段选择</p>


**1. 参数解读**

- **L1**：单频接收机使用L1；
- **L1+L2/E5b**：若 rover 使用 L2 GPS/GLONASS/Beidou 和 Galileo E5b，则用L1+L2/E5b；
- **L1+L2+L5**：L1+L2+L5则表示 rover 还会使用 L5 GPS/GLONASS/Bediou 和 Galileo E5a 数据。

注意这其中不包含 L1+L5 的单独形式，另外 RTKLIB-demo5从demo5 b34h版本开始对北斗的 B2a 频点开始支持了。

**2. 源码解析**

频段的选择，在代码层面上最主要的是如何将合适的频点放到指定的逻辑位置上，以北斗信号为例，分析该逻辑是如何实现的。
北斗信号解析过程在RTKLIB中主要在rtcm3.c和rinex.c中，主要关注引用obs2code和code2idx函数的地方，其他星座信号解析方法相同，具体的：

```c
/* 1. 实时解析：save_msm_obs (rtcm3.c) */
/* signal to rinex obs type */
code[i]=obs2code(sig[i]);
idx[i]=code2idx(sys,code[i]);
…
/* get signal index */
sigindex(sys,code,h->nsig,rtcm->opt,idx);

/* 2. 后处理解析：readrnxobsb/set_index (rinex.c) */
/* set signal index */
for (i=n=0;*tobs[i];i++,n++) {
    ind->code[i]=obs2code(tobs[i]+1);
    ind->type[i]=(p=strchr(obscodes,tobs[i][0]))?(int)(p-obscodes):0;
    ind->idx[i]=code2idx(sys,ind->code[i]);
    ind->pri[i]=getcodepri(sys,ind->code[i],opt);
    ind->pos[i]=-1;
}
```

以下是解析流程的概述：

- 设置索引
  初始化信号标识的索引，将频率带（如B1-2、B2a、B3）及其调制类型（I、Q、I+Q、数据、导频等）映射到内部数据结构。
- obs2code

  ```c
  static char *obscodes[]={       /* rtkcmn.c: observation code strings */
      ""  ,"1C","1P","1W","1Y", "1M","1N","1S","1L","1E", /*  0- 9 */
      "1A","1B","1X","1Z","2C", "2D","2S","2L","2X","2P", /* 10-19 */
      "2W","2Y","2M","2N","5I", "5Q","5X","7I","7Q","7X", /* 20-29 */
      "6A","6B","6C","6X","6Z", "6S","6L","8L","8Q","8X", /* 30-39 */
      "2I","2Q","6I","6Q","3I", "3Q","3X","1I","1Q","5A", /* 40-49 */
      "5B","5C","9A","9B","9C", "9X","1D","5D","5P","5Z", /* 50-59 */
      "6E","7D","7P","7Z","8D", "8P","4A","4B","4X",""    /* 60-69 */
  };
  ```

  此函数将观测码标识（如"2I"、"QX"）转换为obscodes数组中的索引（对应RTKLIB中的CODE_*常量）。例如，B1-2/1561.098的"2I"可能映射为CODE_L1I，表示L1频率的同相分量。其中obscodes数组如上所示。

- code2idx
  确定每个频率数据的存储位置。例如，根据优先级分配位置（如L1在0号位），确保观测数据一致性。其中调用的code2freq_BDS函数，从而最终决定了北斗频点的使用规则。
- getcodepri

  ```c
  static char codepris[7][MAXFREQ][16]={  /* rtkcmn.c: code priority for each freq-index */
      /* L1/E1/B1I L2/E5b/B2I L5/E5a/B2a E6/LEX/B3I E5(a+b)/B2(a+b)       */
      {"CPYWMNSL","CPYWMNDLSX","IQX"     ,""       ,""       }, /* GPS */
      {"CPABX"   ,"CPABX"     ,"IQX"     ,""       ,""       }, /* GLO */
      {"CABXZ"   ,"XIQ"       ,"XIQ"     ,"ABCXZ"  ,"IQX"    }, /* GAL */
      {"CLSXZ"   ,"LSX"       ,"IQXDPZ"  ,"LSXEZ"  ,""       }, /* QZS */
      {"C"       ,"IQX"       ,""        ,""       ,""       }, /* SBS */
      {"IQXDPAN" ,"IQXDPZ"    ,"DPX"     ,"IQXA"   ,"DPX"    }, /* BDS */
      {"ABCX"    ,"ABCX"      ,""        ,""       ,""       }  /* IRN */
  };
  ```

  评估同一频率下不同码（如I、Q、I+Q）的优先级，使用rtkcmn.c中的codepris数组。

  <p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">表C-1 RINEX 3.04 北斗观测码</p>

  <img style="width: 100%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-145636.jpg"/>

  基于预定义优先级选择最高优先级的码，例如I优于I+Q。

---

### C.1.3 pos1-soltype

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-145850.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.1-3 滤波类型</p>

**1. 参数解读**

滤波类型表示卡尔曼滤波在后处理中运行的时间方向。combined模式首先向前运行过滤器，然后向后运行并组合结果。对于每个历元：

- 如果两个方向都有Fix，那么组合的结果是两个Fix状态的平均值，除非两者之间的差异太大，在这种情况下状态将是float的。
- 如果只有一个方向有Fix，则使用该值固定状态。
- 如果两个方向都是float的，那么平均值将被使用，状态将是float的。

使用combined后的结果并不总是更好，因为在任何一个方向上运行时错误的Fix通常会导致合并后的结果是float或异常解。combined的主要优点是，它通常会为您提供数据开头的固定状态，而仅向前的解决方案需要一些时间来收敛。

在combined模式下，偏差状态在开始向后运行之前被重置，以最大限度地提高向前和向后解的独立性。在combined mode-no phase reset中，不重置偏置状态可以避免在反向解开始时需要重新收敛。只有在调试时遇到初始定位困难，想要了解正确的卫星相位偏差时，才使用backward设置。

**2. 源码解析**

由于我更关注实时的逻辑（类似forward），因此这里我不准备解析backward和combined的代码。

---

### C.1.4 pos1-elmask

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-150319.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.1-4 Elevation MASK</p>

**1. 参数解读**

用于计算位置的最小卫星高度，通常将其设置为 10-15 度，以减少多径效应进入解算的概率，但这个设置取决于 rover 的环境。天空视野越开阔，这个值可以设置得越低。低仰角卫星还会带来较大的大气误差，这也是排除最低仰角卫星的另一个原因。

对于城市峡谷环境，最好将仰角设高。另外在往下的角度范围内，固定率与仰角的设置并不具备可参考的规律。

**2. 源码解析**

elmask生效的地方主要在两个地方，一处在单点定位的rescode函数中，另一处在相对定位的zdres函数中（注意这里没有考虑PPP，后文同样如此）。

**a. 单点定位**

```c
/* rescode (pntpos.c): pseudorange residuals */
if (satazel(pos,e,azel+i*2)<opt->elmin) continue;
```

rescode的主要作用是构建残差v和设计矩阵H，同时计算观测的权重var，rescode的主体部分是遍历所有卫星的伪距观测，而elmask就在此时生效，当检测到卫星仰角低于该阈值（opt->elmin）的时候则跳过该卫星的处理。

**b. 相对定位**

```c
/* zdres (rtkpos.c): undifferenced phase/code residuals */
if (satazel(pos,e+i*3,azel+i*2)<opt->elmin) continue;
```

zdres计算的相位、伪距残差，都是基于的原始观测量，还没有进行单差、双差处理。EKF中的观测方程通常不会单独出现，而是以验前残差（新息）的方式存在，因此计算残差就是为了包含观测信息到滤波中（后续的双差残差也是基于非差残差计算而来）。

zdres的主体部分同样是遍历所有卫星的伪距观测，而elmask就在此时生效，当检测到卫星仰角低于该阈值（opt->elmin）的时候则跳过该卫星的处理。

---

### C.1.5 pos1-snrmask

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-150504.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.1-5 SNR MASK</p>

C/N0（dB-Hz）是GNSS领域中最标准的信号质量指标。但是由于叫法上的习惯，大家也会称作SNR（信噪比），其具体区别参考附录A.6。

**1. 参数解读**

snrmask由于两类配置参数共同作用，分别是：pos1-snrmask-r/b和pos1-snrmask_L1/L2/L5。

snrmask的最优值会因接收机类型和天线类型而异，不过在处理较具挑战性的数据集时，最好启用它以移除低质量卫星，从而改善结果。

为每 5 度仰角设置信噪比（SNR）阈值。可以将所有值设为相同，根据标称信噪比选择30到38分贝之间的某个值。这些值仅在pos1-snrmask_x设置为on时使用。如果使用双频，你还需要设置pos1-snrmask_L2及pos1-snrmask_L5。

**2. 源码解析**

与elmask相同，snrmask生效的地方同样在两个地方，一处在单点定位的rescode函数中，另一处在相对定位的zdres函数中，具体的是在zdres的zdres_sat函数内部（注意这里没有考虑PPP，后文同样如此）。具体的代码可以参考C.1.4节。

**a. 单点定位**

```c
/* rescode (pntpos.c): pseudorange residuals */
if (!snrmask(obs+i,azel+i*2,opt)) continue;
```

**b. 相对定位**

```c
/* zdres_sat (rtkpos.c): UD (undifferenced) phase/code residual for satellite */
if (testsnr(base,0,azel[1],obs->SNR[0]*SNR_UNIT,&opt->snrmask)||
            testsnr(base,f2,azel[1],obs->SNR[f2]*SNR_UNIT,&opt->snrmask)) return;
```

---

### C.1.6 pos1-dynamics

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-151852.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.1-6 接收机动态模式</p>

**1. 参数解读**

将 rover 设置为 dynamic 会在滤波器中为 rover 添加速度和加速度状态。这将改善kinematic和static-start模式的结果，但对static模式没有影响。确保根据 rover 的加速度特性适当设置prnaccelh和prnaccelv。dynamic 与movingbase模式不兼容，因此在使用movingbase模式时需将其关闭。 

**2. 源码解析**

```c
/* rtkpos.c: number of pos parameters */
#define NP(opt)     ((opt)->dynamics==0?3:9)
 …
/* relpos (rtkpos.c): relative positioning
if (rtk->opt.dynamics) { /* velocity and covariance */
    for (i=3;i<6;i++) {
        rtk->sol.rr[i]=rtk->xa[i];
        rtk->sol.qv[i-3]=(float)rtk->Pa[i+i*rtk->na];
    }
    rtk->sol.qv[3]=(float)rtk->Pa[4+3*rtk->na];
    rtk->sol.qv[4]=(float)rtk->Pa[5+4*rtk->na];
    rtk->sol.qv[5]=(float)rtk->Pa[5+3*rtk->na];
}
```

启用了dynamic，系统状态会增加6个维度，分别是速度和加速度，这样的状态约束会更紧凑，从而提升了滤波的性能。

---

### C.1.7 pos1-posopt5

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-152118.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.1-7 RAIM</p>

**1. 参数解读**

如果某颗卫星的残差超过阈值，该卫星将被排除。这只会排除误差非常大的卫星，但需要相当多的计算量，因此通常将其禁用。

**2. 源码解析**

```c
/* pntpos (pntpos.c): single-point positioning */
/* RAIM FDE */
if (!stat&&n>=6&&opt->posopt[4]) {
    stat=raim_fde(obs,n,rs,dts,var,svh,nav,&opt_,ssat,sol,azel_,vsat,resp,msg);
}
```

RAIM作用在单点定位中，实际上它带来的增益并不明显。

RAIM FDE 是在此前单点定位失败后触发。RAIM 通过逐一排除一颗卫星（即子集测试），使用剩余卫星重新调用 estpos 计算定位解。对于所有定位成功的子集（可用卫星数 ≥ 5），RAIM 会计算残差均方根（RMS），并选择 RMS 最小的一组作为最终解，并更新接收机位置和其他输出参数。如果没有子集显著优于初始解，RAIM 可能不剔除任何卫星。

---

### C.1.8 pos1-exclsats

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-152309.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.1-8 指定剔除的卫星</p>

**1. 参数解读**

pos1-exclsats用于设置定位时排除的卫星。输入以空格分隔的卫星PRN编号。对于GLONASS、Galileo、QZSS、北斗和SBAS，分别使用Rnn、Enn、Jnn、Cnn和Snn（nn：卫星PRN或槽号）。如果在卫星ID前加上“+”，表示即使卫星状态不佳，该卫星也会被包括在定位中，通常强制添加一颗不健康的卫星用于算法分析。

**2. 源码解析**

与elmask相同，exclsat生效的地方同样在两个地方，一处在单点定位的rescode函数中，另一处在相对定位的zdres函数中（注意这里没有考虑PPP，后文同样如此）。

**a. 单点定位**

```c
/* rescode (pntpos.c): pseudorange residuals */
if (!snrmask(obs+i,azel+i*2,opt)) continue;
```

**b. 相对定位**

```c
/* zdres (rtkpos.c): UD (undifferenced) phase/code residual for satellite */
if (satexclude(obs[i].sat,var[i],svh[i],opt)) continue;
```

---

### C.1.9 pos1-navsys

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-152534.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.1-9 星座选择</p>

**1. 参数解读**

尽量启用所有可用的卫星系统，因为更多观测信息通常更好。例外情况是，如果使用EGNOS卫星，则不应启用SBAS（卫星增强系统）。如果是直接在config文件中设置pos1-navsys的设定，以下是一些常见的取值：

- 41：GPS+GAL+BDS
- 57：GPS+GAL+QZS+BDS
- 61：GPS+GLO+GAL+QZS+BDS

最终取值为1:GPS+2:SBAS+4:GLO+8:GAL+16:QZS+32:BDS+64:NAVIC。在国内CORS运营商没有播发GLO卫星的差分数据，因此通常做相对定位解算时不会考虑GLO卫星，并且GLO卫星的处理也很麻烦，因为它是基于FDMA的，每颗卫星的频段都不同。而欧美场景下没有QZS卫星，它是区域性星座，因此模拟国外场景的话，不会考虑QZS。

**2. 源码解析**

```c
/* rtksvrthread->decoderaw->update_svr->update_obs */
/* update_obs (rktsvr.c): update observation data */
if (svr->rtk.opt.exsats[sat-1]==1||!(sys&svr->rtk.opt.navsys)) {
    continue;
}

/* procpos (postpos.c): process positioning */
if ((satsys(obs_ptr[i].sat,NULL)&popt->navsys)&&
    popt->exsats[obs_ptr[i].sat-1]!=1) obs_ptr[n++]= obs_ptr[i];
```

这部分代码并不在算法代码rtkpos.c和pntpos.c中，而是在更外层的应用层，分别对应了实时程序rtksvr.c和后处理程序postpos.c。
