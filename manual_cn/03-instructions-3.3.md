# 3. 操作指南

## 3.3 STRSVR：数据流处理

当RTKNAVI以相对定位运行时（如RTK），Rover和Base通常来自不同地方。在一些情况下，用户可能会距离这些接收机较远的地方使用定位结果。为了将这些接收机互连，用户必须建立合适的数据通信链路。RTKLB提供了一个流处理工具STRSVR，用户可以通过STRSVR设置系统的输入、输出数据流。STRSVR还具有中继或数据流分割的功能，以便与RTKNAVI协同进行实时定位。

例如，为了在Rover接收机上接收远程基站的观测数据以进行RTK相对定位，用户可以通过STRSVR远程连接到Base接收机。以下示例展示了RTKNAVI和STRSVR的典型应用。

### 3.3.1 应用示例

**1. 单点定位（1）**

![alt text](https://i.ibb.co/3NQgy9q/image.png)

RTKNAVI运行单点定位模式，并将结果存储到文件中。

**2. 单点定位（2）**

![alt text](https://i.ibb.co/T11FmRQ/image.png)

RTKNAVI运行单点定位模式，将结果输出到串行设备，同时将Rover的原始数据记录到Log文件中。

**3. RTK定位（1）**

![alt text](https://i.ibb.co/qxSvcGf/image.png)

RTKNAVI运行RTK定位模式，从两个串行设备输入Rover和Base数据，并将解算结果保存到文件中。

**4. RTK定位（2）**

![alt text](https://i.ibb.co/rybzVBQ/image.png)

RTKNAVI运行RTK定位模式，Rover数据来自串口，而Base数据则是通过无线网络获取自远端的TCP服务器。

**5. RTK定位（3）**

![alt text](https://i.ibb.co/M28V2VC/image.png)

RTKNAVI运行RTK定位模式，通过串口输入Rover数据，并通过4G或5G网络连接输入Base数据。

**6. RTK定位（4）**

![alt text](https://i.ibb.co/qn25hZZ/image.png)

RTKNAVI运行RTK定位模式，从串口输入Rover数据和通过互联网上的NTRIP广播服务器输入Base数据。

**7. RTK定位（5）**

![alt text](https://i.ibb.co/XsgjswK/image.png)

RTKNAVI运行RTK定位模式，通过互联网提供NRTK（网络RTK）服务。

**8. RTK定位（6）**

![alt text](https://i.ibb.co/Ykvkb95/image.png)

单个NRTK服务多个RTK。

**9. PPP定位**

![alt text](https://i.ibb.co/HDGFzCq/image.png)

RTKNAVI运行实时PPP定位模式，实时精密卫星轨道和时钟数据通过NTRIP流的形式提供。

**10. 长基线RTK**

![alt text](https://i.ibb.co/KmN9WL2/image.png)

长基线RTK与FTP下载精密星历。

### 3.3.2 STRSVR操作说明

**1. 软件执行**

执行可执行文件`<install-dir>\rtklib_<ver>\bin\strsvr.exe`。您可以看到STRSVR的主窗口。
![alt text](https://i.ibb.co/KsTz2zm/image.png)

**2. 配置输入流**

要配置输入流，请在“（0）input”处使用下拉菜单选择流类型。可选择的流类型有Serial、TCP Client、TCP Server、NTRIP Client或File。可以设置流选项或启动/关闭命令，以及RTKNAVI的输入流。

**3. 配置输出流**

要配置输出流，请使用下拉菜单在（1）输出、（2）输出或（3）输出处选择流类型。输出流的设置为串行、TCP Client、TCP Server、NTRIP Server、NTRIP Caster或File。

**4. 流格式转换**

在2.4.2版本中，添加了流格式转换功能。要使用该功能，请按Conv输出流下拉菜单右侧的按钮。您可以看到“转换选项”对话框。要启用流格式转换功能，请选中对话框左上角的复选框，然后通过下拉菜单选择输入和输出格式。当前版本支持以下输入和输出格式。

**（a）输入：** RTCM3、RTCM2、NovAtel OEM6、NovAtel-OEM3、u-blox、Superstar II、Hemisphere、SkyTraq、GW10、Javad、NVS BINR和BINEX

**（b）输出：** RTCM 3（RTCM 2尚不支持）

![alt text](https://i.ibb.co/0XHSCWH/image.png)

输出消息应在对话框的消息类型字段中指定为以下形式：
```html
nnnn(ss), nnnn(ss), nnnn(ss), ....
```
指定消息类型为字段 `nnnn`，消息间隔为字段（单位为秒）。消息间隔可以省略。在这种情况下，消息间隔由输入消息间隔决定。下表列出了所有支持的输出 RTCM 消息。对于天线信息消息，字段由“选项”对话框给出。消息输入选项也可以在“选项”字段中指定。有关接收机依赖选项的详细信息，请参考 D.5。

![alt text](https://i.ibb.co/1f102ST/image.png)

**5. STRSVR状态**

在主窗口中点击“开始”按钮。通信状态会在主窗口正中央下方的消息区域中显示。主窗口左侧的状态指示灯也会显示通信状态。指示灯的颜色含义如下：

- <span style="color: orange;">橙色</span>：等待连接；
- <span style="color: darkgreen;">深绿色</span>：已连接；
- <span style="color: lightgreen;">浅绿色</span>：数据活动；
- <span style="color: red;">红色</span>：错误。

输入和输出数据流的总数据量（字节数）和数据速率（bps）也会在右侧显示。

**6. STRSVR选项**

通过点击 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮，您可以通过“选项”对话框设置通信选项。如果要向连接到输入流的服务器发送 NMEA GPGGA 消息，请勾选“NMEA 请求周期”，并设置请求周期（单位为毫秒）以及消息中的纬度/经度。如果需要通过防火墙内的 HTTP 代理服务器连接外部 NTRIP Caster，您可以在“HTTP/NTRIP 代理”字段中以 `<address>：<port>` 的形式输入地址和端口号。对话框中的其他字段用于在使用格式转换功能以及发送天线信息消息时生成天线和基站信息消息。

![alt text](https://i.ibb.co/b1z0Kv9/image.png)
