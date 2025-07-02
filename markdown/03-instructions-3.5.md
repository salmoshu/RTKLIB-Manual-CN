# 3. 操作指南

## 3.5 RTKLIB配置参数

可以按下RTKNAVI或RTKPOST的主窗口中的 <span style="border: 1px solid black; padding: 3px;">Options...</span> 来设置配置参数。可操作的定位参数如下。通过按下对话框上的 <span style="border: 1px solid black; padding: 3px;">Save...</span> 按钮并选择文件路径，可以将这些选项保存到配置文件中。通过按下 <span style="border: 1px solid black; padding: 3px;">Load...</span> 按钮并选择配置文件，从而在配置文件中加载选项。配置参数的更多信息请请参考B.4。下表还显示了可以包含在配置文件中的关键字。这些选项中指定的模型也在附录E中进行了详细解释。

另外demo5作者提供了一份常用的配置参数使用说明，参考附录F。

### 3.5.1 Setting1
<table style="text-align: left;">
  <thead>
    <tr>
      <th>选项</th>
      <th>描述</th>
      <th>配置名称</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Positioning Mode（定位模式）</td>
      <td>
        设置定位模式<br>
        - Single: SPP 或 SBAS DGPS<br>
        - DGPS/DGNSS: 伪距差分<br>
        - Static: 载波相位静态定位（RTK静态）<br>
        - Static-start: 静止启动，先Static模式，后Kinematic模式<br>
        - Kinematic: 载波相位动态定位（RTK动态）<br>
        - Moving-Base: 动基线模式（用于双天线定姿）<br>
        - Fixed: 流动站接收机固定坐标，解算模糊度、对流层、电离层等参数 *<br>
        - PPP Kinematic: 动态精密单点定位<br>
        - PPP Static: 静态精密单点定位<br>
        - PPP Fixed: PPP 固定坐标，解算模糊度、对流层、电离层等参数 *
      </td>
      <td>pos1-posmode</td>
      <td>*<br>用于残差分析</td>
    </tr>
    <tr>
      <td>Frequencies（频段）</td>
      <td>
        设置载波频段<br>
        - L1: L1 单频: L1,R1,E1,B1<br>
        - L1+L2: L1 和 L2 双频: L1/L2, R1/R2, E1/E5b, B1/B2<br>
        - L1+L2+L5: L1, L2 和 L5 三频 L1/L2/L5, R1/R2, E1/E5b/E5a, B1/B2<br>
        - L1+L2+L5+L6: 用于实验测试, 并全支持<br>
      </td>
      <td>pos1-frequency</td>
      <td>Single, PPP-* 不支持</td>
    </tr>
    <tr>
      <td>滤波类型</td>
      <td>
        设置滤波类型<br>
        - Forward: 前向滤波<br>
        - Backward: 后向滤波 *<br>
        - Combined: 前向与后向组合滤波 *, 相位偏置会在前向、后向解间重置。<br>
        - Combined–no phase reset: 前向与后向组合滤波 *, 相位偏置不会在前向、后向解间重置。<br>
      </td>
      <td>pos1-soltype</td>
      <td>
        *<br>RTKNAVI 和 Single 模式不支持。
      </td>
    </tr>
    <tr>
      <td>Elevation Mask</td>
      <td>设置截止仰角（°）阈值。</td>
      <td>
        pos1-elmask<br>
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Elevation Mask</td>
      <td>
        设置截止 SNR 阈值. 点击 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮即可看到 "SNR Mask" 对话框. 可以在每 5 ° 的仰角区间设置 SNR 截止阈值，如果 "Rover" 和 "Base Station" 复选框没有选中，那么 SNR MASK 将不会生效。
      </td>
      <td>
        pos1-<br>
        snrmask_r,<br>
        snrmask_b,<br>
        snrmask_L1,<br>
        snrmask_L2,<br>
        snrmask_L5<br>
      </td>
      <td>
      </td>
    </tr>
    <tr>
      <td>Rec Dynamics</td>
      <td>
        将流动站接收机设置为动态模式。<br>
        - OFF: 不启用动态模式<br>
        - ON: 将估计接收机的速度和加速度。<br>
        利用估计的速度和加速度预测接收机位置。<br>
      </td>
      <td>pos1-dynamics</td>
      <td>仅用于 DGPS/DGNSS 或 Kinematic 模式</td>
    </tr>
    <tr>
      <td>Earth Tides Correction</td>
      <td>
        设置是否应用地球潮汐校正<br>
        - OFF：不应用地球潮汐校正<br>
        - Solid：应用固体地球潮汐校正<br>
        - Solid/OTL：应用固体地球潮汐、OTL（海洋潮汐负载）和极潮校正。*<br>
        要应用OTL校正，请在“Files”选项卡中的“Ocean Loading BLQ Format”中设置OTL系数文件路径，标记名称必须包含在输入的RINEX文件中，以便在BLQ文件中选择站点。<br>
        要应用极潮，请在“Files”菜单的“EOP Data Files”选项中设置ERP（地球自转参数）文件路径。
      </td>
      <td>pos1-tidecorr</td>
      <td>不适用于Single模式，*不适用于RTKNAVI模式</td>
    </tr>
    <tr>
      <td>Ionosphere Correction</td>
      <td>
        设置电离层校正选项。如果设置了该参数，则会为每颗卫星估计垂直电离层延迟。对于长基线情形，电离层估计可以有效抑制电离层延迟的影响。<br>
        - OFF：不应用电离层校正<br>
        - Broadcast：应用广播电离层模型<br>
        - SBAS：应用SBAS电离层模型<br>
        - Iono-Free LC：使用双频（GPS/GLONASS/QZSS的L1-L2或Galileo的L1-L5）测量的无电离层线性组合进行电离层校正<br>
        - Estimate STEC：估计电离层参数STEC（倾斜总电子含量）*<br>
        - IONEX TEC：使用IONEX TEC网格数据<br>
        - QZSS Broadcast：应用QZSS提供的广播电离层模型<br>
        - QZSS LEX：（保留）
      </td>
      <td>pos1-ionoopt</td>
      <td>*不适用于Single模式和PPP-*模式</td>
    </tr>
    <tr>
      <td>Troposphere Correction</td>
      <td>
        设置是否估计对流层参数（Rover和Base位置的天顶总延迟）。<br>
        - OFF：不应用对流层校正<br>
        - Saastamoinen：应用Saastamoien模型<br>
        - SBAS：应用SBAS对流层模型（MOPS）<br>
        - Estimate ZTD：估计ZTD（天顶总延迟）参数为EKF状态*<br>
        - Estimate ZTD+Grad：估计ZTD和水平梯度参数为EKF状态*
      </td>
      <td>pos1-tropopt</td>
      <td>*不应用于Single模式</td>
    </tr>
    <tr>
      <td>Satellite Ephemeris/Clock</td>
      <td>
        设置卫星星历的类型。 <br>
        - Broadcast：使用广播星历 <br>
        - Precise：使用精密星历* <br>
        - Broadcast+SBAS：具有SBAS长期快速校正功能的广播星历 <br>
        - Broadcast+SSR APC：带RTCM SSR校正的广播星历（天线相位中心值） <br>
        - Broadcast+SSR CoM：使用RTCM SSR校正（卫星质心值）的广播星历 <br>
        - QZSS LEX：（保留）
      </td>
      <td>pos1-sateph</td>
      <td></td>
    </tr>
    <tr>
      <td>Sat PCV</td>
      <td>
        设置是否使用卫星天线PCV（相位中心变化）模型。要使用该功能，请在“Files”选项卡中设置“Satellite Antenna PCV File”。
      </td>
      <td>pos1-posopt1</td>
      <td>不应用于Single模式</td>
    </tr>
    <tr>
      <td>Rec PCV</td>
      <td>
        设置是否使用接收机天线PCV型号。要使用该功能，请在“Files”选项卡中设置“Receiver Antenna PCV File”。
      </td>
      <td>pos1-posopt2</td>
      <td>不应用于Single模式</td>
    </tr>
    <tr>
      <td>PhWindup</td>
      <td>设置是否对PPP模式应用相位缠绕修正。</td>
      <td>pos1-posopt3</td>
      <td>仅适用于PPP-*模式。</td>
    </tr>
    <tr>
      <td>Reject Ecl</td>
      <td>
        设置是否排除处于日食状态的GPS Block IIA卫星。由于偏航姿态的不可预测行为，处于日食状态的Block IIA卫星通常会降低PPP解算的精度。
      </td>
      <td>pos1-posopt4</td>
      <td>仅适用于PPP-*模式。</td>
    </tr>
    <tr>
      <td>RAIM FDE</td>
      <td>
        设置是否启用RAIM（接收机自主完整性监测）FDE（故障检测和排除）功能。在启用RAIM FDE的情况下，如果残差的SSE（平方误差之和）超过阈值，则排除卫星。被排除的卫星是使SSE最小的那颗卫星。
      </td>
      <td>pos1-posopt5</td>
      <td></td>
    </tr>
    <tr>
      <td>DBCorr</td>
      <td>日界时钟跳变修正</td>
      <td>pos1-posopt6</td>
      <td>仅适用于PPP-*模式。</td>
    </tr>
    <tr>
      <td>Excluded Satellites (+PRN: Included)</td>
      <td>
        设置定位时排除的卫星。输入以空格分隔的卫星PRN编号。对于GLONASS、Galileo、QZSS、北斗和SBAS，分别使用Rnn、Enn、Jnn、Cnn和Snn（nn：卫星PRN或槽号）。如果在卫星ID前加上“+”，表示即使卫星状态不佳，该卫星也会被包括在定位中。
      </td>
      <td>pos1-exclsats</td>
      <td></td>
    </tr>
    <tr>
      <td>Navigation System</td>
      <td>
        选择用于定位的导航卫星系统。<br>
        - GPS<br>
        - GLONASS<br>
        - Galileo<br>
        - QZSS<br>
        - SBAS<br>
        - BeiDou<br>
        - IRNSS
      </td>
      <td>pos1-navsys</td>
      <td></td>
    </tr>
  </tbody>
</table>

![SNR Mask Dialog of RTKNAVI and RTKPOST Options](https://i.ibb.co/xg0740L/image.png)

### 3.5.2 Setting2
<table>
  <thead>
    <tr>
      <th>选项</th>
      <th>描述</th>
      <th>配置名称</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Integer Ambiguity Resolution (GPS)</td>
      <td>
        为GPS和伽利略系统设置整数模糊度求解策略：<br>
        - OFF：不进行模糊度求解<br>
        - Continuous：持续估计并求解静态整数模糊度 *<br>
        - Instantaneous：按历元逐个估计并求解整数模糊度 *<br>
        - Fix and Hold：持续估计并解算静态整数模糊度。如果验证通过，则将模糊度约束为已解算的值。 *<br>
        - PPP-AR：不支持
      </td>
      <td>pos2-armode</td>
      <td>
        默认值：Continuous不适用于Single模式。<br>
        *仅适用于Kinematic, Static, Movingbaseline 和 Fixed模式。
      </td>
    </tr>
    <tr>
      <td>Integer Ambiguity Resolution (GLO)</td>
      <td>
        为GLONASS设置整数模糊度求解策略：<br>
        - OFF：不固定模糊度。<br>
        - ON：固定模糊度。通常只有相同类型的接收机对（流动站和基准站）的模糊度才能被固定。如果不同类型的接收机存在IFB（频间偏差），则无法通过双差消除。<br>
        - Fix and Hold：在首次对GPS卫星完成固定并保持操作后，消除IF偏差。<br>
        - Autocal：接收机的IF偏差会自动校准，但需要较为准确的初始估计值（参见GLONASS硬件偏差）。。
      </td>
      <td>pos2-gloarmode</td>
      <td>
        默认值：ON<br>
        仅适用于Kinematic, Static, Movingbaseline 和 Fixed 模式。
      </td>
    </tr>
    <tr>
      <td>Integer Ambiguity Resolution (BDS)</td>
      <td>设置北斗整周模糊度解算策略（ON/OFF）</td>
      <td>pos2-bdsarmode</td>
      <td>默认值：OFF</td>
    </tr>
    <tr>
      <td>模糊度固定比率（Min/Norm/Max）</td>
      <td>
        - 设置用于“ratio-test”的整数模糊度验证阈值，该检验使用最佳整数向量与次佳向量的残差平方比。<br>
        - 如果最小值和最大值被设置为等于标称值，则比率是固定的；否则，比率将根据卫星数量进行调整。标称值适用于8对卫星，随着卫星数量的增加，该值会降低，而随着卫星数量的减少，该值会提高。
      </td>
      <td>
        pos2-arthresmin<br>
        pos2-arthres<br>
        pos2-arthresmax
      </td>
      <td>默认值：3.0</td>
    </tr>
    <tr>
      <td>GLO HW Bias</td>
      <td>
        设置每频段的相对GLONASS硬件偏差（单位：m）。仅在pos2-gloarmode设置为“autocal”时使用，用于指定两个不同接收机制造商之间的通道间偏差。
      </td>
      <td>pos2-arthres2</td>
      <td>默认值：0.0</td>
    </tr>
    <tr>
      <td>Min Lock / Elevation to Fix Ambiguity</td>
      <td>
        设置用于固定整数模糊度的最小锁定计数和最小仰角（单位：度）。如果锁定计数或仰角小于设定值，则该模糊度将从固定的整数向量中排除。
      </td>
      <td>pos2-arlockcnt,arelmask</td>
      <td>默认值：0，0</td>
    </tr>
    <tr>
      <td>Min Fix / Elevation to Hold Ambiguity</td>
      <td>如果你选择了“Fix and Hold”模式进行整数模糊度求解，你需要设置最小固定次数和最小仰角（°）来保持模糊度。</td>
      <td>pos2-arminfix,elmaskhold</td>
      <td>默认值：10，0</td>
    </tr>
    <tr>
      <td>Slip Threshs: Doppler (Hz) / Geom-Free (m)</td>
      <td>
        设置多普勒频率与载波相位差分在历元间差异的周跳阈值（单位：Hz）（0表示禁用）<br>
        设置无几何约束的长组合（LC）载波相位差分在历元间的周跳阈值（单位：m）。
      </td>
      <td>pos2-dopthres,pos2-slipthres</td>
      <td>默认值：0，0.05</td>
    </tr>
    <tr>
      <td>Max Age of Diff / Outs to Reset Amb</td>
      <td>
        设置流动站和基准站之间的差分龄期的最大值（秒）。<br>
        设置数据中断次数以重置模糊度。如果数据中断次数超过该值，则估计的模糊度将重置为初始值。
      </td>
      <td>
        pos2-maxage, pos2-aroutcnt
      </td>
      <td>默认值：30，5</td>
    </tr>
    <tr>
      <td>Reject Threshold of GDOP/Innov.</td>
      <td>
        设置GDOP和卡尔曼滤波器新息的拒绝阈值（单位：m）。如果GDOP或载波相位新息超过该阈值，则将该观测值视为异常值并排除在估计过程之外。新息阈值会乘以载波相位/伪距误差比，以生成伪距新息的阈值。
      </td>
      <td>
        pos2-rejgdop,rejionno
      </td>
      <td>默认值：30，1.0</td>
    </tr>
    <tr>
      <td># of Filter Iter</td>
      <td>
        设置估计滤波器测量更新中的迭代次数。如果基线长度非常短，例如1米，迭代可能有助于处理测量方程的非线性。
      </td>
      <td>pos2-niter</td>
      <td>默认值：1</td>
    </tr>
    <tr>
      <td>Sync Solution</td>
      <td>
        设置解决方案的时间同步模式：<br>
        - OFF：最小延迟模式。解决方案在流动站数据输入后尽快输出。延迟的基准站或校正数据将被外推到流动站时间。<br>
        - ON：匹配解决方案模式。解决方案在流动站数据和基准站或校正数据都准备好后输出。解决方案时间可能会有一定的延迟，落后于流动站时间。
      </td>
      <td>pos2-syncsol</td>
      <td>不适用于RTKPOST与Single模式，默认值：OFF</td>
    </tr>
    <tr>
      <td>Baseline Length Constraint</td>
      <td>如果处于移动基线模式，检查并设置基线长度的约束。填写约束的长度（米）和标准差（米）。</td>
      <td>pos2-baselen,basesig</td>
      <td>默认值：0.0</td>
    </tr>
    <tr>
      <td>
        Min Fix Sats<br>
        Min Hold Sats
      </td>
      <td>
        Fix所需的最小模糊度数量<br>
        Hold所需的最小模糊度数量
      </td>
      <td>
        pos2-minfixsat, pos2-minholdsats
      </td>
      <td>默认值：4，5</td>
    </tr>
    <tr>
      <td>Min Drop Sats</td>
      <td>每个历元从模糊度求解中排除单个卫星所需的最小模糊度数量</td>
      <td>pos2-mindropsats</td>
      <td>默认值：10</td>
    </tr>
    <tr>
      <td>Max Pos Var for AR</td>
      <td>
        在GNSS模糊度求解中，模糊度求解的延迟是为了在位置状态的方差达到某个阈值时进行，以最小化错误固定的可能性。这种方法通过等待方差达到一定的水平，可以提高模糊度求解的成功率，从而减少错误固定的风险。
      </td>
      <td>pos2-arthres1</td>
      <td>默认值：0.25</td>
    </tr>
    <tr>
      <td>AR Filter</td>
      <td>
        在发生周跳后，是否需要对新加入或重新加入的卫星进行模糊度求解资格验证（ON/OFF）
      </td>
      <td>pos2-arfilter</td>
      <td>默认值：ON</td>
    </tr>
    <tr>
      <td>Hold Amb Var</td>
      <td>设置模糊度保持反馈的方差（作为模糊度跟踪增益的倒数）</td>
      <td>pos2-varholdamb</td>
      <td>默认值：0.1</td>
    </tr>
    <tr>
      <td>Hold Amb Gain</td>
      <td>在GLONASS“Fix and Hold”模糊度求解模式下，设置频间偏差消除的增益。</td>
      <td>pos2-gainholdamb</td>
      <td>默认值：0.01</td>
    </tr>
  </tbody>
</table>

### 3.5.3 Output

![Options Dialog (Output) of RTKNAVI and RTKPOST](https://i.ibb.co/qpY40zz/image.png)

<table>
  <thead>
    <tr>
      <th>选项</th>
      <th>描述</th>
      <th>配置名称</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Solution Format</td>
      <td>
        设置输出解决方案的格式。<br>
        - Lat/Lon/Height：纬度、经度和高程<br>
        - X/Y/Z-ECEF：ECEF坐标的X/Y/Z分量<br>
        - E/N/U-Baseline：基线向量的E/N/U分量<br>
        - NMEA0183：NMEA GPRMC、GPGGA、GPGSA、GLGSA、GAGSA、GPGSV、GLGSV和GAGSV
      </td>
      <td>out-solformat</td>
      <td>主要应用于RTKNAVI，作为输出流设置</td>
    </tr>
    <tr>
      <td>Output Header</td>
      <td>设置是否输出标题行。</td>
      <td>out-outhead</td>
      <td>不适用于NMEA</td>
    </tr>
    <tr>
      <td>Process Options</td>
      <td>设置是否输出处理选项。</td>
      <td>out-outopt</td>
      <td>仅适用于RTKPOST，不适用于NMEA</td>
    </tr>
    <tr>
      <td>Vel</td>
      <td>设置是否输出速度。</td>
      <td>out-outvel</td>
      <td></td>
    </tr>
    <tr>
      <td>Time Format</td>
      <td>
        设置时间格式：<br>
        - ssssssss.sss GPST：GPS周和周内时间<br>
        - hh:mm:ss GPST：yyyy/mm/dd hh:mm:ss GPST<br>
        - hh:mm:ss UTC：yyyy/mm/dd hh:mm:ss UTC<br>
        - hh:mm:ss JST：yyyy/mm/dd hh:mm:ss JST
      </td>
      <td>outtimesys,timeform</td>
      <td>不适用于NMEA</td>
    </tr>
    <tr>
      <td># of Decimals</td>
      <td>设置时间格式中的小数位数。</td>
      <td>out-timendec</td>
      <td>不适用于NMEA</td>
    </tr>
    <tr>
      <td>Latitude / Longitude Format</td>
      <td>
        如果解决方案格式设置为纬度/经度/高程，设置纬度和经度的格式。<br>
        - ddd.dddddddd：度<br>
        - ddd mm ss.sss：度分秒
      </td>
      <td>out-degform</td>
      <td>不适用于NMEA</td>
    </tr>
    <tr>
      <td>Field Separator</td>
      <td>设置字段的分隔符。</td>
      <td>out-fieldsep</td>
      <td>不适用于NMEA</td>
    </tr>
    <tr>
      <td>Datum</td>
      <td>
        如果解决方案格式选项设置为纬度/经度/高度，则设置基准。<br>
        - WGS84:WGS84基准<br>
        - Tokyo：东京基准（当前版本仅支持WGS84）
      </td>
      <td>-</td>
      <td></td>
    </tr>
    <tr>
      <td>Height</td>
      <td>
        设置高度类型。<br>
        -Ellipsoidal：椭球高度<br>
        -Geodetic：大地测量高度
      </td>
      <td>out-height</td>
      <td></td>
    </tr>
    <tr>
      <td>Geoid Model</td>
      <td>
        如果高度选项设置为大地测量高程，则设置大地水准面模型。<br>
        - Internal：内部大地水准面模型<br>
        - EGM96-BE (15")：EGM96 (15" x 15" 网格) *1<br>
        - EGM2008-SE (2.5")：EGM2008 (2.5 x 2.5" 网格) *2<br>
        - EGM2008-SE (1")：EGM2008 (1 x 1" 网格) *2<br>
        - GSI2000 (1x1.5")：GSI2000 (1x1.5" 网格) *3<br>
        - 如果使用外部大地水准面模型，请在“文件”选项卡中指定大地水准面文件路径。
      </td>
      <td>out-geoid</td>
      <td></td>
    </tr>
    <tr>
      <td>Solution for Static Mode</td>
      <td>
        为Static或PPP-Static模式设置解算类型：<br>
        - All：输出处理期间的所有解算结果。<br>
        - Single：仅输出处理期间的一个解算结果。解算的时间为处理期间的第一个历元。
      </td>
      <td>out-solstatic</td>
      <td>仅适用于RTKPOST</td>
    </tr>
    <tr>
      <td>NMEA Interval (s) RMC/GGA</td>
      <td>设置NMEA GPRMC、GPGGA消息的输出间隔</td>
      <td>out-nmeaintv1</td>
      <td>仅适用于RTKNAVI</td>
    </tr>
    <tr>
      <td>NMEA Interval (s) GSA/GSV</td>
      <td>设置NMEA GPGSA、GLGSA、GAGSA、GPGSV、GLGSV、GAGSV消息的输出间隔</td>
      <td>out-nmeaintv2</td>
      <td>仅适用于RTKNAVI</td>
    </tr>
    <tr>
      <td>Output Solution Status</td>
      <td>
        设置解算状态文件的输出级别。解算状态文件包含估计状态和残差。解算状态文件在当前目录（RTKNAVI）或输出文件目录（RTKPOST）中创建。
      </td>
      <td>out-outstat</td>
      <td></td>
    </tr>
    <tr>
      <td>Output Debug Trace</td>
      <td>设置调试跟踪文件的输出级别。如果设置为关闭，则不会输出任何调试跟踪文件。调试跟踪文件在当前目录（RTKNAVI）或输出文件目录（RTKPOST）中创建。</td>
      <td>-</td>
      <td></td>
    </tr>
  </tbody>
</table>
*1 WW15MGH.DAC (http://earth-info.nga.mil/GandG/wgs84/gravitymod/egm96/binary/binarygeoid.html)<br>
*2 Und_min1x1_egm2008_isw=82_WGS84_TideFree_SE, 
Und_min2.5x2.5_egm2008_isw=82_WGS84_TideFree_SE
(http://earth-info.nga.mil/GandG/wgs84/gravitymod/egm2008/egm08_wgs84.html)<br>
*3 gsigeome.ver4 (http://vldb.gsi.go.jp/sokuchi/geoid/download/down.html)

### 3.5.4 Statistics

<table>
  <thead>
    <tr>
      <th>选项</th>
      <th>描述</th>
      <th>配置名称</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="4"><strong>Measurement Errors</strong></td>
    </tr>
    <tr>
      <td>Code/CarrierPhase Error Rate L1/L2/L5</td>
      <td>设置L1、L2和L5的伪距误差标准差与载波相位误差标准差的比率。</td>
      <td>
        stat-seratio1,<br>
        eratio2,<br>
        eratio5
      </td>
      <td>默认值：300，300，300</td>
    </tr>
    <tr>
      <td>Carrier-Phase Error: a</td>
      <td>设置载波相位误差标准差的基准项（单位：m）。</td>
      <td>stat-serrphase</td>
      <td>默认值：0.003</td>
    </tr>
    <tr>
      <td>Carrier-Phase Error: b/sinEl</td>
      <td>设置载波相位误差标准差的仰角依赖项（单位：m/sin(el)）</td>
      <td>stat-serrphaseel</td>
      <td>默认值：0.003</td>
    </tr>
    <tr>
      <td>Carrier-Phase Error/Baseline</td>
      <td>设置载波相位误差标准差的基线长度依赖项（单位：m/10km）。</td>
      <td>stats-errphasebl</td>
      <td>默认值：0</td>
    </tr>
    <tr>
      <td>Carrier-Phase Error: SNR: e / SNR Max</td>
      <td>
        设置与信噪比相关的载波相位误差标准偏差项。<br>
        e * 10^(0.1*(SNR_MAX - snr))
      </td>
      <td>
        stat-<br>
        serrsnr<br>
        snrmax
      </td>
      <td>默认值：0，0</td>
    </tr>
    <tr>
      <td>Carrier-Phase Error: RCV Errs</td>
      <td>设置载波相位误差标准差的接收机不确定性依赖项。（仅适用于u-blox）</td>
      <td>stats-errrcv</td>
      <td>默认值：0</td>
    </tr>
    <tr>
      <td>Doppler Frequency</td>
      <td>设置多普勒误差的标准差（单位：Hz）。</td>
      <td>stats-errdoppler</td>
      <td>默认值：1</td>
    </tr>
    <tr>
      <td colspan="4"><strong>Process Noises</strong></td>
    </tr>
    <tr>
      <td>Receiver Accel Horiz/Vertical</td>
      <td>
        设置接收机加速度的处理噪声标准差作为水平或垂直分量（单位：m/s2/sqrt(s)）。如果接收机动态设置为关闭，则不使用这些值。
      </td>
      <td>
        stats-<br>
        prnaccelh,<br>
        prnaccelv
      </td>
      <td>默认值：1，0.1</td>
    </tr>
    <tr>
      <td>Carrier-Phase Bias</td>
      <td>设置载波相位偏差（模糊度）的处理噪声标准差（单位：cycle/sqrt(s)）。</td>
      <td>stats-prnbias</td>
      <td>默认值：1E-4</td>
    </tr>
    <tr>
      <td>Vertical Ionospheric Delay</td>
      <td>设置每10公里基线的垂直电离层延迟的处理噪声标准差（单位：m/sqrt(s)）。</td>
      <td>stats-prniono</td>
      <td>默认值：1E-3</td>
    </tr>
    <tr>
      <td>Zenith Tropospheric Delay</td>
      <td>设置天顶对流层延迟的处理噪声标准差（单位：m/sqrt(s)）。</td>
      <td>stats-prntrop</td>
      <td>默认值：1E-4</td>
    </tr>
    <tr>
      <td>Satellite Clock Stability</td>
      <td>设置卫星钟的稳定性（单位：s/s）。该值用于插值基准站观测值。</td>
      <td>stats-clkstab</td>
      <td>默认值：5E-12</td>
    </tr>
  </tbody>
</table>

### 3.5.5 Positions

<table style="text-align: left;">
  <thead>
    <tr>
      <th>选项</th>
      <th>描述</th>
      <th>配置名称</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="4"><strong>Rover</strong></td>
    </tr>
    <tr>
      <td>Lat/Lon/Height (deg/m)</td>
      <td>如果流动站天线是固定的，则设置流动站天线的位置。参见基站的相同字段。</td>
      <td>
        ant1-
        postype, 
        pos1, 
        pos2, pos3
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Antenna Type</td>
      <td>
        选择流动站天线的类型。<br>
        要选择天线类型，请在“文件”中设置接收机天线PCV文件路径。如果使用“*”，则天线类型和天线偏差将通过RINEX观测文件头中的天线信息（RTKPOST）或RTCM天线信息（RTKNAVI）来识别。
      </td>
      <td>ant1-anttype</td>
      <td></td>
    </tr>
    <tr>
      <td>Delta-E/N/U</td>
      <td>
        设置流动站天线的偏差位置，作为天线参考点（ARP）相对于标记点的东/北/上偏移量（单位：米）。
      </td>
      <td>
        ant1-
        antdele,
        antdeln,
        antdelu
      </td>
      <td></td>
    </tr>
    <tr>
      <td colspan="4"><strong>Base Station</strong></td>
    </tr>
    <tr>
      <td>Lat/Lon/Height (deg/m)</td>
      <td>
        设置基准站天线的位置。<br>
        - Lat/Lon/Height (deg/m)：以度和米为单位的纬度、经度和高程。<br>
        - Lat/Lon/Height (dms/m)：以度、分、秒和米为单位的纬度、经度和高程。<br>
        - X/Y/Z-ECEF (m)：ECEF坐标系中的X/Y/Z分量。<br>
        - RTCM Station Position：使用RTCM消息中包含的天线位置 *<br>
        - Average of Single-Pos：使用单点解的平均值 **<br>
        - Get from Position File：使用位置文件中的位置。通过使用流动站观测数据文件路径的前4个字符ID来搜索站点。**<br>
        - RINEX Header Position：使用RINEX观测文件头中的近似位置。**<br>
      </td>
      <td>
        ant2-
        postype, 
        pos1, 
        pos2, pos3
      </td>
      <td>
        高程被指定为椭球高程
        * 仅限RTKNAVI
        ** 仅限RTKPOST
      </td>
    </tr>
    <tr>
      <td>Antenna Type</td>
      <td>
        选择基准站天线的类型。
        要选择天线类型，请在“文件”中设置接收机天线PCV文件。如果使用“*”，则天线类型和天线偏差将通过RINEX观测文件头中的天线信息（RTKPOST）或RTCM天线信息（RTKNAVI）来识别。
      </td>
      <td>ant2-anttype</td>
      <td></td>
    </tr>
    <tr>
      <td>Delta-E/N/U</td>
      <td>
        设置基准站天线的偏差位置，作为天线参考点（ARP）相对于标记点的东/北/上偏移量（单位：m）。
      </td>
      <td>
        ant2-
        antdele,
        antdeln,
        antdelu
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Station Position File</td>
      <td>
        输入站点位置文件路径以从站点列表中检索位置。站点位置文件是一个包含多行的文本文件。每一行代表一个站点的记录。记录包含：<br>
        - Latitude (deg)<br>
        - Longitude (deg)<br>
        - Ellipsoidal height (m)<br>
        - Station ID<br>
        - Station name<br>
        这些信息由空格分隔。以“%”开头的行被视为注释行。一个示例站点位置文件可以在 rtklib_&lt;ver&gt;\data\stations.pos 中找到。<br>
        SINEX站点位置也可以使用。一个示例SINEX文件可以在 rtklib__&lt;ver&gt;\data\igs10P1565_wocov.snx 中找到。
      </td>
      <td>file-staposfile</td>
      <td></td>
    </tr>
  </tbody>
</table>

如果您在“位置”选项卡中设置了站点位置文件，您可以通过在“站点”对话框中点击 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮从站点列表中选择流动站或基准站天线的位置。您可以加载纬度/经度/高程格式或SINEX格式的文件。文件类型会自动识别。您可以使用“查找”按钮和在 <span style="border: 1px solid black; padding: 3px;">Find</span> 字段中指定的关键词来搜索站点ID或名称。

![Positions Dialog of RTKNAVI and RTKPOST Options](https://i.ibb.co/J2F8MLD/image.png)

### 3.5.6 Files

![Options Dialog (Files) of RTKNAVI and RTKPOST](https://i.ibb.co/4SwspFM/image.png)

<table>
  <thead>
    <tr>
      <th>选项</th>
      <th>描述</th>
      <th>配置名称</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Satellite Antenna PCV File ANTEX</td>
      <td>
        如果您使用精密星历或SSR修正，请输入卫星天线PCV（相位中心变化）修正的ANTX天线参数文件路径。通常使用IGS提供的最新igs08.atx文件。<br>
        ANTX文件的示例可以在 rtklib_&lt;ver&gt;/data/igs08.atx 中找到。
      </td>
      <td>file-satantfile</td>
      <td></td>
    </tr>
    <tr>
      <td>Receiver Antenna PCV File ANTEX or NGS PCV</td>
      <td>
        如果您应用接收机天线相位中心偏移和PCV修正，请输入ANTX或NGS类型的天线参数文件路径。
        天线参数文件的示例可以在 rtklib_&lt;ver&gt;\data\igs08.atx 或 rtklib_&lt;ver&gt;\data\ngs_abs.pcv 中找到。
      </td>
      <td>file-rcvantfile</td>
      <td></td>
    </tr>
    <tr>
      <td>Geoid Data File</td>
      <td>如果选择外部模型作为大地水准面模型，请输入大地水准面数据文件的路径。</td>
      <td>file-geoidfile</td>
      <td></td>
    </tr>
    <tr>
      <td>DCB Data File</td>
      <td>
        输入用于PPP的DCB修正的CODE格式文件路径。<br>
        天线参数文件的示例可以在以下位置找到：<br>
        `rtklib_&lt;ver&gt;\data\P1C1_ALL.DCB` <br>
        `rtklib_&lt;ver&gt;\data\P2C2.DCB` <br>
        `rtklib_&lt;ver&gt;\data\P1P2_ALL.DCB`
      </td>
      <td>file-dcbfile</td>
      <td></td>
    </tr>
    <tr>
      <td>EOP Data File</td>
      <td>输入EOP数据文件的路径。EOP数据文件的格式应为IGS ERP格式版本2<sup>[62]</sup>。</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Ocean Loading BLQ Format</td>
      <td>输入OTL系数文件的文件路径。OTL系数文件的格式为BLQ格式。<sup>[63]</sup></td>
      <td>file-blqfile</td>
      <td></td>
    </tr>
    <tr>
      <td>Ionosphere Data File</td>
      <td>输入电离层修正文件（*.yyi）的文件路径。</td>
      <td>-</td>
      <td>仅适用于RTKPOST</td>
    </tr>
    <tr>
      <td>FTP/HTTP Local Directory</td>
      <td>
        输入FTP/HTTP下载的本地目录。下载的文件将保存在该目录中。
      </td>
      <td>file-tempdir</td>
      <td>仅适用于RTKNAVI</td>
    </tr>
  </tbody>
</table>

### 3.5.7 Misc (RTKNAVI)
![Options Dialog (Misc) of RTKNAVI](https://i.ibb.co/Zx3P2mW/image.png)

<table>
  <thead>
    <tr>
      <th>选项</th>
      <th>描述</th>
      <th>配置名称</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Processing Cycle/ Buffer Size</td>
      <td>
        设置系统的处理周期（ms）。通常设置100毫秒或更小的数值。<br>
        设置输入消息缓冲区大小（byte）。通常设置为32768或更高。
      </td>
      <td>misc-svrcycle</td>
      <td></td>
    </tr>
    <tr>
      <td>Timeout/ ReConnect Interval</td>
      <td>
        设置TCP和NTRIP客户端连接的超时和重新连接间隔（以毫秒为单位）。如果超时时间已过，但服务器任未响应，RTKNAVI将在设定的重新连接间隔后重试连接到服务器。
      </td>
      <td>
        misc-timeout,
        reconnect
      </td>
      <td></td>
    </tr>
    <tr>
      <td>
        NMEA Cycle/ File Swap Margin
      </td>
      <td>
        设置发送给NRTK服务器的NMEA GPGGA传输周期（ms）。<br>
        如果启用了输出或日志文件交换，请以秒为单位设置前一个输出文件和新输出文件之间的重叠时段。如果将其设置为0，则这些文件的周期不会重叠。此功能是为了避免输出文件交换丢失瞬时数据。
      </td>
      <td>misc-nmeacycle</td>
      <td></td>
    </tr>
    <tr>
      <td>Solution Buffer/Log Size</td>
      <td>
        设置每个历元内部解算和日志缓冲区大小。若想在“RTK Map”上增加接收机轨迹的长度，可增大解算缓冲区大小。
      </td>
      <td>misc-buffsize</td>
      <td></td>
    </tr>
    <tr>
      <td>Navigation Message Selection</td>
      <td>
        选择要使用的导航信息。<br>
        - (1) All：设置在任意输入流中<br>
        - (2) Rover：设置在流动站接收机流中<br>
        - (3) Base Station：设置在基准站接收机流中<br>
        - (4) Correction：设置在修正流中
      </td>
      <td>misc-navmsgsel</td>
      <td></td>
    </tr>
    <tr>
      <td>SBAS Satellite Selection</td>
      <td>
        如果启用了SBAS DGPS校正，请输入要使用的SBAS卫星PRN号。如果输入0，则使用所有可用的SBAS卫星。
      </td>
      <td>misc-sbasatsel</td>
      <td>默认值：52001</td>
    </tr>
    <tr>
      <td>Monitor Port</td>
      <td>
        设置监视器端口号。监控端口是TCP服务器端口，用于从外部连接或通过RTKPLOT进行实时解决方案监控。如果生成了多个RTKNAVI实例，则会自动使用以下数字。如果设置为0，则不使用监视器端口。
      </td>
      <td>-</td>
      <td></td>
    </tr>
    <tr>
      <td>HTTP/NTRIP Proxy</td>
      <td>
        将HTTP/NTRIP代理服务器地址和端口号设置为&lt;address&gt;：&lt;port&gt;形式，以通过HTTP代理服务器连接NTRIP Caster。
      </td>
      <td>
        misc-proxyaddr
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Solution Font</td>
      <td>在主窗口中选择解决方案显示的字体。</td>
      <td>-</td>
      <td></td>
    </tr>
    <tr>
      <td>TLE Data</td>
      <td>
        指定NORAD TLE（两行元素）卫星轨道元素数据文件。当卫星星历不可用时，TLE数据用于计算用于天球图的卫星位置。TLE数据可以使用两行格式或三行格式。示例TLE数据可以在以下位置找到：<br>
        rtklib_&lt;ver&gt;/data/catalble_2l_2013_01_09_pm.txt。
      </td>
      <td>-</td>
      <td>*1</td>
    </tr>
    <tr>
      <td>Sat No</td>
      <td>
        指定卫星编号文件，该文件用于将GNSS卫星/PRN编号与NORAD TLE数据文件中的TLE卫星目录编号相连接。示例卫星编号文件可以在以下位置找到：<br>
        rtklib_&lt;ver&gt;/data/TLE_GNSS_SATNO.txt。
      </td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

*1 To obtain the latest TLE data file, refer the following URLs:<br>
(a) CelesTrack: http://celestrak.com<br>
(b) SpaceTrack: http://www.space-track.org


### 3.5.8 Misc (RTKPOST)
![Options Dialog (Misc) of RTKPOST](https://i.ibb.co/0J2Jq8M/image.png)

<table>
  <thead>
    <tr>
      <th>选项</th>
      <th>描述</th>
      <th>配置名称</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Time Interpolation of Base Station Observation Data</td>
      <td>
        是否启用基站数据的时间插值。如果选择ON，则基站数据被线性插值到Rover的历元中，并使用它们进行双差。如果未选中，则使用基站数据的最近历元进行双差。
      </td>
      <td>misc-timeinterp</td>
      <td></td>
    </tr>
    <tr>
      <td>SBAS Satellite Selection</td>
      <td>
        如果启用了SBAS DGPS校正，请输入要使用的SBAS卫星PRN号。如果输入0，则使用所有可用的SBAS卫星。
      </td>
      <td>misc-sbasatsel</td>
      <td></td>
    </tr>
    <tr>
      <td>RINEX Opt (Rover)</td>
      <td>
        指定流动站RINEX观测数据的RINEX读取选项，如下所示。多个选项可以用空格分隔输入。<br>
        -GLss[=+n.nn]：选择GPS信号ss<br>
        -RLss[=+n.nn]：选择GLO信号ss<br>
        -ELss[=+n.nn]：选择GAL信号ss<br>
        -JLss[=+n.nn]：选择QZS信号ss<br>
        -CLss[=+n.nn]：选择BDS信号ss<br>
        -SLss[=+n.nn]：选择SBS信号ss<br>
        ss：信号编号（参考附录D.3）<br>
        =+n.nn：相位偏移+n.nn（周期），将被添加到载波相位观测值<br>
        如果没有选项且一个频率中有多个信号，RTKLIB将根据信号的默认优先级选择一个频率的信号。参考附录D.4 多信号的默认优先级。
      </td>
      <td>misc-rnxopt1</td>
      <td></td>
    </tr>
    <tr>
      <td>RINEX Opt (Base)</td>
      <td>
        为基站RINEX观测数据指定与RINEX Opt（Rover）相同的RINEX读取选项。
      </td>
      <td>misc-rnxopt2</td>
      <td></td>
    </tr>
    <tr>
      <td>Station ID List</td>
      <td>
        对于具有多个输入文件或多个会话的批量处理，您可以设置包含以下关键字的输入文件路径或输出文件路径：<br>
        %Y，%y，%m，%d，%n，%W，%D，%h，%H，%r，%b<br>
        这些关键字将被替换为适当的值或在多会话分析中进行扩展。<br>
        要启用时间关键字，请在主窗口中设置开始时间、结束时间和单位（可选）。对于关键字%r、%b，需在下方输入流动站列表或基准站列表。<br>
        如需在线参考，请点击 <span style="border: 1px solid black; padding: 3px;">?</span> 按钮。
      </td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Rovers</td>
      <td>
        输入流动站ID列表以替换输入和输出文件路径中的关键字%r。以“#”开头的行被视为注释。
      </td>
      <td>-</td>
      <td></td>
    </tr>
    <tr>
      <td>Base Stations</td>
      <td>
        输入基站ID列表以替换输入和输出文件路径中的关键字%b。以“#”开头的行被视为注释。
      </td>
      <td>-</td>
      <td></td>
    </tr>
  </tbody>
</table>
