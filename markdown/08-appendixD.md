---
sidebarDepth: 2
---

# 附录D. 文件与报文

## D.1 支持的 RINEX 格式

以下是 RTKLIB 支持的 RINEX 版本和文件类型，如表所示。

<style type="text/css">
.tg .tg-9wq8{text-align:center !important; vertical-align:middle !important}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-9wq8" rowspan="2">RINEX VERSION</th>
    <th class="tg-9wq8" colspan="6">Observation Data</th>
    <th class="tg-9wq8" colspan="6">Navigation Messages</th>
    <th class="tg-9wq8" rowspan="2">Met.</th>
    <th class="tg-9wq8" rowspan="2">CLK</th>
    <th class="tg-9wq8" rowspan="2">GEO<br>BRDC</th>
  </tr>
  <tr>
    <th class="tg-9wq8">G</th>
    <th class="tg-9wq8">R</th>
    <th class="tg-9wq8">E</th>
    <th class="tg-9wq8">J</th>
    <th class="tg-9wq8">C</th>
    <th class="tg-9wq8">S</th>
    <th class="tg-9wq8">G</th>
    <th class="tg-9wq8">R</th>
    <th class="tg-9wq8">E</th>
    <th class="tg-9wq8">J</th>
    <th class="tg-9wq8">C</th>
    <th class="tg-9wq8">S</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-9wq8">2.10</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O*</td>
    <td class="tg-9wq8">O*</td>
    <td class="tg-9wq8">O*</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">G</td>
    <td class="tg-9wq8">N*</td>
    <td class="tg-9wq8">N*</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">H</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
  </tr>
  <tr>
    <td class="tg-9wq8">2.11</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O*</td>
    <td class="tg-9wq8">O*</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">G</td>
    <td class="tg-9wq8">N*</td>
    <td class="tg-9wq8">N*</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">H</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
  </tr>
  <tr>
    <td class="tg-9wq8">2.12</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O*</td>
    <td class="tg-9wq8">O*</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">G</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N*</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">H</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
  </tr>
  <tr>
    <td class="tg-9wq8">3.00</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O*</td>
    <td class="tg-9wq8">O*</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N*</td>
    <td class="tg-9wq8">N*</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">C**</td>
    <td class="tg-9wq8">-</td>
  </tr>
  <tr>
    <td class="tg-9wq8">3.01</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O*</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N*</td>
    <td class="tg-9wq8">N*</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">C**</td>
    <td class="tg-9wq8">-</td>
  </tr>
  <tr>
    <td class="tg-9wq8">3.02</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">O</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">N</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">C**</td>
    <td class="tg-9wq8">-</td>
  </tr>
</tbody></table>

G: GPS, R: GLONASS, E: Galileo, J: QZSS, C: BeiDou, S: SBAS <br>
-: 不支持, O,N,G,H: 支持的RINEX类型 <br>
\* RTKLIB 扩展 (QZSS扩展基于JAX [60][61]), ** 只读

## D.2 支持的接收机消息
RTKLIB支持的RTCM 2、RTCM 3、BINEX以及各接收机私有信息格式如下表所示。

<table class="tg"><thead>
  <tr>
    <th class="tg-9wq8" rowspan="2">格式</th>
    <th class="tg-9wq8" colspan="6">消息类型</th>
  </tr>
  <tr>
    <th class="tg-9wq8">原始观测数据</th>
    <th class="tg-9wq8">卫星星历</th>
    <th class="tg-9wq8">ION/UTC参数</th>
    <th class="tg-9wq8">卫星信息</th>
    <th class="tg-9wq8">SBAS消息</th>
    <th class="tg-9wq8">其他</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-9wq8">RTCM2 [16]</td>
    <td class="tg-9wq8">18,19</td>
    <td class="tg-9wq8">17</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">3,22</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">1*,9*,14,16</td>
  </tr>
  <tr>
    <td class="tg-9wq8">RTCM3 [17][18]</td>
    <td class="tg-9wq8">18,19</td>
    <td class="tg-9wq8">17</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">3,22</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">1*,9*,14,16</td>
  </tr>
  <tr>
    <td class="tg-9wq8">BINEX [19] **</td>
    <td class="tg-9wq8">0x7f-05<br>(Trimble NetR8)</td>
    <td class="tg-9wq8">0x01-01, <br>0x01-02, <br>0x01-03, <br>0x01-04, <br>0x01-06</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
  </tr>
  <tr>
    <td class="tg-9wq8">NovAtel<br>OEM4/V/6 [41][42]</td>
    <td class="tg-9wq8">RANGEB,<br>RANGECMPB</td>
    <td class="tg-9wq8">RAWEPHEMB, <br>GLO-<br>EPHEMERISB, <br>QZSS-<br>RAWEPHEMB, <br>GAL-<br>EPHEMERISB</td>
    <td class="tg-9wq8">IONUTCB, <br>QZSS- IONUTCB, <br>GALIONOB, <br>GALCLOCKB</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">RAWWAAS-FRAMEB,<br>RAWSBAS-FRAMEB,<br>QZSSRAW-SUBFRAMEB</td>
    <td class="tg-9wq8">-</td>
  </tr>
  <tr>
    <td class="tg-9wq8">NovAtel OEM3<br>[43]</td>
    <td class="tg-9wq8">RGEB,<br>RGED</td>
    <td class="tg-9wq8">REPB</td>
    <td class="tg-9wq8">IONB,<br>UTCB</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">FRMB</td>
    <td class="tg-9wq8">-</td>
  </tr>
  <tr>
    <td class="tg-9wq8">u-blox<br>LEA-4T/5T/<br>6T/8/9 [44]</td>
    <td class="tg-9wq8">UBX<br>RXM-RAW<br>RXM-RAWX</td>
    <td class="tg-9wq8">UBX RXM-SFRB</td>
    <td class="tg-9wq8">UBX RXM-SFRB</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">UBX RXM-SFRB</td>
    <td class="tg-9wq8">-</td>
  </tr>
  <tr>
    <td class="tg-9wq8">NovAtel Superstar II<br>[45]</td>
    <td class="tg-9wq8">ID#23</td>
    <td class="tg-9wq8">ID#22</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">ID#67</td>
    <td class="tg-9wq8">ID#20,<br>ID#21</td>
  </tr>
  <tr>
    <td class="tg-9wq8">Hemisphere Crescent,<br>Eclipse<br>[46][47]</td>
    <td class="tg-9wq8">bin 96,<br>bin 76</td>
    <td class="tg-9wq8">bin 95</td>
    <td class="tg-9wq8">bin 94</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">bin 80</td>
    <td class="tg-9wq8">-</td>
  </tr>
  <tr>
    <td class="tg-9wq8">SkyTraq<br>S1315F<br>[48][49]</td>
    <td class="tg-9wq8">msg 0xDD<br>(221)</td>
    <td class="tg-9wq8">msg 0xE0<br>(224)</td>
    <td class="tg-9wq8">msg 0xE0<br>(224)</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">msg 0xDC<br>(220)</td>
  </tr>
  <tr>
    <td class="tg-9wq8">Furuno GW-10-II/III<br>[50]</td>
    <td class="tg-9wq8">msg 0x08</td>
    <td class="tg-9wq8">msg 0x27</td>
    <td class="tg-9wq8">msg 0x27</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">msg 0x27</td>
    <td class="tg-9wq8">msg 0x20</td>
  </tr>
  <tr>
    <td class="tg-9wq8">JAVAD<br>GRIL/GRIES<br>[51][52][53][54]</td>
    <td class="tg-9wq8">[RC],[rc],[CR],<br>[cr],[PC],[pc],<br>[CP],[cp],[DC],<br>[cd],[EC],[CE],<br>[FC],[R1],[r1],<br>[1R],[1r],[P1],<br>[p1],[1P],[1p],<br>[D1],[1d],[E1],<br>[1E],[F1],[R2],<br>[r2],[2R],[2r],<br>[P2],[p2],[2P],<br>[2p],[D2],[2d],<br>[E2],[2E],[F2],<br>[R3],[r3],[3R],<br>[3r],[P3],[p3],<br>[3P],[3p],[D3],<br>[3d],[E3],[3E],<br>[F3],[R5],[r5],<br>[5R],[5r],[P5],<br>[p5],[5P],[5p],<br>[D5],[5d],[E5],<br>[5E],[F5],[Rl],<br>[rl],[lR],[lr],<br>[Pl],[pl],[lP],<br>[lp],[Dl],[ld],<br>[El],[lE],[Fl],<br>[TC]<br></td>
    <td class="tg-9wq8">[GE],[NE],<br>[EN],[WE],<br>[QE]</td>
    <td class="tg-9wq8">[UO],[NU],<br>[EU],[WU],<br>[QU],[IO]</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">[WD]</td>
    <td class="tg-9wq8">[~~],[::],<br>[RD],[ST],<br>[NN]</td>
  </tr>
  <tr>
    <td class="tg-9wq8">NVS<br>NV08C<br>[55][56]</td>
    <td class="tg-9wq8">msg F5h</td>
    <td class="tg-9wq8">msg F7h</td>
    <td class="tg-9wq8">msg 4Ah,<br>msg 4Bh</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
    <td class="tg-9wq8">-</td>
  </tr>
</tbody></table>

\* 仅支持读取，不支持DGPS修正  
** 仅支持大端序、正向和标准CRC消息  

支持的RTCM 3消息类型。
<style type="text/css">
.tg .tg-cly1{text-align:left;vertical-align:middle}
</style>
<table class="tg"><thead>
  <tr>
    <th class="tg-9wq8">消息</th>
    <th class="tg-9wq8">GPS</th>
    <th class="tg-9wq8">GLONASS</th>
    <th class="tg-9wq8">Galileo</th>
    <th class="tg-9wq8">QZSS</th>
    <th class="tg-9wq8">BeiDou</th>
    <th class="tg-9wq8">SBAS</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-cly1">OBS Compact L1<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Full L1<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Compact L1/2<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Full L1/2<br></td>
    <td class="tg-9wq8">1001~<br>1002&nbsp;&nbsp;<br>1003~<br>1004&nbsp;&nbsp;</td>
    <td class="tg-9wq8">1009~<br>1010&nbsp;&nbsp;<br>1011~<br>1012&nbsp;&nbsp;</td>
    <td class="tg-9wq8">-<br>-<br>-<br>-</td>
    <td class="tg-9wq8">-<br>-<br>-<br>-</td>
    <td class="tg-9wq8">-<br>-<br>-<br>-</td>
    <td class="tg-9wq8">-<br>-<br>-<br>-</td>
  </tr>
  <tr>
    <td class="tg-cly1">Ephemeris</td>
    <td class="tg-9wq8">1019<br>-</td>
    <td class="tg-9wq8">1020<br>-</td>
    <td class="tg-9wq8">1045*<br>1046*</td>
    <td class="tg-9wq8">1044*<br>-</td>
    <td class="tg-9wq8">-<br>-</td>
    <td class="tg-9wq8">-<br>-</td>
  </tr>
  <tr>
    <td class="tg-cly1">MSM 1<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7</td>
    <td class="tg-9wq8">1071~<br>1072~<br>1073~<br>1074&nbsp;&nbsp;<br>1075&nbsp;&nbsp;<br>1076&nbsp;&nbsp;<br>1077&nbsp;&nbsp;</td>
    <td class="tg-9wq8">1081~<br>1082~<br>1083~<br>1084&nbsp;&nbsp;<br>1085&nbsp;&nbsp;<br>1086&nbsp;&nbsp;<br>1087&nbsp;&nbsp;</td>
    <td class="tg-9wq8">1091~<br>1092~<br>1093~<br>1094&nbsp;&nbsp;<br>1095&nbsp;&nbsp;<br>1096&nbsp;&nbsp;<br>1097&nbsp;&nbsp;</td>
    <td class="tg-9wq8">1111*~<br>1112*~<br>1113*~<br>1114*  <br>1115*  <br>1116*  <br>1117*  </td>
    <td class="tg-9wq8">1121*~<br>1122*~<br>1123*~<br>1124*  <br>1125*  <br>1126*  <br>1127*  </td>
    <td class="tg-9wq8">1101*~<br>1102*~<br>1103*~<br>1104*  <br>1105*  <br>1106*  <br>1107*  </td>
  </tr>
  <tr>
    <td class="tg-cly1">SSR Orbit Corr.<br>        Clock Corr.<br>        Code Bias<br>        Combined<br>        URA<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;HR-Clock</td>
    <td class="tg-9wq8">1057<br>1058<br>1059<br>1060<br>1061<br>1062</td>
    <td class="tg-9wq8">1063<br>1064<br>1065<br>1066<br>1067<br>1068</td>
    <td class="tg-9wq8">1240*<br>1241*<br>1242*<br>1243*<br>1244*<br>1245*</td>
    <td class="tg-9wq8">1246*<br>1247*<br>1248*<br>1249*<br>1250*<br>1251*</td>
    <td class="tg-9wq8">-<br>-<br>-<br>-<br>-<br>-</td>
    <td class="tg-9wq8">-<br>-<br>-<br>-<br>-<br>-</td>
  </tr>
  <tr>
    <td class="tg-cly1">Antenna Info</td>
    <td class="tg-9wq8" colspan="6">1005 1006 1007 1008 1033</td>
  </tr>
</tbody></table>
* 草案, ~ 仅适用于编码

## D.3 支持的信号标识/观测类型
RTKLIB支持的信号标识/观测类型如下表所示。该表还列出了对应的RINEX 2和RINEX 3观测类型、RTCM 3 MSM信号标识以及BINEX观测码标识。对于RTKCONV和CONVBIN中的RTCM 2、RTCM 3和RINEX到RINEX转换，以及STRSVR和STR2STR中的RTCM 2和BINEX到RTCM 3转换，该表同样适用。

<table class="tg"><thead>
  <tr>
    <th class="tg-9wq8">系统</th>
    <th class="tg-9wq8">频段</th>
    <th class="tg-9wq8">通道或码</th>
    <th class="tg-9wq8">信号ID</th>
    <th class="tg-9wq8">RINEX2<br>*1</th>
    <th class="tg-9wq8">RINEX3<br>*2</th>
    <th class="tg-9wq8">RTCM3<br>*3</th>
    <th class="tg-9wq8">BINEX</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-lboi" rowspan="22">GPS</td>
    <td class="tg-lboi" rowspan="9">L1</td>
    <td class="tg-lboi">C/A</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">C1/CA*</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">2</td>
    <td class="tg-lboi">0,1</td>
  </tr>
  <tr>
    <td class="tg-lboi">L1C(D)</td>
    <td class="tg-lboi">1S</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1S</td>
    <td class="tg-lboi">30</td>
    <td class="tg-lboi">-</td>
  </tr>
  <tr>
    <td class="tg-lboi">L1C(P)</td>
    <td class="tg-lboi">1L</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1L</td>
    <td class="tg-lboi">31</td>
    <td class="tg-lboi">-</td>
  </tr>
  <tr>
    <td class="tg-lboi">L1C(D+P)</td>
    <td class="tg-lboi">1X</td>
    <td class="tg-lboi">CB*</td>
    <td class="tg-lboi">1X</td>
    <td class="tg-lboi">32</td>
    <td class="tg-lboi">6</td>
  </tr>
  <tr>
    <td class="tg-lboi">P</td>
    <td class="tg-lboi">1P</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1P</td>
    <td class="tg-lboi">3</td>
    <td class="tg-lboi">2</td>
  </tr>
  <tr>
    <td class="tg-lboi">Z-tracking and similar (AS on)</td>
    <td class="tg-lboi">1W</td>
    <td class="tg-lboi">P1/C1*</td>
    <td class="tg-lboi">1W</td>
    <td class="tg-lboi">4</td>
    <td class="tg-lboi">3</td>
  </tr>
  <tr>
    <td class="tg-lboi">Y</td>
    <td class="tg-lboi">1Y</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1M</td>
    <td class="tg-lboi">5</td>
    <td class="tg-lboi">4</td>
  </tr>
  <tr>
    <td class="tg-lboi">M</td>
    <td class="tg-lboi">1M</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1M</td>
    <td class="tg-lboi">6</td>
    <td class="tg-lboi">5</td>
  </tr>
  <tr>
    <td class="tg-lboi">codeless</td>
    <td class="tg-lboi">1N</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1N</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">7</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="10">L2</td>
    <td class="tg-lboi">C/A</td>
    <td class="tg-lboi">2C</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2C</td>
    <td class="tg-lboi">8</td>
    <td class="tg-lboi">11</td>
  </tr>
  <tr>
    <td class="tg-lboi">L1(C/A)+(P2-P1) (semi-codeless)</td>
    <td class="tg-lboi">2D</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2C</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">12</td>
  </tr>
  <tr>
    <td class="tg-lboi">L2C(M)</td>
    <td class="tg-lboi">2S</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2S</td>
    <td class="tg-lboi">15</td>
    <td class="tg-lboi">13</td>
  </tr>
  <tr>
    <td class="tg-lboi">L2C(L)</td>
    <td class="tg-lboi">2L</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2L</td>
    <td class="tg-lboi">16</td>
    <td class="tg-lboi">14</td>
  </tr>
  <tr>
    <td class="tg-lboi">L2C(M+L)</td>
    <td class="tg-lboi">2X</td>
    <td class="tg-lboi">C2/CC*</td>
    <td class="tg-lboi">2X</td>
    <td class="tg-lboi">17</td>
    <td class="tg-lboi">15</td>
  </tr>
  <tr>
    <td class="tg-lboi">P</td>
    <td class="tg-lboi">2P</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2P</td>
    <td class="tg-lboi">9</td>
    <td class="tg-lboi">16</td>
  </tr>
  <tr>
    <td class="tg-lboi">Z-tracking and similar (AS on)</td>
    <td class="tg-lboi">2W</td>
    <td class="tg-lboi">P2</td>
    <td class="tg-lboi">2W</td>
    <td class="tg-lboi">10</td>
    <td class="tg-lboi">10,17</td>
  </tr>
  <tr>
    <td class="tg-lboi">Y</td>
    <td class="tg-lboi">2Y</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2Y</td>
    <td class="tg-lboi">11</td>
    <td class="tg-lboi">18</td>
  </tr>
  <tr>
    <td class="tg-lboi">M</td>
    <td class="tg-lboi">2M</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2M</td>
    <td class="tg-lboi">12</td>
    <td class="tg-lboi">19</td>
  </tr>
  <tr>
    <td class="tg-lboi">codeless</td>
    <td class="tg-lboi">codeless</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2N</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">20</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">L5</td>
    <td class="tg-lboi">I</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">22</td>
    <td class="tg-lboi">24</td>
  </tr>
  <tr>
    <td class="tg-lboi">Q</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">23</td>
    <td class="tg-lboi">25</td>
  </tr>
  <tr>
    <td class="tg-lboi">I+Q</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi">C5</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi">24</td>
    <td class="tg-lboi">23,26</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="7">GLONASS</td>
    <td class="tg-lboi" rowspan="2">G1</td>
    <td class="tg-lboi">C/A</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">C1/CA*</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">2</td>
    <td class="tg-lboi">0,1</td>
  </tr>
  <tr>
    <td class="tg-lboi">P</td>
    <td class="tg-lboi">1P</td>
    <td class="tg-lboi">P1/C1*</td>
    <td class="tg-lboi">1P</td>
    <td class="tg-lboi">3</td>
    <td class="tg-lboi">2</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="2">G2</td>
    <td class="tg-lboi">C/A</td>
    <td class="tg-lboi">2C</td>
    <td class="tg-lboi">C2/CD*</td>
    <td class="tg-lboi">2C</td>
    <td class="tg-lboi">8</td>
    <td class="tg-lboi">10,11</td>
  </tr>
  <tr>
    <td class="tg-lboi">P</td>
    <td class="tg-lboi">2P</td>
    <td class="tg-lboi">P2</td>
    <td class="tg-lboi">2P</td>
    <td class="tg-lboi">9</td>
    <td class="tg-lboi">12</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">G3</td>
    <td class="tg-lboi">I</td>
    <td class="tg-lboi">3I</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">3I</td>
    <td class="tg-lboi">11</td>
    <td class="tg-lboi">14</td>
  </tr>
  <tr>
    <td class="tg-lboi">Q</td>
    <td class="tg-lboi">3Q</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">3Q</td>
    <td class="tg-lboi">12</td>
    <td class="tg-lboi">15</td>
  </tr>
  <tr>
    <td class="tg-lboi">I+Q</td>
    <td class="tg-lboi">3X</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">3X</td>
    <td class="tg-lboi">13</td>
    <td class="tg-lboi">13,16</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="19">Galileo</td>
    <td class="tg-lboi" rowspan="5">E1</td>
    <td class="tg-lboi">A PRS</td>
    <td class="tg-lboi">1A</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1A</td>
    <td class="tg-lboi">3</td>
    <td class="tg-lboi">1</td>
  </tr>
  <tr>
    <td class="tg-lboi">B I/NAV OS/CS/SoL</td>
    <td class="tg-lboi">1B</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1B</td>
    <td class="tg-lboi">4</td>
    <td class="tg-lboi">2</td>
  </tr>
  <tr>
    <td class="tg-lboi">C</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">C1</td>
    <td class="tg-lboi">2</td>
    <td class="tg-lboi">0,3</td>
  </tr>
  <tr>
    <td class="tg-lboi">B+C</td>
    <td class="tg-lboi">1X</td>
    <td class="tg-lboi">C1</td>
    <td class="tg-lboi">1X</td>
    <td class="tg-lboi">5</td>
    <td class="tg-lboi">4</td>
  </tr>
  <tr>
    <td class="tg-lboi">A+B+C</td>
    <td class="tg-lboi">1Z</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1Z</td>
    <td class="tg-lboi">6</td>
    <td class="tg-lboi">5</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">E5a</td>
    <td class="tg-lboi">I F/NAV OS</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">22</td>
    <td class="tg-lboi">7</td>
  </tr>
  <tr>
    <td class="tg-lboi">Q no data</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">23</td>
    <td class="tg-lboi">8</td>
  </tr>
  <tr>
    <td class="tg-lboi">I+Q</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi">C5</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi">24</td>
    <td class="tg-lboi">6,9</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">E5b</td>
    <td class="tg-lboi">I I/NAV OS/CS/SoL</td>
    <td class="tg-lboi">7I</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">7I</td>
    <td class="tg-lboi">14</td>
    <td class="tg-lboi">11</td>
  </tr>
  <tr>
    <td class="tg-lboi">Q no data</td>
    <td class="tg-lboi">7Q</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">7Q</td>
    <td class="tg-lboi">15</td>
    <td class="tg-lboi">12</td>
  </tr>
  <tr>
    <td class="tg-lboi">I+Q</td>
    <td class="tg-lboi">7X</td>
    <td class="tg-lboi">C7</td>
    <td class="tg-lboi">7X</td>
    <td class="tg-lboi">16</td>
    <td class="tg-lboi">10,13</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">E5a+E5b</td>
    <td class="tg-lboi">I</td>
    <td class="tg-lboi">8I</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">8I</td>
    <td class="tg-lboi">18</td>
    <td class="tg-lboi">15</td>
  </tr>
  <tr>
    <td class="tg-lboi">Q</td>
    <td class="tg-lboi">8Q</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">8Q</td>
    <td class="tg-lboi">19</td>
    <td class="tg-lboi">16</td>
  </tr>
  <tr>
    <td class="tg-lboi">I+Q</td>
    <td class="tg-lboi">8X</td>
    <td class="tg-lboi">C8</td>
    <td class="tg-lboi">8X</td>
    <td class="tg-lboi">20</td>
    <td class="tg-lboi">14,17</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="5">E6</td>
    <td class="tg-lboi">A PRS</td>
    <td class="tg-lboi">6A</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">6A</td>
    <td class="tg-lboi">9</td>
    <td class="tg-lboi">19</td>
  </tr>
  <tr>
    <td class="tg-lboi">B C/NAV CS</td>
    <td class="tg-lboi">6B</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">6B</td>
    <td class="tg-lboi">10</td>
    <td class="tg-lboi">20</td>
  </tr>
  <tr>
    <td class="tg-lboi">C no data</td>
    <td class="tg-lboi">6C</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">6C</td>
    <td class="tg-lboi">8</td>
    <td class="tg-lboi">21</td>
  </tr>
  <tr>
    <td class="tg-lboi">B+C</td>
    <td class="tg-lboi">6X</td>
    <td class="tg-lboi">C6</td>
    <td class="tg-lboi">6X</td>
    <td class="tg-lboi">11</td>
    <td class="tg-lboi">18,22</td>
  </tr>
  <tr>
    <td class="tg-lboi">A+B+C</td>
    <td class="tg-lboi">6Z</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">6Z</td>
    <td class="tg-lboi">12</td>
    <td class="tg-lboi">23</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="14">QZSS</td>
    <td class="tg-lboi" rowspan="5">L1</td>
    <td class="tg-lboi">C/A</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">C1/CA*</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">2</td>
    <td class="tg-lboi">0,1</td>
  </tr>
  <tr>
    <td class="tg-lboi">L1C(D)</td>
    <td class="tg-lboi">1S</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1S</td>
    <td class="tg-lboi">30</td>
    <td class="tg-lboi">2</td>
  </tr>
  <tr>
    <td class="tg-lboi">L1C(P)</td>
    <td class="tg-lboi">1L</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1L</td>
    <td class="tg-lboi">31</td>
    <td class="tg-lboi">3</td>
  </tr>
  <tr>
    <td class="tg-lboi">L1C(D+P)</td>
    <td class="tg-lboi">1X</td>
    <td class="tg-lboi">CB*</td>
    <td class="tg-lboi">1X</td>
    <td class="tg-lboi">32</td>
    <td class="tg-lboi">4</td>
  </tr>
  <tr>
    <td class="tg-lboi">L1-SAIF</td>
    <td class="tg-lboi">1Z</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">1Z</td>
    <td class="tg-lboi">6</td>
    <td class="tg-lboi">30</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">L2</td>
    <td class="tg-lboi">L2C(M)</td>
    <td class="tg-lboi">2S</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2S</td>
    <td class="tg-lboi">15</td>
    <td class="tg-lboi">8</td>
  </tr>
  <tr>
    <td class="tg-lboi">L2C(L)</td>
    <td class="tg-lboi">2L</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2L</td>
    <td class="tg-lboi">16</td>
    <td class="tg-lboi">9</td>
  </tr>
  <tr>
    <td class="tg-lboi">L2C(M+L)</td>
    <td class="tg-lboi">2X</td>
    <td class="tg-lboi">C2/CC*</td>
    <td class="tg-lboi">2X</td>
    <td class="tg-lboi">17</td>
    <td class="tg-lboi">7,10</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">L5</td>
    <td class="tg-lboi">I</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">22</td>
    <td class="tg-lboi">14</td>
  </tr>
  <tr>
    <td class="tg-lboi">Q</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">23</td>
    <td class="tg-lboi">15</td>
  </tr>
  <tr>
    <td class="tg-lboi">I+Q</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi">24</td>
    <td class="tg-lboi">13,16</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">LEX</td>
    <td class="tg-lboi">S</td>
    <td class="tg-lboi">6S</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">6S</td>
    <td class="tg-lboi">9</td>
    <td class="tg-lboi">20</td>
  </tr>
  <tr>
    <td class="tg-lboi">L</td>
    <td class="tg-lboi">6L</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">6L</td>
    <td class="tg-lboi">10</td>
    <td class="tg-lboi">21</td>
  </tr>
  <tr>
    <td class="tg-lboi">S+L</td>
    <td class="tg-lboi">6X</td>
    <td class="tg-lboi">C6</td>
    <td class="tg-lboi">6X</td>
    <td class="tg-lboi">11</td>
    <td class="tg-lboi">19,22</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="9">BeiDou</td>
    <td class="tg-lboi" rowspan="3">B1</td>
    <td class="tg-lboi">I</td>
    <td class="tg-lboi">2I</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2I</td>
    <td class="tg-lboi">2</td>
    <td class="tg-lboi">1</td>
  </tr>
  <tr>
    <td class="tg-lboi">Q</td>
    <td class="tg-lboi">2Q</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">2Q</td>
    <td class="tg-lboi">3</td>
    <td class="tg-lboi">2</td>
  </tr>
  <tr>
    <td class="tg-lboi">I+Q</td>
    <td class="tg-lboi">2X</td>
    <td class="tg-lboi">C2</td>
    <td class="tg-lboi">2X</td>
    <td class="tg-lboi">4</td>
    <td class="tg-lboi">0,3</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">B2</td>
    <td class="tg-lboi">I</td>
    <td class="tg-lboi">7I</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">7I</td>
    <td class="tg-lboi">14</td>
    <td class="tg-lboi">5</td>
  </tr>
  <tr>
    <td class="tg-lboi">Q</td>
    <td class="tg-lboi">7Q</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">7Q</td>
    <td class="tg-lboi">15</td>
    <td class="tg-lboi">6</td>
  </tr>
  <tr>
    <td class="tg-lboi">I+Q</td>
    <td class="tg-lboi">7X</td>
    <td class="tg-lboi">C7</td>
    <td class="tg-lboi">7X</td>
    <td class="tg-lboi">16</td>
    <td class="tg-lboi">4,7</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">B3</td>
    <td class="tg-lboi">I</td>
    <td class="tg-lboi">6I</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">6I</td>
    <td class="tg-lboi">8</td>
    <td class="tg-lboi">9</td>
  </tr>
  <tr>
    <td class="tg-lboi">Q</td>
    <td class="tg-lboi">6Q</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">6Q</td>
    <td class="tg-lboi">9</td>
    <td class="tg-lboi">10</td>
  </tr>
  <tr>
    <td class="tg-lboi">I+Q</td>
    <td class="tg-lboi">6X</td>
    <td class="tg-lboi">C6</td>
    <td class="tg-lboi">6X</td>
    <td class="tg-lboi">10</td>
    <td class="tg-lboi">8,11</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="4">SBAS</td>
    <td class="tg-lboi">L1</td>
    <td class="tg-lboi">C/A</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">C1/CA*</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">2</td>
    <td class="tg-lboi">0,1</td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">L5</td>
    <td class="tg-lboi">I</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">22</td>
    <td class="tg-lboi">7</td>
  </tr>
  <tr>
    <td class="tg-lboi">Q</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">-</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">23</td>
    <td class="tg-lboi">8</td>
  </tr>
  <tr>
    <td class="tg-lboi">I+Q</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi">C5</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi">24</td>
    <td class="tg-lboi">6,9</td>
  </tr>
</tbody></table>

## D.4 多信号的默认优先级

如果输入的观测数据在同一频率下包含多个信号，RTKLIB 会根据以下默认优先级选择用于处理的信号。要选择适当的信号，请使用 RINEX 选项或附录 D.5 中描述的接收机相关选项。在用户应用程序（APs）中，可使用 API setcodepri() 或 getcodepri() 来更改或获取信号优先级。

<table class="tg"><thead>
  <tr>
    <th class="tg-9wq8" rowspan="2">系统</th>
    <th class="tg-9wq8" rowspan="2">频段</th>
    <th class="tg-9wq8" colspan="10">信号优先级（1: 最高优先级 &gt; 10: 最低优先级）*</th>
  </tr>
  <tr>
    <th class="tg-9wq8">1</th>
    <th class="tg-9wq8">2</th>
    <th class="tg-9wq8">3</th>
    <th class="tg-9wq8">4</th>
    <th class="tg-9wq8">5</th>
    <th class="tg-9wq8">6</th>
    <th class="tg-9wq8">7</th>
    <th class="tg-9wq8">8</th>
    <th class="tg-9wq8">9</th>
    <th class="tg-9wq8">10</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-lboi" rowspan="3">GPS</td>
    <td class="tg-lboi">L1</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">1P</td>
    <td class="tg-lboi">1Y</td>
    <td class="tg-lboi">1W</td>
    <td class="tg-lboi">1M</td>
    <td class="tg-lboi">1N</td>
    <td class="tg-lboi">1S</td>
    <td class="tg-lboi">1L</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">L2</td>
    <td class="tg-lboi">2C</td>
    <td class="tg-lboi">2P</td>
    <td class="tg-lboi">2Y</td>
    <td class="tg-lboi">2W</td>
    <td class="tg-lboi">2M</td>
    <td class="tg-lboi">2N</td>
    <td class="tg-lboi">2D</td>
    <td class="tg-lboi">2L</td>
    <td class="tg-lboi">2S</td>
    <td class="tg-lboi">2X</td>
  </tr>
  <tr>
    <td class="tg-lboi">L5</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="3">GLONASS</td>
    <td class="tg-lboi">G1</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">1P</td>
    <td class="tg-lboi">1A</td>
    <td class="tg-lboi">1B</td>
    <td class="tg-lboi">1X</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">G2</td>
    <td class="tg-lboi">2C</td>
    <td class="tg-lboi">2P</td>
    <td class="tg-lboi">2A</td>
    <td class="tg-lboi">2B</td>
    <td class="tg-lboi">2X</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">G3</td>
    <td class="tg-lboi">3I</td>
    <td class="tg-lboi">3Q</td>
    <td class="tg-lboi">3X</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="5">Galileo</td>
    <td class="tg-lboi">E1</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">1A</td>
    <td class="tg-lboi">1B</td>
    <td class="tg-lboi">1X</td>
    <td class="tg-lboi">1Z</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">E5a</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">E5b</td>
    <td class="tg-lboi">7I</td>
    <td class="tg-lboi">7Q</td>
    <td class="tg-lboi">7X</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">E5a+E5b</td>
    <td class="tg-lboi">8I</td>
    <td class="tg-lboi">8Q</td>
    <td class="tg-lboi">8X</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">E6</td>
    <td class="tg-lboi">6A</td>
    <td class="tg-lboi">6B</td>
    <td class="tg-lboi">6C</td>
    <td class="tg-lboi">6X</td>
    <td class="tg-lboi">6Z</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="4">QZSS</td>
    <td class="tg-lboi">L1</td>
    <td class="tg-lboi">1C</td>
    <td class="tg-lboi">1L</td>
    <td class="tg-lboi">1S</td>
    <td class="tg-lboi">1X</td>
    <td class="tg-lboi">1Z</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">L2</td>
    <td class="tg-lboi">2L</td>
    <td class="tg-lboi">2S</td>
    <td class="tg-lboi">2X</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">L5</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi">5D</td>
    <td class="tg-lboi">5P</td>
    <td class="tg-lboi">5Z</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">LEX</td>
    <td class="tg-lboi">6L</td>
    <td class="tg-lboi">6S</td>
    <td class="tg-lboi">6X</td>
    <td class="tg-lboi">6E</td>
    <td class="tg-lboi">6Z</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">BeiDou</td>
    <td class="tg-lboi">B1</td>
    <td class="tg-lboi">2I</td>
    <td class="tg-lboi">2Q</td>
    <td class="tg-lboi">2X</td>
    <td class="tg-lboi">2D</td>
    <td class="tg-lboi">2P</td>
    <td class="tg-lboi">2A</td>
    <td class="tg-lboi">2N</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi"></td>
    <td class="tg-lboi">B2</td>
    <td class="tg-lboi">7I</td>
    <td class="tg-lboi">7Q</td>
    <td class="tg-lboi">7X</td>
    <td class="tg-lboi">7D</td>
    <td class="tg-lboi">7P</td>
    <td class="tg-lboi">7Z</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi"></td>
    <td class="tg-lboi">B3</td>
    <td class="tg-lboi">6I</td>
    <td class="tg-lboi">6Q</td>
    <td class="tg-lboi">6X</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi" rowspan="2">SBAS</td>
    <td class="tg-lboi">L1</td>
    <td class="tg-lboi">IC</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
  <tr>
    <td class="tg-lboi">L5</td>
    <td class="tg-lboi">5I</td>
    <td class="tg-lboi">5Q</td>
    <td class="tg-lboi">5X</td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
    <td class="tg-lboi"></td>
  </tr>
</tbody></table>

\* 请参考附录 D.3 获取信号 ID。

## D.5 接收机相关的输入选项

<table class="tg"><thead>
  <tr>
    <th class="tg-9wq8">数据格式</th>
    <th class="tg-0lax">选项</th>
    <th class="tg-0lax">描述</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-0lax">RTCM2</td>
    <td class="tg-0lax">-</td>
    <td class="tg-0lax">-</td>
  </tr>
  <tr>
    <td class="tg-0lax">RTCM3</td>
    <td class="tg-0lax">-EPHALL</td>
    <td class="tg-0lax">输入所有的星历数据</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-INVPRR</td>
    <td class="tg-0lax">反转相位范围速率的极性</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-STA=nnn</td>
    <td class="tg-0lax">仅输入具有指定站ID（STAID=nnn）的消息</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-GLss</td>
    <td class="tg-0lax">选择用于GPS MSM的信号（ss=1C, 1P...）* **</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-RLss</td>
    <td class="tg-0lax">选择用于GLONASS MSM的信号（ss=1C, 1P...）* **</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-ELss</td>
    <td class="tg-0lax">选择用于Galileo MSM的信号（ss=1C, 1B...）* **</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-JLss</td>
    <td class="tg-0lax">选择用于QZSS MSM的信号（ss=1C, 2C...）* **</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-CLss</td>
    <td class="tg-0lax">选择用于BDS（北斗） MSM的信号（ss=2I, 7I...）* **</td>
  </tr>
  <tr>
    <td class="tg-0lax">BINEX</td>
    <td class="tg-0lax">-EPHALL</td>
    <td class="tg-0lax">输入所有的星历数据</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-GLss</td>
    <td class="tg-0lax">选择用于GPS MSM的信号（ss=1C, 1P...）* **</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-RLss</td>
    <td class="tg-0lax">选择用于GLONASS MSM的信号（ss=1C, 1P...）* **</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-ELss</td>
    <td class="tg-0lax">选择用于Galileo MSM的信号（ss=1C, 1B...）* **</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-JLss</td>
    <td class="tg-0lax">选择用于QZSS MSM的信号（ss=1C, 2C...）* **</td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">-CLss</td>
    <td class="tg-0lax">选择用于BDS（北斗） MSM的信号（ss=2I, 7I...）* **</td>
  </tr>
  <tr>
    <td class="tg-0lax">其他格式</td>
    <td class="tg-0lax">-</td>
    <td class="tg-0lax">-</td>
  </tr>
</tbody></table>

\* 参考附录 D.3 获取信号 ID，** 参考附录 D.4 获取默认信号优先级。