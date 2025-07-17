# 2. 实时定位程序

RTKLIB 提供了 GUI 界面程序 RTKNAVI、和 CUI 命令行程序 RTKRCV 以进行实时定位解算。建议在看源码前，先实际动手操作 RTKNAVI，初步理解其中的概念。

## 2.1 RTKNAVI 使用示例

该部分主要参考自资料[25]。

### 2.1.1 软件和数据下载

RTKLIB 版本为 [demo5 b34K](https://github.com/rtklibexplorer/RTKLIB/releases)，另外这里提供一段录制好的 [RTCM 数据](https://github.com/salmoshu/RTKLIB-Manual-CN/tree/main/sample_data)。

通常 RTKNAV 接入的是实时数据流，例如流动站会通过串口连接 GNSS 芯片，而基站通过 TCP 网络连接千寻的 CORS 服务。不过 RTKNAVI 也支持将这些数据录制下来，需要注意的时候，实时数据的录制与回放需要将相应的时间戳记录下来：
- 录制对应 RTKNAVI -> [L] -> Time-Tag 复选框；
- 回放对应 RTKNAVI -> [I] -> Time 复选框，同时将设置回放速度（如x1）。

### 2.1.2 打开 RTKNAVI

<img style="width: 80%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-173840.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.1-1 RTKNAVI 主界面</p>

### 2.1.3 加载配置文件（.conf）

<img style="width: 60%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-180244.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.1-2 RTKNAVI 配置选项</p>

通过 [Options] 按钮进入配置选项页面，随后点击其中的 [Load] 按钮，加载配置文件（`/sample_data/rtcm/example.conf`）。

### 2.1.4 选择输入文件

<img style="width: 60%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-180809.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.1-3 RTKNAVI 输入文件</p>

点击 RTKNAVI 主页面中的按钮 [I]，随后在输入文件页面中选择输入文件。

输入流动站和基准站的文件路径。点击“输入文件路径”字段右侧的 [...]，在第一列选择 `/sample_data/rtcm/rover.rtcm3`。同样在第二列选择 `/sample_data/rtcm/base.rtcm3`。这段数据是一段绕园区的 1Hz 步行数据，流动站为移远双频模块 LG69T，基站为千寻 CORS。勾选“Time”，并通过右侧下拉菜单选择 x1 来设置回放速度，然后在对话框中点击 [OK]。

### 2.1.5 使用 RTKPLOT 查看结果

<img style="width: 60%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-182801.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.1-4 RTKPLOT 窗口</p>

点击 RTKNAVI 上的 [Plot...] 打开 RTKPLOT 窗口，随后在 [File] 中的 [Open Shapefile...] 中选择一段 GIS Shape 文件 （`/sample_data/converted.shp/*`），注意这里是导入全部文件。

### 2.1.6 运行 RTKNAVI

<img style="width: 60%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-183624.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.1-5 RTKNAVI 运行结果</p>

点击 RTKNAVI 上的 [Start] 按钮。RTKNAVI 开始回放已记录的数据。启动几秒后，您就能得到 RTK 解。解的状态为 FIX 表示已获得模糊度固定的 RTK 解，其精度通常为厘米级。

### 2.1.7 结果对比

<img style="width: 60%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-183908.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.1-6 结果对比</p>

使用 RTKPLOT 左上角的 [连接与断开] 按钮，断开 RTKPLOT 与 RTKNAVI 的连接，随后双击按钮 [2] 并选择参考文件 `/sample_data/rtcm/baseline.nmea`，该文件是同时录制的和芯星通 UM960 的数据。从结果可以看到即使是优化过的 RTKLIB demo5 性能也与商业模块 UM960 存在有一些差距。

这主要是因为 RTKLIB 中的抗差和模糊度固定策略还存在改进空间。

## 2.2 数据传递过程

### 2.2.1 概述

<img style="width: 90%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-194544.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.2-1 RTKNAVI 代码流程</p>

RTKNAVI 对应的 CUI 程序是 RTKRCV，其主程序为 `/RTKLIB-b34L/app/consapp/rtkrcv/rtkrcv.c`。

经过梳理发现 rtkrcv.c 的主函数主要流程如下：

- 使用 `rtkrcvinit()` 函数初始化 `svr_t` 结构体（`svr_t` 是一个基本揽括所有的庞大结构体）；
- 通过 `resetsysopt()` 函数重置系统参数，如果有配置文件，会读取配置文件。否则会使用内部默认参数（`rtkcmn.c/prcopt_default`）；
- 通常包含两个线程用以进行实时处理（以串口获取数据为例）：
  - `serialthread()`：通过获取获取 16 进制的 RTCM 数据；
  - `rtksvrthread()`：解析来自串口的数据，将其转换为 `obs_t`, `nav_t`, `rtk_t` 等结构体。
- 核心算法运算 `rtkpos()`;
- 输出结果 `writesol()`。

<img style="width: 80%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-200703.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.2-2 RTKNAVI 数据流程</p>

上图则显示了 RTKNAVI 从 RTCM 的数据、解析再到算法运算的数据流通流程。通常流动站会使用 MSM7 的数据类型，而基站端则使用 MSM4 的数据类型（数据更精简，便于传输）。基站数据中通常不包含星历数据，不过如果需要的话，也可以通过 MSM7 来获取，并在 RTKNAVI 中打开相应的设置（misc-navmsgsel）。

### 2.2.2 RTCM 的读取

<img style="width: 80%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-201545.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.2-3 RTCM 数据流</p>

通过观测正在运行的的 RTKNAVI 程序的监视窗口（monitor），可以发现 RTCM 数据基本是以 D3 开头的 16进制数据。

<img style="width: 50%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-201932.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.2-4 RTCM 数据类别</p>

具体的 RTCM 报文类别如上所示。

### 2.2.2 RTCM 的解析

<img style="width: 100%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-202409.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.2-5 RTCM 数据解析</p>

- 通过 `strread()` 函数读取 16 进制的 RTCM 数据，数据存储在 `svr->buff` 中；
- 在经过 `decoderaw()` 进行解析时：
  - 逐字节读取报文的过程中，每当检测到一个完整的 RTCM 报文就会进行一次报文的拷贝（将 s`vr->buff[]` 中单个的 RTCM 报文拷贝到 `svr.rtcm->buff` 中；
  - 会根据报文的类型，完成相应的解析工作（如 `decode_msm7()`, `decode_1019()` 等），会根据报文的类型分别存储到 `rtcm->obs` 或 `rtcm->nav` 结构体中，例如：
    - 观测数据：解析 MSM7 数据，会将其存储到 `rtcm->obs` 中；
    - 星历数据：解析 1019 数据，会将其存储到 `rtcm->nav` 中；
    - 定位数据：解析 1006 数据，会将其存储到 `rtcm->sta` 中（1006 报文是芯片提供的一个单点定位结果）。
  - 随后利用 `updatesvr()` 将解析好的 `rtcm->obs` 或 `rtcm->nav` 数据合入 `svr->obs` 或 `svr->nav` 结构体中，需要注意的是在解析观测数据（如 MSM7 数据）时，需要检测多个星座是否为同一历元，如果是同一历元，则将其存储在一起（由 RTCM 报文中的同步标识作为判断依据）。

### 2.2.3 定位解算

<img style="width: 100%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-210920.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.2-6 定位前数据准备</p>

定位之前的星历数据和基站位置数据是通过引用传递进去的，不过观测数据则会进行一次拷贝，具体的是定义一个新的 `obs_t` 数组，而后将流动站和基站的观测数据顺序存入该数组中。

<img style="width: 60%; margin: 10px auto; display: block;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250716-211731.jpg"/>
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图2.2-7 定位解算</p>

最后就是进入定位算法的解算了，更多细节可以参考后续伪距定位、相对定位和精密单点定位的章节。

对于定位算法以外的部分业务函数，它并非笔者所关注的重点，因此将不会对其进行更多的分析，也不会解析其源码。
