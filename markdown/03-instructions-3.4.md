# 3. 操作指南

## 3.4 RTKPOST：后处理分析

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