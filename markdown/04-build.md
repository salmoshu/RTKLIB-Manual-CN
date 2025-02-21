---
sidebarDepth: 2
---

# 4. 应用构建

## 4.1 在Windows中重新构建 GUI 和 CUI 应用

要修改RTKLIB的一些功能或在RTKLIB中安装您的新模型，您可能需要重新构建应用程序（APs）。为了修复现有错误，您也可能需要重新构建APs以应用补丁。要在Windows上重新构建图形用户界面（GUI）和命令行界面（CUI）APs，您需要Embarcadero C++ Builder（http://www.embarcadero.com）。由于网络原因，上述网页的解析并未成功。如果您需要访问该网页，请检查链接的合法性，并尝试重新加载网页。如果您在下载或访问过程中遇到问题，建议稍后再试或检查网络连接。

RTKLIB内部仅使用C++ Builder提供的基本视觉组件库（VCL）函数。至少，基本的“启动版”就足够用来重新构建RTKLIB APs。以下说明展示了如何在Windows上重新构建GUI和CUI APs。

（1）执行Embarcadero C++ Builder。

（2）执行C++ Builder中的菜单“File” - “Open Project...”，以打开目标应用程序的C++ Builder项目文件（`<app>.cbproj`或`_<app>.cbproj`，其中`<app>`是目标应用程序，如rtkpost、rtkplot或rtknavi），该文件位于应用程序目录中（`<install dir>\rtklib_<ver>\app\<app> or <install dir>\rtklib_<ver>\app\<app>\bcc\`）。

（3）执行C++ Builder中的菜单“Project” - “Rebuild`<app>`”来重新构建目标应用程序。

（4）执行（双击文件或输入命令）与项目文件同一目录下的Windows批处理文件install.bat。它会将新构建的可执行二进制程序复制到RTKLIB二进制程序目录（`<install dir>\rtklib_<ver>\bin`）。

（5）要重新构建所有的GUI应用程序或CUI应用程序，打开C++ Builder组项目文件`<install dir>\rtklib_<ver>\app\rtklib_winapp.gourppro`j或`<install dir>\rtklib_<ver>\app\rtklib_consapp.gourpproj`。执行C++ Builder中的菜单“项目” - “构建所有项目”。执行同一目录下的批处理文件install_winapp.bat或install_consapp.bat，将它们复制到二进制程序目录。

## 4.2 编译 CUI 应用

要构建CUI应用程序，您可以使用许多C编译器，例如gcc。RTKLIB包中包含了适用于gcc的标准makefile。根据您的编译器、库或目录，您可能需要修改makefile，以便根据您的环境生成应用程序。

（1）切换到目标应用程序的程序目录（`rtklib_<ver>/app/<app>`）。
```shell
cd <install_dir>/rtklib_<ver>/app/<app>
```

（2）移动到gcc目录
```shell
cd gcc
```

（3）编辑并修改makefile，以适应您的环境。
```shell
vi makefile
```

（4）执行make来构建应用程序，然后执行make install将二进制文件安装到适当的BIN目录。
```shell
make
make install
```

## 4.3 在用户程序中开发和使用 RTKLIB

RTKLIB提供了以下可从用户应用程序（AP）调用的通用C函数。用户可以使用这些函数来开发原创的定位应用程序。

（1）矩阵和向量函数 <br>
（2）时间和字符串函数 <br>
（3）坐标转换和大地水准面模型 <br>
（4）导航处理 <br>
（5）定位模型（对流层、电离层、天线PCV） <br>
（6）SBAS差分GPS/DGNSS修正 <br>
（7）单点定位 <br>
（8）基于载波和基于码的相对定位 <br>
（9）实时整数模糊度解算 <br>
（10）接收机原始二进制数据输入 <br>
（11）定位解算/NMEA输入/输出 <br>
（12）RINEX观测数据/导航电文输入/输出 <br>
（13）精密星历输入 <br>
（14）流数据通信库 <br>
（15）NTRIP（通过互联网协议传输RTCM的网络化传输）库 <br>
（16）RTK-GPS/GNSS定位服务器 <br>
（17）RTCM 2.3和3.0/3.1/3.2消息处理 <br>
（18）下载器功能

以下说明展示了如何在用户应用程序中利用RTKLIB的库。

（1）
在用户应用程序的源程序中添加以下包含指令。
```c
#include "rtklib.h"
```

（2）
设置以下编译器选项，将RTKLIB源代码目录路径添加到编译器的包含路径中。
```shell
-I rtklib_<版本>\src
```

（3）
将必要的RTKLIB库源文件添加到用于构建应用程序的源程序集中。关于库函数列表和RTKLIB提供的源程序，参考附录C库API。
