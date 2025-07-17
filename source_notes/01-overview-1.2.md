## 1.2 基本使用

### 1.2.1 下载 RTKLIB

以下为当前的版本汇总（截至2025年7月7日）：

<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">表1.2-1 RTKLIB 主要版本汇总</p>

|  版本  | 日期       | 可执行 Windows 版本   | 源码  | 备注 |
|---------|------------|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|----------------------------------|
| 2.4.2 p13 | 2018/01/29 | [GitHub](https://github.com/tomojitakasu/RTKLIB_bin)                                   | [GitHub](https://github.com/tomojitakasu/RTKLIB)                                      | 官方版本2.4.2 |
| 2.4.3 b34 | 2020/12/29 | [GitHub](https://github.com/tomojitakasu/RTKLIB_bin/tree/rtklib_2.4.3)                 | [GitHub](https://github.com/tomojitakasu/RTKLIB/tree/rtklib_2.4.3)                    | 官方版本2.4.3 |
| demo5-b34? | 持续更新 | [Github](https://github.com/rtklibexplorer/RTKLIB/releases) | [GitHub](https://github.com/rtklibexplorer/RTKLIB/releases) | 基于2.4.3，面向低成本接收机优化 |
| rtklib-py | 2022/07/20 | - | [GitHub](https://github.com/rtklibexplorer/rtklib-py) | Python版本 |

**建议下载 demo5 版本**，因为它继承了 2.4.3 版本的功能，但包含更多优化项，有更多值得学习的地方，并且它的主要框架与 2.4.3 相差不大。通过绿色按钮 **Code** 的下拉菜单，再点击 **Download ZIP** 可以下载目标文件：

![image-20231016092003733](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231016092003733.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.2-1 RTKLIB 源码下载</p>

解压两个压缩文件，得到的文件目录如下：

![image-20231026103732552](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231026103732552.png)
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1.2-2 RTKLIB 文件目录</p>

**Source Programs and Data** 是程序的源文件，**Binary APs for Windows** 是编译好的 Windows 可执行文件，可以移至于 **Source Programs and Data** 的 bin 目录下以方便查找。对于经常使用的应用，例如用于原始数据和结果数据绘图的 **rtkplot** ，建议创建桌面快捷方式。

### 1.2.2 使用方法

**使用 RTKLIB 实现 GNSS 数据处理与定位解算**。如果你拥有自己的 GNSS 模块和天线，可以通过串口或网口将其连接到电脑或树莓派，然后将 RTKLIB 部署其中作为数据处理与定位解算的工具。以下是 RTKLIB 的主要功能和操作步骤：

**1. 实时定位解算**
- **单点定位（SPP）**：使用 RTKNAVI 或 RTKRCV 进行实时单点定位，精度可达米级。
- **差分定位（RTK）**：接入差分数据（如自建基准站或使用 CORS 服务）可显著提高精度，实现厘米级定位。
- **PPP（精密单点定位）**：提前下载精密改正文件（如 IGS 提供的 SSR 数据），并通过 RTKLIB 实现实时 PPP 解算。

**2. 数据传输与存储**
- **数据记录**：在实时解算过程中，可通过 LOG 功能记录数据流，便于后续分析；或者直接使用 STRSVR 或 STR2STR 将数据存储到本地文件。
- **远程传输**：通过 Ntrip 或 TCP Server将数据广播出去，支持远端解算。
- **基准站搭建**：可以将数据作为 Ntrip 数据源挂载到 NtripCaster，搭建自己的基准站。

**3. 数据准备**
- **星历文件下载**：如果手头只有观测数据（伪距、载波相位、多普勒和C/N0），那么还需要下载星历文件。可以通过 RTKGET 自动下载，或手动从 IGS 网站（如 [武汉大学 IGS 中心](ftp://igs.gnsswhu.cn/pub/gps/)）获取。
- **PPP 改正文件下载**：进行 PPP 解算时，需下载精密改正文件（如 IGS 提供的 SSR 数据）。

> 星历的下载，笔者喜欢使用MGEX-AGNSS服务，可以参考 [附录A.3](/algorithm/RTKLIB-Source-Notes/A-appendixA.html#a-3-mgex-agnss服务)。

**4. 后处理分析**

当实时解算效果不理想，或是需要更高精度的结果时，可借助后处理分析来对数据或算法进行研究：

- **数据格式转换**：接收机采集的原始数据通常为 RTCM 或其他二进制格式，可通过 CONVBIN 或 RTKCONV 将其转换为 RINEX 格式，便于后处理。
- **后处理解算工具**：
  - **RTKPOST**：图形界面程序，方便设置解算参数并导出配置文件，适合处理单个数据文件。
  - **RNX2RTKP**：命令行程序，适合批量处理。可通过配置文件设置解算参数，支持脚本自动化。
  - **自定义后处理脚本**：如果希望同时运行多组配置或多组数据，可以编写自定义脚本，这里提供笔者编写的一个示例程序，具体参考[附录A.4](/algorithm/RTKLIB-Source-Notes/A-appendixA.html#a-4-批处理程序)。
- **输出文件分析**：
  - **结果文件（Solution）**：包含定位和速度结果，支持 NMEA 或 RTKLIB 自定义格式。
  - **解算中间结果文件（Solution Status）**：记录解算过程中的中间结果，如卫星残差、高度角、模糊度、对流层延迟等。
  - **调试文件（Trace）**：用于辅助调试，记录程序执行过程中的错误、警告等信息。Trace 级别分为 1-5 级，其中：
    - **1 级**：致命错误，程序无法继续执行。
    - **2 级**：警告，程序可能继续执行但可能无法解算。
    - **3 级**：程序主要执行流程。
    - **4 级**：更详细的执行流程。
    - **5 级**：解算的详细中间过程。
- **数据质量分析**：使用 RTKPLOT 分析原始数据质量，包括卫星数量、观测值连续性、卫星几何分布（天空图）、信噪比等。
- **问题排查**：
  - 如果解算失败，可将 Trace 级别设为 2，查看错误或警告信息，检查文件路径、数据格式或解算参数设置；
  - 如果是算法问题，可以先将 Trace 级别设为 3；
  - 如果结果精度不达标，可分析解算中间结果文件（Solution Status），通过 RTKPLOT 作图优化解算参数。

<GiscusTalk />
