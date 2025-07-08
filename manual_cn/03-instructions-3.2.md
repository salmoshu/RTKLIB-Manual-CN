# 3. 操作指南

## 3.2 RTKNAVI：实时定位程序

实时定位应用程序 RTKNAVI 输入 GPS/GNSS 接收机的原始观测数据，并进行实时定位解算。通过将定位模式设置为动态（Kinematic）并配置流动站（Rover）和基准站（Base）接收机的数据输入，可以启用 RTK 功能，并进行实时地（on-the-fly）整周模糊度解算。

### 3.2.1 软件执行

执行二进制AP文件`<install dir>\rtklib_<ver>\bin\rtknavi.exe`（双击图标或在命令控制台中输入完整路径）。您可以看到RTKNAVI的主窗口。

![Main Window of RTKNAVI](https://i.ibb.co/Ptcn1cb/image.png)
<p style="text-align: center;">图3.2-1 RTKNAVI主窗口</p> 

### 3.2.2 输入输出

RTKNAVI的数据流如下图所示。需要设置输入流（I）、输出流（O，可选）和日志流（L，可选）以进行实时定位。有关这些流的几个示例框架，请参阅3.3节。

![Data Flow of RTKNAVI](https://i.ibb.co/8syLVjF/image.png)
<p style="text-align: center;">图3.2-2 RTKNAVI数据流示意图</p> 

**1. 输入流设置**

RTKNAVI进行实时定位，需要将来自GNSS接收机的原始观测数据和卫星星历数据输入到软件中。可以通过主窗口中上角的按钮 <span style="border: 1px solid black; padding: 3px;">I</span>来设置输入流。

![Input Streams Dialog of RTKNAVI](https://i.ibb.co/M96gbPG/image.png)
<p style="text-align: center;">图3.2-3 RTKNAVI数据流对话框</p> 

在对话框中选中并设置Rover、Base或Correction的流类型。如果将“Positioning Mode”选项设置为“Single”（单点定位），则不需要“Base”和“Correction”的输入流。可以从以下选项中选择流类型。

- **Serial：** 从串口（RS232或USB）输入数据
- **TCP Client：** 通过连接到对应的TCP服务器从而获取数据
- **TCP Server：** 接受多个TCP客户端，并提供指定格式的数据，该选项通常作为Output
- **NTRIP Client：** 连接至NTRIP caster<sup>[20]</sup>并通过NTRIP输入数据，NRTK（网络RTK）服务器支持NTRIP和RTCM 2/3
- **File：** 从日志文件中输入数据。
- **FTP：** 通过FTP下载文件后输入数据（仅用于Correction）
- **HTTP：** 通过HTTP下载文件后输入数据（仅用于Correction）

数据流的格式需要在“Format”的下拉菜单中进行选择。RTKLIB支持的消息请参考附录D.2。需要将RTKNAVI连接的GNSS接收机配置为能够输出观测数据和导航数据（星历）的状态。不同接收机的使用方法，需要参考对应的用户手册。

- **RTCM2：** RTCM 2.3
- **RTCM3：** RTCM 3.0、3.1 (with amendment 1-5)、3.2
- **NovAtel OEM6：** NovAtel OEM4/V/6、OEMStar二进制
- **u-blox：** u-blox LEA-4T、5T、6T 二进制
- **Swiftnav：** Swiftnav Piksi Multi
- **Hemisphere：** Hemisphere Crescent/Eclipse 二进制
- **SkyTraq：** SkyTraq S1315F 二进制
- **Javad：** JAVAD GRIL/GREIS 二进制
- **NVS BINR：** NVS NV08C BINR 格式
- **BINEX：** BINEX （仅支持大端序、正向、正则CRC）
- **SP3：** SP3精确星历（仅用于Correction）

注意：上述仅列出了原始官方RTKLIB支持的部分品牌和型号，具体支持情况会因版本差异而有所不同，例如最新的demo5代码已经开始支持和芯星通Unicore的专有格式。

**2. 输出流设置**

要通过RTKNAVI输出定位解，您需要设置输出流。输出流的设置请使用主窗口右上角的按钮 <span style="border: 1px solid black; padding: 3px;">O</span> 。在“输出流”对话框中选中并设置解算结果的流类型。您最多可以配置两个独立的输出流。

您可以从“Serial”、“TCP Client”、“TCP Server”、“NTRIP Server”和“File”中选择流类型。配置选项与输入流类似。您还必须选择以下的输出格式。

- **Lat/Lon/Height** : Latitude, longitude and height
- **X/Y/Z-ECEF** : X/Y/Z components in ECEF frame
- **E/N/U-Baseline** : E/N/U components of the baseline
- **NMEA0183** : NMEA0183 GPRMC, GPGGA, GPGSA, GLGSA, GAGSA, GPGSV, GLGSV and GAGSV

![Output Streams Dialog of RTKNAVI](https://i.ibb.co/xKkzKMc/image.png)
<p style="text-align: center;">图3.2-4 RTKNAVI输出流对话框</p> 

输出消息中的时间、纬度/经度格式、高度类型和大地水准面模型以及NMEA请求时间间隔也可以通过3.5中描述的配置参数进行调整。

另外，定位解算结果同时会被记录到内部缓冲区中。您也可以通过点击解算显示下方的 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮，将内部解算缓冲区的内容保存到文件中。解算缓冲区的大小以及保存的解算日志可以通过“Options”对话框进行配置。

**3. Log流设置**

要转存输入流的数据（不经过算法），可以设置日志流（Log Stream）。通过主窗口右上角的 <span style="border: 1px solid black; padding: 3px;">L</span> 按钮，可以对Log流进行设置。Log流的设置方法整体上与输出流类似。

如果你希望稍后将日志文件作为输入流回放，你必须选中“Time-Tag（时间标签）”选项并同时输出时间标签文件。时间标签文件的输出路径会自动设置为`<outputfilepath>.tag`。文件路径中的关键词替换以及交换间隔与“Output Streams（输出流）”对话框中的设置相同。

![Log Streams Dialog of RTKNAVI](https://i.ibb.co/ZGtmwxL/image.png)
<p style="text-align: center;">图3.2-5 RTKNAVI Log流对话框</p> 

**4. 文件名关键字替换**

如果选择“File”作为输出流或日志流类型，则可以在文件路径中包含一些关键字，这些关键字将被日期或时间替换。 <span style="border: 1px solid black; padding: 3px;">?</span> 中显示了文件路径中的关键字替换方法。如果您设置了“Swap Intv（交换间隔）”选项，输出文件将按照指定的周期定期交换。要使用文件交换功能，文件路径必须包含将被交换时间替换的关键字，以避免覆盖之前的文件。

![Keyword Replacement Dialog of RTKNAVI](https://i.ibb.co/vXdsMfq/image.png)
<p style="text-align: center;">图3.2-6 RTKNAVI关键字替换对话框</p> 

### 3.2.3 串口设置

如果您选择“Serial”作为流类型，请按下“Serial”旁“Opt”标签下的 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮，可以在“Serial Options”对话框设置“端口（Port）”、“比特率（Bitrate）”、“字节大小（Byte Size）”、“奇偶校验（Parity）”、“停止比特数（Stop Bits）”和“流量控制（Flow Control）”选项。

![Serial Option Dialog of RTKNAVI](https://i.ibb.co/T1ythKY/image.png)
<p style="text-align: center;">图3.2-7 RTKNAVI串口配置页面</p> 

### 3.2.4 TCP设置

如果选择“TCP Client”或“TCP Server”作为流类型，则可以使用“TCP Client Options”或“TCP Server Options”对话框设置“TCP Server Address（目标服务器的IP地址）”（仅适用于TCP客户端）和“Port（端口）”选项。如果选择“TCP Server”作为流类型（只需要设置端口号即可），则允许多个TCP客户端连接。

![TCP Client Options Dialog of RTKNAVI](https://i.ibb.co/TvTqk6N/image.png)
<p style="text-align: center;">图3.2-8 RTKNAVI TCP客户端选项对话框</p> 

![TCP Server Options Dialog of RTKNAVI](https://i.ibb.co/yXQLvzf/image.png)
<p style="text-align: center;">图3.2-9 RTKNAVI TCP服务器选项对话框</p> 

### 3.2.5 NTRIP设置

如果选择“NTRIP Client”作为流类型，可以使用“NTRIP Client Options”对话框设置“NTRIP Caster Host（NTRIP地址）”、“Port（端口）”、NTRIP Caster的“Mountpoint（挂载点）”、“User-ID（用户ID）”和“Password（密码）”选项。如果将“Port”字段留空，程序将会使用NTRIP的默认端口号（2101）。请注意，“User-ID”不能包含字符“:”。

![NTRIP Client Options Dialog of RTKNAVI](https://i.ibb.co/fDT8XYD/image.png)
<p style="text-align: center;">图3.2-10 RTKNAVI NTRIP客户端选项对话框</p> 

### 3.2.6 文件设置

如果选择“File”作为流类型，请在文本字段“Input File Paths（输入文件路径）”中输入文件路径。可以直接填写路径，或通过按下 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮来查找并选中文件。输入文件需要是接收机原始数据的日志。您可以在“Time”字段中设置日志文件的回放速度和初始时间偏移（要使用该功能，您必须使用时间标签文件，即tag文件）。

![Input Streams Dialog of RTKNAVI](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250307-232235.jpg)
<p style="text-align: center;">图3.2-11 RTKNAVI数据流对话框</p> 

### 3.2.7 命令回传

**1. 命令回传使用**

如果选择“Serial”、“TCP Client”或“TCP Server”作为流类型，您可以配置启动和关闭时的回传命令，它们会通过流发送到GNSS接收机。如果想设置命令，请按“Cmd”标签下的 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮。在“Serial/TCP Commands”对话框中的文本字段中填写命令。如果不选中“Commands at startup”或“Commands at shutdown”，那么这些命令将不会发送到接收机。

您还可以通过 <span style="border: 1px solid black; padding: 3px;">Load…</span> 按钮从命令文件中加载命令，或者通过 <span style="border: 1px solid black; padding: 3px;">Save…</span> 按钮将命令保存到文件中。命令文件只是一个文本文件，包含了启动命令和关闭命令，它由“@”开头的行分隔。在`<install-dir>\rtklib_<ver>/data/*.cmd`中可以找到一些典型GNSS接收机的示例命令文件。

![Serial/TCP Commands Dialog of RTKNAVI](https://i.ibb.co/GWWYzyL/image.png)
<p style="text-align: center;">图3.2-12 RTKNAVI回传命令</p> 

**2. 回传命令格式**

命令中以“!”开头的行表示接收机的二进制命令。详情请参阅相应的接收机用户手册。

### 3.2.8 接收机选项

通过输入流“Format”右侧的“Opt”按钮，您可以使用“Receiver Optiont”对话框设置接收机相关选项，如“-GL1X -RL1C -EPHALL”。可以使用多个选项，用空格分隔。有关接收机相关选项的详细信息，请参阅附录D.4接收机相关描述。如果输入观测数据流在一个频率中包含多个信号，则按照默认信号优先级选择用于解决方案的信号，而不使用此类选项。请参阅附录D.3多信号的默认优先级。

![Receiver Option Dialog of RTKNAVI](https://i.ibb.co/7G99FbV/image.png)
<p style="text-align: center;">图3.2-13 RTKNAVI接收机选项对话框</p> 

### 3.2.9 网络RTK请求

如果使用NRTK（网络RTK）服务，需要使用NMEA GPGGA消息来选择参考站或设置VRS（虚拟参考站）位置，请使用“Transmit NMEA GPGGA to Base Station"”的下拉菜单选择消息内容。如果选择“Latitude/Longitude"（纬度/经度）”发送固定位置，请以度为单位填写NMEA GPGGA消息中的经纬度数据（负表示西或南）。

### 3.2.10 Correction数据流

对于Correction流，您可以选择“FTP”或“HTTP”作为流类型。按下“Opt”按钮后，您必须使用“FTP Option”或“HTTP Option”对话框配置FTP或HTTP选项。首先，在“Download Address（下载地址）”字段中填写服务器地址和文件路径，格式为`<server address>/<file path>`。通常，您可能需要在`<file-path>`中包含日期或时间关键字。

例如，在从NASA GSFC CDDIS数据服务器下载IGS星历（IGS ultra-rapid ephemeris）的情况下，您可以输入下载地址，如：
```shell
CDDIS.GSFC.NASA.gov/gps/products/%W/igu%W%D_%hb.sp3.Z
```

在上述示例中，关键字 %W、%D 和 %hb 将分别根据 GPS 时间中的下载时间替换为 GPS 周数、周内的天数和一天中的 6 小时。对于文件路径中可以使用的其他关键字，请按下 <span style="border: 1px solid black; padding: 3px;">?</span> 按钮并查看对话框。您还可以设置下载间隔、下载偏移（例如，Interval = 6H 、 Offset = 2 H 表示下载将在GPS时间的2:00、8:00、14:00和20:00进行）、文件路径中的时间偏移（用于替换文件路径中的关键字）、重试间隔、用户（仅限 FTP）和服务器的密码（仅限 FTP）。对于用户和密码，通常对匿名 FTP 服务器使用“anonymous”和您的邮件地址。

![FTP Option Dialog of RTKNAVI](https://i.ibb.co/WyQKFG4/image.png)
<p style="text-align: center;">图3.2-14 RTKNAVI FTP选项对话框</p> 

要使用下载文件，您还必须在“Input”对话框中设置文件格式。当前版本仅支持 SP3 精确星历。下载的文件将保存在本地目录。本地目录路径可以通过“Options”对话框 - “Files” - “FTP/HTTP Local Directory”进行设置。

### 3.2.11 NTRIP 服务器

如果选择“NTRIP Server”作为输出流类型，需要设置“NTRIP Caster Host（NTRIP Caster的IP地址）”、连接NTRIP Caster的“Port（端口）”、“Mount point（挂载点）”、“Password”以及向 NTRIP Caster 显示源表参数的“String”。如果“Port”字段为空，则使用默认端口号（80）。点击 <span style="border: 1px solid black; padding: 3px;">Ntrip...</span> 将启动NTRIP浏览器应用，以显示NTRIP Caster提供的信息源，请参阅 3.10 NTRIP Browser。

![NTRIP Server Options Dialog of RTKNAVI](https://i.ibb.co/vm3sgPm/image.png)
<p style="text-align: center;">图3.2-15 RTKNAVI NTRIP服务器选项对话框</p> 

### 3.2.12 定位参数配置

要配置定位参数（含算法参数和软件的处理逻辑），请使用 <span style="border: 1px solid black; padding: 3px;">Options...</span> 按钮，并在“Options”对话框中设置相应的参数。有关定位参数的详细信息，请参阅3.5节。

### 3.2.13 RTKNAVI运行状态

**1. 输入输出状态**

按下 <span style="border: 1px solid black; padding: 3px;">Start</span> 按钮。每个流的状态会显示在右上角的指示器上。从左侧开始，它们显示了Rover（输入1）、Base（输入2）、Correction（输入3）、定位解算状态、Solution1（输出1）、Solution2（输出2）、Log Rover（Log1）、Log Base（Log2）和Log Correction（Log3）的数据流或算法运行状态。

- <span style="color: grey;">灰色</span>：表示未使用；
- <span style="color: orange;">橙色</span>：表示等待连接；
- <span style="color: darkgreen;">深绿色</span>：表示已连接或正在运行；
- <span style="color: lightgreen;">浅绿色</span>：表示数据活动（输入、输出或处理）；
- <span style="color: red;">红色</span>：表示发生异常。

一些状态信息也会显示在主窗口的正下方的消息显示区域。

![Main Window of RTKNAVI (RUNNING)](https://i.ibb.co/q1pdZKX/image.png)
<p style="text-align: center;">图3.2-16 RTKNAVI主窗口</p> 

**2. 结算结果状态**

在输入观测数据和星历是完整且有效的情况下，RTKNAVI就能正常运行（进行定位解算），并将定位结果与状态显示在主窗口左侧区域中，显示解状态（<span style="color: green;">Fix</span>、<span style="color: orange;">Float</span>、<span style="color: #0000cc;">DGPS</span>、<span style="color: #ff00ff;">SBAS</span>、<span style="color: red;">Single</span>或<span style="color: #006699;">PPP</span>）、E/N/U或X/Y/Z向的标准差、差分龄期、Ratio值（模糊验证比率因子）和有效卫星数量。要切换Solutiom显示中的格式，请按右上角的 <span style="border: 1px solid black; padding: 3px;">▼</span> 按钮。您可以将解决方案切换到纬度/经度/高度（度/分/秒）、纬度/经度-高度（度）、X/Y/ZECEF（m）、E/N/U基线（m）和俯仰/偏航/长度基线（度，m）。在纬度/经度/高度模式下，标签“H”和“He”分别表示大地（水准面）高度和椭球高度。

![Solution Status Display of RTKNAVI](https://i.ibb.co/RDnb1jW/image.png)
<p style="text-align: center;">图3.2-17 RTKNAVI解运行状态</p> 

**3. 卫星状态**

在主窗口右侧的状态显示区域中，会显示观测信噪比（信噪比，C/N0）状态或天空图（skyplot）中的可见卫星。通过点击状态显示区域右上角的 <span style="border: 1px solid black; padding: 3px;">▼</span> 按钮，您可以切换内容以显示以下信息：流动站与基准站信噪比、流动站信噪比、流动站天空图、基准站天空图、基线图，以及宽模式流动站与基准站信噪比、宽模式（Wide-mode）流动站信噪比等，这些内容会交替显示。

通过按下右上角附近的第二个 <span style="border: 1px solid black; padding: 3px;">▼</span> 按钮，可以切换图表中显示的频率。在SNR图或Skyplots中，除灰色外的未使用颜色表示信号SNR为：><span style="color: green;">45...</span> <span style="color: orange;">40...</span> <span style="color: ff00ff;">35...</span> <span style="color: ff00ff;">25...</span> < <span style="color: grey;">25</span>（dBHz）。信噪比图中卫星ID的颜色也表示卫星系统为：<span style="color: pink;">GPS（G）</span>、<span style="color: #cccc00;">GLONASS（R）</span>、<span style="color: #993366;">Galileo（E）</span>、<span style="color: #0000cc;">QZSS（J）</span>、<span style="color: #993300;">北斗（C）</span>和<span style="color: grey;">SBAS（S）</span>。

![Satellite and Signal Status Display of RTKNAVI](https://i.ibb.co/cyYbdVt/image.png)
<p style="text-align: center;">图3.2-18 RTKNAVI卫星和频点信号状态显示</p> 

**4. 时间格式切换**

要切换时间显示中的格式，请按下主窗口中上部的GPST按钮。您也可以将时间系统切换为GPST、UTC、LT（当地时间）和GPST（GPS Week/TOW）。

### 3.2.14 解算结果可视化

通过推送 <span style="border: 1px solid black; padding: 3px;">Plot...</span> 按钮，您可以执行RTKPLOT在图形上绘制Rover接收机的当前位置。有关使用RTKPLOT的详细信息，请参阅3.7使用RTKPLOT查看和绘制Solution以及观测数据。

![RTKPLOT Window executed by RTKNAVI](https://i.ibb.co/4t9kGb3/image.png)
<p style="text-align: center;">图3.2-19 RTKNAVI中启用RTKPLOT</p> 

### 3.2.15 Monitor监视器

**1. Monitor监视器查看**

通过点击主窗口左下角的 <span style="border: 1px solid black; padding: 3px;">Monitor</span> 按钮，您可以打开“RTK Monitor”窗口。通过该窗口，您可以查看RTKNAVI的内部状态。您可以使用窗口左上角的下拉菜单选择以下内容。允许同时显示多个“RTK监视”窗口。

![RTK Monitor Window of RTKNAVI](https://i.ibb.co/kh9M6GQ/image.png)
<p style="text-align: center;">图3.2-20 RTKNAVI监视器</p> 

- **(a) RTK：** 内部定位解算的总体状态
- **(b) Obs Data：** 输入的观测数据。RCV=1 表示移动站，2 表示基准站
- **(c) Nav GPS：** GPS 卫星导航信息
- **(d) Nav GLONASS：** 格洛纳斯（GLONASS）卫星导航信息
- **(e) Nav Galileo：** 伽利略（Galileo）卫星导航信息
- **(f) Nav QZSS：** 准天顶卫星系统（QZSS）导航信息
- **(g) Nav BeiDou：** 北斗卫星导航信息
- **(h) Nav GEO：** 地球静止轨道卫星（GEO/SBAS）导航信息
- **(i) Time/Iono：** 时间和电离层参数
- **(j) Streams：** 输入、输出和日志数据流的状态
- **(k) Sat GPS：** GPS 卫星状态
- **(l) Sat GLONASS：** 格洛纳斯（GLONASS）卫星状态
- **(m) Sat Galileo：** 伽利略（Galileo）卫星状态
- **(n) Sat QZSS：** 准天顶卫星系统（QZSS）卫星状态
- **(o) Sat BeiDou：** 北斗卫星状态
- **(p) Sat GEO：** 地球静止轨道卫星（GEO/SBAS）卫星状态
- **(q) States：**  滤波器的状态向量值
- **(r) Covariance：** 滤波器的协方差矩阵
- **(s) SBAS Msgs：** 输入的 SBAS 消息的十六进制转储
- **(t) SBAS Long：** SBAS 长期卫星修正
- **(u) SBAS Iono：** SBAS 电离层延迟修正
- **(v) SBAS Fast：** SBAS 快速修正
- **(w) RTCM Msgs：** RTCM 2/3 消息的状态

**2. Monitor输入数据流**

如果为“RTK Monitor”选择（1）Rover、（2）Base 或（3）Correction，您可以通过窗口上方中央的下拉菜单选择输入消息格式，并转存输入流中的消息。每行转存信息包含以下字段：消息类型、（消息长度，以字节为单位）：消息内容，具体取决于消息格式。

![Stream Dump in RTK Monitor Window of RTKNAVI](https://i.ibb.co/7r541JW/image.png)
<p style="text-align: center;">图3.2-21 RTKNAVI监视器</p> 
