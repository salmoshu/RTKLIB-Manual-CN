# 附录C: 配置参数解析

> 本章节参考：
> 
> [RTKLIB-Manual-CN 3.5节-配置参数](/algorithm/RTKLIB-Manual-CN/03-instructions-3.5.html)。
> 
> [RTKLIB-Manual-CN 附录F-配置文件](/algorithm/RTKLIB-Manual-CN/10-appendixF.html)。

我主要对一些重要的或我感兴趣的参数进行了分析，基于的代码版本是RTKLIB-demo5-b34K。配置参数与源码中的变量对应关系，可以查看sysopts数组（options.c）。

## C.2 Setting2

### C.2.1 pos2-armode

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-152829.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-1 模糊度固定模式</p>

**1. 参数解读**

- **Continuous**：持续估计并求解整数模糊度
  认为模糊度是连续解，通过将前面历元解算的结果约束到滤波中，从而提高后续历元模糊度固定的精度，相邻历元的无周跳模糊度给一个极小的过程噪声。
  该模式的固定率不太稳定，大部分情况下固定率相对较低，可能原因是RTKLIB数据预处理和抗差不够完善，浮点解精度很差。 
- **Instantaneous**：按历元逐个估计并求解整数模糊度
  该模式中相邻历元模糊度没有相关性，即每个历元做一个最小二乘，然后直接就去尝试固定模糊度。单历元模式下本历元结果仅与当前历元有关，不需要考虑滤波发散问题，缺点是未充分利用历元间的相关性。
- **Fix and Hold**：将固定的模糊度约束为已解算的值
  该模式与Continues模式的差异在于，仅当模糊度固定时才会将结果约束到滤波器中。不过如果模糊度存在假固定，会导致浮点解出现较大偏差，导致较长时间的无法固定。当然这个问题Continues模式同样存在，甚至更严重。

综合来看，Continuous模式不利用固定解来调整相位偏差状态，所以它对假固定最不敏感。Fix and Hold则会利用固定解的反馈来帮助跟踪与约束模糊度。

对于低成本接收机，可以使用Fix and Hold并将跟踪增益（pos2-varholdamb）调整得足够低，以最大限度地减少假固定的概率。如果armode未设置为Fix and Hold，那么后续提到的和 hold 相关的任何配置选项都不再适用。

**2. 源码解析**

```c
/* rtklib.h: AR mode */
#define ARMODE_OFF  0                  /* AR mode: off */
#define ARMODE_CONT 1                  /* AR mode: continuous */
#define ARMODE_INST 2                  /* AR mode: instantaneous */
#define ARMODE_FIXHOLD 3               /* AR mode: fix and hold */
```

模糊度固定的模式状态有上面的几个宏来决定，我将从查看上面的宏在rtkpos.c中是如何被使用的。

**a. ARMODE_CONT**

ARMODE_CONT（Continues），我并没有发现它被调用，它可能出现在一些条件判断语句的else部分。

**b. ARMODE_INST**

 ARMODE_INST的四次调用均在udbias（rtkpos.c）函数中，udbias随后被udstate函数调用，而udstate表示的是滤波器状态向量的时间更新操作，udstate包括：
 - udpos：位置、速度和加速度的时间更新，及相应协方差矩阵的更新；
 - udion：电离层参数更新；
 - udtrop：对流层参数更新；
 - udrcvbias：GLONASS接收机硬件偏移时间更新；
 - udbias函数：相位偏差的更新，并最终更新单差模糊度，及相应协方差矩阵。

具体而言，udbias用以维护一个相位偏差量，这里的相位偏差是一个动态估计的单差相位偏移量，用于校正载波相位观测中的模糊度（从而完成单差模糊度的时间更新）。它通过相位与伪距的差值近似计算，并在周跳检测和状态更新中不断调整。

ARMODE_INST的4次调用如下所示：

```c
/* udbias(rtkpos.c): temporal update of phase biases */
 
/* 1. 如果所配置的AR的模式为instantaneous，则重置单差相位偏移状态量 */
if (rtk->opt.modear==ARMODE_INST&&rtk->x[IB(i,k,&rtk->opt)]!=0.0) {…}
 
/* 2. 卫星载波相位的中断次数rtk->ssat[i-1].outc大于配置中所设置的最大次数rtk->opt.maxout，重置单差相位偏移状态量 */
if (rtk->opt.modear==ARMODE_INST&&rtk->x[IB(i,k,&rtk->opt)]!=0.0) {…}
 
/* 3. AR模式为instantaneous，则跳过当前卫星的因周跳而在后续进行的相位偏移重置操作，不需要基于周跳来调整连续性状态*/
if (rtk->opt.modear==ARMODE_INST||(!(slip&1)&&rejc<2)) continue;
 
/* 4. 只有在非instantaneous模式下，当卫星的单差相位偏移状态量被初始化时，将该卫星的锁相计数器重置为负值（-rtk->opt.minlock），表示信号锁定状态从头开始计数。这有助于确保在连续解析模式中，只有经过足够长的稳定观测后，模糊度才被认为可靠。 */
if (rtk->opt.modear!=ARMODE_INST) {…}
```

注意：单独看ARMODE_INST的话会比较抽象，建议结合代码上下文一起看。

**c. ARMODE_FIXHOLD**

与ARMODE_ FIXHOLD相关的比较重要的函数是holdamb函数，holdamb重新构建了v和H，对满足条件的模糊度重新进行了一次量测更新（filter函数），并将约束添加到了P矩阵中。这次量测更新相对浮点的EKF是独立存在的，因此，因此代码中有一种说法是“伪新息”，即新构建的v。

```c
/* holdamb(rtkpos.c): hold integer ambiguity */
/* 满足保持模糊度（hold ambiguity）的条件，如果不满足则提前退出函数 */
if (rtk->opt.modear==ARMODE_FIXHOLD&&nv<rtk->opt.minholdsats) {…}
 
/* relpos(rtkpos.c): relative positioning */
/* 相对定位核心函数，触发保持模糊度的机制 */
if (rtk->opt.modear==ARMODE_FIXHOLD||rtk->opt.glomodear==GLO_ARMODE_FIXHOLD)
   holdamb(rtk,xa);
```

注意：单独看ARMODE_ FIXHOLD的话会比较抽象，建议结合代码上下文一起看。

### C.2.2 pos2-varholdamb (适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-153637.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-2 模糊度Hold反馈方差</p>

**1. 参数解读**

通过该参数来调整fix-and-hold模式下的跟踪增益。实际上，它是一个方差（对应EKF中R阵的对角线数值），而不是增益，因此较大的值会导致增益较低。

默认值为0.1，任何超过100的值基本都会让fix-and-hold模式失效。这个值被作用在hold过程中生成的伪测量的方差（浮点解算之后进行的一次新的量测更新），这些伪测量为卡尔曼滤波器中的偏差状态提供反馈，促使其向整数值靠拢。将这个值设置在0.1到1.0之间，既能提供足够的增益来辅助跟踪，又能避免在大多数情况下跟踪到错误的固定解。

**2. 源码解析**

```c
/* holdamb(rtkpos.c): hold integer ambiguity */
for (i=0;i<nv;i++) R[i+i*nv]=rtk->opt.varholdamb;
```

参数 varholdamb 对应于EKF中测量噪声协方差矩阵 R 的对角线数值。需要注意的是，该R矩阵用于在浮点解测量更新完成后执行的一次新的测量更新。因此，该R矩阵仅适用于这一新的测量更新过程，与浮点解过程是独立的。新测量更新完成后，状态协方差矩阵P得到更新，从而对hold的模糊度状态施加约束。这些更新后的状态随后作为下一轮迭代的先验信息。

### C.2.3 pos2-gloarmode (fix-and-hold, atuocal适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-153844.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-3 GLONASS模糊度固定模式</p>

**1. 参数解读**

该参数与GLONASS卫星的整数模糊度解算相关。设置方法：

- **ON**。如果你的接收机类型相同，或者两者的GLONASS硬件偏差均为零，你可以将此参数设置为“on”。
- **Fix and Hold**。如果你的接收机偏差不同，则需要考虑通道间偏差。最简单的处理方法是将此参数设置为“fix-and-hold”。在这种情况下，GLONASS卫星在通道间偏差校准完成之前不会用于模糊度解算，而通道间偏差的校准从第一次hold开始。
- **Auto Cal**。作为另一种选择，你可以将此参数设置为“autocal”，然后通过“pos2-arthres2”参数指定基准站和移动站之间的差分硬件偏移。这将允许GLONASS卫星立即用于模糊度解算，因此通常比“fix-and-hold”设置表现更好。此外，“autocal”功能还可以通过迭代方法，使用零基线或短基线来确定通道间偏差。

**2. 源码解析**

这一部分暂时不在笔者的兴趣范围内，延伸阅读参考附录[A.8：GLONASS模糊度固定的算法优化](/algorithm/RTKLIB-Source-Notes/14-appendixA.html#a-8-glonass模糊度固定的算法优化)。

### C.2.4 pos2-gainholdamb (适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-154139.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-4 GLONASS卫星通道间偏差校准增益</p>

**1. 参数解读**

在demo5代码中，可以通过这个参数来调整GLONASS卫星通道间偏差校准的增益。

**2. 源码解析**

该部分可以参考C.2.2节。

### C.2.5 pos2-arthres

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-154345.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-5 Ratio-Test阈值</p>

**1. 参数解读**

用于判断模糊度固定解是否有足够的置信度（比率阈值），从而确定固定解。它是次优解的残差平方与最优解的残差平方的比值（ratio-test），通常将比值保持在默认值3.0，并同时调整其他参数。

尽管较大的ratio阈值表示比较低的阈值具有更高的置信度，但两者之间并没有确定的关系。卡尔曼滤波器状态中的误差越大，对于给定的AR比值，对该解的置信度就越低。一般来说，卡尔曼滤波器的误差在首次收敛时最大，因此此时最有可能出现错误的固定解。降低pos2-arthres1参数可以帮助避免这种情况。

**2. 源码解析**

```c
/* resamb_LAMBDA(rtkpos.c): resolve integer ambiguity by LAMBDA */
/* 1. 根据查找表计算ratio，该部分参考3.6节 */
coeff[i] = coeff[i]*opt->thresar[0]+ar_poly_coeffs[i][j];
…
/* 2. 当minAR==maxAR的时候，直接使用固定ratio（thresar[0]） */
rtk->sol.thres=(float)opt->thresar[0];
 
/* manage_amb_LAMBDA(rtkpos.c): resolve integer ambiguity by LAMBDA using partial fix techniques and multiple attempts */
/* 3. 如果不满足固定规则，则省略模糊度固定过程*/
if (rtk->opt.mode<=PMODE_DGPS||rtk->opt.modear==ARMODE_OFF||
        rtk->opt.thresar[0]<1.0||posvar>rtk->opt.thresar[1]) {…}
…
/* 4. 如果结果比前一历元差很多或低于ratio阈值，则移除新的卫星。 */
if (nb>=0 && rtk->sol.prev_ratio2>=rtk->sol.thres && ((rtk->sol.ratio<rtk->sol.thres) || (rtk->sol.ratio<rtk->opt.thresar[0]*1.1 && rtk->sol.ratio<rtk->sol.prev_ratio1/2.0))) {…}
```

pos2-arthres参数主要由rtk->opt.thresar[0]控制，demo5-b34k版本中模糊度固定的ratio阈值由卫星数目动态决定。

### C.2.6 pos2-arthresmin/max (适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-154644.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-6 可变Ratio-Test阈值</p>

**1. 参数解读**

如果这些值被设置为等于pos2-arthres，那么模糊度解算阈值将被固定。否则，阈值将根据用于模糊度解算的卫星数量进行调整。对于8对卫星的情况，将使用标准值，随着卫星数量的增加，阈值会降低，而随着卫星数量的减少，阈值会增加。调整后的值将被最小和最大阈值限制所限制。调整率基于FFRT方法所使用的值，demo5的实现过程仅根据卫星数量进行调整，而不考虑模型强度。

**2. 源码解析**

自适应ratio方法的核心是通过多项式拟合，根据卫星对数量（最多 50 对）动态调整 AR 比率。这里的理论来自于论文[8]，其中部分摘录的内容参考[附录B.3：自适应Ratio](/algorithm/RTKLIB-Source-Notes/14-appendixA.html#a-9-自适应ratio)。与其相关的代码主要出现在resamb_LAMBDA（rtkpos.c）中。

```c
coeff[i] = coeff[i]*opt->thresar[0]+ar_poly_coeffs[i][j];
```

通过遍历一个 3×5 的查找表（ar_poly_coeffs），随后基于标准 AR 阈值（opt->thresar[0]）计算三个多项式系数（coeff[0]、coeff[1]、coeff[2]）。

```c
rtk->sol.thres = rtk->sol.thres*1/(nb1+1)+coeff[i];
```

使用二次多项式拟合，根据卫星对数量 nb1 调整 AR 比率。每次迭代将当前值乘以 1/(nb1+1) 并加上下一个系数，生成最终的调整后比率 rtk->sol.thres。

### C.2.7 pos2-arfilter (适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-154914.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-7 AR Filter新卫星处理功能</p>

**1. 参数解读**

将此“arfilter”参数设置为“on”将允许对新卫星或从周跳中恢复的卫星进行有效性检验。如果一颗卫星在首次加入时显著恶化了模糊度比率（AR ratio），那么它在模糊度解算中的使用将被延迟。启用此功能可以让你减少对“arlockcnt”参数的使用，该参数虽然也有类似的作用，但它是通过固定的延迟计数来实现的，不是很灵活。

**2. 源码解析**

```c
/* manage_amb_LAMBDA(rtkpos.c): resolve integer ambiguity by LAMBDA using partial fix techniques and multiple attempts */
/* 如果不满足固定规则，则省略模糊度固定过程*/
if (rtk->opt.arfilter) {…}
```

arfilter通过检测 AR 比率下降（当前解质量低于前一历元或阈值），识别并剔除可能引入噪声的新卫星，改善模糊度解析的可靠性。其具体的过程如下所示：

- 检查 AR 滤波器是否启用。
- 比较当前和前一历元的 AR 比率，若质量显著下降，进入剔除逻辑。
- 遍历卫星，剔除锁定计数为 0 的新卫星，并设置延迟。
- 若有卫星被剔除，则重新运行 LAMBDA 解析。

该逻辑主要依赖于 AR 比率的动态变化，旨在避免因新卫星引入的错误固定（false fix），并通过分阶段延迟减少对新卫星的过度剔除。此机制在滤波器收敛初期尤为重要，因为新卫星可能因信号不稳定导致解质量下降。

### C.2.8 pos2-arthres1 (适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-155820.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-8 模糊度求解延迟位置方差阈值</p>

**1. 参数解读**

整数模糊度解算将延迟到位置状态的方差达到此阈值时才开始进行。其目的是避免在滤波器中的偏差状态尚未收敛之前出现的假固定。如果你将“eratio1”设置为大于100的值，并且使用单星座或单频解，那么将此参数设置为相对较低的值尤为重要。如果你在解算结果中看到模糊度比率（AR ratio）为零的情况持续过长，那么你可能需要增加这个阈值，因为这意味着由于未达到阈值，模糊度解算被禁用了。demo5作者发现0.004到0.10的值通常对一些低成本接收机很有效，但如果你的测量数据质量较低，你可能需要增加这个值，以避免首次固定解的延迟过长，或者在多次发生周跳后丢失固定解。

**2. 源码解析**

```c
/* udpos(rtkpos.c): temporal update of position/velocity/acceleration */
/* include accel terms if filter is converged */
if (var<rtk->opt.thresar[1]) {
    for (i=0;i<3;i++) {
        F[i+(i+6)*nx]=(tt>=0?1:-1)*SQR(tt)/2.0;
    }
}
```

这表明当滤波器收敛时，位置状态的更新会考虑加速度的贡献，提高动态建模的精度。如果 var 超过 thresar[1]，状态转移矩阵 F 仅包含位置与速度的线性关系（F[i + (i + 3) * nx] = tt），忽略加速度效应。在方差较大的情况下，不加入加速度项并重置状态的设计可能是为了防止发散。

```c
/* manage_amb_LAMBDA(rtkpos.c): resolve integer ambiguity by LAMBDA using partial fix techniques and multiple attempts */
/* skip AR if don't meet criteria */
if (rtk->opt.mode<=PMODE_DGPS||rtk->opt.modear==ARMODE_OFF||
    rtk->opt.thresar[0]<1.0||posvar>rtk->opt.thresar[1]) {
    trace(3,"Skip AR\n");
    rtk->sol.ratio=0.0;
    rtk->sol.prev_ratio1=rtk->sol.prev_ratio2=0.0;
    rtk->nb_ar=0;
    return 0;
}
```

这段代码的作用是作为 AR 过程的前置条件，根据当前的定位模式、AR 配置和位置估计质量（方差）决定是否执行 LAMBDA 模糊度解析。如果不满足条件，则不进行AR操作并重置相关状态。arthres1的作用主要是希望位置方差收敛到一定程度以后再进行AR操作，这样避免了在滤波器中的偏差状态尚未收敛之前出现的假固定。

### C.2.9 pos2-arthres2 (适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-161050.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-9 GLONASS硬件偏差</p>

**1. 参数解读**

按频率槽计算的相对GLONASS硬件偏差（m）。仅当pos2-gloarmode设置为“autocal”时，才会使用此参数，用于指定两个不同接收机制造商之间的通道间偏差。有关常见接收机类型的合适值，以及如何使用此参数进行迭代搜索以找到未指定接收机类型的值。

**2. 源码解析**

这一部分暂时不在笔者的兴趣范围内，详情请参阅资料[13]。

### C.2.10 pos2-arthres3 (适用于demo5)

**1. 参数解读**

GLONASS硬件偏差状态的初始方差。仅当pos2-gloarmode设置为“autocal”时，才会使用此参数。较小的值会使pos2-arthres2中指定的初始值获得更大的权重。当pos2-arthres2设置为已知偏差时，demo5作者使用1e-9；而在进行迭代搜索时，demo5作者使用1e-7。

**2. 源码解析**

这一部分暂时不在笔者的兴趣范围内，详情请参阅资料[13]。


### C.2.11 pos2-arthres4 (适用于demo5)

**1. 参数解读**

GLONASS硬件偏差状态的卡尔曼滤波器过程噪声。较小的值会使pos2-arthres2中指定的初始值获得更大的权重。当pos2-arthres2设置为已知偏差时，demo5作者使用0.00001；而在进行迭代搜索时，demo5作者使用0.001。

**2. 源码解析**

这一部分暂时不在笔者的兴趣范围内，详情请参阅资料[13]。

### C.2.12 pos2-arlockcnt

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-161342.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-10 用于模糊度固定的最小锁定计数</p>

**1. 参数解读**

在将新卫星或从周跳中恢复的卫星用于整数模糊度解算之前，需要延迟的采样数。这可以避免因包含尚未收敛的卫星而导致模糊度比值（AR ratio）被破坏。需与“arfilter”配合使用。请注意，单位是采样数，而不是时间单位，因此如果你更改了移动站测量的采样率，可能需要对其进行调整。demo5作者通常将此参数设置为零，因为u-blox接收机在标记可疑观测值方面表现非常好，但对于其他接收机，demo5作者会将其设置得更高。

**2. 源码解析**

```c
/* udbias(rtkpos.c): temporal update of phase biases */
/* 1. reset phase-bias if instantaneous AR or expire obs outage counter */
if (rtk->opt.modear!=ARMODE_INST&&reset) {
    rtk->ssat[i-1].lock[k]=-rtk->opt.minlock;
}

/* update phase bias noise and check for cycle slips */
for (i=0;i<ns;i++) {
…
if (rtk->opt.modear == ARMODE_INST || (!(slip & 1) && rejc < 2)) continue; 
/* 2. reset phase-bias state if detecting cycle slip or outlier */ 
rtk->x[j] = 0.0; 
rtk->ssat[sat[i]-1].rejc[k] = 0;
    rtk->ssat[sat[i]-1].lock[k]=-rtk->opt.minlock;
    …
}

/* set initial states of phase-bias */
for (i = 0; i < ns; i++) {
    if (bias[i] == 0.0 || rtk->x[IB(sat[i],k,&rtk->opt)] != 0.0) continue;
    initx(rtk, bias[i], SQR(rtk->opt.std[0]), IB(sat[i],k,&rtk->opt));
    trace(3, "     sat=%3d, F=%d: init phase=%.3f\n", sat[i], k+1, bias[i]);
    if (rtk->opt.modear != ARMODE_INST) {
        rtk->ssat[sat[i]-1].lock[k] = -rtk->opt.minlock;
    }
}
```

minlock在udbias中调用了三次，minlock 在这三处都用于延迟新卫星或不稳定卫星的加入，确保信号质量达到要求后才参与整数模糊度解析。具体作用分别是：

- 第一次：在观测中断计数溢出（reset）时，将卫星锁定计数设为负值，延迟其使用，适用于AR 模式不为instantaneous的中断恢复。
- 第二次：在检测到周跳或异常值时，重置相位偏移并延迟卫星使用，确保信号稳定后再参与解算。
- 第三处：管理新初始化的相位偏移。

```c
/* manage_amb_LAMBDA(rtkpos.c): resolve integer ambiguity by LAMBDA using partial fix techniques and multiple attempts */
if (rtk->ssat[sat[i]-1].lock[f] == 0) {
    trace(3, "remove sat %d:%d lock=%d\n", sat[i], f, rtk->ssat[sat[i]-1].lock[f]);
    rtk->ssat[sat[i]-1].lock[f] = -rtk->opt.minlock - dly;  /* delay use of this sat with stagger */
    dly += 2;  /* stagger next try of new sats */
    rerun = 1;
}
```

minlock 在此处的角色是延迟新卫星的加入，确保其信号稳定后才用于模糊度解析。具体通过将锁定计数设为负值（-minlock - dly），配合分阶段延迟和重新运行 AR，减少了新卫星引入的不确定性。

### C.2.13 pos2-minfixsats (适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-161630.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-11 获得固定解所需的最小卫星数量</p>

**1. 参数解读**

获得固定解所需的最小卫星数量。用于避免因极少数卫星而导致的假固定，尤其是在频繁发生周跳的期间。

**2. 源码解析**

```c
/* resamb_LAMBDA(rtkpos.c): resolve integer ambiguity by LAMBDA */
if ((nb=ddidx(rtk,ix,gps,glo,sbs))<(rtk->opt.minfixsats-1)) {  /* nb is sat pairs */
    errmsg(rtk,"not enough valid double-differences\n");
    free(ix);
    return -1; /* flag abort */
}
```

minfixsats 的作用是作为模糊度固定过程的最小卫星对数阈值，确保 AR 过程有足够的数据基础。如果有效双差对数 nb 小于 minfixsats - 1，则中止 LAMBDA 固定，防止因卫星数量不足导致的错误解。

### C.2.14 pos2-minholdsats (适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-161815.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-12 整数模糊度结果进入 hold 阶段所需的最小卫星数量</p>

**1. 参数解读**

整数模糊度结果进入 hold 阶段所需的最小卫星数量。用于避免因极少数卫星而导致的错误保持，尤其是在频繁发生周跳的期间。

**2. 源码解析**

```c
/* holdamb(rtkpos.c): hold integer ambiguity */
/* return if less than min sats for hold (skip if fix&hold for GLONASS only) */
if (rtk->opt.modear==ARMODE_FIXHOLD&&nv<rtk->opt.minholdsats) {
    trace(3,"holdamb: not enough sats to hold ambiguity\n");
    free(v); free(H);
    return;
}
```

minholdsats 的作用是作为“Fix-and-Hold”模式下模糊度进入hold状态的最小卫星对数阈值，确保有足够的数据支持保持过程。如果双差对数 nv 小于 minholdsats，则中止模糊度hold，防止因卫星数量不足导致的解不稳定或错误的hold。注意这里的minholdsats没有像前面的minfixsats一样减1。

### C.2.15 pos2-mindropsats (适用于demo5)

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-161949.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-13 每个历元排除单颗卫星所需的最小模糊度数量</p>

**1. 参数解读**

启用每历元排除单颗卫星进行模糊度解算所需的最小卫星数量。在每个历元中，会排除一颗不同的卫星。如果排除某颗卫星后，模糊度比值显著改善，则该卫星将从用于模糊度解算的卫星列表中移除。

**2. 源码解析**

```c
/* manage_amb_LAMBDA(rtkpos.c): resolve integer ambiguity by LAMBDA using partial fix techniques and multiple attempts */
if (rtk->sol.prev_ratio2<rtk->sol.thres&&rtk->nb_ar>=rtk->opt.mindropsats) {…}
```

这段代码的作用是在前一历元未固定且卫星对数足够时，剔除列表中的下一个卫星（由 arsats[rtk->excsat] 确定），以排除可能影响 AR 质量的卫星。rtk->excsat 的递增和重置机制实现了逐步试错，延迟剔除卫星的重新使用，支持后续 AR 和hold过程的优化。

这样做的效果是在未来历元中，逐步剔除卫星并尝试模糊度的固定，带有一定的随机性，不过在工程上它是有效的。

### C.2.16 pos2-arelmask

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-162437.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-14 用于模糊度固定的最小仰角</p>

**1. 参数解读**

功能上与默认值0没有区别，因为小于“elmask”的高度角不会用于模糊度解算，但demo5作者将其更改以避免混淆。


### C.2.17 pos2-arminfix

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-163406.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-15 模糊度进入hold所需的连续固定样本数量</p>

**1. 参数解读**

模糊度进入hold所需的连续固定样本数量。增加此值可能是减少错误hold的最有效方法，但同时也会增加首次hold的时间和重新获得hold的时间。随着模糊度跟踪增益的降低（即pos2-varholdamb增加）和观测数量的增加，可以减少arminfix。请注意，如果移动站测量的采样率发生变化，可能需要调整此值。

**2. 源码解析**

```c

```

### C.2.18 pos2-elmaskhold

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-162613.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-16 设置最小仰角来保持模糊度</p>

**1. 参数解读**

```c

```

**2. 源码解析**

```c

```

### C.2.19 pos2-aroutcnt

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-163633.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-17 数据中断阈值（以重置模糊度）</p>

**1. 参数解读**

```c

```

**2. 源码解析**

```c

```

### C.2.20 pos2-maxage

<img style="width: 80%; margin: 20px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250709-163803.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图C.2-18 差分龄期的最大值</p>

**1. 参数解读**

```c

```

**2. 源码解析**

```c

```

### C.2.21 pos2-rejionno

**1. 参数解读**

```c

```

**2. 源码解析**

```c

```
