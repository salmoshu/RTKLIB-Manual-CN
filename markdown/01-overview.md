---
sidebarDepth: 2
---

# 1. 简介

本文档翻译自Tim Everett（rtkexplorer）维护的最新RTKLIB手册：[manual_demo5](https://rtkexplorer.com/pdfs/manual_demo5.pdf)

RTKLIB是一个用于全球导航卫星系统（GNSS）标准定位和精密定位的开源项目，由日本东京海洋大学的高须知二（Tomoji Takasu）开发。它由一个核心程序库和多个命令行程序与界面程序组成。RTKLIB具有以下特点：

## 1.1 支持的星座

RTKLIB支持GPS<sup>[1][2][3]</sup>、GLONASS<sup>[4]</sup>、Galileo<sup>[5]</sup>、QZSS<sup>[6]</sup>、北斗<sup>[7]</sup>和SBAS<sup>[8]</sup>等卫星导航系统。

## 1.2 定位模式

RTKLIB支持多种GNSS实时和后处理定位模式：

| 定位类别             | 模式类型                              | 特点                                                                 |
|----------------------|--------------------------------------------|----------------------------------------------------------------------|
| **伪距定位**         | Single - 伪距单点定位                      | 单接收机定位，精度较低。                                             |
|                      | DGPS/DGNSS - 伪距差分                     | 参考站修正伪距，提高精度。                                           |
| **载波相位定位**     | Kinematic - 载波动态相对定位（动态RTK）    | 流动站动态定位，适合车载，需实时解模糊度。                           |
|                      | Static - 载波静态相对定位（静态RTK）       | 两站静止，高精度静态测量。                       |
|                      | Static-Start - 静态固定后切换为动态（demo5自定义） | 先在静态状态下固定模糊度，而后动态定位，缩短首次固定时间。 |
|                      | Moving-Baseline - 双站均移动（双天线模式） | 双站移动，解算相对位置和姿态，适合定姿。                             |
|                      | Fixed - 固定坐标定位                       | 已知坐标，解算模糊度、对流层等参数，非定位用途。                     |
| **精密单点定位（PPP）** | PPP-Kinematic - 动态精密单点定位           | 单站动态定位，使用精密轨道和钟差，适合移动场景。                       |
|                      | PPP-Static - 静态精密单点定位              | 单站静态定位，使用精密轨道和钟差，高精度静止测量。                         |
|                      | PPP-Fixed - 固定坐标精密定位               | 已知坐标，解算模糊度、对流层等参数，非定位用途。                     |

## 1.3 支持的格式与标准

RTKLIB支持多种GNSS标准格式和协议（详细信息参考附录D.1和D.2）：
   
- **RINEX：** RINEX 2.10 <sup>[9]</sup>, 2.11 <sup>[10]</sup>, 2.12 <sup>[11]</sup> OBS/NAV/GNAV/HNAV/LNAV/QNAV, RINEX 3.00 <sup>[12]</sup>, 3.01 <sup>[13]</sup>, 3.02 <sup>[14]</sup> OBS/NAV, <u>RINEX 3.02 CLK</u> <sup>[15]</sup>
- **RTCM：** RTCM ver.2.3 <sup>[16]</sup>, RTCM ver.3.1（修订1-5）<sup>[17]</sup>,<u>RTCM ver.3.2</u> <sup>[18]</sup>；
- **其他标准：** BINEX <sup>[19]</sup>, NTRIP 1.0 <sup>[20]</sup>, <u>NMEA 0183</u> <sup>[21]</sup>, SP3-c <sup>[22]</sup>, ANTEX 1.4 <sup>[23]</sup>, IONEX 1.0 <sup>[24]</sup>， NGS PCV<sup>[25]</sup>和EMS 2.0<sup>[26]</sup>

## 1.4 支持的私有格式

RTKLIB支持以下几种GNSS接收机的专有消息格式（详细信息参考附录D.2）：

| **品牌**     | **支持型号**                              |
| ---------- | ------------------------------------- |
| NovAtel <sup>[27]</sup>    | OEM4/V/6, OEM3, OEMStar, Superstar II |
| Hemisphere <sup>[28]</sup> | Eclipse, Crescent                     |
| u-blox <sup>[29]</sup>     | LEA-4T/5T/6T                          |
| SkyTraq <sup>[30]</sup>    | S1315F                                |
| JAVAD <sup>[31]</sup>      | GRIL/GREIS                            |
| Furuno <sup>[32]</sup>     | GW-10-II/III                          |
| NVS <sup>[33]</sup>        | NV08C BINR                            |

注意：上述表格仅列出了原始官方RTKLIB支持的部分品牌和型号，具体支持情况会因版本差异而有所不同，例如最新的demo5代码已经开始支持和芯星通Unicore的专有格式。

## 1.5 支持的通信方式
   
支持多种通信方式，用于接收机数据传输、差分定位、实时PPP以及数据记录和调试：
- **串口：** 接收机通过串口直接连接电脑，传输观测数据和星历数据。
- **TCP/IP：** 通过网络接口实现数据传输。
- **NTRIP：** 用于获取差分定位的基准站数据和实时PPP的SSR（状态空间表示）数据。
- **本地日志文件：**
  - 支持记录原始数据流到日志文件。
  - 支持日志文件回放，用于模拟实时数据流进行调试。
  - 可将日志文件转换为RINEX格式，从而进行后处理操作。
- **FTP/HTTP：** 支持自动下载数据。

## 1.6 核心库函数

RTKLIB提供了许多库函数和API（应用程序接口）：
   
- **基础工具：** 卫星和导航系统函数、矩阵运算函数、时间与字符串函数、坐标转换、平台相关函数、选项配置
- **数学模型：** 标准定位、精密定位、整周模糊度固定、星历与时钟函数、大气模型、天线模型、潮汐模型、大地水准面模型、大地基准转换、SBAS
- **数据处理：** 输入和输出函数、接收机原始数据处理、解函数（定位结果处理）、KML转换器
- **协议支持：** RINEX、RTCM、NMEA
- **数据传输：** 数据流服务器、数据下载
- **程序调试：** 调试与跟踪

## 1.7 应用程序

RTKLIB包含以下GUI（图形用户界面）和CUI（命令行用户界面）程序：
<table style="text-align: center;">
 <thead>
    <tr>
      <th>功能</th>
      <th>GUI应用</th>
      <th>CUI应用</th>
      <th>备注</th>
    </tr>
 </thead>
 <tbody>
    <tr>
      <td style="text-align: left;">实时定位</td>
      <td>RTKNAVI<br>(3.2, 3.3, 3.5)</td>
      <td>RTKRCV<br>(3.11, A.1)</td>
      <td>-</td>
    </tr>
    <tr>
      <td style="text-align: left;">流处理</td>
      <td>STRSVR<br>(3.3)</td>
      <td>STR2STR<br>(3.11, A.5)</td>
      <td>-</td>
    </tr>
    <tr>
      <td style="text-align: left;">后处理分析</td>
      <td>RTKPOST<br>(3.4, 3.5)</td>
      <td>RNX2RTKP<br>(3.11, A.2)</td>
      <td>-</td>
    </tr>
    <tr>
      <td style="text-align: left;">RINEX转换</td>
      <td>RTKCONV<br>(3.6)</td>
      <td>CONVBIN<br>(3.11, A.4)</td>
      <td>-</td>
    </tr>
    <tr>
      <td style="text-align: left;">数据可视化</td>
      <td>RTKPLOT<br>(3.7, 3.8)</td>
      <td>-</td>
      <td>-</td>
    </tr>
    <tr>
      <td style="text-align: left;">GNSS产品与数据下载</td>
      <td>RTKGET<br>(3.9)</td>
      <td>-</td>
      <td>-</td>
    </tr>
    <tr>
      <td style="text-align: left;">NTRIP浏览器</td>
      <td>SRCTBLBROWS<br>(3.10)</td>
      <td>-</td>
      <td>-</td>
    </tr>
 </tbody>
</table>
