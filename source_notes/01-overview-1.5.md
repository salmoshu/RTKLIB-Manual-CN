## 1.5 延伸项目

以 RTKLIB 为基础的延伸项目，其中包含定位解算算法做增强项目、组合导航项目、服务端程序、软件接收机项目、上层应用项目应用等等：

| 项目名称 | 链接 | 简介 |
| --- | --- | --- |
| RTKLIB-demo5 | [GITHUB](https://github.com/rtklibexplorer/RTKLIB) | 针对低成本接收机做了算法增强。 |
| rtklib-py | [GITHUB](https://github.com/rtklibexplorer/rtklib-py) | 用 Python 编写的免费开源 RTKLIB 子集。最初基于 CSSRlib，但已重新编写以与 RTKLIB 的 demo5 版本高度一致。 |
| GAMP | [https://geodesy.noaa.gov/gps-toolbox/GAMP.htm](https://geodesy.noaa.gov/gps-toolbox/GAMP.htm) | 山科大周锋写的双频浮点解 PPP，在 RTKLIB 基础上做精简和算法的增强，比原版 RTKLIB 简单，是入门学习 PPP 不错的选择。 |
| Ginan | [GITHUB](https://github.com/GeoscienceAustralia/ginan) | 来自澳大利亚，包括精密定位程序 PEA、定轨程序 POD，文档很详细，代码比较难懂。 |
| GraphGNSSLib | [GITHUB](https://github.com/weisongwen/GraphGNSSLib) | 港理工项目，支持图优化 SPP、RTK，作者在知乎很活跃，发过一些科普文章。 |
| GLIO | [GITHUB](https://github.com/XikunLiu-huskit/GLIO) | 在 GraphGNSSLib 基础上做的 GNSS-IMU-Lidar 图优化紧组合。 |
| PPPLIB | [https://geodesy.noaa.gov/gps-toolbox/PPPLib.htm](https://geodesy.noaa.gov/gps-toolbox/PPPLib.htm) | 作者老师在矿大读研时所写，支持三频 SPP、PPK、PPP 和 IMU 组合。 |
| GINAV | [GITHUB](https://github.com/kaichen686/GINav) | MATLAB 紧组合项目，文件名和 RTKLIB 函数名一样。虽说是组合导航，但也可只用其中的 GNSS 部分，相比 goGPS 简单不少。 |
| GICI-LIB | [GITHUB](https://github.com/chichengcn/gici-open) | 上海交大池澄博士开源的 GNSS-IMU-Camera 图优化多源融合程序，以 GNSS 为主，实现了 RTK、PPP 的模糊度固定。 |
| PPP-AR | [GITHUB](https://github.com/PrideLab/PRIDE-PPPAR) | 武大 GNSS 中心开源的后处理 PPP，使用配套的产品可以实现 PPP 模糊度固定，支持五频数据处理，使用了 rnx2rtkp 可执行程序计算测站初值坐标。 |
| IGNAV | [GITHUB](https://github.com/Erensu/ignav) | 武大 GNSS 中心项目。 |
| pppwizard | [http://www.ppp-wizard.net/](http://www.ppp-wizard.net/) | 暂无详细介绍。 |
| GNSS-SDR | [GITHUB](https://github.com/gnss-sdr/gnss-sdr) | GNSS 软件接收机，实现基带算法直接对接收机输出的数字中频信号处理，PVT 部分用了 RTKLIB。 |
| PocketSDR | [GITHUB](https://github.com/tomojitakasu/PocketSDR) | RTKLIB 作者新开源的软件接收机，包含一个射频前端和一套后处理 GNSS 接收机程序（只支持后处理），实现了一整套完整的 GNSS 接收机功能，采用 C、Python 编写，支持几乎所有的 GNSS 信号（比商业接收机支持的还要多），引入 RTKLIB 做库，用到了 RTKLIB 的一些结构体。 |
| APOLLO | [GITHUB](https://github.com/ApolloAuto/apollo) | 百度的开源无人驾驶系统，用到了 RTKLIB 的 NMEA 结构体。 |

<GiscusTalk />
