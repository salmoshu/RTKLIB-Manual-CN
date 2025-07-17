---
sidebarDepth: 2
---

# 4. 应用构建

## 4.1 重新构建Windows应用

为了修改RTKLIB中的一些功能，或者将您的新模型集成到RTKLIB中，您可能需要重新构建应用程序。此外，为了修复现有漏洞，您也可能需要重新构建应用并应用补丁。如果要在Windows上重新构建图形用户界面（GUI）和命令行界面（CUI）的应用程序，您需要使用 [Embarcadero C++ Builder](http://www.embarcadero.com) 或 [Visual Studio](https://visualstudio.microsoft.com) 。

demo5作者在最新的RTKLIB版本中提供了组织好的Visual Studio项目文件，位于 `<install dir>\rtklib_<ver>\app\<app> or <install dir>\rtklib_<ver>\app\consapp\<app>\msc` 中。而下面展示的是如何在Windows上通过Embarcadero重新构建GUI和CUI应用程序。

1.&nbsp;执行Embarcadero C++ Builder。

2.&nbsp;执行C++ Builder中的菜单“File” - “Open Project...”，以打开目标应用程序的C++ Builder项目文件（`<app>.cbproj`或`_<app>.cbproj`，其中`<app>`是目标应用程序，如rtkpost、rtkplot或rtknavi），该文件位于应用程序目录中（`<install dir>\rtklib_<ver>\app\<app> or <install dir>\rtklib_<ver>\app\<app>\bcc\`）。

3.&nbsp;执行C++ Builder中的菜单“Project” - “Rebuild`<app>`”来重新构建目标应用程序。

4.&nbsp;执行（双击文件或输入命令）与项目文件同一目录下的Windows批处理文件install.bat。它会将新构建的可执行二进制程序复制到RTKLIB二进制程序目录（`<install dir>\rtklib_<ver>\bin`）。

5.&nbsp;要重新构建所有的GUI应用程序或CUI应用程序，打开C++ Builder组项目文件`<install dir>\rtklib_<ver>\app\rtklib_winapp.gourppro`j或`<install dir>\rtklib_<ver>\app\rtklib_consapp.gourpproj`。执行C++ Builder中的菜单“项目” - “构建所有项目”。执行同一目录下的批处理文件install_winapp.bat或install_consapp.bat，将它们复制到二进制程序目录。

## 4.2 编译CUI应用

要构建CUI应用程序，您可以使用许多C编译器，例如gcc。RTKLIB包中包含了适用于gcc的标准makefile。根据您的编译器、库或目录，您可能需要修改makefile，以便根据您的环境生成应用程序。

1.&nbsp;切换到目标应用程序的程序目录（`rtklib_<ver>/app/<app>`）。
```shell
cd <install_dir>/rtklib_<ver>/app/<app>
```

2.&nbsp;移动到gcc目录
```shell
cd gcc
```

3.&nbsp;编辑并修改makefile，以适应您的环境。
```shell
vi makefile
```

4.&nbsp;执行make来构建应用程序，然后执行make install将二进制文件安装到适当的BIN目录。
```shell
make
make install
```

## 4.3 开发与使用RTKLIB

RTKLIB提供了以下可从用户应用程序调用的通用C函数。用户可以使用这些函数来开发原创的定位应用程序。

- 矩阵和向量函数
- 时间和字符串函数
- 坐标转换和大地水准面模型
- 导航处理
- 定位模型（对流层、电离层、天线PCV）
- SBAS差分GPS/DGNSS修正
- 单点定位
- 基于载波和基于码的相对定位
- 实时整数模糊度解算
- 接收机原始二进制数据输入
- 定位解算/NMEA输入/输出
- RINEX观测数据/导航电文输入/输出
- 精密星历输入
- 流数据通信库
- NTRIP（通过互联网协议传输RTCM的网络化传输）库
- RTK-GPS/GNSS定位服务器
- RTCM 2.3和3.0/3.1/3.2消息处理
- 下载器功能

以下说明展示了如何在用户应用程序中利用RTKLIB的库。

1.&nbsp;在用户应用程序的源程序中添加以下包含指令。
```c
#include "rtklib.h"
```

2.&nbsp;设置以下编译器选项，将RTKLIB源代码目录路径添加到编译器的包含路径中。
```shell
-I rtklib_<ver>\src
```

3.&nbsp;将必要的RTKLIB库源文件添加到用于构建应用程序的源程序集中。关于库函数列表和RTKLIB提供的源程序，参考附录C库API。

<GiscusTalk />
