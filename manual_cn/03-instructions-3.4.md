# 3. 操作指南

## 3.4 RTKPOST：后处理分析

RTKLIB包含后处理分析软件 RTKPOST。RTKPOST能够输入标准的RINEX格式（如3.02）的观测数据文件和导航电文文件（支持GPS、GLONASS、伽利略、QZSS、北斗和SBAS），并可以通过多种定位模式计算定位解，包括单点定位、DGPS/DGNSS、DGPS/DGNSS、Kinematic、Static、PPP-Kinematic 和 PPP-Static。

### 3.4.1 软件执行

![Main Window of RTKPOST](https://i.ibb.co/sRjkHVn/image.png)

打开可执行文件`<install-dir>\rtklib_<ver>\bin\rtkpost.exe`。您可以看到RTKPOST的主窗口。在完成文件路径与参数配置后，点击 <span style="border: 1px solid black; padding: 3px;">Execute</span> 按钮开启后处理程序，程序的处理状态会显示在主窗口中央下方。当在这里出现“done”消息时，程序就运行完成了。如果想在途中停止处理，请按 <span style="border: 1px solid black; padding: 3px;">Abort</span> 按钮。

### 3.4.2 观测文件输入

在“RINEX OBS(: Rover)”文本框中输入流动站接收机的RINEX观测数据文件路径。可以通过手动填写文件路径，或者点击 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮调出文件选择对话框来选择文件。您可以使用经过GZIP <sup>[57]</sup>（.gz 或 gz）、COMPRESS（.z 或 z）或Hatanaka Compression <sup>[58]</sup>（.yy dd）压缩的RINEX观测数据文件。系统会根据文件扩展名识别文件是否经过压缩。如果文件路径中包含通配符（**），则会自动展开通配符并读取多个文件。

如果您在相对定位模式下处理RINEX数据，如DGPS/DGNSS、Kinematic、Static、Moving-Base或Fixed模式，那么除了流动站观测数据文件之外,您需要在“RINEX OBS: Base Station”字段中输入基准站接收机的第二个文件路径。

### 3.4.3 导航电文输入

您还需要在“RINEX \*NAV/CLK, SP3, IONEX 或 SBS/EMS”字段中输入GPS、GLONASS、伽利略、QZSS和SBAS的RINEX导航电文文件路径。如果您未填写第一和第二字段，则会使用将观测数据文件扩展名替换为.*nav（.obs obs）或. yyN、.yyG、.yyH、.yyQ和. yyP（.yyO yyO）的文件路径，作为GPS、GLONASS、伽利略、QZSS、北斗和SBAS的导航电文文件。如果文件路径中包含通配符（\*），则会自动展开通配符并像使用观测数据文件一样使用多个文件。

为了在PPP-Kinematic、PPP-Static或PPP-Fix模式下使用精密星历和钟差，您可以在该字段中输入一个SP3-c（精密卫星星历和钟差）或RINEX CLK（精密卫星钟差）文件路径。您可以输入一个IONEX 1.0文件路径以用于电离层VTEC网格修正。对于SBAS修正，您可以输入一个RTKLIB格式或EMS（EGNOS消息服务器）2.0格式的SBAS消息日志文件路径。您也可以在这些文件路径中包含通配符（*）。通配符会被展开，并使用多个文件。

您还可以在输入文件字段中输入以RTCM 3消息格式表示的SSR（状态空间表示）修正。这些输入文件的格式将根据其扩展名被识别，如下所示：

- .sp3，.SP3，.eph，.EPH：SP3-c精密星历文件<sup>[22]</sup>
- .sbs，.SBS，ems，.EMS：SBAS消息日志文件（附录B.2和<sup>[26]</sup>）
- .rtcm3，.RTCM3：RTCM 3 SSR校正消息文件<sup>[18]</sup>
- *i、 *I：IONEX VTEC网格数据文件<sup>[24]</sup>
- 其他：RINEX OBS、NAV或CLK（自动识别）<sup>[9]-[15]</sup>

### 3.4.4 输出结果文件

在文本字段“Solution”中输入运算结果文件的路径。该字段会自动按照第一个输入文件名扩展为.pos或.nmea文件。如果选中“Dir”并填写该字段，则文件将指定目录输出。

### 3.4.5 配置选项

点击 <span style="border: 1px solid black; padding: 3px;">Options...</span> 按钮设置算法配置和一些软件选项。有关RTKPOST配置的详细信息，请参阅3.5节。

![Time Dialog of RTKPOST](https://i.ibb.co/GxTzccm/image.png)

您可以在主窗口中的“Time Start（GPST）”或“Time End（GPST）”字段来设置开始与结束时间。您还可以通过“Interval”字段来设置程序运行的时间间隔。参考 <span style="border: 1px solid black; padding: 3px;">?</span> 按钮，您可以查看GPS时间锁对应的UTC时间、GPS Week/Tow、年某天、星期、某一天的具体时间和闰秒。

如果您同时选中“Time Start”和“Time End”字段，则可以选中以小时为单位的“Unit”选项进行多会话（session）分析，具体来说，如果设置了“Unit”字段，那么程序将按指定的单位时间分为多个会话。为了避免覆盖之前的输出文件，输出文件路径必须包含根据会话时间替换的关键字。有关输入或输出文件路径中关键字替换的详细信息，请参考3.5节。

### 3.4.6 文本查看器

![Text Viewer showing Solutions by RTKPOST](https://i.ibb.co/8jKTgXz/image.png)

程序运行完成后，通过 <span style="border: 1px solid black; padding: 3px;">View...</span> 按钮查看输出文件的内容。您可以通过按下“文本查看器”窗口中的🔁按钮重新加载输出文件。您可以通过点击 <span style="border: 1px solid black; padding: 3px;">Options...</span> 来配置“文本查看器”。您还可以使用 <span style="border: 1px solid black; padding: 3px;">Find</span> 按钮在文本中搜索关键字。

### 3.4.7 Plot可视化

![RTKPLOT Window executed by RTKPOST](https://i.ibb.co/rsGp2n9/image.png)

点击 <span style="border: 1px solid black; padding: 3px;">Plot...</span> 按钮，可以使用RTKPLOT绘制解算结果。详情请参考3.7节。

### 3.4.8 Google Earth文件转换

![Google Earth Converter Dialog of RTKPOST](https://i.ibb.co/2k7YR7P/image.png)

通过 <span style="border: 1px solid black; padding: 3px;">KML/GPX...</span> 按钮，输出文件可以通过“KML/GPX”对话框转换为谷歌地球KML文件。设置好相应的配置，然后按对话框中的 <span style="border: 1px solid black; padding: 3px;">Convert</span> 按钮。您可以通过按下Google Earth按钮，使用生成的KML/KMZ文件启动Google Earth。要指定Google Earth执行文件，请配置“Options”-“Files”-“Google Earth Exe File”。

### 3.4.9 观测数据可视化

通过主窗口中的观测数据输入框旁边的 <span style="border: 1px solid black; padding: 3px;">🌐</span> 按钮，您可以通过RTKPLOT对观测数据进行可视化分析。您还可以通过按下 <span style="border: 1px solid black; padding: 3px;">📄</span> 按钮，使用文本查看器显示输入文件的内容。

### 3.4.10 跟踪与调试

如果想输出解算结果的统计信息或调试跟踪，请按主窗口左下角的 <span style="border: 1px solid black; padding: 3px;">📄</span> 按钮查看解决方案统计文件和调试跟踪文件。为了在结果异常的情况下检查处理异常或警告，需要将“Options”-“Output”对话框中的“Output Debug Trace”设置为“Level2”（跟踪错误和警告），并查看输出调试跟踪文件。
