# 3. 操作指南

## 3.1 下载与使用

Tim Everett（rtkexplorer）维护的demo5版本RTKLIB可以在其 Github 相应的 [Releases](https://github.com/rtklibexplorer/RTKLIB/releases) 页面下载，发行版本页面通常会提供编译好的Windows GUI应用和源码两种选项。前者是RTKLIB包含的工具集，后者为RTKLIB源码。RTKLIB源码目录结构如下所示：

```html
/rtklib_<ver>
├── \src             : RTKLIB核心源码程序
│   ├── \rcv            : 不同厂商GPS/GNSS接收机处理程序
├── \bin             : Windows可执行程序和动态链接库
├── \data            : 示例数据
├── \app             : RTKLIB不同应用的主程序
│   ├── \rtknavi		: RTKNAVI 	(GUI)
│   └── \rtknavi_mkl	: RTKNAVI_MKL (GUI) *
│   └── \strsvr		    : STRSVR 	(GUI)
│   └── \rtkpost		: RTKPOST 	(GUI)
│   └── \rtkpost_mkl	: RTKPOST_MKL (GUI) *
│   └── \rtkplot		: RTKPLOT 	(GUI)
│   └── \rtkconv		: RTKCONV 	(GUI)
│   └── \srctblbrows 	: NTRIP Browser 	(GUI)
│   └── \rtkget	 	    : RTKGET 	(GUI)
│   └── \rtklaunch		: RTKLAUNCH 	(GUI)
│   └── \rtkrcv		    : RTKRCV 	(CUI)
│   └── \rnx2rtkp		: RNX2RTKP 	(CUI)
│   └── \pos2kml		: POS2KML 	(CUI)
│   └── \convbin		: CONVBIN 	(CUI)
│   └── \str2str		: STR2STR 	(CUI)
│   └── \appcmn		    : GUI应用的通用例程
│   └── \icon		    : GUI应用的图标
├── \lib			: 库的生成环境
└── \test			: 测试程序或数据
└── \util			: 工具集
└── \doc			: RTKLIB使用手册

* MKL应用表示链接了Intel MKL库的版本，能在多核CPU或多处理器PC上实现更快的矩阵计算
```

### 3.1.1 基本使用

RTKLIB提供了一个入门的[Demo示例](https://www.rtklib.com/rtklib_tutorial.htm)，该示例展示了RTKNAVI的使用方法与性能，不过该示例暂且只能使用较老的 [2.4.1](https://www.rtklib.com/rtklib.htm) 版本运行，因为其中提供的Rover数据格式无法被最新版本兼容。

不过最新版本RTKNAVI的使用方法与该示例中所展示的相同，所以并不影响研究。

### 3.1.2 GUI程序的INI配置

操作图形用户界面应用程序（GUI AP）时的操作与配置信息通常保存在INI文件（.ini）中，这些文件位于`<install dir>\rtklib_<ver>\bin`目录下，即RTKLIB可执行文件的同级目录。下面提供一些方便的使用技巧：

- `-i`指令。某些程序（RTKCONV、RTKPOST、RTKNAVI、RTKGET和STRSVR）可以在终端中，通过命令行选项`-i <inifile>.ini`执行，以选择不同的INI文件；
- `-t`指令。可以通过命令行选项以及`-t <title>`选项来更改窗口标题，从而切换这些应用程序的选项。

用户可以通过创建多个快捷方式（Windows 桌面快捷方式）并为每个快捷方式设置不同的命令行参数（包括 -i 和 -t），从而实现为同一个应用程序加载不同配置的目的。例如，在 GNSS 数据处理中，可能需要针对不同的任务（如静态定位、动态定位）或不同的硬件设备加载不同的参数配置。

### 3.1.3 MKL版本应用程序

为了使用MKL（链接Intel MKL库的应用，可在多核CPU或多处理器PC上实现更快的矩阵计算），请根据CPU核心数量将Windows环境变量`OMP_NUM_THREADS`设置为2、4或8。这将启用多线程矩阵计算，从而缩短处理时间。

使用 MKL 要求正确安装以及在编译环境 MKLDIR 选项中配置 Intel MKL 库，并设置 -DMKL 编译选项。

### 3.1.4 RTKLAUNCH

为了方便运行图形用户界面应用程序，2.4.2版本中新增了一个应用程序启动器RTKLAUNCH。不过由于大部分人只会关注部分应用程序，所以该启动器实际使用并不多。

要运行RTKLAUNCH，请执行`<install dir>\rtklib_<ver>\bin\rtklaunch.exe`。您可以在RTKLAUNCH窗口中点击应用程序图标，或通过任务栏图标弹出菜单来运行RTKLIB应用程序。RTKLAUNCH接受接受`-tray`选项，用于以任务栏图标的形式启动启动器。

![](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250707-184507.jpg)
<p style="text-align: center;">图3.1-1 RTKLAUNCH 主界面与应用程序图标</p> 
