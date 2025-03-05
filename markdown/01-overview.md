---
sidebarDepth: 2
---

# 1. 摘要

RTKLIB是一个用于全球导航卫星系统（GNSS）标准定位和精密定位的开源程序包，由日本东京海洋大学的高须知二（Tomoji Takasu）开发。它由一个核心程序库和多个命令行程序与界面程序组成。RTKLIB具有以下特点：

1. **支持的星座**。支持GPS<sup>[1][2][3]</sup>、GLONASS<sup>[4]</sup>、Galileo<sup>[5]</sup>、QZSS<sup>[6]</sup>、北斗<sup>[7]</sup>和SBAS<sup>[8]</sup>等卫星导航系统。
2. **定位模式**。支持多种GNSS实时和后处理定位模式：
   - Single：伪距单点定位；
   - DGPS/DGNSS：伪距差分；
   - kinematic：载波动态相对定位，动态RTK，假设流动站是移动的，可以做车载定位；
   - Static：载波静态相对定位，静态RTK，两站都是静止的；
   - Static-Start：该模式是demo5作者自己增加的模式，Static固定后切换位Kinematic；
   - Moving-Baseline：两站都动，双天线，主要用来定姿；
   - Fixed：固定坐标，解算模糊度、对流层、电离层等参数；
   - PPP-Kinematic：动态精密单点定位；
   - PPP-Static：静态精密单点定位；
   - PPP-Fixed：PPP 固定坐标，解算模糊度、对流层、电离层等参数。
3. **支持的格式与标准**。它支持多种GNSS标准格式和协议：
   RINEX 2.10 <sup>[9]</sup>, 2.11 <sup>[10]</sup>, 2.12 <sup>[11]</sup> OBS/NAV/GNAV/HNAV/LNAV/QNAV, RINEX 3.00 <sup>[12]</sup>, 3.01 <sup>[13]</sup>, 3.02 <sup>[14]</sup> OBS/NAV, <u>RINEX 3.02 CLK</u> <sup>[15]</sup>, RTCM ver.2.3 <sup>[16]</sup>, RTCM ver.3.1（修订1-5）<sup>[17]</sup>,<u>RTCM ver.3.2</u> <sup>[18]</sup>, BINEX <sup>[19]</sup>, NTRIP 1.0 <sup>[20]</sup>, <u>NMEA 0183</u> <sup>[21]</sup>, SP3-c <sup>[22]</sup>, ANTEX 1.4 <sup>[23]</sup>, IONEX 1.0 <sup>[24]</sup>， NGS PCV<sup>[25]</sup>和EMS 2.0<sup>[26]</sup>（详情参见附录D.1和D.2）。
4. **支持的消息格式**。它支持几种GNSS接收机的专有消息：NovAtel <sup>[27]</sup>: OEM4/V/6， OEM3, OEMStar, Superstar II, Hemisphere <sup>[28]</sup>: Eclipse, Crescent, u-blox <sup>[29]</sup>: LEA-4T/5T/6T, SkyTraq <sup>[30]</sup>: S1315F, JAVAD <sup>[31]</sup> GRIL/GREIS， Furuno <sup>[32]</sup> GW-10-II/III和NVS <sup>[33]</sup> NV08C BINR（详细信息参见附录D.2）。
5. **支持的外部通信方式**。支持串口、TCP/IP、NTRIP、本地日志文件（记录和回放）、FTP/HTTP（自动下载）。接收机一般通过串口或网口可以直接连电脑传输观测数据和星历数据；差分定位用的基准站数据和实时PPP用的SSR数据一般通过NTRIP接入；实时定位的时候可以保存原始数据流到日志文件，可以通过日志文件来模拟实时数据流解算来进行调试，也可以转为 RINEX 后处理。
6. **核心库函数**。它提供了许多库函数和API（应用程序接口）：卫星和导航系统函数、矩阵和矢量函数、时间和字符串函数、坐标变换、输入和输出函数、调试跟踪函数、平台相关函数、定位模型、大气模型、天线模型、潮汐模型、大地水准面模型、基准变换、RINEX函数、星历和时钟函数、精密星历和时钟函数、接收机原始数据函数、RTCM函数、解函数、KML转换器、SBAS功能、选项功能、流数据输入输出功能、整数歧义解析、标准定位、精确定位、后处理定位、流服务器功能、RTK服务器功能、下载器功能。
7. **应用程序**。RTKLIB包含以下GUI（图形用户界面）和CUI（命令行用户界面）程序：
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
