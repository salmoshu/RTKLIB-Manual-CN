# 3. 操作指南

## 3.3 STRSVR：数据流处理

当RTKNAVI以相对定位运行时（如RTK），Rover和Base通常放在不同的位置。在一些情况下，用户可能会距离这些接收机较远的地方使用定位结果。为了将这些接收机互连，用户必须建立数据通信链路。RTKLB提供了一个流处理工具STRSVR，用户可以通过STRSVR设置系统的输入、输出数据流。STRSVR还具有中继或数据流分割的功能，以便与RTKNAVI协同进行实时定位。

例如，为了在Rover接收机上接收远程基站的观测数据以进行RTK相对定位，用户可以通过STRSVR远程连接到Base接收机。以下示例显示了RTKNAVI和SVRSVR的典型应用。

### 3.3.1 应用示例

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

### 3.3.2 STRSVR操作说明

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
