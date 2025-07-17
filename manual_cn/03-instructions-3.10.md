# 3. 操作指南

## 3.10 SRCTBLBROWS：NTRIP资源列表

NTRIP（通过互联网协议传输RTCM的网络化传输）是一种用于交换GPS/GNSS相关数据的通信协议，例如接收机的原始观测数据、星历以及用于差分GPS或RTK-GPS的修正信息。NTRIP规定了所谓的“源表”（Source Table）的表格格式，该表列出了NTRIP服务器提供的数据内容清单。RTKLIB包含了一个简单的NTRIP源表浏览器。

### 3.10.1 软件执行

执行二进制AP文件`rtklib_<ver>\bin\srctblbrows.exe`。您将看到NTRIP源表浏览器的主窗口。

![Main Window of NTRIP Browser](https://i.ibb.co/1rzZbhp/image.png)

点击主窗口左上角的 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132038.jpg"/> 按钮，同时保持右侧的NTRIP caster列表下拉菜单为空白。如果底部状态栏显示“connecting...”，随后显示“update caster list”，则表示NTRIP caster列表已更新。如果下拉菜单为空白，浏览器将从默认的NTRIP caster`rtcm.ntrip.org:2101`获取NTRIP caster列表并更新该列表。若要更改列表的来源，请在下拉菜单中填写NTRIPcaster地址，格式为`<address>:<port>`，然后点击 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132038.jpg"/> 按钮。如果省略端口号，浏览器将使用默认端口号2101。

在下拉菜单中选择caster并点击<img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-151557.jpg"/>按钮。如果状态栏显示“source table received”，则浏览器已成功从选定的NTRIP caster接收了一个NTRIP源表，并在窗口中显示它。如果出现问题，状态栏也会显示错误信息。

![NTRIP Stream List View by NTRIP Browser](https://i.ibb.co/pPBDXn8/image.png)

通过点击字段标题，您可以按照该字段列对列表进行排序。您还可以点击STR、CAS、NET、SRC，以在流列表、caster列表、网络列表和原始源表之间切换源表的内容。

![NTRIP Source Table View by NTRIP Browser](https://i.ibb.co/xYjwhSM/image.png)

### 3.10.2 地图选址

通过点击工具栏中的MAP按钮，您可以使用谷歌地图打开NTRIP挂载点位置的地图视图。在地图视图中点击标记，可以显示挂载点的详细信息。通过在NTRIP浏览器窗口中选择一个挂载点，可以通过改变标记的颜色（红色）来在地图上指示该挂载点的位置。

![Map View of Station Positions by NTRIP Browser](https://i.ibb.co/X59hsj3/image.png)

<GiscusTalk />
