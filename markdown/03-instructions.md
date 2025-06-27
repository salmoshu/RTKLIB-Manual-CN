---
sidebarDepth: 1
---

# 3. 操作指南

## 3.1 安装与卸载
**1. 软件目录**

将程序包`rtklib_<ver>.zip`或`rtklib_<ver>_bin.zip`解压缩到相应的目录`<install-dir>`（`<ver>`表示版本号）。RTKLIB目录结构如下。

```html
/rtklib_<ver>
├── \src             : RTKLIB核心源码程序 *
│   ├── \rcv            : 不同厂商GPS/GNSS接收机处理程序 *
├── \bin             : Windows可执行程序和动态链接库
├── \data            : 示例数据
├── \app             : RTKLIB不同应用的主程序 *
│   ├── \rtknavi		: RTKNAVI 	(GUI) *
│   └── \rtknavi_mkl	: RTKNAVI_MKL (GUI) *
│   └── \strsvr		    : STRSVR 	(GUI) *
│   └── \rtkpost		: RTKPOST 	(GUI) *
│   └── \rtkpost_mkl	: RTKPOST_MKL (GUI) *
│   └── \rtkplot		: RTKPLOT 	(GUI) *
│   └── \rtkconv		: RTKCONV 	(GUI) *
│   └── \srctblbrows 	: NTRIP Browser 	(GUI) *
│   └── \rtkget	 	    : RTKGET 	(GUI) *
│   └── \rtklaunch		: RTKLAUNCH 	(GUI) *
│   └── \rtkrcv		    : RTKRCV 	(CUI) *
│   └── \rnx2rtkp		: RNX2RTKP 	(CUI) *
│   └── \pos2kml		: POS2KML 	(CUI) *
│   └── \convbin		: CONVBIN 	(CUI) *
│   └── \str2str		: STR2STR 	(CUI) *
│   └── \appcmn		    : GUI应用的通用例程 *
│   └── \icon		    : GUI应用的图标 *
├── \lib			: 库的生成环境 *
└── \test			: 测试程序或数据 *
└── \util			: 工具集 *
└── \doc			: RTKLIB使用手册

* 编译好的可执行应用 rtklib_<ver>_bin.zip 中不包含上述内容。
```

**2. 基站使用**

在`<install dir>\rtklib_<ver>\bin`中为图形用户界面应用程序（GUI AP）的可执行文件创建快捷方式。为了运行命令行界面应用程序（CUI AP），请将`<install dir>\rtklib_<ver>\bin`添加到命令路径中。RTKLIB中的图形用户界面应用程序（GUI AP）和命令行界面应用程序（CUI AP）均不使用Windows注册表。要卸载该软件包，只需删除安装目录中的所有文件和文件夹即可。

**3. GUI程序INI配置**

图形用户界面应用程序（GUI AP）的可选设置通常保存在INI文件（.ini）中，这些文件通常位于`<install dir>\rtklib_<ver>\bin`目录中。某些图形用户界面应用程序（RTKCONV、RTKPOST、RTKNAVI、RTKGET和STRSVR）可以通过命令行选项`-i <inifile>.ini`执行，以选择其他INI文件进行不同的可选设置。您可以通过命令行选项以及`-t <title>`选项来更改窗口标题，从而切换这些应用程序的选项。因此，您可以通过设置快捷方式的属性，为同一个图形用户界面应用程序配置多个快捷方式，并为每个快捷方式设置不同的选项。

**4. MKL版本的GUI程序**

为了使用RTKPOST_MKL或RTKNAVI_MKL（这是链接了Intel MKL库的RTKPOST或RTKNAVI版本，可在多核CPU或多处理器PC上实现更快的矩阵计算），请根据CPU核心数量将Windows环境变量`OMP_NUM_THREADS`设置为2、4或8。这将启用多线程矩阵计算，从而缩短处理时间。

**5. RTKLAUNCH**

为了方便运行图形用户界面应用程序，2.4.2版本中新增了一个应用程序启动器RTKLAUNCH。要运行RTKLAUNCH，请执行`<install dir>\rtklib_<ver>\bin\rtklaunch.exe`。您可以在RTKLAUNCH窗口中点击应用程序图标，或通过任务栏图标弹出菜单来运行RTKLIB应用程序。RTKLAUNCH接受`-mkl`选项，用于启动RTKPOST_MKL和RTKNAVI_MKL，而不是RTKPOST和RTKNAVI；同时接受`-tray`选项，用于以任务栏图标的形式启动启动器。

## 3.2 实时定位软件RTKNAVI
实时定位软件RTKNAVI接收GPS/GNSS接收机的原始观测数据，并实时执行导航处理。通过将定位模式设置为Kinematic模式，并配置流动站(Rover)和基准站(Base)接收机的数据输入，RTK-GPS/GNSS能够实现实时地（on-the-fly）整数模糊度解算。

**1 软件执行**

执行二进制AP文件`<install dir>\rtklib_<ver>\bin\rtknavi.exe`（双击图标或在命令控制台中输入完整路径）。您可以看到RTKNAVI的主窗口。您也可以选择执行`<install dir>\rtklib_<ver>\bin\rtknavi_mkl.exe`。

![Main Window of RTKNAVI](https://i.ibb.co/Ptcn1cb/image.png)
<p style="text-align: center;">图3.2-1 RTKNAVI的主窗口</p> 

**2. 数据流选项**

RTKNAVI的数据流如下图所示。需要设置输入流（I）、输出流（O，可选）和日志流（L，可选）以进行实时定位。有关这些流的几个示例配置，请参阅3.3节：配置RTKNAVI的输入、输出和日志流。

![Data Flow of RTKNAVI](https://i.ibb.co/8syLVjF/image.png)
<p style="text-align: center;">图3.2-2 RTKNAVI的数据流示意图</p> 

**3. 输入设置**

RTKNAVI进行实时定位，需要将来自GNSS接收机的原始观测数据和卫星星历数据输入到软件中。可以点击按主窗口中上角的按钮 <span style="border: 1px solid black; padding: 3px;">I</span>来设置输入流。

![Input Streams Dialog of RTKNAVI](https://i.ibb.co/M96gbPG/image.png)
<p style="text-align: center;">图3.2-3 RTKNAVI的数据流对话框</p> 

**4. 输入流类型**

在对话框中选中并设置Rover、Base或Correction的流类型。如果将“Positioning Mode”选项设置为“Single”（单点定位），则不需要“Base”和“Correction”的输入流。可以从以下选项中选择流类型。

- Serial：从串口（RS232或USB）输入数据 <br>
- TCP Client：通过连接到对应的TCP服务器从而获取数据 <br>
- TCP Server：接受多个TCP客户端，并提供指定格式的数据 <br>
- NTRIP Client：连接至NTRIP caster<sup>[20]</sup>并通过NTRIP输入数据。NRTK（网络RTK）服务器支持NTRIP和RTCM 2/3。 <br>
- File：从日志文件中输入数据。 <br>
- FTP：通过FTP下载文件后输入数据（仅用于Correction） <br>
- HTTP：通过HTTP下载文件后输入数据（仅用于Correction） <br>

数据流地格式需要在“Format”的下拉菜单中进行选择。RTKLIB支持的消息请参考附录D.2。需要将RTKNAVI连接的GNSS接收机配置为能够输出观测数据和导航数据（星历）的状态。不同接收机的使用方法，需要参考对应的用户手册。

- RTCM2 : RTCM 2.3 <br>
- RTCM3 : RTCM 3.0、3.1 (with amendment 1-5)、3.2 <br>
- NovAtel OEM6 : NovAtel OEM4/V/6、OEMStar二进制 <br>
- u-blox : u-blox LEA-4T、5T、6T 二进制 <br>
- Swiftnav : Swiftnav Piksi Multi <br>
- Hemisphere : Hemisphere Crescent/Eclipse 二进制 <br>
- SkyTraq : SkyTraq S1315F 二进制 <br>
- Javad : JAVAD GRIL/GREIS 二进制 <br>
- NVS BINR : NVS NV08C BINR 格式 <br>
- BINEX : BINEX （仅支持大端序、正向、正则CRC） <br>
- SP3 : SP3精确星历（仅用于Correction） <br>

**5. 串口设置**

如果您选择“Serial”作为流类型，请按下“Serial”旁“Opt”标签下的 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮，可以在“Serial Options”对话框设置“端口（Port）”、“比特率（Bitrate）”、“字节大小（Byte Size）”、“奇偶校验（Parity）”、“停止比特数（Stop Bits）”和“流量控制（Flow Control）”选项。

![Serial Option Dialog of RTKNAVI](https://i.ibb.co/T1ythKY/image.png)
<p style="text-align: center;">图3.2-4 RTKNAVI的串口配置页面</p> 

**6. 命令回传**

如果选择“Serial”、“TCP Client”或“TCP Server”作为流类型，您可以配置启动和关闭时的回传命令，它们会通过流发送到GNSS接收机。如果想设置命令，请按“Cmd”标签下的 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮。在“Serial/TCP Commands”对话框中的文本字段中填写命令。如果不选中“Commands at startup”或“Commands at shutdown”，那么这些命令将不会发送到接收机。

您还可以通过 <span style="border: 1px solid black; padding: 3px;">Load…</span> 按钮从命令文件中加载命令，或者通过 <span style="border: 1px solid black; padding: 3px;">Save…</span> 按钮将命令保存到文件中。命令文件只是一个文本文件，包含了启动命令和关闭命令，它由“@”开头的行分隔。在`<install-dir>\rtklib_<ver>/data/*.cmd`中可以找到一些典型GNSS接收机的示例命令文件。

![Serial/TCP Commands Dialog of RTKNAVI](https://i.ibb.co/GWWYzyL/image.png)
<p style="text-align: center;">图3.2-5 RTKNAVI的回传命令</p> 

**7. 回传命令格式**

命令中以“!”开头的行表示接收机的二进制命令。详情请参阅相应的接收机用户手册。

**8. TCP设置**

如果选择“TCP Client”或“TCP Server”作为流类型，则可以使用“TCP Client Options”或“TCP Server Options”对话框设置“TCP Server Address（目标服务器的IP地址）”（仅适用于TCP客户端）和“Port（端口）”选项。如果选择“TCP Server”作为流类型（只需要设置端口号即可），则允许多个TCP客户端连接。

![TCP Client Options Dialog of RTKNAVI](https://i.ibb.co/TvTqk6N/image.png)
<p style="text-align: center;">图3.2-6 RTKNAVI的TCP客户端选项对话框</p> 

![TCP Server Options Dialog of RTKNAVI](https://i.ibb.co/yXQLvzf/image.png)
<p style="text-align: center;">图3.2-7 RTKNAVI的TCP服务器选项对话框</p> 

**9. NTRIP设置**

如果选择“NTRIP Client”作为流类型，可以使用“NTRIP Client Options”对话框设置“NTRIP Caster Host（NTRIP地址）”、“Port（端口）”、NTRIP Caster的“Mountpoint（挂载点）”、“User-ID（用户ID）”和“Password（密码）”选项。如果将“Port”字段留空，程序将会使用NTRIP的默认端口号（2101）。请注意，“User-ID”不能包含“:”字符。

![NTRIP Client Options Dialog of RTKNAVI](https://i.ibb.co/fDT8XYD/image.png)
<p style="text-align: center;">图3.2-8 RTKNAVI的NTRIP客户端选项对话框</p> 

**10. 文件设置**

如果选择“File”作为流类型，请在文本字段“Input File Paths（输入文件路径）”中输入文件路径。可以直接填写路径，或通过按下 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮来查找和选中文件。输入文件应该是接收机原始数据日志。您可以在“Time”字段中设置日志文件的回放速度和初始时间偏移（要使用该功能，您必须使用时间标签文件记录日志，即tag文件）。

![Input Streams Dialog of RTKNAVI](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250307-232235.jpg)
<p style="text-align: center;">图3.2-9 RTKNAVI的数据流对话框</p> 

**11. 接收机选项**

通过输入流“Format”右侧的“Opt”按钮，您可以使用“Receiver Optiont”对话框设置接收机相关选项，如“-GL1X -RL1C -EPHALL”。可以使用多个选项，用空格分隔。有关接收机相关选项的详细信息，请参阅附录D.4接收机相关描述。如果输入观测数据流在一个频率中包含多个信号，则按照默认信号优先级选择用于解决方案的信号，而不使用此类选项。请参阅附录D.3多信号的默认优先级。

![Receiver Option Dialog of RTKNAVI](https://i.ibb.co/7G99FbV/image.png)
<p style="text-align: center;">图3.2-10 RTKNAVI接收机选项对话框</p> 

**12. 网络RTK请求**

如果使用NRTK（网络RTK）服务，需要使用NMEA GPGGA消息来选择参考站或设置VRS（虚拟参考站）位置，请使用“Transmit NMEA GPGGA to Base Station"”的下拉菜单选择消息内容。如果选择“Latitude/Longitude"（纬度/经度）”发送固定位置，请以度为单位填写NMEA GPGGA消息中的经纬度数据（负表示西或南）。

**13. Correction数据流**

对于Correction流，您可以选择“FTP”或“HTTP”作为流类型。按下“Opt”按钮后，您必须使用“FTP Option”或“HTTP Option”对话框配置FTP或HTTP选项。首先，在“Download Address（下载地址）”字段中填写服务器地址和文件路径，格式为`<server address>/<file path>`。通常，您可能需要在`<file-path>`中包含日期或时间关键字。

例如，在从NASA GSFC CDDIS数据服务器下载IGS星历（IGS ultra-rapid ephemeris）的情况下，您可以输入下载地址，如：
```shell
CDDIS.GSFC.NASA.gov/gps/products/%W/igu%W%D_%hb.sp3.Z
```

在上述示例中，关键字 %W、%D 和 %hb 将分别根据 GPS 时间中的下载时间替换为 GPS 周数、周内的天数和一天中的 6 小时。对于文件路径中可以使用的其他关键字，请按下 <span style="border: 1px solid black; padding: 3px;">?</span> 按钮并查看对话框。您还可以设置下载间隔、下载偏移（例如，Interval = 6H 、 Offset = 2 H 表示下载将在GPS时间的2:00、8:00、14:00和20:00进行）、文件路径中的时间偏移（用于替换文件路径中的关键字）、重试间隔、用户（仅限 FTP）和服务器的密码（仅限 FTP）。对于用户和密码，通常对匿名 FTP 服务器使用“anonymous”和您的邮件地址。

![FTP Option Dialog of RTKNAVI](https://i.ibb.co/WyQKFG4/image.png)
<p style="text-align: center;">图3.2-11 RTKNAVI FTP选项对话框</p> 

要使用下载文件，您还必须在“Input”对话框中设置文件格式。当前版本仅支持 SP3 精确星历。下载的文件将保存在本地目录。本地目录路径可以通过“Options”对话框 - “Files” - “FTP/HTTP Local Directory”进行设置。

**14. 输出设置**

要通过RTKNAVI输出定位解，您需要设置输出流。输出流的设置请使用主窗口右上角的按钮 <span style="border: 1px solid black; padding: 3px;">O</span> 。在“输出流”对话框中选中并设置解决方案的流类型。您最多可以配置两个独立的输出流。您可以从“Serial”、“TCP Client”、“TCP Server”、“NTRIP Server”和“File”中选择流类型。配置选项与输入流类似。您还必须选择以下的输出格式。输出消息中的时间、纬度/经度格式、高度类型和大地水准面模型以及NMEA间隔也可以通过3.5中描述的配置参数进行调整。

- **Lat/Lon/Height** : Latitude, longitude and height
- **X/Y/Z-ECEF** : X/Y/Z components in ECEF frame
- **E/N/U-Baseline** : E/N/U components of the baseline
- **NMEA0183** : NMEA0183 GPRMC, GPGGA, GPGSA, GLGSA, GAGSA, GPGSV, GLGSV and GAGSV

![Output Streams Dialog of RTKNAVI](https://i.ibb.co/xKkzKMc/image.png)
<p style="text-align: center;">图3.2-12 RTKNAVI的输出流对话框</p> 

**15. 文件名关键字替换**

如果选择“File”作为输出流类型，则可以在文件路径中包含一些关键字，这些关键字将被日期或时间替换。 <span style="border: 1px solid black; padding: 3px;">?</span> 中显示了文件路径中的关键字替换方法。如果您设置了“Swap Intv（交换间隔）”选项，输出文件将按照指定的周期定期交换。要使用文件交换功能，文件路径必须包含将被交换时间替换的关键字，以避免覆盖之前的文件。

![Keyword Replacement Dialog of RTKNAVI](https://i.ibb.co/vXdsMfq/image.png)
<p style="text-align: center;">图3.2-13 RTKNAVI关键字替换对话框</p> 

**16. NTRIP 服务器**

如果选择“NTRIP Server”作为输出流类型，需要设置“NTRIP Caster Host（NTRIP Caster的IP地址）”、连接NTRIP Caster的“Port（端口）”、“Mount point（挂载点）”、“Password”以及向 NTRIP Caster 显示源表参数的“String”。如果“Port”字段为空，则使用默认端口号（80）。点击 <span style="border: 1px solid black; padding: 3px;">Ntrip...</span> 将启动NTRIP浏览器应用，以显示NTRIP Caster提供的信息源，请参阅 3.10 NTRIP Browser。

![NTRIP Server Options Dialog of RTKNAVI](https://i.ibb.co/vm3sgPm/image.png)
<p style="text-align: center;">图3.2-14 RTKNAVI NTRIP服务器选项对话框</p> 

**17. Log流**

要转存输入流的数据（不经过算法），可以设置日志流（Log Stream）。通过主窗口右上角的 <span style="border: 1px solid black; padding: 3px;">L</span> 按钮，可以对Log流进行设置。Log流的设置方法整体上与输出流类似。

如果你希望稍后将日志文件作为输入流回放，你必须选中“Time-Tag（时间标签）”选项并同时输出时间标签文件。时间标签文件的输出路径会自动设置为`<outputfilepath>.tag`。文件路径中的关键词替换以及交换间隔与“Output Streams（输出流）”对话框中的设置相同。

![Log Streams Dialog of RTKNAVI](https://i.ibb.co/ZGtmwxL/image.png)
<p style="text-align: center;">图3.2-15 RTKNAVI Log流对话框</p> 

**18. 定位选项配置**

要配置定位参数（含算法参数和软件的处理逻辑），请使用 <span style="border: 1px solid black; padding: 3px;">Options...</span> 按钮，并在“Options”对话框中设置相应的参数。有关定位参数的详细信息，请参阅3.5 RTKNAVI和RTKPOST的配置参数选项。

**19. RTKNAVI运行状态1**

按下 <span style="border: 1px solid black; padding: 3px;">Start</span> 按钮。每个流的状态会显示在右上角的指示器上。从左侧开始，它们显示了Rover（输入1）、Base（输入2）、Correction（输入3）、定位解算状态、Solution1（输出1）、Solution2（输出2）、Log Rover（Log1）、Log Base（Log2）和Log Correction（Log3）的数据流或算法运行状态。

- <span style="color: grey;">灰色</span>：表示未使用；
- <span style="color: orange;">橙色</span>：表示等待连接；
- <span style="color: darkgreen;">深绿色</span>：表示已连接或正在运行；
- <span style="color: lightgreen;">浅绿色</span>：表示数据活动（输入、输出或处理）；
- <span style="color: red;">红色</span>：表示发生异常。

一些状态信息也会显示在主窗口的正下方的消息显示区域。

![Main Window of RTKNAVI (RUNNING)](https://i.ibb.co/q1pdZKX/image.png)
<p style="text-align: center;">图3.2-16 RTKNAVI主窗口</p> 

**20. RTKNAVI运行状态2**

在输入观测数据和星历是完整且有效的情况下，RTKNAVI就能正常运行（进行定位解算），并将定位结果与状态显示在主窗口左侧区域中，显示解状态（<span style="color: green;">Fix</span>、<span style="color: orange;">Float</span>、<span style="color: #0000cc;">DGPS</span>、<span style="color: #ff00ff;">SBAS</span>、<span style="color: red;">Single</span>或<span style="color: #006699;">PPP</span>）、E/N/U或X/Y/Z向的标准差、差分龄期、Ratio值（模糊验证比率因子）和有效卫星数量。要切换Solutiom显示中的格式，请按右上角的 <span style="border: 1px solid black; padding: 3px;">▼</span> 按钮。您可以将解决方案切换到纬度/经度/高度（度/分/秒）、纬度/经度-高度（度）、X/Y/ZECEF（m）、E/N/U基线（m）和俯仰/偏航/长度基线（度，m）。在纬度/经度/高度模式下，标签“H”和“He”分别表示大地（水准面）高度和椭球高度。

![Solution Status Display of RTKNAVI](https://i.ibb.co/RDnb1jW/image.png)
<p style="text-align: center;">图3.2-17 RTKNAVI解运行状态</p> 

**21. 时间格式**

要切换时间显示中的格式，请按下主窗口中上部的GPST按钮。您也可以将时间系统切换为GPST、UTC、LT（当地时间）和GPST（GPS Week/TOW）。

**22. RTKNAVI运行状态3**

在主窗口右侧的状态显示区域中，会显示观测信噪比（信噪比，C/N0）状态或天空图（skyplot）中的可见卫星。通过点击状态显示区域右上角的 <span style="border: 1px solid black; padding: 3px;">▼</span> 按钮，您可以切换内容以显示以下信息：流动站与基准站信噪比、流动站信噪比、流动站天空图、基准站天空图、基线图，以及宽模式流动站与基准站信噪比、宽模式（Wide-mode）流动站信噪比等，这些内容会交替显示。

通过按下右上角附近的第二个 <span style="border: 1px solid black; padding: 3px;">▼</span> 按钮，可以切换图表中显示的频率。在SNR图或Skyplots中，除灰色外的未使用颜色表示信号SNR为：><span style="color: green;">45...</span> <span style="color: orange;">40...</span> <span style="color: ff00ff;">35...</span> <span style="color: ff00ff;">25...</span> < <span style="color: grey;">25</span>（dBHz）。信噪比图中卫星ID的颜色也表示卫星系统为：<span style="color: pink;">GPS（G）</span>、<span style="color: #cccc00;">GLONASS（R）</span>、<span style="color: #993366;">Galileo（E）</span>、<span style="color: #0000cc;">QZSS（J）</span>、<span style="color: #993300;">北斗（C）</span>和<span style="color: grey;">SBAS（S）</span>。

![Satellite and Signal Status Display of RTKNAVI](https://i.ibb.co/cyYbdVt/image.png)
<p style="text-align: center;">图3.2-18 RTKNAVI卫星和频点信号状态显示</p> 

**23. Plot**

通过推送 <span style="border: 1px solid black; padding: 3px;">Plot...</span> 按钮，您可以执行RTKPLOT在图形上绘制Rover接收机的当前位置。有关使用RTKPLOT的详细信息，请参阅3.7使用RTKPLOT查看和绘制Solution以及观测数据。

![RTKPLOT Window executed by RTKNAVI](https://i.ibb.co/4t9kGb3/image.png)
<p style="text-align: center;">图3.2-19 RTKNAVI中启用RTKPLOT</p> 

**24. 定位结果的保存**

定位解算结果会同时记录在内部缓冲区中。您可以通过点击解算显示下方的 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮，将内部解算缓冲区的内容保存到文件中。解算缓冲区的大小以及保存的解算日志可以通过“Options”对话框进行配置。

**25. Monitor监视器**

通过点击主窗口左下角的 <span style="border: 1px solid black; padding: 3px;">Monitor</span> 按钮，您可以打开“RTK Monitor”窗口。通过该窗口，您可以查看RTKNAVI的内部状态。您可以使用窗口左上角的下拉菜单选择以下内容。允许同时显示多个“RTK监视”窗口。

![RTK Monitor Window of RTKNAVI](https://i.ibb.co/kh9M6GQ/image.png)
<p style="text-align: center;">图3.2-20 RTKNAVI监视器</p> 

- (a) RTK : General status of the internal positioning process
- (b) Obs Data : Input observation data. RCV=1 means rover and 2 means base-station
- (c) Nav GPS : GPS satellite navigation messages
- (d) Nav GLONASS : GLONASS satellite navigation messages
- (e) Nav Galileo : Galileo satellite navigation messages
- (f) Nav QZSS : QZSS satellite navigation messages
- (g) Nav BeiDou : BeiDou satellite navigation messages
- (h) Nav GEO : GEO/SBAS satellite navigation messages
- (i) Time/Iono : Time and Ionosphere parameters
- (j) Streams : Status of input, output and log streams
- (k) Sat GPS : Status of GPS satellites
- (l) Sat GLONASS : Status of GLONASS satellites
- (m) Sat Galileo : Status of Galileo satellites
- (n) Sat QZSS : Status of QZSS satellites
- (o) Sat BeiDou : Status of BeiDou satellites
- (p) Sat GEO : Status of GEO/SBAS satellites
- (q) States : State vector values of the estimation filter
- (r) Covariance : Covariance matrix of the estimation filter 
- (s) SBAS Msgs : HEX dump of input SBAS messages
- (t) SBAS Long : SBAS long term satellite corrections
- (u) SBAS Iono : SBAS ionospheric delay corrections
- (v) SBAS Fast : SBAS fast corrections
- (w) RTCM Msgs : Status of RTCM 2 or 3 messages

**26. Monitor输入数据流**

如果为“RTK Monitor”选择（1）Rover、（2）Base 或（3）Correction，您可以通过窗口上方中央的下拉菜单选择输入消息格式，并转储输入流中的消息。每行转储信息包含以下字段：消息类型、（消息长度，以字节为单位）：消息内容，具体取决于消息格式。

![Stream Dump in RTK Monitor Window of RTKNAVI](https://i.ibb.co/7r541JW/image.png)
<p style="text-align: center;">图3.2-21 RTKNAVI监视器</p> 

## 3.3 RTKNAVI输入输出配置
当RTKNAVI以相对定位运行时（如RTK），Rover和Base通常放在不同的位置。在一些情况下，用户可能会距离这些接收机较远的地方使用定位结果。为了将这些接收机互连，用户必须建立数据通信链路。RTKLB提供了一个流处理工具STRSVR，用户可以通过STRSVR设置系统的输入、输出数据流。STRSVR还具有中继或数据流分割的功能，以便与RTKNAVI协同进行实时定位。

例如，为了在Rover接收机上接收远程基站的观测数据以进行RTK相对定位，用户可以通过STRSVR远程连接到Base接收机。以下示例显示了RTKNAVI和SVRSVR的典型应用。

（1） 单点定位模式，并存储其结果到文件中
![alt text](https://i.ibb.co/3NQgy9q/image.png)

（2） 单点定位模式，将结果输出到串行设备，同时将Rover的Log数据记录到文件
![alt text](https://i.ibb.co/T11FmRQ/image.png)

（3） RTK模式，从两个串行设备输入Rover和Base数据，并将结果保存到文件
![alt text](https://i.ibb.co/qxSvcGf/image.png)

（4） RTK模式，Rover数据来自串口而Base数据则是通过Wi-Fi获取自远端的TCP服务器
![alt text](https://i.ibb.co/rybzVBQ/image.png)

（5） RTK模式，通过串口输入Rover数据，并通过4G或5G连接输入Base数据
![alt text](https://i.ibb.co/M28V2VC/image.png)

（6） RTK模式、来自串口的输入数据和通过互联网上的NTRIP广播服务器输入基站数据。
![alt text](https://i.ibb.co/qn25hZZ/image.png)

（7） RTK通过互联网提供NRTK（网络RTK）服务
![alt text](https://i.ibb.co/XsgjswK/image.png)

（8） 具有单个NRTK服务的多个RTK
![alt text](https://i.ibb.co/Ykvkb95/image.png)

（9） 实时PPP，实时卫星轨道和时钟以NTRIP流的形式提供。
![alt text](https://i.ibb.co/HDGFzCq/image.png)

（10） 长基线RTK与FTP下载精确星历
![alt text](https://i.ibb.co/KmN9WL2/image.png)

以下为STRSVR的操作说明：

（1） 执行可执行文件`<install-dir>\rtklib_<ver>\bin\strsvr.exe`。您可以看到STRSVR的主窗口。
![alt text](https://i.ibb.co/KsTz2zm/image.png)

（2） 要配置输入流，请在“（0）input”处使用下拉菜单选择流类型。可选择的流类型有Serial、TCP Client、TCP Server、NTRIP Client或File。可以设置流选项或启动/关闭命令，以及RTKNAVI的输入流。

（3） 要配置输出流，请使用下拉菜单在（1）输出、（2）输出或（3）输出处选择流类型。输出流的设置为串行、TCP Client、TCP Server、NTRIP Server、NTRIP Caster或File。

（4） 在2.4.2版本中，添加了流格式转换功能。要使用该功能，请按Conv输出流下拉菜单右侧的按钮。您可以看到“转换选项”对话框。要启用流格式转换功能，请选中对话框左上角的复选框，然后通过下拉菜单选择输入和输出格式。当前版本支持以下输入和输出格式。
（a） 输入：RTCM3、RTCM2、NovAtel OEM6、NovAtel-OEM3、u-blox、Superstar II、Hemisphere、SkyTraq、GW10、Javad、NVS BINR和BINEX
（b） 输出：RTCM 3（RTCM 2尚不支持）
![alt text](https://i.ibb.co/0XHSCWH/image.png)

输出消息应在对话框的消息类型字段中指定为以下形式：
```html
nnnn(ss), nnnn(ss), nnnn(ss), ....
```
将消息类型指定为字段nnnn，将消息间隔指定为字段（ss）（以秒为单位）。消息间隔可以省略。在这种情况下，消息间隔由输入消息间隔决定。下表显示了所有支持的输出RTCM消息。对于天线信息消息，字段由“选项”对话框给出。消息输入选项也可以在“选项”字段中指定。有关接收机相关选项的详细信息，请参阅D.5。

![alt text](https://i.ibb.co/1f102ST/image.png)

（5） 按下主窗口中的“开始”按钮。通信状态显示在主窗口中央下方的消息区域。主窗口左侧的状态指示器也显示通信状态。指示器颜色表示：橙色：等待连接，深绿色：已连接，浅绿色：数据活动，红色：错误。右侧还显示了输入和输出流的总数据量（字节）和数据速率（bps）。要停止通信，请按停止按钮。

（6） 通过按下选项。。。按钮，您可以使用“选项”对话框设置通信选项。要向连接输入流的服务器发送NMEA GPGGA消息，请选中“NMEA请求周期”，并在消息中设置请求周期（ms）和纬度/经度。要通过HTTP代理服务器从防火墙内部连接外部NTRIP caster，您可以在“HTTP/NTRIP proxy”字段中以`<address>：<port>`的形式输入地址和端口号。对话框中的其他字段用于在使用格式转换功能和发送天线信息消息的情况下生成天线和电台信息消息。

![alt text](https://i.ibb.co/b1z0Kv9/image.png)

## 3.4 使用RTKPOST进行后处理分析

RTKLIB包含后处理分析软件 RTKPOST。RTKPOST输入标准RINEX文件如3.02的观测数据和导航消息文件（GPS、GLONASS、Galileo、QZSS、北斗和SBAS），可以通过单点定位、DGPS/DGNSS、DGPS/DGNSS、Kinematic、Static、PPP-Kinematic 和 PPP-Static等各种定位模式计算定位解。

（1） 打开可执行文件`<install-dir>\rtklib_<ver>\bin\rtkpost.exe`。您可以看到RTKPOST的主窗口，而可执行文件`<install-dir>\rtklib_<ver>\bin\rtkpost_mkl.exe`则是链接intel mkl库的rtkpost版本，用于实现矩阵计算的加速。

![Main Window of RTKPOST](https://i.ibb.co/sRjkHVn/image.png)

（2） 在文本字段“RINEX OBS(: Rover)”中输入Rover接收机的RINEX观测数据文件路径。通过 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮显示的文件选择对话框填写文件路径或选择文件。您可以使用GZIP<sup>[57]</sup>（.gz）、COMPRESS（.z）或Hatanaka Compression<sup>[58]</sup>（.yyd）的压缩文件来处理RINEX观测数据。是否为压缩文件主要由文件的扩展名来识别。如果文件路径中包含通配符（*），则将展开通配符并读取多个文件。

（3） 如果您在相对定位模式下处理RINEX数据，如DGPS/DGNSS、Kinematic、Static、Moving-Base或Fixed模式，除了Rover观测数据文件外，您还必须在“RINEX OBS: Base Station”字段中输入基站接收机的文件路径。

（4） 您还必须在“RINEX *NAV/CLK, SP3, IONEX or SBS/ EMS”字段中输入RINEX导航电文文件的路径。如果文件路径中包含通配符（*），则通配符将展开，多个文件将像观测数据文件一样使用。要在PPP-Kinematic、PPP-Static或PPP-Fixed模式下使用精密星历时钟，您可以在字段中输入SP3-c（精密星历、钟文件）或RINEX CLK（精密钟文件）文件路径。您可以输入电离层VTEC网格校正的IONEX 1.0文件路径。对于SBAS更正，您可以输入RTKLIB格式或EMS（EGNOS消息服务器）2.0格式文件的SBAS消息日志文件路径。这里同样可以使用通配符（*）进行文件输入扩展。另外还可以以RTCM 3消息的形式输入SSR（状态空间表示）校正。这些输入文件的格式由其扩展名识别，如下所示：

（a） .sp3，.SP3，.eph，.EPH：SP3-c精密星历文件<sup>[22]</sup><br/>
（b）.sbs，.SBS，ems，.EMS：SBAS消息日志文件（附录B.2和<sup>[26]</sup>）<br/>
（c）.rtcm3，.RTCM3：RTCM 3 SSR校正消息文件<sup>[18]</sup><br/>
（d）*i、 *I：IONEX VTEC网格数据文件<sup>[24]</sup><br/>
（e）其他：RINEX OBS、NAV或CLK（自动识别）<sup>[9]-[15]</sup><br/>

（5） **输出结果文件**。在文本字段“Solution”中输入运算结果文件的路径。该字段会自动按照第一个输入文件名扩展为.pos或.nmea文件。如果选中“Dir”并填写该字段，则文件将指定目录输出。

(6) Push Options... button to set the processing options. For the detailed options for RTKPOST, refer 3.5 Configure Positioning Options for RTKNAVI and RTKPOST. You can set the start time or end time by checking and setting Time Start (GPST) or Time End (GPST) field in the main window. You also set the time interval by checking and setting the "Interval" field. With the ? button, the input time in GPS time can be converted to UTC, GPS Week/TOW, Day of Year, Day of Week, Time of Day and Leap Seconds.

（6） **配置按钮**。点击 <span style="border: 1px solid black; padding: 3px;">Options...</span> 按钮设置算法处理和一些可调的软件选项。有关RTKPOST的详细信息，请参阅3.5节有关配置选项的描述。您可以在主窗口中的“Time Start（GPST）”或“Time End（GPST）”字段来设置开始与结束时间。您还可以通过“Interval”字段来设置程序运行的时间间隔。参考 <span style="border: 1px solid black; padding: 3px;">?</span> 按钮，您可以查看GPS时间锁对应的UTC时间、GPS Week/Tow、年某天、星期、某一天的具体时间和闰秒。

![Time Dialog of RTKPOST](https://i.ibb.co/GxTzccm/image.png)

（7） **多会话分析**。如果您同时选中“Time Start”和“Time End”字段，则可以选中以小时为单位的“Unit”选项进行多会话（session）分析，具体来说，如果设置了“Unit”字段，那么程序将按指定的单位时间分为多个会话。为了避免覆盖之前的输出文件，输出文件路径必须包含根据会话时间替换的关键字。有关输入或输出文件路径中关键字替换的详细信息，请参阅3.5节有关配置选项的描述。

（8） **程序的执行与中止**。点击 <span style="border: 1px solid black; padding: 3px;">Execute</span> 按钮开启后处理程序，程序的处理状态会显示在主窗口中央下方。当当您在这里看到“done”消息时，程序就运行完成了。如果您想在途中停止处理，请按 <span style="border: 1px solid black; padding: 3px;">Abort</span> 按钮。

（9） **文本查看器**。程序运行完成后，按 <span style="border: 1px solid black; padding: 3px;">View...</span> 按钮查看输出文件的内容。您可以通过按下“文本查看器”窗口中的🔁按钮重新加载输出文件。您可以通过点击 <span style="border: 1px solid black; padding: 3px;">Options...</span> 来配置“文本查看器”。您还可以使用 <span style="border: 1px solid black; padding: 3px;">Find</span> 按钮在文本中搜索关键字。

![Text Viewer showing Solutions by RTKPOST](https://i.ibb.co/8jKTgXz/image.png)

（10） **Plot可视化**。点击 <span style="border: 1px solid black; padding: 3px;">Plot...</span> 按钮，您还可以使用RTKPLOT绘制结果。详情请参阅3.7使用RTKPLOT查看和绘制解决方案与观测数据章节。

![RTKPLOT Window executed by RTKPOST](https://i.ibb.co/rsGp2n9/image.png)

（11） **Google Earth文件转换**。通过 <span style="border: 1px solid black; padding: 3px;">KML/GPX...</span> 按钮，输出文件可以通过“KML/GPX”对话框转换为谷歌地球KML文件。设置好相应的配置，然后按对话框中的 <span style="border: 1px solid black; padding: 3px;">Convert</span> 按钮。您可以通过按下Google Earth按钮，使用生成的KML/KMZ文件启动Google Earth。要指定Google Earth执行文件，请配置“Options”-“Files”-“Google Earth Exe File”。

![Google Earth Converter Dialog of RTKPOST](https://i.ibb.co/2k7YR7P/image.png)

（12） 通过主窗口中的观测数据输入框旁边的 <span style="border: 1px solid black; padding: 3px;">🌐</span> 按钮，您可以通过RTKPLOT对观测数据进行可视化分析。您还可以通过按下 <span style="border: 1px solid black; padding: 3px;">📄</span> 按钮，使用文本查看器显示输入文件的内容。

（13） 如果想输出解决方案统计信息或调试跟踪，请按主窗口左下角的 <span style="border: 1px solid black; padding: 3px;">📄</span> 按钮查看解决方案统计文件和调试跟踪文件。为了在结果异常的情况下检查处理异常或警告，需要将“Options”-“Output”对话框中的“Output Debug Trace”设置为“Level2”（跟踪错误和警告），并查看输出调试跟踪文件。

## 3.5 配置RTKNAVI和RTKPOST的参数

可以按下RTKNAVI或RTKPOST的主窗口中的 <span style="border: 1px solid black; padding: 3px;">Options...</span> 来设置配置参数。可操作的定位参数如下。通过按下对话框上的 <span style="border: 1px solid black; padding: 3px;">Save...</span> 按钮并选择文件路径，可以将这些选项保存到配置文件中。通过按下 <span style="border: 1px solid black; padding: 3px;">Load...</span> 按钮并选择配置文件，从而在配置文件中加载选项。配置参数的更多信息请请参考B.4。下表还显示了可以包含在配置文件中的关键字。这些选项中指定的模型也在附录E中进行了详细解释。

（1） Setting1
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
        - Fixed: 流动站接收机固定坐标，解算模糊度、对流层、电离层等参数 \*<br>
        - PPP Kinematic: 动态精密单点定位<br>
        - PPP Static: 静态精密单点定位<br>
        - PPP Fixed: PPP 固定坐标，解算模糊度、对流层、电离层等参数 \*
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
      <td>Single, PPP-\* 不支持</td>
    </tr>
    <tr>
      <td>滤波类型</td>
      <td>
        设置滤波类型<br>
        - Forward: 前向滤波<br>
        - Backward: 后向滤波 *<br>
        - Combined: 前向与后向组合滤波 \*, 相位偏置会在前向、后向解间重置。<br>
        - Combined–no phase reset: 前向与后向组合滤波 \*, 相位偏置不会在前向、后向解间重置。<br>
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
        设置截止 SNR 阈值. 点击 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮即可看到 "SNR Mask" 对话框. 可以在每 5 ° 的仰角区间设置 SNR 截止阈值，如何 "Rover" 和 "Base Station" 复选框没有选中，那么 SNR MASK 将不会应用。
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
        - ON: 将使用接收机的速度和加速度用于预测。<br>
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
        - Solid/OTL：应用固体地球潮汐、OTL（海洋潮汐负载）和极潮校正。\*<br>
        要应用OTL校正，请在“Files”选项卡中的“Ocean Loading BLQ Format”中设置OTL系数文件路径，标记名称必须包含在输入的RINEX文件中，以便在BLQ文件中选择站点。<br>
        要应用极潮，请在“Files”菜单的“EOP Data Files”选项中设置ERP（地球自转参数）文件路径。
      </td>
      <td>pos1-tidecorr</td>
      <td>不适用于Single模式，\*不适用于RTKNAVI模式</td>
    </tr>
    <tr>
      <td>Ionosphere Correction</td>
      <td>
        设置电离层校正选项。如果设置了该参数，则会为每颗卫星估计垂直电离层延迟。对于长基线情形，电离层估计可以有效抑制电离层延迟的影响。<br>
        - OFF：不应用电离层校正<br>
        - Broadcast：应用广播电离层模型<br>
        - SBAS：应用SBAS电离层模型<br>
        - Iono-Free LC：使用双频（GPS/GLONASS/QZSS的L1-L2或Galileo的L1-L5）测量的无电离层线性组合进行电离层校正<br>
        - Estimate STEC：估计电离层参数STEC（倾斜总电子含量）\*<br>
        - IONEX TEC：使用IONEX TEC网格数据<br>
        - QZSS Broadcast：应用QZSS提供的广播电离层模型<br>
        - QZSS LEX：（保留）
      </td>
      <td>pos1-ionoopt</td>
      <td>\*不适用于Single模式和PPP-\*模式</td>
    </tr>
    <tr>
      <td>Troposphere Correction</td>
      <td>
        设置是否估计对流层参数（Rover和Base位置的天顶总延迟）。<br>
        - OFF：不应用对流层校正<br>
        - Saastamoinen：应用Saastamoien模型<br>
        - SBAS：应用SBAS对流层模型（MOPS）<br>
        - Estimate ZTD：估计ZTD（天顶总延迟）参数为EKF状态\*<br>
        - Estimate ZTD+Grad：估计ZTD和水平梯度参数为EKF状态\*
      </td>
      <td>pos1-tropopt</td>
      <td>*不应用于Single模式</td>
    </tr>
    <tr>
      <td>Satellite Ephemeris/Clock</td>
      <td>
        设置卫星星历的类型。
        - Broadcast：使用广播星历
        - Precise：使用精密星历\*
        - Broadcast+SBAS：具有SBAS长期快速校正功能的广播星历
        - Broadcast+SSR APC：带RTCM SSR校正的广播星历（天线相位中心值）
        - Broadcast+SSR CoM：使用RTCM SSR校正（卫星质心值）的广播星历
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
      <td>设置是否应用PPP模式的相位饱和校正。</td>
      <td>pos1-posopt3</td>
      <td>仅适用于PPP-\*模式。</td>
    </tr>
    <tr>
      <td>Reject Ecl</td>
      <td>
        设置是否排除处于日食状态的GPS Block IIA卫星。处于日食状态的Block IIA卫星常常由于偏航姿态的不可预测行为而降低PPP解的质量。
      </td>
      <td>pos1-posopt4</td>
      <td>仅适用于PPP-\*模式。</td>
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
      <td>仅适用于PPP-\*模式。</td>
    </tr>
    <tr>
      <td>Excluded Satellites (+PRN: Included)</td>
      <td>
        设置定位时排除的卫星。输入以空格分隔的卫星PRN编号。对于GLONASS、Galileo、QZSS、北斗和SBAS，分别使用Rnn、Enn、Jnn、Cnn和Snn（nn：卫星PRN或槽号）.如果在卫星ID前加上“+”，即使卫星状态不佳，该卫星也会被包括在定位中。
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

（2） Setting2
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
        - Continuous：持续估计并求解静态整数模糊度 \*<br>
        - Instantaneous：按历元逐个估计并求解整数模糊度 \*<br>
        - Fix and Hold：持续估计并求解静态整数模糊度。如果验证通过，模糊度将被约束为求解值 \*<br>
        - PPP-AR：不支持
      </td>
      <td>pos2-armode</td>
      <td>
        默认值：Continuous不适用于Single模式。<br>
        \*仅适用于Kinematic, Static, Movingbaseline 和 Fixed模式。
      </td>
    </tr>
    <tr>
      <td>Integer Ambiguity Resolution (GLO)</td>
      <td>
        为GLONASS设置整数模糊度求解策略：<br>
        - OFF：不固定模糊度。<br>
        - ON：固定模糊度。通常只有相同类型的接收机对（流动站和基准站）的模糊度才能被固定。如果不同类型的接收机存在IFB（频间偏差），则无法通过双差消除。<br>
        - Fix and Hold：在首次固定并保持GPS卫星后消除IF偏差。<br>
        - Autocal：接收机IF偏差自动校准，但需要合理的初始估计（参见GLONASS硬件偏差）。
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
        - 设置“ratio-test”的整数模糊度检验阈值，使用最佳整数向量与次佳向量的平方残差之比。<br>
        - 如果最小值和最大值设置为等于标称值，则比率是固定的，否则比率将根据卫星的数量进行调整。标称值用于8个卫星对，并且随着卫星数量的增加而降低，随着卫星数量的减少而增加。
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
        设置GLONASS相对硬件偏差，单位为米每频率槽。仅在pos2-gloarmode设置为“autocal”时使用，用于指定两个不同接收机制造商之间的通道间偏差。
      </td>
      <td>pos2-arthres2</td>
      <td>默认值：0.0</td>
    </tr>
    <tr>
      <td>Min Lock / Elevation to Fix Ambiguity</td>
      <td>
        设置固定整数模糊度所需的最小锁定计数和最小仰角（°）。如果锁定计数或仰角小于该值，则该模糊度将被排除在固定的整数向量之外。
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
        设置多普勒和载波相位差之间的频率差的周跳检测阈值（Hz）（0 表示 禁用）<br>
        设置无几何约束线性组合载波相位差之间的周跳检测阈值（m）。
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
        设置GDOP和卡尔曼滤波器新息的拒绝阈值（米）。如果GDOP或载波相位新息超过该值，则观测值将作为异常值从估计过程中排除。
        新息阈值将乘以载波相位/伪距误差比，以生成伪距新息的阈值。
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
        设置解决方案的时间同步模式：
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

（3） Output

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

（4） Statistics
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
      <td colspan="4">Measurement Errors</td>
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
      <td colspan="4">Process Noises</td>
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

（5） Positions
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
      <td colspan="4">Rover</td>
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
      <td colspan="4">Base Station</td>
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
        要选择天线类型，请在“文件”中设置接收机天线PCV文件。如果使用“\*”，则天线类型和天线偏差将通过RINEX观测文件头中的天线信息（RTKPOST）或RTCM天线信息（RTKNAVI）来识别。
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

（6） Files

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

(8) Misc (RTKNAVI)
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


(7) Misc (RTKPOST)
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

## 3.6 使用RTKCONV将接收机原始数据转换为RINEX
RINEX（Receiver Independent Exchange Format，接收机无关交换格式）是一种被许多接收机或GPS/GNSS后处理分析软件支持的标准GPS/GNSS数据格式。RTKLIB后处理分析应用RTKPOST也可以将RINEX数据作为输入。为了准备RINEX文件，RTKLIB提供了RINEX格式转换应用RTKCONV，它将接收机的原始（私有）数据、RTCM或BINEX消息转换为RINEX OBS（观测数据）、RINEX NAV（GNSS导航消息）。RTKCONV还可以从接收机原始数据中提取SBAS消息，并输出SBAS日志文件。<br>
RTKLIB支持的RINEX版本有2.10、2.11、2.12、3.00、3.01以及3.02，并带有RTKLIB扩展。有关SBAS日志文件的详细信息请参阅附录B.2，有关支持的RINEX文件的详细信息请参阅附录D.1。

（1） 运行二进制应用`<install-dir>\rtklib_<ver>\bin\rtkconv.exe`。您可以看到RTKCONV的主窗口。

![Main Window of RTKCONV](https://i.ibb.co/C6m7Ddn/image.png)

（2）将接收机原始数据文件路径输入到文本字段RTCM、RCV RAW或RINEX OBS中。可以直接填写文件路径，也可以通过点击 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮打开文件选择对话框来选择文件。你还可以将原始数据文件的图标拖放到RTKCONV主窗口中。支持的接收机原始数据格式如下。关于每种格式支持的详细消息，请参阅附录D.2。关于在RINEX版本2、RINEX版本3、RTCM 3 MSM和BINEX中表达的多个观测码之间转换时应使用的对应关系，请参阅附录D.3。

(a) RTCM2：RTCM 2.3版本。<br>
(b) RTCM3：RTCM 3.0、3.1（含修正案1-5）以及3.2版本。<br>
(c) NovAtel OEM6：NovAtel OEM4/OEMV/OEM6以及OEMStar二进制格式。<br>
(d) NovAtel OEM3：NovAtel OEM3（Millennium）二进制格式。<br>
(e) u-blox：u-blox LEA-4T、5T、6T二进制格式。<br>
(f) Superstar II：NovAtel Superstar II二进制格式。<br>
(g) Hemisphere：Hemisphere Crescent/Eclipse二进制格式。<br>
(h) SkyTraq：SkyTraq S1315F二进制格式。<br>
(i) GW10：Furuno GW-10-II/III二进制格式。<br>
(j) Javad：JAVAD GRIL/GREIS二进制格式。<br>
(k) NVS BINR：NVS NV08C BINR二进制格式。<br>
(l) BINEX：BINEX格式（仅支持大端序、正向、标准CRC消息）。<br>
(m) RINEX：RINEX OBS/NAV格式。

如果在文件路径中使用通配符（*），则通配符会被扩展为多个文件，并且这些文件将会被读取。通过将“RINEX”设置为输入文件格式，并设置输出RINEX版本，你可以将RINEX版本2转换为版本3，或者将RINEX版本3转换为版本2。在这种情况下，你可以提取并输出带有选定卫星、信号、输入RINEX数据的时间跨度或时间间隔的RINEX数据。

（3）通过下拉菜单“Format”选择格式。如果你选择“Auto”，RTKCONV会根据以下文件扩展名来识别文件格式。

(a) RTCM2 : .rtcm2<br>
(b) RTCM3 : .rtcm3<br>
(c) NovAtel OEM6 : .gps<br>
(d) u-blox : .ubx<br>
(e) Superstar II : .log<br>
(f) Hemisphere : .bin<br>
(g) SkyTraq : .stq<br>
(h) Javad : .jps<br>
(i) BINEX : .bnx, .binex <br>
(j) RINEX : .obs,.*o,.nav,.*n,.*p,.*g,.*h,.*q,.*l

（4）输入RINEX OBS（观测数据）、RINEX NAV（GPS导航消息）、RINEX GNAV（GLONASS导航消息）、RINEX HNAV（GEO卫星导航消息）、RINEX QNAV（QZSS导航消息）、RINEX LNAV（Galileo导航消息）和SBAS日志文件的输出路径。可以直接填写文件路径，也可以通过点击 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮打开文件选择对话框来选择文件。如果你没有勾选左侧的复选框，该文件就不会被输出。RINEX GNAV、HNAV、QNAV和LNAV仅在RINEX版本2中支持。如果选择版本3作为输出RINEX，所有导航数据都将输出到一个合并（混合）的NAV类型RINEX文件中。输出文件可以包含关键词。这些关键词会被时间、日期或测站ID所替换。点击“？”按钮，可以查看输出文件中关键词的替换规则。

（5）如果你想将文件输出到一个目录中，勾选“Output Directory”并输入输出目录。可以通过点击 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮来选择目录。如果未勾选“Output Directory”，文件将输出到输入文件所在的同一目录中。如果输出目录不存在，该目录将自动创建。即使在这种情况下，父目录也必须存在。否则，目录创建将会失败。

（6）你可以通过勾选并设置主窗口上方的“Time Start (GPST)”或“Time End (GPST)”字段，可选地设置开始时间或结束时间。你还可以通过勾选并设置“Interval”字段来设置时间间隔选项。如果你输入了“Time Start”和“Time End”，勾选“Unit”并输入“Time Unit”，你可以启动多会话转换。在这种情况下，请在输入文件路径和输出文件路径中包含将被日期和时间替换的关键词。请注意，在这种多会话转换的情况下，RTKCONV不会确认输出文件是否存在，从而可能导致文件被覆盖。

（7）你可以点击 <span style="border: 1px solid black; padding: 3px;">Options...</span> 按钮来配置RINEX选项。通过“Options”对话框设置RINEX头信息、导航系统、观测类型或频率的选项。如果你勾选了RINEX Name，输出文件路径将符合RINEX文件命名规范。请输入Station ID以完成符合RINEX文件命名规范的输出文件名。"Option"字段是接收机依赖的选项。像"-EPHALL -GL1X"这样用空格分隔来指定选项。具体细节，请参阅附录D.5 接收机依赖的输入选项。对于RINEX 3，你最好勾选"Scan Obs Types"以获取输入文件中有效的OBS TYPES。在这种情况下，输入文件首先会被扫描以获取可用的OBS TYPE列表作为第一个转换步骤，然后RTKCONV在第二个转换步骤中输出RINEX。如果未勾选"Scan Obs Types"，输出RINEX文件中的OBS TYPES将由默认的OBS TYPES决定，这些默认的OBS TYPES取决于输入格式和下面描述的"Signal Mask"设置。

![Options Dialog of RTKCONV](https://i.ibb.co/W5tJXSf/image.png)

（8）要设置输出到观测文件的OBS TYPES的掩码，使用在“Options”对话框中点击 <span style="border: 1px solid black; padding: 3px;">Mask...</span> 按钮显示的“Signal Mask”对话框。你应在该对话框中勾选要输出到RINEX观测文件中的信号。对于不希望输出到RINEX观测文件中的信号，应取消勾选。请注意，即使勾选了，那些不在默认OBS TYPES集合中或不在输入数据中的信号也不会被输出。可以使用 <span style="border: 1px solid black; padding: 3px;">Set All</span> 或 <span style="border: 1px solid black; padding: 3px;">Unset All</span> 按钮来选择或取消选择所有信号。关于输入数据和输出RINEX文件中的观测类型或信号ID，请参阅附录D.3。

![Signal Mask Dialog of RTKCONV Options](https://i.ibb.co/nnsrD3H/image.png)

（8）点击 <span style="border: 1px solid black; padding: 3px;">Convert</span> 按钮开始将接收机原始数据转换为RINEX和SBAS日志文件。如果你想中途停止转换，点击 <span style="border: 1px solid black; padding: 3px;">Abort</span> 按钮。状态会在主窗口下方中央的消息区域显示。消息O=nnn表示已转换的观测数据（历元）的数量。消息N=nnn、G=nnn、H=nnn、Q=nnn、L=nnn、S=nnn和E=nnn分别表示导航消息（NAV、GNAV、HNAV、QNAV和LNAV）、SBAS消息和错误的数量。

（14）完成转换后，你可以通过点击 <span style="border: 1px solid black; padding: 3px;">Plot...</span> 按钮使用RTKPLOT查看观测数据图表。详细信息请参阅3.7节“绘制和查看解算结果及观测数据”。你还可以通过点击按钮 <span style="border: 1px solid black; padding: 3px;">📄</span> 使用文本查看器查看输出文件。

（15）点击 <span style="border: 1px solid black; padding: 3px;">Process...</span> 按钮，你可以执行RTKPOST_MKL应用来处理转换后的RINEX OBS/NAV文件。详细信息请参阅3.4节“使用RTKPOST进行后处理分析”。

## 3.7 使用RTKPLOT查看和绘制解决方案

RTKLIB包含AP RTKPLOT，用于通过图形用户界面查看和绘制RTKPOST和RTKNAVI的定位解。RTKPLOT还接受通用NMEA 0183文件或流来生成解决方案图。

（1） 执行二进制AP文件`<install-dir>\rtklib_<ver>\bin\rtkplot.exe`。您可以看到RTKPLOT的主窗口。通过推送Plot。。。按钮或RTKPOST和RTKCONV、RTKPLOT的一些按钮也被执行。

![Main Window of RTKPLOT](https://i.ibb.co/1qzLWB0/image.png)

（2）执行菜单“File” - “Open Solution 1”，并通过文件选择对话框选择解决方案文件。输入的解决方案文件可以是RTKLIB解决方案格式或NMEA-0183格式。如果文件格式是NMEA-0183，文件必须至少包含NMEA GPRMC和GPGGA语句。如果解决方案文件有效，接收机的地面轨迹将在地图上的窗口中绘制出来。可以通过菜单“Edit” - “Options”更改绘制中标记、线条和网格的颜色。主窗口底部的状态栏还会显示时间范围、解决方案历元的数量（N=nnnn）、基线长度（B=0.0-x.xkm）、每种质量解决方案的数量和百分比（Q=1:nnn(pp%)，2:nnn(pp%)，...）。质量标志Q和标记颜色的含义为：1：Fixed，2：Float，4：DGPS，5：Single（颜色可以通过绘制选项更改）。要按质量标志Q筛选标记，选择工具栏中的第二个下拉菜单。通过将解决方案文件图标拖放至RTKPLOT主窗口，也可以读取和绘制解决方案文件。

![GND TRK Plot by RTKPLOT](https://i.ibb.co/544F33t/image.png)

（3） 通过在绘图上按住鼠标左键拖动鼠标，您可以向上、向下、向左和向右拖动地图。您还可以通过右键上下拖动鼠标或旋转鼠标滚轮来更改地图的比例。

（4）通过选择工具栏右侧的绘制类型下拉菜单，你可以将绘制切换到接收机位置的东/北/天（E/N/U）分量（位置）、接收机速度的东/北/天（E/N/U）分量（速度）或接收机加速度的东/北/天（E/N/U）分量（加速度）。你可以通过左键拖动来拖动X/Y轴，并通过在X/Y轴区域右键拖动来改变比例尺。你可以通过点击绘制类型下拉菜单右侧的三个按钮来隐藏或显示这三个绘制图。

![POSITION Plot by RTKPLOT](https://i.ibb.co/pdftbRq/image.png)

（5）通过选择绘制类型下拉菜单，你可以将绘制切换到NSat/Age/Ratio（有效卫星数量、差分年龄、模糊度验证的比率因子）。如果你将“Output Solution Status”选项设置为“Residuals”，你可以显示残差绘制图。你可以通过选择L1/LC、L2或L5来切换频率。在残差绘制模式下，你可以通过右侧下拉菜单选择一颗卫星，也可以选择所有卫星。在载波相位的残差绘制中，红色线条表示周跳，灰色线条表示奇偶性未知标志（这意味着载波相位中的半周模糊度未解决）。

![RESIDUALS Plot by RTKPLOT](https://i.ibb.co/BCjwQTg/image.png)

（6）通过点击工具栏中的工具按钮，你可以使用 来将当前位置居中，使用 来调整X轴的比例尺，使用 来调整Y轴的比例尺，使用 来以大标记显示当前位置，使用 将当前轨迹位置固定在水平中心，使用 将当前轨迹位置固定在垂直中心，使用 开始动画播放，使用 停止动画播放。你还可以滑动“Time Scroll Bar”（时间滚动条）来改变当前历元。要清除已读取的数据，执行菜单“File” - “Clear”或点击工具栏中的 按钮。要重新加载解决方案文件，执行菜单“File” - “Reload”或点击工具栏中的 按钮。

（7）通过执行菜单“File” - “Open Map Image”，你可以读取一张JPEG图像，并在“Gnd Trk”（地面轨迹）绘制类型的情况下将其作为背景地图图像绘制在绘制区域中。可以通过点击工具栏中的 按钮来启用或禁用该图像。

![Map Image Overlay by RTKPLOT](https://i.ibb.co/J52r993/image.png)

（8）要调整地图图像中的位置，执行菜单“Edit” - “Map Image”，并在“Map Image”对话框中输入图像中心的纬度和经度、沿X轴或Y轴的图像比例尺。完成后，点击“Save Tag”按钮将调整信息保存到图像标签文件中。图像标签文件的路径是原始地图图像文件路径 + “.tag”。如果图像标签文件已存在，它将与地图图像本身一起自动读取。当前版本不支持地图图像的旋转。请选择北方向正确对准上方的地图图像。例如，你可以通过Google Earth的菜单“File” - “Save”获取一张JPEG图像。要将北方向固定在上方，点击Google Earth中的“N”按钮。为了避免地图图像的变形，请将坐标原点设置在地图图像内部或附近。

![Map Image Options Dialog of RTKPLOT](https://i.ibb.co/jrgdRMX/image.png)

（9）在2.4.2版本中，为RTKPLOT添加了Google Map View功能，以便在谷歌地图上绘制解决方案。要打开Google Map View，在读取解决方案后，执行RTKPLOT的菜单“View” - “Google Map View”。也可以使用工具栏按钮 或 来显示这些视图。请注意，始终需要互联网连接才能使用这些基于谷歌提供的服务的视图。

（10）在谷歌地图视图中，只有一个工具栏按钮用于将轨迹点中心固定，与谷歌地球视图相同。对于谷歌地图视图的其他操作，使用谷歌地图视图中的控件。

![Google Map View of RTKPLOT](https://i.ibb.co/GcSfLbt/image.png)

（11）通过执行菜单“Edit” - “Waypoints...”，你可以看到“Waypoints”对话框。通过该对话框，你可以以列表形式加载、保存、添加和删除航点。点击“Add”按钮并编辑点名，可以将当前接收机位置添加到航点列表中。当按钮按下时，航点的位置会在“Gnd Trk”（地面轨迹）绘制图中显示。

![Waypoints Dialog of RTKPLOT](https://i.ibb.co/kHKwdY7/image.png)

（12）要绘制多个解决方案文件，执行菜单“File” - “Open Solutions-2”，并通过文件选择对话框选择文件。你可以通过工具栏中的1 2按钮来切换解决方案1和2的绘制开/关。要绘制解决方案1和解决方案2之间的差异，点击工具栏中的1-2按钮。

（13）要设置解决方案的时间范围和时间间隔，执行菜单“Edit” - “Time Span/Interval”，并在“Time Span/Interval”对话框中勾选并设置Time Start、Time End和Interval字段。

![Time Span/Interval Dialog of RTKPLOT](https://i.ibb.co/W0kQR38/image.png)

（14）通过执行菜单“编辑”-“解决方案源”，您可以以文本形式查看解决方案的源。

![Solution Source View of RTKPLOT](https://i.ibb.co/PFrsshy/image.png)

（15）要实时绘制解决方案，执行菜单“File” - “Connection Settings”，并在“Connection Setting”对话框中设置解决方案参数。你可以为解决方案1和解决方案2选择流类型（Stream Type）、流选项（Stream Option，简称Opt）、流命令（Stream Commands，简称Cmd）、解决方案格式（Solution Format）、时间格式（Time Format）、经纬度格式（Lat/Lon Format）和字段分隔符（Field Sep）。设置好连接参数后，执行菜单“File” - “Connect”或点击工具栏中的按钮来建立连接。要断开外部设备的连接，执行菜单“File” - “Disconnect”或再次点击连接按钮。例如，如果选择串行（serial）作为流类型，并选择NMEA0183作为解决方案格式，你可以在RTKPLOT窗口中监测外部接收机的NMEA输出。

![Connection Settings Dialog of RTKPLOT](https://i.ibb.co/TMG6Wh2/image.png)

（16）通过点击RTKNAVI中的Plot...按钮，RTKPLOT会自动运行并连接到RTKNAVI的监控端口。如果你想连接到在远程电脑上运行的RTKNAVI，需要在连接设置对话框中配置连接设置，例如将流类型设置为TCP客户端，输入远程电脑的IP地址以及RTKNAVI监控端口的端口号，然后连接到远程的RTKNAVI。在这种情况下，允许多台运行RTLPLOT的电脑进行多个客户端连接。

（17）要配置RTKPLOT的绘制选项，执行菜单“Edit” - “Options...”，并使用以下的“Options”对话框设置选项。

![Options Dialog of RTKPLOT](https://i.ibb.co/chvmBnz/image.png)

<table>
  <thead>
    <tr>
      <th>选项</th>
      <th>描述</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Time Format</td>
      <td>
        选择时间格式。<br>
        (wwww/ssss, h:m:s GPST, h:m:s UTC or h:m:s LT).
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Lat/Lon Format</td>
      <td>
        选择经纬度格式
        (ddd.ddddd or ddd mm ss.ss)
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Show Statistics</td>
      <td>选择是否显示统计信息</td>
      <td></td>
    </tr>
    <tr>
      <td>Cycle-Slip</td>
      <td>
        设置是否在卫星可见性图中显示周跳位置。如果选择“LG Jump”，则使用双频无几何LC（线性组合）来检测周跳。在“LLI Flag”的情况下，使用RINEX观测数据中的LLI（失锁指示器）。周跳在卫星可见性图中以红色垂直线显示。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Parity Unknown</td>
      <td>
        设置是否在卫星可见性图中显示奇偶性未知状态。奇偶性未知的历元在卫星可见性图中以灰色垂直线显示。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Ephemeris</td>
      <td>
        设置是否在卫星可见性图中显示星历状态。星历以观测数据下方的灰色线显示。灰色点表示Toe（星历时间）。红色的星历线表示卫星不健康。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Elevation Mask</td>
      <td>
        设置卫星能见度图的仰角限制角度（度）。仰角限制也用于DOP/NSat图。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Elevation Mask Pattern</td>
      <td>设置是否使用仰角限制模式。</td>
      <td></td>
    </tr>
    <tr>
      <td>Hide Low Satellite</td>
      <td>设置是否显示高程遮罩和高程遮罩图案下的低高程卫星。</td>
      <td></td>
    </tr>
    <tr>
      <td>Maximum DOP </td>
      <td>设置DOP/NSat图的y轴限制。</td>
      <td></td>
    </tr>
    <tr>
      <td>Receiver Position</td>
      <td>
        设置卫星能见度图或skyplot的接收机位置。“单一解决方案”通过使用观测和导航数据，将单点结果用作接收机位置。对于移动接收机，您应进行设置。“Lat/Lon/Hgt”使用纬度、经度和高度来指定以下Lat/Lon/Hgt字段中的静态接收机。“RINEX标头”使用RINEX观测数据标头中的“APPROX POSITION XYZ”作为接收机位置。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Satellite System</td>
      <td>选中视图中选中的导航系统</td>
      <td></td>
    </tr>
    <tr>
      <td>Excluded Sats</td>
      <td>设置剔除的卫星。以空格作为间隔，填写卫星号或ID</td>
      <td></td>
    </tr>
    <tr>
      <td>Error Bar/Circle</td>
      <td>
        设置在解决方案显示中是否显示误差条或误差圆。您可以选择“条/圆”或“点”作为格式。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Direction Arrow</td>
      <td>设置在解决方案地面轨迹图中是否显示方向箭头和速度箭头。</td>
      <td></td>
    </tr>
    <tr>
      <td>Graph Label</td>
      <td>
        设置在解决方案显示中是否显示图形标签。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Grid/Grid Label</td>
      <td>
        设置在解决方案显示中是否显示网格和网格标签。对于圆形网格，将其设置为“圆圈”或“圆圈/标签”。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Compass</td>
      <td>
        设置在解决方案地面轨迹图中是否显示罗盘。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Scale</td>
      <td>设置在解决方案地面轨迹图中是否显示比例尺。</td>
      <td></td>
    </tr>
    <tr>
      <td>Auto Fit</td>
      <td>
        设置解决方案图表的比例尺是否自动调整。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Y-Range (+/-)</td>
      <td>设置解决方案图表中Y轴的范围。</td>
      <td></td>
    </tr>
    <tr>
      <td>RT Buffer Size</td>
      <td>
        设置实时解决方案图表的缓冲区大小（以历元为单位）。超出缓冲区大小的旧解决方案将从实时图表中删除。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Coordinate Origin</td>
      <td>
        选择解决方案显示的原点位置如下：<br>
        Start Pos：第一个解决方案位置<br>
        End Pos：最后一个解决方案位置<br>
        Average Pos：所有解决方案位置的平均值<br>
        Linear Fit Pos：基于线性拟合的位置<br>
        Base Station：基准站位置<br>
        Lat/Lon/Hgt：指定的纬度、经度和高程<br>
        Auto Input：以下自动位置<br>
        Waypoint：一个航点位置<br>
        如果您选择“Lat/Lon/Height”，则需要在下方的文本框中输入原点的纬度、经度和椭球高。如果选择“自动输入”，则假定接收机ID为解决方案文件名头部的4个字符，并从位置文件中读取位置。可以通过点击...按钮并点击位置列表对话框中的“加载”按钮来选择位置文件。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Mark Color 1(1-6)</td>
      <td>
        设置图表中解决方案编号1或观测数据的标记颜色。点击右侧的颜色面板，并通过颜色选择对话框选择颜色。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Mark Color 2(1-6)</td>
      <td>
        设置图表中解决方案编号2的标记颜色。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Line Color</td>
      <td>设置视图的线条颜色</td>
      <td></td>
    </tr>
    <tr>
      <td>Text Color</td>
      <td>设置视图中的文本颜色</td>
      <td></td>
    </tr>
    <tr>
      <td>Grid Color</td>
      <td>设置视图中网格颜色</td>
      <td></td>
    </tr>
    <tr>
      <td>Background Color</td>
      <td>设置图表中的背景颜色。</td>
      <td></td>
    </tr>
    <tr>
      <td>Plot Style</td>
      <td>
        选择图表中的绘图样式。若要删除除轨迹点标记外的所有标记和线条，将其设置为“None”。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Mark Size</td>
      <td>设置视图中的标记尺寸</td>
      <td></td>
    </tr>
    <tr>
      <td>Font</td>
      <td>选择图表中的字体。点击...按钮，并通过字体选择对话框来选择字体。</td>
      <td></td>
    </tr>
    <tr>
      <td>Animation Interval</td>
      <td>设置解决方案或观测数据图表的动画间隔。</td>
      <td></td>
    </tr>
    <tr>
      <td>Update Cycle (ms)</td>
      <td>
        设置实时图表的绘图更新周期时间（以毫秒为单位）。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Lat/Lon/Hgt</td>
      <td>
        设置原点的纬度、经度和高程。可以直接填写数值，或者点击...按钮并选择一个站点位置。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>QC Command</td>
      <td>
        设置观测数据的质量控制（QC）命令及其选项。默认情况下，设置了TEQC QC模式选项。该命令用于执行菜单“Edit” - “Obs Data QC...”。命令必须位于命令搜索路径中，或者位于RTKLIB可执行文件的目录中。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>RINEX Opt</td>
      <td>
        设置RINEX读取选项。有关RINEX读取选项的详细信息，请参阅&lt;install dir&gt;\rtklib_&lt;ver&gt;\src\rinex.c。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>TLE Data</td>
      <td>
        指定NORAD TLE卫星轨道元素数据文件。如果卫星星历不可用，TLE数据将用于计算天空图中的卫星位置。TLE数据可以使用两行格式或三行格式。示例TLE数据可以在以下位置找到：<br>
        &lt;install dir&gt;\rtklib_&lt;ver&gt;\data\catalbe_2l_2013_01_09_pm.txt。
      </td>
      <td>另请参阅RTKNAVI选项对话框</td>
    </tr>
    <tr>
      <td>Sat No</td>
      <td>
        指定卫星编号文件，该文件用于将GNSS卫星/PRN编号与NORAD TLE数据文件中的TLE卫星目录编号相连接。示例卫星编号文件可以在以下位置找到：<br>
        &lt;install dir&gt;\rtklib_&lt;ver&gt;\data\TLE_GNSS_SATNO.txt。
      </td>
      <td></td>
    </tr>
  </tbody>
</table>

（19）RTKPLOT提供了以下菜单。部分菜单可以通过点击工具栏上的按钮来执行。

<table>
  <thead>
    <tr>
      <th></th>
      <th>菜单</th>
      <th>工具栏</th>
      <th>描述</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td></td>
      <td>Open Solution-1...</td>
      <td></td>
      <td>打开No.1中的解决方案</td>
      <td>*双击打开</td>
    </tr>
    <tr>
      <td></td>
      <td>Open Solution-2...</td>
      <td></td>
      <td>打开No.2中的解决方案</td>
      <td>*双击打开</td>
    </tr>
    <tr>
      <td></td>
      <td>Open Map Image...</td>
      <td></td>
      <td>为解决方案图表打开地图图像数据。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Open Obs Data...n</td>
      <td></td>
      <td>打开观测数据。导航数据也会自动打开。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Open Nav Data...</td>
      <td></td>
      <td>手动打开星历数据。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Open Elev Mask...</td>
      <td></td>
      <td>打开仰角限制数据。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Visibility Analysis...</td>
      <td></td>
      <td>执行卫星可见性分析。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Save Image...</td>
      <td></td>
      <td>将视图保存为图片。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Save # of Sats/DOP...</td>
      <td></td>
      <td>将卫星数量和DOP保存到文本文件中。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Save SNR,MP and AZ/EL..</td>
      <td></td>
      <td>将信噪比、多径效应以及方位角/仰角数据保存到文本文件中。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Connect...</td>
      <td></td>
      <td>连接到外部实时解决方案数据流。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Disconnect...</td>
      <td></td>
      <td>断开连接。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Connection Settings...</td>
      <td></td>
      <td>显示“连接设置”对话框以配置连接选项。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Reload</td>
      <td></td>
      <td>重新加载解决方案、观测和导航数据。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Clear</td>
      <td></td>
      <td>清空解决方案、观测数据和导航数据。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Exit</td>
      <td></td>
      <td>关闭并退出RTKPLOT。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Time Span/Interval...</td>
      <td></td>
      <td>设置“时间跨度/间隔”对话框以设置时间跨度和时间间隔。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Map Image...</td>
      <td></td>
      <td>显示“地图图像”对话框以配置图像数据的大小、位置和比例。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Waypoints...</td>
      <td></td>
      <td>显示“Waypoint”对话框用以增加或修改Waypoint</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Solution Source...</td>
      <td></td>
      <td>通过文本查看器显示解决方案数据的来源。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Obs Data Source...</td>
      <td></td>
      <td>通过文本查看器显示观测数据的来源。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Obs Data QC...</td>
      <td></td>
      <td>执行观测数据的质量控制（QC），并使用文本查看器显示结果。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Copy To Clipboard</td>
      <td></td>
      <td>将图像拷贝到粘贴板</td>
      <td></td>
    </tr>
    <tr> 
      <td></td>
      <td>Options</td>
      <td></td>
      <td>设置对话框。</td>
      <td></td>
    </tr>
    <!-- View -->
    <tr>
      <td></td>
      <td>Show Tool Bar</td>
      <td></td>
      <td>显示或隐藏工具栏</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Show Status Bar</td>
      <td></td>
      <td>显示或隐藏状态栏</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Google Earth View...</td>
      <td></td>
      <td>显示谷歌地球视图</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Google Map View...</td>
      <td></td>
      <td>显示谷歌地图视图</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Input Monitor 1...</td>
      <td></td>
      <td>显示“输入监视器”窗口以查看实时输入流1号。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Input Monitor 2...</td>
      <td></td>
      <td>显示“输入监视器”窗口以查看实时输入流2号。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Center Origin</td>
      <td></td>
      <td>将坐标原点移动到绘图区域的中心。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Fit Horizontal</td>
      <td></td>
      <td>在绘图中调整解决方案或观测数据的水平范围以适应视图。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Fit Vertical</td>
      <td></td>
      <td>在绘图中调整解决方案数据的垂直范围以适应视图。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Show Track Point</td>
      <td></td>
      <td>在绘图中显示或隐藏解决方案或观测数据的轨迹点。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Fix Track Center</td>
      <td></td>
      <td>将轨迹点固定在绘图区域的中心。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Fix Track Horizontal</td>
      <td></td>
      <td>在绘图中水平固定轨迹点。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Fix Track Vertical</td>
      <td></td>
      <td>在绘图中垂直固定轨迹点。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Show Map Image</td>
      <td></td>
      <td>显示或隐藏地图图像。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Show Path/Waypoints</td>
      <td></td>
      <td>显示或隐藏地图路径数据。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Animation Start</td>
      <td></td>
      <td>开始绘图的动画效果。</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td>Animation Stop</td>
      <td></td>
      <td>停止绘图的动画效果。</td>
      <td></td>
    </tr>
    <!-- Help -->
    <tr>
      <td></td>
      <td>About...</td>
      <td></td>
      <td>显示“About...”对话框</td>
      <td></td>
    </tr>
  </tbody>
</table>

## 3.8 使用RTKPLOT查看和绘制观测数据

RTKPLOT 也用于查看和绘制 RINEX 观测数据以及进行卫星可见性分析。

（1）要绘制 RINEX 观测数据，请执行 RTKPLOT 的菜单“文件”--“打开观测数据”，并选择 RINEX 观测文件和导航电文文件。您可以选择多个 RINEX 文件。如果您没有选择 RINEX 导航电文文件，RTKPOST 会读取将扩展名替换为 .*nav（.obs）或 .yyN、.yyG、.yyH、.yyQ、.yyL、.yyP（.yyO）的观测数据文件路径，作为 GPS、GLONASS、SBAS、QZSS、Galileo 或组合导航数据。如果您想单独读取 RINEX 导航电文文件，请执行菜单“文件”--“打开导航电文”。如果输入文件能够正确读取，您可以看到一个卫星可见性图。您可以使用左键或右键拖动来改变时间跨度。您也可以使用一些工具栏按钮，就像在解决方案绘图中一样。

![Satellite Visibility Plot of RTKPLOT](https://i.ibb.co/jgrkv0q/image.png)

（2）通过选择工具栏中的下拉菜单，您可以将图形切换为天空图中的卫星可见性（SkyPlot）或可见卫星数量的图形，即DOP/NSat图形（DOP/NSat），信噪比/多径/仰角图形（SNR/MP/EL）以及信噪比/多径 - 仰角图形（SNR/MP-EL）。要显示信噪比图形，观测数据必须包含信噪比（C/N0）信息。要显示正确的多径图形，观测数据必须包含双频（GPS、GLONASS和QZSS为L1-L2，伽利略为L1-L5，北斗为L2-L7）的伪距和载波相位。RTKPLOT 内部使用这些观测值生成 MP LC（线性组合）以绘制多径图。为了计算卫星的仰角，RTKPLOT 还需要卫星的位置信息。当输入 RINEX 导航文件时，卫星位置是通过使用 RINEX 导航文件中的卫星星历来计算的。如果无法获取 RINEX 导航文件，RTKPLOT 会使用 NORAD TLE（两行元素）数据集来确定卫星位置。在这种情况下，您必须在“选项”对话框中以纬度、经度和高度的形式指定接收机位置。您可以通过工具栏上方的下拉菜单选择信号和卫星。在卫星选择中，“G”“R”“E”“J”“C”和“S”分别代表 GPS、GLONASS、伽利略、QZSS、北斗和 SBAS。

![Skyplot by RTKPLOT](https://i.ibb.co/cyCFYNY/image.png)

![# of Visible Satellites and DOP Plot by RTKPLOT](https://i.ibb.co/YT8HxdL/image.png)

![SNR/Multipath Plot by RTKPLOT](https://i.ibb.co/nR8Qr1S/image.png)

![SNR/Multipath - Elevation Plot by RTKPLOT](https://i.ibb.co/Qn5x38G/image.png)

（3）您可以通过菜单“文件” - “打开仰角掩码”读取仰角掩码数据，并设置选项“仰角掩码模式”为“开”来应用仰角掩码。关于仰角掩码数据的格式，请参考位于`<install dir>\rtklib_<ver>\data\elmask_sample.txt`的示例文件。

![Elevation Mask in Skyplot by RTKPLOT](https://i.ibb.co/27pW7sh/image.png)

（4）通过执行菜单“编辑” - “观测数据源”或“观测数据质量控制”，您可以以文本形式查看解决方案的来源或质量控制结果。

![QC Result View by RTKPLOT](https://i.ibb.co/tHP90RV/image.png)

（5）在2.4.2版本中，新增了卫星可见性分析功能。借助北美航空航天防御司令部（NORAD）的两行元素（TLE）数据集，您可以使用RTKPLOT随时随地预测全球导航卫星系统（GNSS）卫星的可见性。要启用此功能，请通过“选项”对话框设置TLE数据和卫星编号文件的路径。以下图表展示了这些文件的示例。

![Examples of TLE Data (upper) and Satellite Number File (lower)](https://i.ibb.co/68DBK50/image.png)
![Examples of TLE Data (upper) and Satellite Number File (lower)](https://i.ibb.co/Ln5rcwn/image.png)

NORAD TLE数据文件可以在CelesTrak（http://celestrack.com）或SpaceTrack（http://www.space-track.org）免费获取。由于网络原因，上述网页的解析并未成功，这可能与链接本身有关，也可能与网络连接有关。如果您需要访问这些网页，请检查链接的合法性，并尝试重新加载网页。如果您下载TLE数据时遇到问题，建议稍后再试或检查网络连接。
您需要下载包含目标GNSS卫星轨道元素的TLE数据。对于所有GNSS卫星，最好使用SpaceTrack提供的“完整目录”，其中包含了所有可用卫星的信息。为了获得更准确的卫星位置，建议使用更新的TLE数据。另一个卫星编号文件用于将TLE卫星编号转换为GNSS卫星编号，如G23、R03、139等。一个示例卫星编号文件可以在`<install dir>\rtklib_<ver>\data\TLE_GNSS_SATNO.txt`找到。请注意，由于系统星座变化，GNSS卫星编号与卫星的对应关系有时会发生变化。在这种情况下，您需要自行编辑和修改卫星编号文件。为了预测GNSS卫星的可见性，通过“选项”对话框设置用户位置，包括纬度、经度和高度。您需要将“接收机位置”设置为“纬度/经度/高度”。然后执行菜单“文件” - “可见性分析...”，您将看到“时间跨度/间隔”对话框。

![Time Span/Interval Dialog of RTKPLOT](https://i.ibb.co/d42mhVk/image.png)

通过该对话框，设置“开始时间”和“结束时间”为GPS时间，“间隔”用于分析，然后点击“确定”按钮。您将获得指定位置的卫星可见性图表。在图表中，默认情况下，颜色表示卫星系统，如GPS、GLONASS、伽利略、QZSS、北斗和SBAS。您可以像观测数据图表一样，将图表切换为天空图（Skyplot）、DOP/NSat或SNR/MP/EL。

![Satellite Visibility (Predicted) Plot by RTKPLOT](https://i.ibb.co/KKS1L3s/image.png)

![Skyplot (Predicted) by RTKPLOT](https://i.ibb.co/fp8S8Rj/image.png)

（6）关于RTKPLOT用于解算数据绘图的选项以及RTKPLOT提供的菜单，请参阅3.7节 使用RTKPLOT查看和绘制解算数据。

## 3.9 通过RTKGET下载GNSS产品和数据

对于PPP（精密单点定位）分析，您通常需要下载IGS（国际GNSS服务）的精密GNSS产品，如卫星轨道和钟差信息。在其他情况下，您可能想要通过互联网从GNSS数据档案中下载CORS（连续运行参考站）网络的观测数据。为了下载这些GNSS相关的产品和数据，RTKLIB提供了一个有用的图形用户界面下载工具AP RTKGET。RTKGET是在2.4.2版本中新增的功能。

（1）执行二进制AP文件`<install dir>\rtklib_<ver>\bin\rtkget.exe`。您将看到RTKGET的主窗口。

![Main Window of RTKGET](https://i.ibb.co/vsg9qfN/image.png)

（2）首先，您需要配置GNSS数据的URL列表文件。在主窗口中点击“选项...”按钮。您将看到“选项”对话框。

![Options Dialog of RTKGET](https://i.ibb.co/VwqNysJ/image.png)

（3）填写“GNSS数据的URL列表文件”的文件路径，或者通过点击“...”来选择文件。关于文件格式，请参考附录B.5。GNSS数据的URL列表文件示例可以在`<install dir>\rtklib_<ver>\data\URL_LIST.txt`找到。如果您留空该字段，RTKGET将使用默认的URL列表文件`<install dir>\rtklib_<ver>\data\URL_LIST.txt`。

（4）在主窗口中通过左侧的列表框选择下载数据类型。您可以在列表中选择多种数据类型。使用列表框上方的组合框，可以根据数据类型类别过滤列表。下载数据的URL地址显示在第一个消息区域的中央。本地目录也显示在第二个消息区域。

![Selection of Download Data Type by RTKGET](https://i.ibb.co/LJ3yzDL/image.png)

（5）使用“开始”、“结束”和“间隔”字段以GPS时间指定时间跨度。下载数据URL地址中的时间关键字将被指定的时间跨度中的时间替换。如果URL地址包含关键字%N，则该关键字将被“编号”字段中的数字替换。要指定一系列数字，可以在“编号”字段中使用“-”来表示范围，例如“1-99”。

（6）如果URL地址包含关键字%s、%S或%r，则这些关键字将被右侧列表框“站点”中选定的站点名称替换。要选择站点名称，请点击主窗口右上角的“...”按钮。您将看到“站点”对话框。您可以在该对话框中输入或编辑站点名称列表。站点名称列表中的每一行表示一个将用于替换URL地址中关键字的站点名称。要从外部文本文件加载站点名称列表，请点击“加载...”按钮来选择文件。您可以通过点击“保存...”按钮将站点名称列表保存到外部文本文件。外部文本文件是一个包含站点名称文本行的简单文本文件。在输入或编辑站点名称列表后，点击“确定”按钮关闭对话框，并将站点名称列表设置到主窗口。

![Stations Dialog of RTKGET](https://i.ibb.co/7rk2GMZ/image.png)

（7）如果是通过FTP下载，需要设置“FTP登录”和“密码”以登录FTP服务器。对于大多数匿名FTP服务器，您可以使用“anonymous”和您的电子邮件地址来填写这些字段。然后检查或取消检查以下下载选项：<br>
(a) 跳过现有文件：设置如果本地文件已存在，是否跳过下载。<br>
(b) 解压/解包文件：设置如果下载的文件是压缩的，是否对其进行解压或解包。<br>
(c) 本地目录：检查并设置下载文件的本地目录路径。点击“...”通过“目录选择”对话框来选择目录。如果未勾选，则使用URL列表文件中的默认下载目录。目录路径可以包含关键字，这些关键字将被日期、时间、站点名称和环境变量替换，就像URL列表中的替换一样。点击“?”按钮以显示目录路径中详细的替换内容。

![Keyword Replacement Dialog of RTKGET](https://i.ibb.co/qjgV1g4/image.png)

（8）在“站点”列表框中选择站点名称。点击“A”按钮以选择所有站点，点击“D”按钮以取消选择所有站点。

（9）点击“下载”按钮开始下载。第三个消息区域显示每个文件的下载状态指示器。指示器“_”表示正在进行，“o”表示下载成功，“.”表示已跳过，“x”表示无数据，“X”表示下载出错。若想中途终止下载，点击“终止”按钮。即使点击了该按钮，正在进行的最后一次下载也无法取消。请稍等片刻，待最后一次下载完成后再终止。

![Data Downloading by RTKGET](https://i.ibb.co/kMwqqVd/image.png)

（10）完成所有下载或终止下载后，点击主窗口中的“文件...”按钮，以执行显示已下载本地目录的Windows资源管理器。要查看下载日志，点击“日志...”按钮。您可以在“文本查看器”窗口中看到下载日志。要启用下载日志功能，您需要在“选项”对话框中设置“下载日志文件”字段。如果为下载日志勾选了“追加”，则日志将添加到现有的日志文件中（如果存在）。如果没有勾选，则每次下载尝试时都会新生成下载日志。

![Download Log View by RTKGET](https://i.ibb.co/crxnF20/image.png)

（11）在实际下载数据之前，测试本地文件中GNSS数据的存在性，设置完成后（与上述实际下载设置相同），点击“测试...”按钮。您可以在“文本查看器”窗口中看到本地数据可用性报告。通过点击“保存...”按钮并指定文件保存对话框中的文件路径，您可以保存该报告。本地数据可用性报告的列数和日期格式可以通过“选项”对话框进行修改。

![Local File Test View by RTKGET](https://i.ibb.co/BP85z6L/image.png)

（12）默认情况下，下载进度和错误的详细日志会自动删除。若要保留此类错误日志，请在“选项”对话框中勾选“下载错误时保留错误信息”。每个下载文件的详细错误日志将被保存为<本地目录>\<文件名>.err文件。为了便于分析问题，也可以勾选“保留远程目录列表”。远程文件列表将被保存为当前目录下的.listing文件。

## 3.10 NTRIP列表

NTRIP（通过互联网协议传输RTCM的网络化传输）是一种用于交换GPS/GNSS相关数据的通信协议，例如接收机原始观测数据、星历以及用于差分GPS或RTK-GPS的修正数据。NTRIP规定了所谓的源表（Source Table）的表格格式，该表表示NTRIP服务器提供的数据内容列表。RTKLIB包含一个用于浏览NTRIP源表的简单浏览器。

（1）执行二进制AP文件`rtklib_<ver>\bin\srctblbrows.exe`。您将看到NTRIP源表浏览器的主窗口。

![Main Window of NTRIP Browser](https://i.ibb.co/1rzZbhp/image.png)

（2）点击主窗口左上角的按钮，同时保持右侧的NTRIP广播器列表下拉菜单为空。如果底部状态栏显示“正在连接...”，然后显示“更新广播器列表”，则NTRIP广播器列表已更新。如果下拉菜单为空，浏览器将从默认的NTRIP信息广播器rtcm-ntrip.org:2101获取NTRIP广播器列表并更新该列表。要更改列表的来源，请在下拉菜单中填写NTRIP广播器地址，格式为<地址>:<端口>，然后点击按钮。如果您省略端口号，浏览器将使用默认端口号2101。

（3）在下拉菜单中选择广播器并点击按钮。如果状态栏显示“已接收源表”，则浏览器已成功从选定的NTRIP广播器接收了一个NTRIP源表，并在窗口中显示它。如果出现问题，状态栏也会显示错误信息。

![NTRIP Stream List View by NTRIP Browser](https://i.ibb.co/pPBDXn8/image.png)

（4）通过点击字段标题，您可以按照该字段列对列表进行排序。您还可以点击STR、CAS、NET、SRC，以在流列表、广播器列表、网络列表和原始源表之间切换源表的内容。

![NTRIP Source Table View by NTRIP Browser](https://i.ibb.co/xYjwhSM/image.png)

（5）通过点击工具栏中的MAP按钮，您可以使用谷歌地图打开NTRIP挂载点位置的地图视图。在地图视图中点击标记，可以显示挂载点的详细信息。通过在NTRIP浏览器窗口中选择一个挂载点，可以通过改变标记的颜色（红色）来在地图上指示该挂载点的位置。

![Map View of Station Positions by NTRIP Browser](https://i.ibb.co/X59hsj3/image.png)

## 3.11 RTKLIB CUI程序

RTKLIB包含了以下命令行界面（CUI）应用程序。这些CUI应用程序仅使用标准ANSI C（C89）函数和少量标准C库，以确保其可移植性。因此，您可以在非Windows环境，如LINUX、UNIX、MAC OS X等上构建这些CUI应用程序。您也可以在嵌入式CPU，如ARM上构建和执行这些应用程序。关于这些应用程序（包括命令行选项）的详细信息，请参考附录A CUI命令参考。关于构建这些应用程序的步骤，请参考4.2节 RTKLIB的CUI应用程序构建。请注意，您可能需要修改makefile以适应构建环境，但在大多数情况下，程序本身无需修改即可移植。

(1) RTKRCV <br>
Real-time Positioning. The console AP version of RTKNAVI.

(2) RNX2RTKPV <br>
Post-Processing Analysis. The console AP version of RTKPOST.

(3) POS2KMLV <br>
Google Earth KML converter for solution files.

(4) CONVBINV <br>
RINEX Converter of receiver raw data. The console AP version of RTKCONV.

(5) STR2STRV <br>
Stream Server. Console AP version of STRSVR.
