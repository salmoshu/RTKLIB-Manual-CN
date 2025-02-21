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
这是RTKLIB提供的实时定位应用程序的命令行版本。要启动或停止RTK服务器，配置选项或打印解算/状态信息，请登录控制台并输入命令。默认情况下，使用标准输入/输出（stdin/stdout）作为控制台。使用-p选项可通过telnet协议进行网络登录。要显示可用的命令，在控制台中输入?或help。初始处理选项从默认配置文件rtkrcv.conf加载。要更改该文件，使用-o选项。要配置处理选项，可以编辑配置文件，或在控制台上使用set、load或save命令。要关闭程序，可以在控制台上使用shutdown命令，或向进程发送USR2信号。关于配置文件，请参考B.4节。

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

start<br>
启动RTK服务器。如果程序使用-s选项运行，则无需此命令。

stop<br>
停止RTK服务器。

restart<br>
重启RTK服务器。如果已设置处理选项，执行此命令以启用更改。

solution [cycle]<br>
显示解算结果。不带选项时，仅显示一个解算结果。带选项时，解算结果将每隔cycle秒（s）显示一次。要停止循环显示，发送中断信号（Ctrl-C）。

status [cycle]<br>
显示RTK状态。使用cycle选项进行循环显示。

satellite [cycle]<br>
显示卫星状态。使用cycle选项进行循环显示。

observ [cycle]<br>
显示观测数据。使用cycle选项进行循环显示。

navidata [cycle]<br>
显示导航数据。使用cycle选项进行循环显示。

stream [cycle]<br>
显示流状态。使用cycle选项进行循环显示。

ssr [cycle]<br>
显示SSR修正。

error<br>
显示错误/警告信息。要停止显示信息，发送中断信号（Ctrl-C）。

option [opt]<br>
显示处理选项的值。不带选项时，显示所有选项。带选项时，仅显示匹配模式的选项。

set opt [val]<br>
将处理选项的值设置为val。不带val选项时，将显示提示消息以输入值。在重启RTK服务器之前，处理选项的更改不会生效。

load [file]<br>
从文件加载处理选项。不带选项时，使用默认文件rtkrcv.conf。要启用更改，需重启RTK服务器。

save [file]<br>
将当前处理选项保存到文件。不带选项时，使用默认文件rtkrcv.conf。

log [file|off]<br>
将控制台日志记录到文件。要停止记录日志，使用off选项。

help|? [path]<br>
显示命令列表。带path选项时，显示流路径选项。

exit<br>
退出并注销控制台。此命令不影响RTK服务器的状态。

shutdown<br>
关闭RTK服务器并退出程序。

!command [arg...]<br>
通过系统shell执行命令。不要使用交互式命令。

注意事项<br>
允许使用命令的缩写形式。在使用缩写形式时，命令将根据首字符进行区分。

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
-?   打印帮助信息
-k   文件 从配置文件中输入选项 [关闭]
-o   文件 设置输出文件 [标准输出]
-ts  ds ts 开始日期/时间（ds=年/月/日 ts=时:分:秒） [观测开始时间]
-te  de te 结束日期/时间（de=年/月/日 te=时:分:秒） [观测结束时间]
-ti  tint 时间间隔（秒） [全部]
-p   模式（0:单点，1:差分，2:动态，3:静态，4:移动基座，5:固定，6:PPP动态，7:PPP静态） [2]
-m   遮蔽角 仰角遮蔽角（度） [15]
-f   频率 相对模式下使用的频率数（1:L1，2:L1+L2，3:L1+L2+L5） [2]
-v   阈值 整数模糊度验证阈值（0.0:无整数模糊度解算） [3.0]
-b   向后解算 [关闭]
-c   前向/后向组合解算 [关闭]
-i   瞬时整数模糊度解算 [关闭]
-h   整数模糊度解算的固定和保持 [关闭]
-e   输出X/Y/Z-ECEF位置 [纬度/经度/高度]
-a   输出E/N/U基线 [纬度/经度/高度]
-n   输出NMEA-0183 GGA语句 [关闭]
-g   以ddd mm ss.ss [ddd.ddd]的形式输出纬度/经度
-t   以yyyy/mm/dd hh:mm:ss.ss [sssss.ss]的形式输出时间
-u   以UTC输出时间 [GPST]
-d   小数点后位数 [3]
-s   字段分隔符 [' ']
-r   x y z 参考（基准）接收机ECEF位置（米） [单点定位平均值]
-l   纬度 经度 高度 参考（基准）接收机纬度/经度/高度（度/米）
-y   级别 输出解算状态（0:关闭，1:状态，2:残差） [0]
-x   级别 调试跟踪级别（0:关闭） [0]
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

-h 打印帮助信息<br>
-o 文件 输出文件 [输入文件 + .kml]<br>
-c 颜色 轨迹颜色<br>
(0:关闭，1:白色，2:绿色，3:橙色，4:红色，5:黄色) [5]<br>
-p 颜色 点的颜色<br>
(0:关闭，1:白色，2:绿色，3:橙色，4:红色，5:根据质量标志) [5]<br>
-a 输出高度信息 [关闭]<br>
-ag 输出大地高度 [关闭]<br>
-tg 输出GPST时间戳 [关闭]<br>
-tu 输出UTC时间戳 [GPST]<br>
-i tint 输出时间间隔（秒）（0:全部） [0]<br>
-q qflg 输出质量标志（0:全部） [0]<br>
-f n e h 向位置添加北/东/高偏移（米） [0 0 0]<br>
-gpx 输出GPX文件

## A.4 CONVBIN

**概要**

```shell
convbin [-ts y/m/d h:m:s] [-te y/m/d h:m:s] [-ti tint] [-r format] [-ro opts]
        [-f freq] [-hc comment] [-hm marker] [-hn markno] [-ht marktype]
        [-ho observ] [-hr rec] [-ha ant] [-hp pos] [-hd delta] [-v ver] [-od]
        [-os] [-x sat] [-y sys] [-d dir] [-c satid] [-o ofile] [-n nfile]
        [-g gfile] [-h hfile] [-q qfile] [-s sfile] file
```
convbin [-ts y/m/d h:m:s] [-te y/m/d h:m:s] [-ti tint] [-r 格式] [-ro 选项]
[-f 频率] [-hc 注释] [-hm 标记] [-hn 标记号] [-ht 标记类型]
[-ho 观测] [-hr 接收机] [-ha 天线] [-hp 位置] [-hd 偏移] [-v 版本] [-od]
[-os] [-x 卫星] [-y 系统] [-d 目录] [-c 卫星ID] [-o 输出文件] [-n 导航文件]
[-g SBAS文件] [-h 电离层文件] [-q 修正文件] [-s 状态文件] 文件

**描述**

将RTCM、接收机原始数据日志和RINEX文件转换为RINEX和SBAS/LEX消息文件。SBAS消息文件符合RTKLIB SBAS/LEX消息格式。它支持以下消息或文件：<br>
RTCM 2 : 类型 1, 3, 9, 14, 16, 17, 18, 19, 22<br>
RTCM 3 : 类型 1002, 1004, 1005, 1006, 1010, 1012, 1019, 1020 类型 1071-1127（MSM，不包括紧凑消息）<br>
NovAtel OEMV/4/6,OEMStar: RANGECMPB, RANGEB, RAWEPHEMB, IONUTCB, RAWWASSFRAMEB<br>
Swiftnav :<br>
Septentrio :<br>
u-blox M8T/F9P : RXM-RAW, RXM-SFRB, RXM-RAWX<br>
Hemisphere : BIN76, BIN80, BIN94, BIN95, BIN96<br>
SkyTraq S1315F : msg0xDD, msg0xE0, msg0xDC<br>
GW10 : msg0x08, msg0x03, msg0x27, msg0x20<br>
Javad : [R*],[r*],[R],[r],[P],[p],[*P],[p],[D],[d],[E],[E],[F],[TC],[GE],[NE],[EN],[QE],[UO],[IO],[WD]<br>
NVS : NVS NV08C BINR<br>
BINEX : 大端序，常规CRC，前向记录（0xE2）<br>
0x01-01,0x01-02,0x01-03,0x01-04,0x01-06,0x7f-05<br>
RINEX : OBS, NAV, GNAV, HNAV, LNAV, QNAV

**选项**

file 输入接收机二进制日志文件<br>
-ts y/m/d h:m:s 开始时间 [全部]<br>
-te y/m/d h:m:s 结束时间 [全部]<br>
-tr y/m/d h:m:s RTCM消息的近似时间<br>
-ti tint 观测数据间隔（秒） [全部]<br>
-tt ttol 观测数据历元容差（秒） [0.005]<br>
-r 格式 日志格式类型<br>
rtcm2= RTCM 2<br>
rtcm3= RTCM 3<br>
nov = NovAtel OEMV/4/6,OEMStar<br>
oem3 = NovAtel OEM3<br>
ubx = u-blox LEA-4T/5T/6T/7T/M8/F9<br>
sbp = Swift Navigation SBP<br>
ss2 = NovAtel Superstar II<br>
hemis= Hemisphere Eclipse/Crescent<br>
stq = SkyTraq S1315F<br>
javad= Javad<br>
nvs = NVS BINR<br>
binex= BINEX<br>
rinex= RINEX<br>
-ro opt 接收机选项<br>
-f 频率 频率数 [2]<br>
-hc 注释 RINEX头：注释行<br>
-hm 标记 RINEX头：标记名称<br>
-hn 标记号 RINEX头：标记编号<br>
-ht 标记类型 RINEX头：标记类型<br>
-ho 观测 RINEX头：观测者名称和机构，用/分隔<br>
-hr 接收机 RINEX头：接收机编号、类型和版本，用/分隔<br>
-ha 天线 RINEX头：天线编号和类型，用/分隔<br>
-hp 位置 RINEX头：近似位置x/y/z，用/分隔<br>
-hd 偏移 RINEX头：天线偏移h/e/n，用/分隔<br>
-v 版本 RINEX版本 [2.11]<br>
-od 在RINEX观测中包含多普勒频率 [关闭]<br>
-os 在RINEX观测中包含信噪比 [关闭]<br>
-oi 在RINEX导航头中包含电离层修正 [关闭]<br>
-ot 在RINEX导航头中包含时间修正 [关闭]<br>
-ol 在RINEX导航头中包含闰秒 [关闭]<br>
-halfc 半周模糊度修正 [关闭]<br>
-mask [sig[,...]] 信号掩码（sig={G|R|E|J|S|C|I}L{1C|1P|1W|...}）<br>
-nomask [sig[,...]] 信号不掩码（同上）<br>
-x 卫星 排除卫星<br>
-y 系统 排除系统（G:GPS, R:GLONASS, E:Galileo, J:QZSS, S:SBAS, C:北斗）<br>
-d 目录 输出目录 [与输入文件相同]<br>
-c 站ID 使用带有站ID的RINEX文件命名约定 [关闭]<br>
-o ofile 输出RINEX OBS文件<br>
-n nfile 输出RINEX NAV文件<br>
-g gfile 输出RINEX GNAV文件<br>
-h hfile 输出RINEX HNAV文件<br>
-q qfile 输出RINEX QNAV文件<br>
-l lfile 输出RINEX LNAV文件<br>
-b cfile 输出RINEX CNAV文件<br>
-i ifile 输出RINEX INAV文件<br>
-s sfile 输出SBAS消息文件<br>
-trace 级别 输出跟踪级别 [关闭]<br>

如果指定了任何输出文件，则使用默认输出文件（`<file>.obs`, `<file>.nav`, `<file>.gnav`, `<file>.hnav`, `<file>.qnav`, `<file>.lnav` 和 `<file>.sbs`）。<br>
如果未指定接收机类型，则根据输入文件扩展名识别类型，如下所示：<br>
*.rtcm2 RTCM 2<br>
*.rtcm3 RTCM 3<br>
*.gps NovAtel OEMV/4/6,OEMStar<br>
*.ubx u-blox LEA-4T/5T/6T/8T/8/9<br>
*.bin Hemisphere Eclipse/Crescent<br>
*.stq SkyTraq S1315F<br>
*.jps Javad<br>
*.bnx,*binex BINEX<br>
.obs,.*o RINEX OBS

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
rtcm2 : RTCM 2（仅输入）
rtcm3 : RTCM 3（输入和输出）
nov : NovAtel OEMV/4/6,OEMStar（仅输入）
swiftnav : Swiftnav（仅输入）
ubx : u-blox LEA-4T/5T/6T/8/9（仅输入）
hemis : Hemisphere Eclipse/Crescent（仅输入）
stq : SkyTraq S1315F（仅输入）
javad : Javad（仅输入）
sbf : Septentrio（仅输入）
nvs : NVS BINR（仅输入）
binex : BINEX（仅输入）
-msg 类型[(时间间隔)][,类型[(时间间隔)]...]
RTCM消息类型和输出间隔（秒）
-sta 站ID 站点ID
-opt 选项 接收机依赖选项
-s 毫秒 超时时间（毫秒） [10000]
-r 毫秒 重新连接间隔（毫秒） [10000]
-n 毫秒 NMEA请求周期（毫秒） [0]
-f 秒 文件交换边界（秒） [30]
-c 文件 输入命令文件 [无]
-c1 文件 输出1命令文件 [无]
-c2 文件 输出2命令文件 [无]
-c3 文件 输出3命令文件 [无]
-c4 文件 输出4命令文件 [无]
-p 纬度 经度 高度 站点位置（纬度/经度/高度）（度，米）
-px x y z 站点位置（x/y/z-ECEF）（米）
-a 天线信息 天线信息（用逗号分隔）
-i 接收机信息 接收机信息（用逗号分隔）
-o 东 北 上 天线偏移（东，北，上）（米）
-l 本地目录 FTP/HTTP本地目录 []
-x 代理地址 HTTP/NTRIP代理地址 [无]
-b 输出流号 将输出流的消息回传到输入流 [无]
-t 级别 跟踪级别 [0]
-fl 文件 日志文件 [str2str.trace]
-h 打印帮助信息
```
