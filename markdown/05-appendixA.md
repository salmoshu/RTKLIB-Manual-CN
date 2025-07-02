---
sidebarDepth: 2
---

# 附录A. CUI指令

## A.1 RTKRCV

**概要**
```shell
rtkrcv [-s][-p port|-d dev][-o file][-t level]
```

**描述**<br>
RTKRCV是基于RTKLIB的实时定位AP命令行版本。要启动或停止RTK服务器、配置选项或打印解算结果/状态，请登录控制台并输入命令。默认情况下，控制台使用标准输入（stdin）和标准输出（stdout）。如果通过Telnet协议进行网络登录，请使用-p选项。在控制台中输入 "?" 或 "help" 可以显示可用命令。初始处理选项将从默认配置文件 rtkrcv.conf 加载。若要更改文件，请使用-o选项。要配置处理选项，可以通过编辑配置文件或在控制台上使用 set、load 或 save 命令来完成。要关闭程序，可以在控制台上使用 shutdown 命令，或向进程发送 USR2 信号。关于配置文件，可参考附录 B.4。

**参数**<br>
```shell
-s   在程序启动时启动RTK服务器
-p   端口 用于telnet控制台的端口号
-m   端口 用于监控流的端口号
-d   设备 用于控制台的终端设备
-o   文件 配置文件
-w   密码 远程控制台的登录密码（“”: 无密码）
-r   级别 输出解算状态文件（0: 关闭，1: 状态，2: 残差）
-t   级别 调试跟踪级别（0: 关闭，1-5: 开启）
-sta 站名 接收机dcb的站点名称
```

**命令**

```shell
# 启动RTK服务器。如果程序使用-s选项运行，则无需此命令。
start

# 停止RTK服务器。
stop

# 重启RTK服务器。如果已设置处理选项，执行此命令以启用更改。
restart

# 显示解算结果。不带选项时，仅显示一个解算结果。带选项时，解算结果将每隔cycle秒（s）显示一次。要停止循环显示，发送中断信号（Ctrl-C）。
solution [cycle]

# 显示RTK状态。使用cycle选项进行循环显示。
status [cycle]

# 显示卫星状态。使用cycle选项进行循环显示。
satellite [cycle]

# 显示观测数据。使用cycle选项进行循环显示。
observ [cycle]

# 显示导航数据。使用cycle选项进行循环显示。
navidata [cycle]

# 显示流状态。使用cycle选项进行循环显示。
stream [cycle]

# 显示SSR修正。
ssr [cycle]

# 显示错误/警告信息。要停止显示信息，发送中断信号（Ctrl-C）。
error

# 显示处理选项的值。不带选项时，显示所有选项。带选项时，仅显示匹配模式的选项。
option [opt]

# 将处理选项的值设置为val。不带val选项时，将显示提示消息以输入值。在重启RTK服务器之前，处理选项的更改不会生效。
set opt [val]

# 从文件加载处理选项。不带选项时，使用默认文件rtkrcv.conf。要启用更改，需重启RTK服务器。
load [file]

# 将当前处理选项保存到文件。不带选项时，使用默认文件rtkrcv.conf。
save [file]

# 将控制台日志记录到文件。要停止记录日志，使用off选项。
log [file|off]

# 显示命令列表。带path选项时，显示流路径选项。
help|? [path]

# 退出并注销控制台。此命令不影响RTK服务器的状态。
exit

# 关闭RTK服务器并退出程序。
shutdown

# 通过系统shell执行命令。不要使用交互式命令。
!command [arg...]

```
注意：允许使用命令的缩写形式。在使用缩写形式时，命令将根据首字符进行区分。

## A.2 RNX2RTKP

**概要**
```shell
rnx2rtkp [option ...] file file [...]
```

**描述**

读取RINEX OBS/NAV/GNAV/HNAV/CLK、SP3、SBAS消息日志文件，并计算接收机（流动站）位置，输出位置解算结果。
第一个RINEX OBS文件应包含接收机（流动站）的观测数据。对于相对模式，第二个RINEX OBS文件应包含参考站（基准站）接收机的观测数据。输入文件中至少应包含一个RINEX NAV/GNAV/HNAV文件。要使用SP3精密星历，需在文件路径中指定其路径。SP3文件的扩展名应为.sp3或.eph。所有输入文件路径都可以包含通配符（*）。为了避免命令行展开通配符，对于包含通配符的路径，请使用“...”。

命令行选项如下（[]：默认值）。使用-k选项时，处理选项将从配置文件中输入。在这种情况下，命令行选项优先于配置文件中的选项。关于配置文件，请参考B.4节。

**参数**

```shell
-?        打印帮助信息
-k file   从配置文件中输入选项 [关闭]
-o file   设置输出文件 [标准输出]
-ts ds ts 开始日期/时间（ds=年/月/日 ts=时:分:秒） [观测开始时间]
-te de te 结束日期/时间（de=年/月/日 te=时:分:秒） [观测结束时间]
-ti tint  时间间隔（秒） [全部]
-p        模式（0:单点，1:差分，2:动态，3:静态，4:移动基座，5:固定，6:PPP动态，7:PPP静态） [2]
-m mask   仰角遮蔽角（度） [15]
-f freq   相对模式下使用的频率数（1:L1，2:L1+L2，3:L1+L2+L5） [2]
-v thres  整数模糊度验证阈值（0.0:无整数模糊度解算） [3.0]
-b        向后解算 [关闭]
-c        前向/后向组合解算 [关闭]
-i        瞬时整数模糊度解算 [关闭]
-h        整数模糊度解算的固定和保持 [关闭]
-e        输出X/Y/Z-ECEF位置 [纬度/经度/高度]
-a        输出E/N/U基线 [纬度/经度/高度]
-n        输出NMEA-0183 GGA语句 [关闭]
-g        以ddd mm ss.ss [ddd.ddd]的形式输出纬度/经度
-t        以yyyy/mm/dd hh:mm:ss.ss [sssss.ss]的形式输出时间
-u        以UTC输出时间 [GPST]
-d col    小数点后位数 [3]
-s sep    字段分隔符 [' ']
-r x y z  基准站的ECEF位置（m） [单点定位平均值]
-l lat lon hgt 参考（基准）接收机纬度/经度/高度（度/m）
-y level  输出解算状态（0:关闭，1:状态，2:残差） [0]
-x level  调试跟踪级别（0:关闭） [0]
```

**示例**

示例1. 动态定位，L1+L2，输出纬度/经度/高度到标准输出。
```shell
rnx2rtkp 07590920.05o 30400920.05o 30400920.05n
```

示例2. 单点定位，仰角遮蔽角=15度，输出NMEA GGA到文件out.pos
```shell
rnx2rtkp -p 0 -m 15 -n -o out.pos 07590920.05o 30400920.05n
```

示例3. 静态定位，L1，时间格式为yyyy/mm/dd hh:mm:ss，输出X/Y/Z-ECEF位置
```shell
rnx2rtkp -p 3 -f 1 -t -e 07590920.05o 30400920.05o 30400920.05n
```

示例4. 动态定位，瞬时整数模糊度解算，验证阈值=2，逗号分隔符
```shell
rnx2rtkp -i -v 2 -s , 07590920.05o 30400920.05o 30400920.05n
```

## A.3 POS2KML

**概要**
```shell
pos2kml [option ...] file [...]
```

**描述**

读取位置文件并将其转换为Google Earth KML文件。输入文件的每一行应包含时间、位置字段（纬度/经度/高度或X/Y/Z-ECEF）以及质量标志（可选）。以'%'、'#'、';'开头的行被视为注释。命令选项如下。([]：默认值)

**选项**

```shell
-h       打印帮助信息
-o       文件 输出文件 [输入文件 + .kml]
-c       颜色 轨迹颜色
(0:关闭，1:白色，2:绿色，3:橙色，4:红色，5:黄色) [5]
-p       颜色 点的颜色
(0:关闭，1:白色，2:绿色，3:橙色，4:红色，5:根据质量标志) [5]
-a       输出高度信息 [关闭]
-ag      输出大地高度 [关闭]
-tg      输出GPST时间戳 [关闭]
-tu      输出UTC时间戳 [GPST]
-i tint  输出时间间隔（秒）（0:全部） [0]
-q qflg  输出质量标志（0:全部） [0]
-f n e h 向位置添加北/东/高偏移（米） [0 0 0]
-gpx     输出GPX文件
```

## A.4 CONVBIN

**概要**

```shell
convbin [-ts y/m/d h:m:s] [-te y/m/d h:m:s] [-ti tint] [-r format] [-ro opts]
        [-f freq] [-hc comment] [-hm marker] [-hn markno] [-ht marktype]
        [-ho observ] [-hr rec] [-ha ant] [-hp pos] [-hd delta] [-v ver] [-od]
        [-os] [-x sat] [-y sys] [-d dir] [-c satid] [-o ofile] [-n nfile]
        [-g gfile] [-h hfile] [-q qfile] [-s sfile] file
```

**描述**

将RTCM、接收机原始数据日志和RINEX文件转换为RINEX和SBAS/LEX消息文件。SBAS消息文件符合RTKLIB SBAS/LEX消息格式。它支持以下消息或文件：<br>

```shell
RTCM 2            : Type 1, 3, 9, 14, 16, 17, 18, 19, 22
RTCM 3            : Type 1002, 1004, 1005, 1006, 1010, 1012, 1019, 1020
                    Type 1071-1127（MSM，不包括紧凑消息）
NovAtel OEMV/4/6,OEMStar : RANGECMPB, RANGEB, RAWEPHEMB, IONUTCB, RAWWASSFRAMEB
Swiftnav          :
Septentrio        :
u-blox M8T/F9P    : RXM-RAW, RXM-SFRB, RXM-RAWX
Hemisphere        : BIN76, BIN80, BIN94, BIN95, BIN96
SkyTraq S1315F    : msg0xDD, msg0xE0, msg0xDC
GW10              : msg0x08, msg0x03, msg0x27, msg0x20
Javad             : [R*],[r*],[*R],[*r],[P*],[p*],[*P],[*p],[D*],[*d],
                    [E*],[*E],[F*],[TC],[GE],[NE],[EN],[QE],[UO],[IO],
                    [WD]
NVS               : NVS NV08C BINR
BINEX             : 大端序，常规CRC，前向记录（0xE2）
                    0x01-01,0x01-02,0x01-03,0x01-04,0x01-06,0x7f-05
RINEX             : OBS, NAV, GNAV, HNAV, LNAV, QNAV
```

**选项**

```shell
file         输入接收机二进制日志文件
-ts y/m/d h\:m:s 开始时间 [全部]
-te y/m/d h\:m:s 结束时间 [全部]
-tr y/m/d h\:m:s RTCM消息的近似时间
-ti tint     观测数据间隔（秒） [全部]
-tt ttol     观测数据历元容差（秒） [0.005]
-r format    日志格式类型
             rtcm2= RTCM 2
             rtcm3= RTCM 3
             nov = NovAtel OEMV/4/6,OEMStar
             oem3 = NovAtel OEM3
             ubx = u-blox LEA-4T/5T/6T/7T/M8/F9
             sbp = Swift Navigation SBP
             ss2 = NovAtel Superstar II
             hemis= Hemisphere Eclipse/Crescent
             stq = SkyTraq S1315F
             javad= Javad
             nvs = NVS BINR
             binex= BINEX
             rinex= RINEX
-ro opt      接收机选项
-f freq      频率数 [2]
-hc comment  RINEX头：注释行
-hm marker   RINEX头：标记名称
-hn markno   RINEX头：标记编号
-ht marktype RINEX头：标记类型
-ho observ   RINEX头：观测者名称和机构，用/分隔
-hr rec      RINEX头：接收机编号、类型和版本，用/分隔
-ha ant      RINEX头：天线编号和类型，用/分隔
-hp pos      RINEX头：近似位置x/y/z，用/分隔
-hd delta    RINEX头：天线偏移h/e/n，用/分隔
-v ver       RINEX版本 [2.11]
-od          在RINEX观测中包含多普勒频率 [关闭]
-os          在RINEX观测中包含信噪比 [关闭]
-oi          在RINEX导航头中包含电离层修正 [关闭]
-ot          在RINEX导航头中包含时间修正 [关闭]
-ol          在RINEX导航头中包含闰秒 [关闭]
-halfc       半周模糊度修正 [关闭]
-mask [sig[,...]] 信号掩码（sig={G|R|E|J|S|C|I}L{1C|1P|1W|...}）
-nomask [sig[,...]] 信号不掩码（同上）
-x sat       排除卫星
-y sys       排除系统（G:GPS, R:GLONASS, E:Galileo, J:QZSS, S:SBAS, C:北斗）
-d dir       输出目录 [与输入文件相同]
-c staid     使用带有站ID的RINEX文件命名约定 [关闭]
-o ofile     输出RINEX OBS文件
-n nfile     输出RINEX NAV文件
-g gfile     输出RINEX GNAV文件
-h hfile     输出RINEX HNAV文件
-q qfile     输出RINEX QNAV文件
-l lfile     输出RINEX LNAV文件
-b cfile     输出RINEX CNAV文件
-i ifile     输出RINEX INAV文件
-s sfile     输出SBAS消息文件
-trace level 输出跟踪级别 [关闭]
```

如果指定了任何输出文件，则使用默认输出文件（`<file>.obs`, `<file>.nav`, `<file>.gnav`, `<file>.hnav`, `<file>.qnav`, `<file>.lnav` 和 `<file>.sbs`）。

如果未指定接收机类型，则根据输入文件扩展名识别类型，如下所示：

```shell
*.rtcm2 RTCM 2
*.rtcm3 RTCM 3
*.gps NovAtel OEMV/4/6,OEMStar
*.ubx u-blox LEA-4T/5T/6T/8T/8/9
*.bin Hemisphere Eclipse/Crescent
*.stq SkyTraq S1315F
*.jps Javad
*.bnx,*binex BINEX
*.obs,.*o RINEX OBS
```

## A.5 STR2STR

**概要**
```shell
str2str -in stream[#...] -out stream[#...] [-out stream[#...]...] [options]
```

**描述**

从一个流中输入数据，并将其分割并输出到多个流。输入流可以是串行端口、TCP客户端、TCP服务器、NTRIP客户端或文件。输出流可以是串行端口、TCP客户端、TCP服务器、NTRIP服务器或文件。str2str是一个常驻类型的应用程序。要停止它，如果在前台运行，则在控制台中输入Ctrl-C；如果在后台运行，则发送SIGINT信号。如果输入流和输出流都遵循#格式，则输入消息的格式将转换为输出格式。要指定输出消息，请使用-msg选项。
命令选项如下。

**选项**

-in 流[#格式] 输入流路径和格式
-out 流[#格式] 输出流路径和格式

**流路径**
```shell
serial      ://端口[:波特率[:字节大小[:奇偶校验[:停止位[:流控]]]]][#tcp端口]
tcp server  : tcpsvr://:端口
tcp client  : tcpcli://地址[:端口]
ntrip client: ntrip://[用户[:密码]@]地址[:端口][/挂载点]
ntrip server: ntrips://[:密码@]地址[:端口][/挂载点[:字符串]]（仅输出）
ntrip caster: ntripc://[用户:密码@][:端口]/挂载点[:源表]（仅输出）
file        : [file://]路径[::T][::+开始][::x间隔][::S=交换]
```

**格式**
```shell
rtcm2    : RTCM 2（仅输入）
rtcm3    : RTCM 3（输入和输出）
nov      : NovAtel OEMV/4/6,OEMStar（仅输入）
swiftnav : Swiftnav（仅输入）
ubx      : u-blox LEA-4T/5T/6T/8/9（仅输入）
hemis    : Hemisphere Eclipse/Crescent（仅输入）
stq      : SkyTraq S1315F（仅输入）
javad    : Javad（仅输入）
sbf      : Septentrio（仅输入）
nvs      : NVS BINR（仅输入）
binex    : BINEX（仅输入）
```

**消息类型和输出间隔**
```shell          
-msg type [(tint)][,type[(tint)]...]

-sta staid     站点ID
-opt opt       接收机依赖选项
-s msec        超时时间（毫秒） [10000]
-r msec        重新连接间隔（毫秒） [10000]
-n msec        NMEA请求周期（毫秒） [0]
-f sec         文件交换边界（秒） [30]
-c file        输入命令文件 [无]
-c1 file       输出1命令文件 [无]
-c2 file       输出2命令文件 [无]
-c3 file       输出3命令文件 [无]
-c4 file       输出4命令文件 [无]
-p lat lon hgt 站点位置（纬度/经度/高度）（度，米）
-px x y z      站点位置（x/y/z-ECEF）（米）
-a antinfo     天线信息（用逗号分隔）
-i rcvinfo     接收机信息（用逗号分隔）
-o e n u       天线偏移（东，北，上）（米）
-l local_dir   FTP/HTTP本地目录 []
-x proxy_addr  HTTP/NTRIP代理地址 [无]
-b str_no      将输出流的消息回传到输入流 [无]
-t level       跟踪级别 [0]
-fl file       日志文件 [str2str.trace]
-h             打印帮助信息
```
