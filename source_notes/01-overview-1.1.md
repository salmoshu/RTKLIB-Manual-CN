# 1. 初识RTKLIB

## 1.1 简介

RTKLIB是一个用于全球导航卫星系统（GNSS）标准定位和精密定位的开源项目，由日本东京海洋大学的高须知二（Tomoji Takasu）开发。它由一个核心程序库和多个命令行程序与界面程序组成。

### 1.1.1 应用方向

RTKLIB 可以初步实现以下功能，与商业软件相比，可靠性没那么高，但对于部分科研已经能够满足：

| 功能 | 说明 |
| --- | --- |
| **静态短基线解算**         | 相对定位，例如将一个测站安装在比较稳定的地区，而将另一测站安装在目标区域进行变形监测。 |
| **动态后处理差分 PPK**     | 例如无人机遥感、倾斜摄影测量等，需要高精度的位置和姿态解算精度。 |
| **实时动态差分 RTK**       | 例如车载导航定位和室外机器人定位等。 |
| **精密单点定位 PPP**       | 可以用来解算基准站坐标，可以进行精密定轨、电离层对流层建模、时间传递。 |
| **实时精密单点定位 RT-PPP** | 例如接收实时的精密卫星改正数，通过接收机数据进行实时单点定位。用途比较广泛，例如在海洋上，海啸的监测预警、海平面变化的监测、船只定位、海上石油平台作业等。 |

### 1.1.2 RTKLIB组成

RTKLIB的项目结构如下所示：

![RTKLIB](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/RTKLIB.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1-1 RTKLIB 项目结构</p>

RTKLIB 界面程序包括以下内容，可以直接使用编译好的程序，也可以用作者提供的 Qt 和 C++ builder 两套界面程序的源码自己编译。

![](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250707-184507.jpg)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1-2 RTKLAUNCH 主界面与应用程序图标</p>

* **rtklaunch**：界面程序启动器，界面如上，用来启动 RTKLIB 的主要 GUI 应用程序；
* **rtkget**：下载 GNSS 数据，包括 OBS、EPH、ATX、CLK、DCB 等多种文件，可同时下载起止时间内多个机构、多个测站的数据，不过下载速度可能较慢；
* **rtkcov**：GNSS 数据格式转换，把采集的接收机原始数据转成 RINEX；
* **rtkplot**：原始数据绘图与解算结果绘图，可以用来进行原始数据质量分析、结果精度分析、结果轨迹绘图；
* **rtkpost**：后处理定位解算，传入原始观测文件、星历文件和其它改正信息文件，设置好解算选项进行后处理操作；
* **rtknavi**：实时定位解算，接通导航数据流，实时定位解算绘图；
* **strsvr**：数据流的连接、转换与播发；
* **srctblbrows**：NTRIP 资源列表浏览器。

命令行程序的功能和界面程序功能基本对应。界面程序便于使用，命令行程序代码则便于学习与二次开发；可以通过界面程序学软件的用法，理解程序运行逻辑。然后再通过阅读命令行程序的源码，来更深入地理解。命令行程序还有一个优点就是便于通过脚本进行批处理。

* **rnx2rtkp**：后处理定位解算，功能类似 rtkpos。
* **rtkrcv**：实时定位解算，功能类似 rtknavi。
* **str2str**：数据流的连接、转换与播发，功能类似 strsvr。
* **convbin**：数据格式转换，功能类似 rtkcov。
* **pos2kml**：定位结果转谷歌地图数据格式。
