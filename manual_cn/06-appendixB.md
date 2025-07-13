---
sidebarDepth: 2
---

# 附录B. 文件格式

## B.1 解算结果文件

**描述**

解算结果文件仅仅是RTKNAVI或RTKPOST输出的文本文件。该文件通过CR/LF分隔为记录或行。每条记录包含多个字段。下表展示了解算结果文件的格式。

<table>
    <thead>
    <tr>
        <th>序号</th>
        <th>记录/域</th>
        <th>描述</th>
        <th>备注</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>1</td>  
        <td>文件头</td>  
        <td>
            以“%”开头的行是文件头行。文件头行包含一些额外信息或处理选项，如下所示。<br>
            % program : 程序版本<br>
            % inp file : 输入文件路径<br>
            % obs start: 观测数据开始时间（GPS时间）<br>
            % obs end: 观测数据结束时间（GPS时间）<br>
            % pos mode: 定位模式选项<br>
            % freqs: 频率选项<br>
            % solution: 解算类型选项<br>
            % elev mask: 仰角遮蔽角选项<br>
            % snr mask: 信噪比遮蔽选项<br>
            % ionos est: 电离层参数估计选项<br>
            % tropos est: 对流层参数估计选项<br>
            % amb res: 整数模糊度解算选项<br>
            % val thres: 整数模糊度验证选项<br>
            % ref pos: 基准站天线的位置
        </td>  
        <td></td>  
    </tr>
    <tr>
        <td>2</td>  
        <td>字段指示符</td>  
        <td>
            以“%”开头的字段指示符行位于文件头之后。为了识别字段格式，RTKLIB使用这些行。请不要删除它们。
        </td>
        <td></td>  
    </tr>
    <tr>
        <td>3</td>  
        <td>解算结果主体</td>  
        <td>
            解算结果主体包含以下字段。字段内容会根据定位选项而变化。
        </td>  
        <td></td>  
    </tr>
    <tr>
        <td>(1)</td>  
        <td>Time</td>  
        <td>
            解算历元时间，表示真实的接收机信号接收时间（不是接收机时钟指示的时间）。格式会根据选项变化。<br>
            yyyy/mm/dd HH:MM:SS.SSS ：<br>
            日历时间，以GPST、UTC或JST表示，时间系统在字段指示符中标识。<br>
            WWWW SSSSSSS.SSS ：<br>
            GPS周和周内时间（TOW，以秒为单位）。
        </td>  
        <td></td>  
    </tr>
    <tr>
        <td>(2)</td>  
        <td>接收机位置</td>  
        <td>
            根据定位选项变化的流动站接收天线或标记位置的估计值。<br>
            +ddd.ddddddddd +ddd.dddddddd hhhh.hhhh ：<br>
            纬度和经度（度）以及高度（m）。负值表示南纬或西经。高度根据定位选项表示为椭球高度或大地高度。<br>
            +ddd mm ss.sss +ddd mm ss.sss hhhh.hhhh ：<br>
            纬度和经度（度、分、秒）以及高度（m）。<br>
            +xxxxxxxxx.xxxx +yyyyyyyyy.yyyy +zzzzzzzz.zzzz ：<br>
            ECEF坐标系中的X/Y/Z分量（m）。<br>
            +eeeeeeeee.eeee +nnnnnnnnn.nnnn +uuuuuuuuu.uuuu ：<br>
            基线向量的E/N/U分量（m）。本地坐标系参考流动站位置。
        </td>  
        <td></td>  
    </tr>
    <tr>
        <td>(3)</td>  
        <td>Quality flag (Q)</td>  
        <td>
            表示解算质量的标志。<br>
            1: Fixed，通过载波相对定位得到的解算结果，并且整数模糊度已正确解算。<br>
            2: Float，通过载波相对定位得到的解算结果，但整数模糊度未解算。<br>
            3: Reserved<br>
            4: DGPS,，通过码基差分GPS解算或带有SBAS修正的单点定位得到的解算结果<br>
            5: Single，通过单点定位得到的解算结果
        </td>  
        <td></td>  
    </tr>
    <tr>
        <td>(4)</td>  
        <td>Number of valid satellites (ns)</td>  
        <td>
            用于解算估计的有效卫星数量。
        </td>  
        <td></td>  
    </tr>
    <tr>
        <td>(5)</td>  
        <td>
            Standard deviations(sdn, sde, sdu, sdne, sdeu, sdun)
        </td>  
        <td>
            假设先验误差模型和定位选项中的误差参数，解算的估计标准差。
            sdn、sde或sdu分别表示标准差的北（N）、东（E）或上（U）分量，单位为米。sdne、sdeu或sdun的绝对值表示估计协方差矩阵的NE、EU或UN分量的绝对值的平方根。符号表示协方差的符号。利用所有这些值，用户可以重构完整的协方差矩阵。
        </td>  
        <td></td>  
    </tr>
    <tr>
        <td>(6)</td>  
        <td>Age of differential (age)</td>  
        <td>
            流动站接收机与基准站观测数据历元之间的时间差，单位为秒。
        </td>  
        <td></td>  
    </tr>
    <tr>
        <td>(7)</td>  
        <td>
            Ratio factor (ratio)
        </td>  
        <td>
            标准整数模糊度验证策略中“ratio-test”的比率因子。该值表示次佳整数向量与最佳整数向量的残差平方和的比率。
        </td>  
        <td></td>  
    </tr>
    <tr>
        <td>(8)</td>  
        <td>Velocity (optional) (vn,ve,vu, sdvn,sdve sdvu,sdvne sdveu,sdvun)</td>  
        <td>
            速度分量及其标准差（从选项菜单启用）
        </td>  
        <td></td>  
    </tr>
    </tbody>
</table>

**示例**
```text
% program : RTKLIB ver.2.3.0b
% inp file : G:\rtklibtest\20090831\omre196a.09o
% inp file : G:\rtklibtest\20090831\tevc196a.09o
% inp file : G:\rtklibtest\20090831\omre196a.09n
% obs start : 2009/07/15 07:10:00.0 GPST (week1540 285000.0s)
% obs end : 2009/07/15 07:59:50.0 GPST (week1540 287990.0s)
% pos mode : kinematic
% freqs : L1+L2
% solution : forward
% elev mask : 15.0 deg
% snr mask : 0.0 dBHz
% ionos est : on
% tropo est : on
% amb res : continuous
% val thres : 3.0
% ref pos : 32.574831620 -117.126551777 -28.1471
%
% (lat/lon/height=WGS84/ellipsoidal,Q=1:fix,2:float,4:dgps,5:single,ns=# of satellites)
% GPST latitude(deg) longitude(deg) height(m) Q ns sdn(m) sde(m) sdu(m) sdne(m) sdeu(m) sdun(m) age(s) ratio
2009/07/15 07:10:00.000 32.560273272 -116.953525346 118.6783 1 10 0.0186 0.0202 0.0899 -0.0072 0.0089 -0.0249 0.00 4.5
2009/07/15 07:10:10.000 32.560273266 -116.953525340 118.6877 1 10 0.0144 0.0154 0.0776 -0.0058 0.0082 -0.0199 0.00 5.3
2009/07/15 07:10:20.000 32.560273262 -116.953525365 118.6853 1 10 0.0124 0.0131 0.0720 -0.0051 0.0078 -0.0173 0.00 5.3
2009/07/15 07:10:30.000 32.560273251 -116.953525345 118.6825 1 10 0.0111 0.0117 0.0686 -0.0046 0.0075 -0.0157 0.00 5.6
2009/07/15 07:10:40.000 32.560273275 -116.953525412 118.6827 1 10 0.0103 0.0108 0.0662 -0.0043 0.0073 -0.0146 0.00 4.7
2009/07/15 07:10:50.000 32.560273277 -116.953525429 118.6812 1 10 0.0097 0.0102 0.0644 -0.0041 0.0071 -0.0138 0.00 4.1
2009/07/15 07:11:00.000 32.560273249 -116.953525449 118.6817 1 10 0.0092 0.0097 0.0630 -0.0039 0.0069 -0.0132 0.00 4.2
2009/07/15 07:11:10.000 32.560273271 -116.953525464 118.6729 1 10 0.0088 0.0093 0.0618 -0.0038 0.0067 -0.0127 0.00 5.2
2009/07/15 07:11:20.000 32.560273246 -116.953525468 118.6772 1 10 0.0085 0.0089 0.0607 -0.0037 0.0066 -0.0123 0.00 6.1
2009/07/15 07:11:30.000 32.560273219 -116.953525461 118.6733 1 10 0.0083 0.0087 0.0598 -0.0036 0.0065 -0.0119 0.00 7.9
2009/07/15 07:11:40.000 32.560273216 -116.953525478 118.6771 1 10 0.0081 0.0085 0.0590 -0.0035 0.0064 -0.0117 0.00 9.0
2009/07/15 07:11:50.000 32.560273206 -116.953525489 118.6726 1 10 0.0079 0.0083 0.0582 -0.0035 0.0062 -0.0114 0.00 8.6
2009/07/15 07:12:00.000 32.560273201 -116.953525497 118.6744 1 10 0.0078 0.0081 0.0575 -0.0034 0.0061 -0.0112 0.00 7.5
2009/07/15 07:12:10.000 32.560273212 -116.953525455 118.6731 1 10 0.0077 0.0080 0.0568 -0.0034 0.0060 -0.0110 0.00 7.9
```

## B.2 SBAS 日志文件

**描述**

SBAS日志文件是RTKCONV的输出，这是一个文本文件，其中每一行包含由GPS/GNSS接收机捕获的SBAS消息。下表展示了SBAS日志文件的格式。

<table>
    <thead>
        <tr>
            <th>序号</th>
            <th>记录/域</th>
            <th>描述</th>
            <th>备注</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>SBAS messages</td>
            <td>一行包含一个SBAS导航数据帧，该数据帧由以下字段组成。</td>
            <td></td>
        </tr>
        <tr>
            <td>(1)</td>
            <td>GPS week number</td>
            <td>SBAS导航数据帧的GPS周号。</td>
            <td></td>
        </tr>
        <tr>
            <td>(2)</td>
            <td>Time of week</td>
            <td>SBAS导航数据帧的周内时间（以秒为单位）。</td>
            <td></td>
        </tr>
        <tr>
            <td>(3)</td>
            <td>PRN number</td>
            <td>
                传输导航数据的SBAS卫星的PRN编号
            </td>
            <td></td>
        </tr>
        <tr>
            <td>(4)</td>
            <td>Message type</td>
            <td>
                帧中SBAS消息的类型ID（0 - 63）。详细信息请参考SBAS规范：<br>
                RTCA/DO-229C，全球定位系统/广域增强系统机载设备的最低运行性能标准
            </td>
            <td></td>
        </tr>
        <tr>
            <td>(5)</td>
            <td>Separator</td>
            <td>:</td>
            <td></td>
        </tr>
        <tr>
            <td>(6)</td>
            <td>SBAS message</td>
            <td>
                226位SBAS消息的十六进制转储，不包含24位奇偶校验字段。消息尾部用0填充以对齐到8位边界。详细消息格式请参考SBAS规范。
            </td>
            <td></td>
        </tr>
    </tbody>
</table>

**示例**
```text
1471 349007 137 25 : C666A0A7F1FE6000027F80000003468000000000000000000000000000
1471 349007 129 25 : C666A0A7F4FE6004047F80000003468000000000000000000000000000
1471 349008 129  4 : 53109FFFFF5FFDFFDFFDFFFFC7FA9FFDFFDFFDFFDFFF9BBBBB33FFFFC0
1471 349008 137  4 : 53129FFC001FFDFFDFFDFFFFA0009FFDFFDFFDFFDFFF9BBBBB33FFFFC0
1471 349009 137  3 : 9A0C9FFDFFDFFFFFDFFC017FF9FFDFFC009FFC015FFFBB97B9BB9FBB80
1471 349009 129  2 : 9A0A9FFFFC9FFFFE9FFDFFDFFDFFDFFDFFFFF7F93FFBE79BBBBBB9FA00
1471 349010 137  2 : C60A9FFFFD1FFFFFDFFDFFDFFDFFDFFDFFC003F88003E79FBBFBB9FA00
1471 349010 129  3 : C60E9FFDFFDFFFFE9FFC007FEDFFDFFFFFDFFFFFDFFFBB97B9BB9FBB80
1471 349011 137 26 : 536A0029E0EF0FF05F829C11C076033015A09D047037C1DE14F08FE000
1471 349011 129 26 : 536A0821A0DD05E82B813E0EF0F7897C27C12E08683B419C0BE057E000
1471 349012 137 28 : 9A723440E44E810029FF1F1F379C0BC35D4BE2B8078F15903253960200
1471 349012 129 28 : 9A722CB5D8739087B46B107DA8D9E828694B55F843782100AF146AD980
1471 349013 129  9 : C62434198D3F5D92BA855704800236DFE84FE06FFA47FE7FF0008E0240
1471 349013 137  9 : C6260C198D32310732404C1D40183CDFD187C8F3FF7FFD800FF7D6BE40
1471 349014 129  4 : 53119FFFFF9FFDFFDFFDFFFFD3FA5FFDFFDFFDFFDFFF9BBBBB33FFFFC0
1471 349014 137  4 : 53109FFC005FFDFFDFFDFFFFAFFFDFFDFFDFFDFFDFFF9BBBBB33FFFFC0
1471 349015 129  2 : 9A089FFFFC5FFFFEDFFDFFDFFDFFDFFDFFFFFBF8FFFFE79BBBBBB9FA00
1471 349015 137  3 : 9A0D9FFDFFDFFFFF9FFC017FFDFFDFFC00DFFC015FFFBB97B9BB9FBB80
1471 349016 137  2 : C6089FFFFD5FFC001FFDFFDFFDFFDFFDFFFFFFF8BFFFE79FBBFBB9FA00
1471 349016 129  3 : C60C9FFDFFDFFFFE5FFC007FF1FFDFFC001FFFFFDFFFBB97B9BB9FBB80
1471 349017 137 25 : 5366587803FE3FF0010080FFFF835E8000000000000000000000000000
1471 349017 129 25 : 5366587FFDFEDFF40400800000035E8000000000000000000000000000
1471 349018 129 63 : 9AFC000000000000000000000000000000000000000000000000000000
1471 349018 137 63 : 9AFC000000000000000000000000000000000000000000000000000000
1471 349019 129 26 : C66A0C53E26F09704781DC0DE06702FC19E1EF09F047821C0EF05FE000
1471 349019 137 26 : C66A0431E17E0A704741F80DC05E827815C0EF09F877C2FC15E096E000
1471 349020 137  4 : 53119FFC001FFDFFDFFDFFFF9C001FFDFFDFFDFFDFFF9BBBBB33FFFFC0
1471 349020 129  4 : 53129FFFFF5FFDFFDFFDFFFFC3FA9FFDFFDFFDFFDFFF9BBBBB33FFFFC0
```

## B.3 解状态文件

**描述**

解算状态文件是RTKNAVI或RTKPOST的输出，是一个包含定位过程内部状态的文本文件。内部状态包括卡尔曼滤波器的估计状态和测量残差，用于分析解算质量。下表展示了解算状态文件的格式。

<table>
    <thead>
        <tr>
            <th>序号</th>
            <th>记录/域</th>
            <th>描述</th>
            <th>备注</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>
            <td>Position States</td>
            <td>
                滤波器中估计的流动站位置。记录的格式如下。<br>
                `$POS,week,tow,stat,posx,posy,posz,posxf,posyf,poszf` <br>
                week/tow : GPS周号/周内时间（秒）<br>
                stat : 解算状态<br>
                posx/posy/posz : ECEF坐标系中的位置x/y/z（m，浮点数）<br>
                posxf/posyf/poszf : ECEF坐标系中的位置x/y/z（m，固定解）
            </td>
            <td></td>
        </tr>
        <tr>
            <td>2</td>
            <td>Velocity/Acceleration States</td>
            <td>
                滤波器中估计的流动站速度和加速度。记录的格式如下。<br>
                `$VELACC,week,tow,stat,vele,veln,velu,acce,accn,accu,velef,velnf,veluf,accef,accnf,accuf`<br>
                week/tow : GPS周号/周内时间（秒）<br>
                stat : 解算状态<br>
                vele/veln/velu : 速度东/北/上（m/s，浮点数）<br>
                acce/accn/accu : 加速度东/北/上（m/s²，浮点数）<br>
                velef/velnf/veluf : 速度东/北/上（m/s，固定解）<br>
                accef/accnf/accuf : 加速度东/北/上（m/s²，固定解）
            </td>
            <td></td>
        </tr>
        <tr>
            <td>3</td>
            <td>Receiver Clock-bias States</td>
            <td>
                估计的接收机钟差参数。记录的格式如下。<br>
                `$CLK,week,tow,stat,rcv,clk1,clk2,clk3,clk4,cmn_bias`<br>
                week/tow : GPS周号/周内时间（秒）<br>
                stat : 解算状态<br>
                clk1 : 接收机钟差GPS（纳秒）<br>
                clk2 : 接收机钟差GLONASS-GPS（纳秒）<br>
                clk3 : 接收机钟差Galileo-GPS（纳秒）<br>
                clk4 : 接收机钟差北斗-GPS（纳秒）<br>
                cmn_bias : 从所有信号中移除的公共相位偏差
            </td>
            <td></td>
        </tr>
        <tr>
            <td>4</td>
            <td>Ionosphere Parameter States</td>
            <td>
                估计的电离层参数（L1电离层延迟差）。记录的格式如下。<br>
                `$ION,week,tow,stat,sat,az,el,ion,ion-fixed`<br>
                week/tow : GPS周号/周内时间（秒）<br>
                stat : 解算状态<br>
                sat : 卫星ID<br>
                az/el : 方位角/仰角（度）<br>
                ion : L1电离层延迟（m，浮点数）<br>
                ion-fixed : L1电离层延迟（m，固定解）
            </td>
            <td></td>
        </tr>
        <tr>
            <td>5</td>
            <td>Troposphere Parameter States</td>
            <td>
                估计的对流层参数（垂直对流层延迟残差）。记录的格式如下。<br>
                `$TROP,week,tow,stat,rcv,ztd,ztdf`<br>
                week/tow : GPS周号/周内时间（秒）<br>
                stat : 解算状态<br>
                rcv : 接收机（1:流动站，2:基准站）<br>
                ztd : 顶点总延迟（m，浮点数）<br>
                ztdf : 顶点总延迟（m，固定解）
            </td>
            <td></td>
        </tr>
        <tr>
            <td>6</td>
            <td>Receiver H/W bias States</td>
            <td>
                估计的GLONASS接收机H/W偏差差。记录的格式如下。<br>
                `$HWBIAS,week,tow,stat,frq,bias,biasf`<br>
                week/tow : GPS周号/周内时间（秒）<br>
                stat : 解算状态<br>
                frq : 频率（1:L1,2:L2,3:L5,...）<br>
                bias : H/W偏差系数（米/兆赫，浮点数）<br>
                biasf : H/W偏差系数（米/兆赫，固定解）
            </td>
            <td></td>
        </tr>
        <tr>
            <td>7</td>
            <td>Residuals</td>
            <td>
                伪距和载波相位观测值的残差。记录的格式如下。<br>
                `$SAT,week,tow,sat,frq,az,el,resp,resc,vsat,snr,fix,slip,lock,outc,slipc,rejc,icbias,bias,bias_var,lambda`<br>
                week/tow : GPS周号/周内时间（秒）<br>
                sat/frq : 卫星ID/频率（1:L1,2:L2,3:L5,...）<br>
                az/el : 方位角/仰角（度）<br>
                resp : 伪距残差（m）<br>
                resc : 载波相位残差（m）<br>
                vsat : 有效数据标志（0:无效，1:有效）<br>
                snr : 信号强度（dbHz）<br>
                fix : 模糊度标志（0:无数据，1:不是AR集合的一部分，2:是AR集合的一部分，3:是保持集合的一部分）<br>
                slip : 周跳标志（位1:周跳，位2:奇偶性未知）<br>
                lock : 载波锁定计数<br>
                outc : 数据中断计数<br>
                slipc : 周跳计数<br>
                rejc : 数据拒绝（异常值）计数<br>
                icbias : 通道间偏差（GLONASS）<br>
                bias : 相位偏差<br>
                bias_var : 相位偏差的方差<br>
                lambda : 波长
            </td>
            <td></td>
        </tr>
    </tbody>
</table>

**示例**
```text
$POS,1557,432000.000,2,-3869295.9628,3436570.2567,3717367.6546,0.0000,0.0000,0.0000
$HWBIAS,1557,432000.000,2,1,-0.3503,0.0000
$HWBIAS,1557,432000.000,2,2,0.0108,0.0000
$SAT,1557,432000.000,3,1,253.2,64.3,0.3219,-0.0006,1,48,1,1,1,0,1,0
$SAT,1557,432000.000,3,2,253.2,64.3,-0.0629,-0.0006,1,33,1,1,1,0,1,0
$SAT,1557,432000.000,13,1,298.4,24.1,-0.6732,0.0003,1,42,1,1,1,0,1,0
$SAT,1557,432000.000,13,2,298.4,24.1,0.8081,0.0003,1,17,1,1,1,0,1,0
$SAT,1557,432000.000,16,1,42.0,59.5,0.5037,-0.0005,1,47,1,1,1,0,1,0
$SAT,1557,432000.000,16,2,42.0,59.5,-0.5170,-0.0005,1,30,1,1,1,0,1,0
$SAT,1557,432000.000,19,1,229.8,39.0,-0.1948,-0.0003,1,44,1,0,1,0,0,0
$SAT,1557,432000.000,19,2,229.8,39.0,-0.0806,-0.0003,1,28,1,1,1,0,1,0
$SAT,1557,432000.000,21,1,61.1,28.1,-1.0704,0.0001,1,41,1,1,1,0,1,0
$SAT,1557,432000.000,21,2,61.1,28.1,1.0139,0.0001,1,19,1,1,1,0,1,0
$SAT,1557,432000.000,23,1,257.9,29.9,-1.3258,-0.0000,1,45,1,1,1,0,1,0
$SAT,1557,432000.000,23,2,257.9,29.9,0.4155,0.0000,1,23,1,1,1,0,1,0
$SAT,1557,432000.000,25,1,317.0,24.7,0.8868,0.0002,1,41,1,1,1,0,1,0
$SAT,1557,432000.000,25,2,317.0,24.7,0.1811,0.0003,1,19,1,1,1,0,1,0
$SAT,1557,432000.000,31,1,145.1,32.5,0.6140,-0.0001,1,44,1,1,1,0,1,0
$SAT,1557,432000.000,31,2,145.1,32.5,-0.2397,-0.0001,1,26,1,1,1,0,1,0
$SAT,1557,432000.000,R9,1,105.7,78.1,-0.1172,-0.0001,1,45,1,1,1,0,1,0
$SAT,1557,432000.000,R9,2,105.7,78.1,0.0000,0.0000,0,0,0,0,0,1,0,0
$SAT,1557,432000.000,R10,1,331.5,41.7,-0.1425,0.0002,1,43,1,1,1,0,1,0
$SAT,1557,432000.000,R10,2,331.5,41.7,0.0349,0.0001,1,30,1,1,1,0,1,0
$SAT,1557,432000.000,R19,1,18.6,61.2,-0.7708,-0.0000,1,46,1,1,1,0,1,0
$SAT,1557,432000.000,R19,2,18.6,61.2,0.1898,-0.0001,1,39,1,0,1,0,0,0
$SAT,1557,432000.000,R20,1,235.7,55.6,1.0305,-0.0000,1,42,1,1,1,0,1,0
$SAT,1557,432000.000,R20,2,235.7,55.6,-0.2247,-0.0001,1,39,1,1,1,0,1,0
```

## B.4 配置文件

**描述**

一个包含处理选项、解算选项和文件选项的配置文件，用于RTKNAVI、RTKPOST、RTKRCV和RNX2RTKP。这是一个文本文件，包含表示各种选项的“关键字 = 值”形式的记录。对于枚举值，可选值可以是一个数字（0,1,2,...）或一个枚举标签（off, on, ...）。以#开头的行以及行中#后的文本被视为注释。关于配置文件的内容，请参考3.5节 配置RTKNAVI和RTKPOST的定位选项。

```text
# RTKNAVI options (2013/03/01 10:41:04, v.2.4.2)
pos1-posmode =single # (0:single,1:dgps,2:kinematic,3:static,4:movingbase,5:fixed,6:ppp-kine,7:ppp-static)
pos1-frequency =l1+l2 # (1:l1,2:l1+l2,3:l1+l2+l5)
pos1-soltype =forward # (0:forward,1:backward,2:combined)
pos1-elmask =10 # (deg)
pos1-snrmask_r =off # (0:off,1:on)
pos1-snrmask_b =off # (0:off,1:on)
pos1-snrmask_L1 =0,0,0,0,0,0,0,0,0
pos1-snrmask_L2 =0,0,0,0,0,0,0,0,0
pos1-snrmask_L5 =0,0,0,0,0,0,0,0,0
pos1-dynamics =off # (0:off,1:on)
pos1-tidecorr =off # (0:off,1:on)
pos1-ionoopt =brdc # (0:off,1:brdc,2:sbas,3:dual-freq,4:est-stec,5:ionex-tec,6:qzs-brdc,7:qzs-lex,8:vtec_sf,9:vtec_ef,10:gtec)
pos1-tropopt =saas # (0:off,1:saas,2:sbas,3:est-ztd,4:est-ztdgrad)
pos1-sateph =brdc # (0:brdc,1:precise,2:brdc+sbas,3:brdc+ssrapc,4:brdc+ssrcom)
pos1-posopt1 =on # (0:off,1:on)
pos1-posopt2 =on # (0:off,1:on)
pos1-posopt3 =on # (0:off,1:on)
pos1-posopt4 =on # (0:off,1:on)
pos1-posopt5 =off # (0:off,1:on)
pos1-exclsats = # (prn ...)
pos1-navsys =63 # (1:gps+2:sbas+4:glo+8:gal+16:qzs+32:comp)
pos2-armode =fix-and-hold # (0:off,1:continuous,2:instantaneous,3:fix-and-hold)
pos2-gloarmode =off # (0:off,1:on,2:autocal)
pos2-arthres =3
pos2-arlockcnt =0
pos2-arelmask =20 # (deg)
pos2-arminfix =0
pos2-elmaskhold =0 # (deg)
pos2-aroutcnt =5
pos2-maxage =30 # (s)
pos2-slipthres =0.05 # (m)
pos2-rejionno =30 # (m)
pos2-rejgdop =30
pos2-niter =1
pos2-baselen =0 # (m)
pos2-basesig =0 # (m)
out-solformat =llh # (0:llh,1:xyz,2:enu,3:nmea)
out-outhead =off # (0:off,1:on)
out-outopt =off # (0:off,1:on)
out-timesys =gpst # (0:gpst,1:utc,2:jst)
out-timeform =hms # (0:tow,1:hms)
out-timendec =3
out-degform =deg # (0:deg,1:dms)
out-fieldsep =
out-height =geodetic # (0:ellipsoidal,1:geodetic)
out-geoid =internal # (0:internal,1:egm96,2:egm08_2.5,3:egm08_1,4:gsi2000)
out-solstatic =all # (0:all,1:single)
out-nmeaintv1 =1 # (s)
out-nmeaintv2 =1 # (s)
out-outstat =off # (0:off,1:state,2:residual)
stats-eratio1 =300
stats-eratio2 =300
stats-errphase =0.003 # (m)
stats-errphaseel =0.003 # (m)
stats-errphasebl =0 # (m/10km)
stats-errdoppler =1 # (Hz)
stats-stdbias =30 # (m)
stats-stdiono =0.03 # (m)
stats-stdtrop =0.3 # (m)
stats-prnaccelh =10 # (m/s^2)
stats-prnaccelv =10 # (m/s^2)
stats-prnbias =0.0001 # (m)
stats-prniono =0.001 # (m)
stats-prntrop =0.0001 # (m)
stats-clkstab =5e-12 # (s/s)
ant1-postype =llh # (0:llh,1:xyz,2:single,3:posfile,4:rinexhead,5:rtcm)
ant1-pos1 =90 # (deg|m)
ant1-pos2 =0 # (deg|m)
ant1-pos3 =-6335367.6285 # (m|m)
ant1-anttype =NOV702GG
ant1-antdele =0 # (m)
ant1-antdeln =0 # (m)
ant1-antdelu =0 # (m)
ant2-postype =rtcm # (0:llh,1:xyz,2:single,3:posfile,4:rinexhead,5:rtcm)
ant2-pos1 =0 # (deg|m)
ant2-pos2 =0 # (deg|m)
ant2-pos3 =0 # (m|m)
ant2-anttype =TRM29659.00
ant2-antdele =0 # (m)
ant2-antdeln =0 # (m)
ant2-antdelu =0 # (m)
misc-timeinterp =off # (0:off,1:on)
misc-sbasatsel =0 # (0:all)
misc-rnxopt1 =
misc-rnxopt2 =
file-satantfile =Y:\madoca\data\igs08.atx
file-rcvantfile =Y:\madoca\data\igs08.atx
file-staposfile =
file-geoidfile =
file-ionofile =
file-dcbfile =Y:\madoca\data\dcb\P1P21201.DCB
file-eopfile =
file-blqfile =
file-tempdir =C:\Temp
file-geexefile =
file-solstatfile =
file-tracefile =
inpstr1-type =ntripcli # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,7:ntripcli,8:ftp,9:http)
inpstr2-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,7:ntripcli,8:ftp,9:http)
inpstr3-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,7:ntripcli,8:ftp,9:http)
inpstr1-path =kaiyodai:tuomsat00@mgex.igs-ip.net:2101/CUT07:
inpstr2-path =
inpstr3-path =
inpstr1-format =rtcm3 # (0:rtcm2,1:rtcm3,2:oem4,3:oem3,4:ubx,5:ss2,6:hemis,7:skytraq,8:gw10,9:javad,15:sp3)
inpstr2-format =rtcm3 # (0:rtcm2,1:rtcm3,2:oem4,3:oem3,4:ubx,5:ss2,6:hemis,7:skytraq,8:gw10,9:javad,15:sp3)
inpstr3-format =rtcm3 # (0:rtcm2,1:rtcm3,2:oem4,3:oem3,4:ubx,5:ss2,6:hemis,7:skytraq,8:gw10,9:javad,15:sp3)
inpstr2-nmeareq =off # (0:off,1:latlon,2:single)
inpstr2-nmealat =26.37293571 # (deg)
inpstr2-nmealon =127.143649075 # (deg)
outstr1-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,6:ntripsvr)
outstr2-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,6:ntripsvr)
outstr1-path =
outstr2-path =
outstr1-format =llh # (0:llh,1:xyz,2:enu,3:nmea)
outstr2-format =nmea # (0:llh,1:xyz,2:enu,3:nmea)
logstr1-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,6:ntripsvr)
logstr2-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,6:ntripsvr)
logstr3-type =off # (0:off,1:serial,2:file,3:tcpsvr,4:tcpcli,6:ntripsvr)
logstr1-path =
logstr2-path =
logstr3-path =
misc-svrcycle =10 # (ms)
misc-timeout =30000 # (ms)
misc-reconnect =10000 # (ms)
misc-nmeacycle =5000 # (ms)
misc-buffsize =32768 # (bytes)
misc-navmsgsel =all # (0:all,1:rover,2:base,3:corr)
misc-proxyaddr =
misc-fswapmargin =30 # (s)
```

## B.5 GNSS数据的URL列表文件

一个包含互联网资源中GNSS数据URL列表的文件。该文件用于RTKGET下载GNSS数据。每一行表示一个GNSS数据类型的URL记录。#号后的字符串被视为注释。下表展示了GNSS数据URL列表文件的格式。

<table>
    <thead>
        <tr>
            <th>序号</th>
            <th>记录/域</th>
            <th>描述</th>
            <th>备注</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td>  
            <td>URL</td>  
            <td>
                一行包含一个GNSS数据类型的URL，该URL由以下字段组成，字段之间以空格分隔。
            </td>  
            <td></td>  
        </tr>
        <tr>
            <td>(1)</td>  
            <td>Data type</td>  
            <td>
                GNSS数据类型ID。该ID不包含空格。数据类型ID中的下划线“_”表示每个数据类型类别的分隔符。
            </td>  
            <td></td>  
        </tr>
        <tr>
            <td>(2)</td>  
            <td>URL address</td>  
            <td>
                GNSS数据类型的URL地址。URL地址应为：<br>
                ftp://&lt;host address>/&lt;file path&gt; 或<br>
                http://&lt;host address&gt;/&lt;file path&gt;<br>
                ftp, http : 使用的下载协议<br>
                &lt;host address&gt;:：主机的地址<br>
                &lt;file path&gt;：主机上的下载文件路径。文件路径可以包含以下关键字，这些关键字将被日期、时间、站点名称和环境变量替换。<br>
                %Y -> yyyy : 年（4位数字）（2000-2099）<br>
                %y -> yy : 年（2位数字）（00-99）<br>
                %m -> mm : 月（01-12）<br>
                %d -> dd : 月中的日期（01-31）<br>
                %h -> hh : 小时（00-23）<br>
                %H -> a : 小时代码（a-x）<br>
                %M -> mm : 分钟（00-59）<br>
                %n -> ddd : 一年中的日期（001-366）<br>
                %W -> wwww : GPS周（0001-9999）<br>
                %D -> d : GPS周中的日期（0-6）<br>
                %N -> d : 序列号（0- ）<br>
                %s -> ssss : 站点名称（小写）<br>
                %S -> SSSS : 站点名称（大写）<br>
                %r -> rrrr : 站点名称<br>
                %{env} -> env : 环境变量
            </td>  
            <td></td>  
        </tr>
        <tr>
            <td>(3)</td>  
            <td>Default local directory</td>  
            <td>
                如果在RTKGET中未指定本地目录，则下载的文件将保存在该目录中。目录路径可以包含与URL地址相同的关键词。
            </td>  
            <td></td>  
        </tr>
    </tbody>
</table>

**示例**
```text
#--------------------------------------------------------------------------------------
# data type url Address default local dir
#--------------------------------------------------------------------------------------
#! PRODUCT (CDDIS)
IGS_EPH ftp://cddis.gsfc.nasa.gov/gps/products/%W/igs%W%D.sp3.Z c:\product\%W
IGS_EPH_GLO ftp://cddis.gsfc.nasa.gov/gps/products/%W/igl%W%D.sp3.Z c:\product\%W
IGS_CLK ftp://cddis.gsfc.nasa.gov/gps/products/%W/igs%W%D.clk.Z c:\product\%W
IGS_CLK_30S ftp://cddis.gsfc.nasa.gov/gps/products/%W/igs%W%D.clk_30s.Z c:\product\%W
IGS_ERP ftp://cddis.gsfc.nasa.gov/gps/products/%W/igs%W7.erp.Z c:\product\%W
IGR_EPH ftp://cddis.gsfc.nasa.gov/gps/products/%W/igr%W%D.sp3.Z c:\product\%W
IGR_CLK ftp://cddis.gsfc.nasa.gov/gps/products/%W/igr%W%D.clk.Z c:\product\%W
IGR_ERP ftp://cddis.gsfc.nasa.gov/gps/products/%W/igr%W%D.erp.Z c:\product\%W
IGU_EPH ftp://cddis.gsfc.nasa.gov/gps/products/%W/igu%W%D_%h.sp3.Z c:\product\%W
IGU_ERP ftp://cddis.gsfc.nasa.gov/gps/products/%W/igu%W%D_%h.erp.Z c:\product\%W
IGS_POS ftp://cddis.gsfc.nasa.gov/gps/products/%W/igs%yP%W.snx.Z c:\product\%W
IGS_TEC ftp://cddis.gsfc.nasa.gov/gps/products/ionex/%Y/%n/igsg%n0.%yi.Z c:\product\%W
IGR_TEC ftp://cddis.gsfc.nasa.gov/gps/products/ionex/%Y/%n/igrg%n0.%yi.Z c:\product\%W
...
```
