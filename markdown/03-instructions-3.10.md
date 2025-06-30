# 3. 操作指南

## 3.10 SRCTBLBROWS：NTRIP资源列表

demo5中的名称为srctblbrows。

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