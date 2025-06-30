# 3. 操作指南

## 3.6 RTKCONV：数据格式转换
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