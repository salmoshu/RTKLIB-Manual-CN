# 3. 操作指南

## 3.7 RTKPLOT：解算结果可视化

RTKPLOT用于查看和绘制RTKPOST和RTKNAVI（或相应的CUI应用）输出的定位解。RTKPLOT使用文件或实时流来可视化解算结果，具体的格式可以是NMEA0183，也可以是RTKLIB自定义的pos格式（附录B.1）。

### 3.6.1 软件执行

![Main Window of RTKPLOT](https://i.ibb.co/1qzLWB0/image.png)

执行二进制AP文件`<install-dir>\rtklib_<ver>\bin\rtkplot.exe`，您可以看到 RTKPLOT 的主窗口。可以单独打开，也可以从RTKPOST、RTKNAVI或RTKCONV中的 <span style="border: 1px solid black; padding: 3px;">Plot</span> 按钮打开。

也可以通过RTKNAVI或RTKPOST中的 <span style="border: 1px solid black; padding: 3px;">Plot</span> 按钮进入RTKPOST。如果是RTKNAVI，RTKPLOT会自动运行并连接到RTKNAVI的监控端口。通过这种方式可以连接到在远程电脑上运行的RTKNAVI，例如将流类型设置为TCP客户端，输入远程电脑的IP地址以及RTKNAVI监控端口的端口号，然后连接到远程的RTKNAVI。在这种情况下，允许多台运行RTLPLOT的电脑进行多个客户端连接。

### 3.6.2 输入与配置

![GND TRK Plot by RTKPLOT](https://i.ibb.co/544F33t/image.png)

执行菜单“File” - “Open Solution 1”，并通过文件选择对话框选择解算结果文件。输入的解算结果文件可以是RTKLIB定义的pos格式或NMEA-0183格式。如果文件格式为NMEA-0183，则文件必须至少包含NMEA的GPRMC和GPGGA语句。也可以通过将解算结果文件图标拖放至RTKPLOT中，从而读取和绘制解算结果文件。

如果解算结果文件有效，接收机的轨迹将在地图窗口中绘制出来。通过菜单“Edit” - “Options”更改绘制中标记、线条和网格的颜色。主窗口底部的状态栏还会显示时间范围、解算结果历元的数量（N=nnnn）、基线长度（B=0.0-x.xkm）、每种质量解算结果的数量和百分比（Q=1:nnn(pp%)，2:nnn(pp%)，...）。

质量标志Q和标记颜色的含义为：1：<span style="color: green;">Fix</span>，2：<span style="color: orange;">Float</span>，4：<span style="color: #0000cc;">DGPS</span>，5：<span style="color: red;">Single</span>（颜色可以通过绘制选项更改）。要按质量标志Q筛选标记，选择工具栏中的第二个下拉菜单。

### 3.6.3 结果展示

通过在绘图上按住鼠标左键拖动鼠标，您可以向上、向下、向左和向右拖动地图。您还可以通过右键上下拖动鼠标或旋转鼠标滚轮来更改地图的比例。

**1. 位置信息**

![POSITION Plot by RTKPLOT](https://i.ibb.co/pdftbRq/image.png)

通过选择工具栏右侧的绘图类型下拉菜单，您可以将绘图切换为接收机位置的东/北/天（E/N/U）分量（位置）、接收机速度的东/北/天（E/N/U）分量（速度）或接收机加速度的东/北/天（E/N/U）分量（加速度）。您可以通过左键拖动来移动X/Y轴，并通过在X/Y轴区域右键拖动来改变比例尺。您还可以通过点击绘图类型下拉菜单右侧的三个按钮，来隐藏或显示这三个绘图。

**2. 其他信息**

![RESIDUALS Plot by RTKPLOT](https://i.ibb.co/BCjwQTg/image.png)

通过选择绘图类型下拉菜单，您可以将绘图切换为NSat/Age/Ratio（有效卫星数量、差分数据的时效性、模糊度验证的比率因子）。

如果你将“Output Solution Status”选项设置为“Residuals”，则可以显示残差信息。您可以通过选择L1/LC、L2或L5来切换频率。在残差图模式下，您可以通过右侧的下拉菜单选择一颗卫星或所有卫星。在载波相位的残差图中，红色线表示周跳，灰色线表示奇偶性未知标志（这意味着载波相位中的半周模糊度未被解析）。

通过点击工具栏中的工具按钮，你可以使用 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132436.jpg"/> 来将当前位置居中，使用 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132459.jpg"/> 来调整X轴的比例尺，使用 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132555.jpg"/> 来调整Y轴的比例尺，使用 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132621.jpg"/> 来以大标记显示当前位置，使用 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132954.jpg"/> 将当前轨迹位置固定在水平中心，使用 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-133021.jpg"/> 将当前轨迹位置固定在垂直中心，使用 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-133141.jpg"/> 开始动画播放，使用 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-133201.jpg"/> 停止动画播放。你还可以滑动“Time Scroll Bar”（时间滚动条）来改变当前历元。要清除已读取的数据，执行菜单“File” - “Clear”或点击工具栏中的 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132127.jpg"/> 。要重新加载解算结果文件，执行菜单“File” - “Reload”或点击工具栏中的 <img width="24" height="24" style="display: inline;" src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132038.jpg"/> 。

**3. 多结果展示**

要绘制多个解算结果文件，执行菜单“File” - “Open Solutions-2”，并通过文件选择对话框选择文件。你可以通过工具栏中的1 2按钮来切换解算结果1和2的绘制开/关。要绘制解算结果1和解算结果2之间的差异，点击工具栏中的1-2按钮。

**4. 文本内容查看**

![Solution Source View of RTKPLOT](https://i.ibb.co/PFrsshy/image.png)

通过执行菜单“编辑”-“解算结果源”，您可以以文本形式查看解算结果的源。

### 3.6.4 加载地图

**1. 地图图片**

![Map Image Overlay by RTKPLOT](https://i.ibb.co/J52r993/image.png)

通过执行菜单“File” - “Open Map Image”，你可以读取一张JPEG图像，并在“Gnd Trk”（地面轨迹）绘制类型的情况下将其作为背景地图图像绘制在绘制区域中。可以通过点击工具栏中的 **[地图按钮]** 来启用或禁用该图像。

![Map Image Options Dialog of RTKPLOT](https://i.ibb.co/jrgdRMX/image.png)

如果要调整地图图像中的位置，执行菜单“Edit” - “Map Image”，并在“Map Image”对话框中输入图像中心的纬度和经度、沿X轴或Y轴的图像比例尺。完成后，点击“Save Tag”按钮将调整信息保存到图像标签文件中。图像标签文件的路径是原始地图图像文件路径 + “.tag”。如果图像标签文件已存在，它将与地图图像本身一起自动读取。当前版本不支持地图图像的旋转。请选择北方向正确对准上方的地图图像。例如，你可以通过Google Earth的菜单“File” - “Save”获取一张JPEG图像。要将北方向固定在上方，点击Google Earth中的“N”按钮。为了避免地图图像的变形，请将坐标原点设置在地图图像内部或附近。

**2. Google Map**

![Google Map View of RTKPLOT](https://i.ibb.co/GcSfLbt/image.png)

要打开Google Map View，在读取解算结果后，执行RTKPLOT的菜单“View” - “Google Map View”。也可以使用工具栏 **[地图加载按钮]** 来显示这些视图。请注意，始终需要互联网连接才能使用这些基于谷歌提供的服务的视图。

在谷歌地图视图中，只有一个工具栏按钮用于将轨迹点中心固定，与谷歌地球视图相同。对于谷歌地图视图的其他操作，使用谷歌地图视图中的控件。

### 3.6.5 Waypoints

![Waypoints Dialog of RTKPLOT](https://i.ibb.co/kHKwdY7/image.png)

通过执行菜单“Edit” - “Waypoints...”，你可以看到“Waypoints”对话框。通过该对话框，你可以以列表形式加载、保存、添加和删除航点。点击“Add”按钮并编辑点名，可以将当前接收机位置添加到航点列表中。当按钮按下时，航点的位置会在“Gnd Trk”（地面轨迹）绘制图中显示。

### 3.6.6 时间设置

![Time Span/Interval Dialog of RTKPLOT](https://i.ibb.co/W0kQR38/image.png)

要设置解算结果的时间范围和时间间隔，执行菜单“Edit” - “Time Span/Interval”，并在“Time Span/Interval”对话框中勾选并设置Time Start、Time End和Interval字段。

### 3.6.7 实时数据显示

![Connection Settings Dialog of RTKPLOT](https://i.ibb.co/TMG6Wh2/image.png)

要实时绘制解算结果，执行菜单“File” - “Connection Settings”，并在“Connection Setting”对话框中设置解算结果参数。你可以为解算结果1和解算结果2选择流类型（Stream Type）、流选项（Stream Option，简称Opt）、流命令（Stream Commands，简称Cmd）、解算结果格式（Solution Format）、时间格式（Time Format）、经纬度格式（Lat/Lon Format）和字段分隔符（Field Sep）。设置好连接参数后，执行菜单“File” - “Connect”或点击工具栏中的按钮来建立连接。要断开外部设备的连接，执行菜单“File” - “Disconnect”或再次点击连接按钮。例如，如果选择串行（serial）作为流类型，并选择NMEA0183作为解算结果格式，你可以在RTKPLOT窗口中监测外部接收机的NMEA输出。

### 3.6.8 RTKPLOT选项设置

要配置RTKPLOT的绘制选项，执行菜单“Edit” - “Options...”，并使用以下的“Options”对话框设置选项。

![Options Dialog of RTKPLOT](https://i.ibb.co/chvmBnz/image.png)

**1. RTKPLOT选项**

<table>
  <thead>
    <tr>
      <th>选项</th>
      <th>描述</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Time Format</td>
      <td>
        选择时间格式。<br>
        (wwww/ssss, h:m:s GPST, h:m:s UTC or h:m:s LT).
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Lat/Lon Format</td>
      <td>
        选择经纬度格式
        (ddd.ddddd or ddd mm ss.ss)
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Show Statistics</td>
      <td>选择是否显示统计信息</td>
      <td></td>
    </tr>
    <tr>
      <td>Cycle-Slip</td>
      <td>
        设置是否在卫星可见性图中显示周跳位置。如果选择“LG Jump”，则使用双频无几何LC（线性组合）来检测周跳。在“LLI Flag”的情况下，使用RINEX观测数据中的LLI（失锁指示器）。周跳在卫星可见性图中以红色垂直线显示。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Parity Unknown</td>
      <td>
        设置是否在卫星可见性图中显示奇偶性未知状态。奇偶性未知的历元在卫星可见性图中以灰色垂直线显示。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Ephemeris</td>
      <td>
        设置是否在卫星可见性图中显示星历状态。星历以观测数据下方的灰色线显示。灰色点表示Toe（星历时间）。红色的星历线表示卫星不健康。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Elevation Mask</td>
      <td>
        设置卫星能见度图的仰角限制角度（度）。仰角限制也用于DOP/NSat图。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Elevation Mask Pattern</td>
      <td>设置是否使用仰角限制模式。</td>
      <td></td>
    </tr>
    <tr>
      <td>Hide Low Satellite</td>
      <td>设置是否显示高程遮罩和高程遮罩图案下的低高程卫星。</td>
      <td></td>
    </tr>
    <tr>
      <td>Maximum DOP </td>
      <td>设置DOP/NSat图的y轴限制。</td>
      <td></td>
    </tr>
    <tr>
      <td>Receiver Position</td>
      <td>
        设置卫星能见度图或skyplot的接收机位置。“单一解算结果”通过使用观测和导航数据，将单点结果用作接收机位置。对于移动接收机，您应进行设置。“Lat/Lon/Hgt”使用纬度、经度和高度来指定以下Lat/Lon/Hgt字段中的静态接收机。“RINEX标头”使用RINEX观测数据标头中的“APPROX POSITION XYZ”作为接收机位置。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Satellite System</td>
      <td>选中视图中选中的导航系统</td>
      <td></td>
    </tr>
    <tr>
      <td>Excluded Sats</td>
      <td>设置剔除的卫星。以空格作为间隔，填写卫星号或ID</td>
      <td></td>
    </tr>
    <tr>
      <td>Error Bar/Circle</td>
      <td>
        设置在解算结果显示中是否显示误差条或误差圆。您可以选择“条/圆”或“点”作为格式。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Direction Arrow</td>
      <td>设置在解算结果地面轨迹图中是否显示方向箭头和速度箭头。</td>
      <td></td>
    </tr>
    <tr>
      <td>Graph Label</td>
      <td>
        设置在解算结果显示中是否显示图形标签。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Grid/Grid Label</td>
      <td>
        设置在解算结果显示中是否显示网格和网格标签。对于圆形网格，将其设置为“圆圈”或“圆圈/标签”。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Compass</td>
      <td>
        设置在解算结果地面轨迹图中是否显示罗盘。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Scale</td>
      <td>设置在解算结果地面轨迹图中是否显示比例尺。</td>
      <td></td>
    </tr>
    <tr>
      <td>Auto Fit</td>
      <td>
        设置解算结果图表的比例尺是否自动调整。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Y-Range (+/-)</td>
      <td>设置解算结果图表中Y轴的范围。</td>
      <td></td>
    </tr>
    <tr>
      <td>RT Buffer Size</td>
      <td>
        设置实时解算结果图表的缓冲区大小（以历元为单位）。超出缓冲区大小的旧解算结果将从实时图表中删除。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Coordinate Origin</td>
      <td>
        选择解算结果显示的原点位置如下：<br>
        Start Pos：第一个解算结果位置<br>
        End Pos：最后一个解算结果位置<br>
        Average Pos：所有解算结果位置的平均值<br>
        Linear Fit Pos：基于线性拟合的位置<br>
        Base Station：基准站位置<br>
        Lat/Lon/Hgt：指定的纬度、经度和高程<br>
        Auto Input：以下自动位置<br>
        Waypoint：一个航点位置<br>
        如果您选择“Lat/Lon/Height”，则需要在下方的文本框中输入原点的纬度、经度和椭球高。如果选择“自动输入”，则假定接收机ID为解算结果文件名头部的4个字符，并从位置文件中读取位置。可以通过点击...按钮并点击位置列表对话框中的“加载”按钮来选择位置文件。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Mark Color 1(1-6)</td>
      <td>
        设置图表中解算结果编号1或观测数据的标记颜色。点击右侧的颜色面板，并通过颜色选择对话框选择颜色。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Mark Color 2(1-6)</td>
      <td>
        设置图表中解算结果编号2的标记颜色。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Line Color</td>
      <td>设置视图的线条颜色</td>
      <td></td>
    </tr>
    <tr>
      <td>Text Color</td>
      <td>设置视图中的文本颜色</td>
      <td></td>
    </tr>
    <tr>
      <td>Grid Color</td>
      <td>设置视图中网格颜色</td>
      <td></td>
    </tr>
    <tr>
      <td>Background Color</td>
      <td>设置图表中的背景颜色。</td>
      <td></td>
    </tr>
    <tr>
      <td>Plot Style</td>
      <td>
        选择图表中的绘图样式。若要删除除轨迹点标记外的所有标记和线条，将其设置为“None”。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Mark Size</td>
      <td>设置视图中的标记尺寸</td>
      <td></td>
    </tr>
    <tr>
      <td>Font</td>
      <td>选择图表中的字体。点击...按钮，并通过字体选择对话框来选择字体。</td>
      <td></td>
    </tr>
    <tr>
      <td>Animation Interval</td>
      <td>设置解算结果或观测数据图表的动画间隔。</td>
      <td></td>
    </tr>
    <tr>
      <td>Update Cycle (ms)</td>
      <td>
        设置实时图表的绘图更新周期时间（以毫秒为单位）。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>Lat/Lon/Hgt</td>
      <td>
        设置原点的纬度、经度和高程。可以直接填写数值，或者点击...按钮并选择一个站点位置。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>QC Command</td>
      <td>
        设置观测数据的质量控制（QC）命令及其选项。默认情况下，设置了TEQC QC模式选项。该命令用于执行菜单“Edit” - “Obs Data QC...”。命令必须位于命令搜索路径中，或者位于RTKLIB可执行文件的目录中。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>RINEX Opt</td>
      <td>
        设置RINEX读取选项。有关RINEX读取选项的详细信息，请参阅&lt;install dir&gt;\rtklib_&lt;ver&gt;\src\rinex.c。
      </td>
      <td></td>
    </tr>
    <tr>
      <td>TLE Data</td>
      <td>
        指定NORAD TLE卫星轨道元素数据文件。如果卫星星历不可用，TLE数据将用于计算天空图中的卫星位置。TLE数据可以使用两行格式或三行格式。示例TLE数据可以在以下位置找到：<br>
        &lt;install dir&gt;\rtklib_&lt;ver&gt;\data\catalbe_2l_2013_01_09_pm.txt。
      </td>
      <td>另请参阅RTKNAVI选项对话框</td>
    </tr>
    <tr>
      <td>Sat No</td>
      <td>
        指定卫星编号文件，该文件用于将GNSS卫星/PRN编号与NORAD TLE数据文件中的TLE卫星目录编号相连接。示例卫星编号文件可以在以下位置找到：<br>
        &lt;install dir&gt;\rtklib_&lt;ver&gt;\data\TLE_GNSS_SATNO.txt。
      </td>
      <td></td>
    </tr>
  </tbody>
</table>

### 3.6.9 RTKPLOT菜单汇总

<table>
  <thead>
    <tr>
      <th>菜单</th>
      <th>工具栏</th>
      <th>描述</th>
      <th>备注</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="4" align="center"><strong>File</strong></td>
    </tr>
    <tr>
      <td>Open Solution-1...</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-131201.jpg" alt=""></td>
      <td>打开No.1中的解算结果</td>
      <td>*双击打开</td>
    </tr>
    <tr>
      <td>Open Solution-2...</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-131410.jpg" alt=""></td>
      <td>打开No.2中的解算结果</td>
      <td>*双击打开</td>
    </tr>
    <tr>
      <td>Open Map Image...</td>
      <td align="center">-</td>
      <td>为解算结果图表打开地图图像数据。</td>
      <td></td>
    </tr>
    <tr>
      <td>Open Obs Data...n</td>
      <td align="center">-</td>
      <td>打开观测数据。导航数据也会自动打开。</td>
      <td></td>
    </tr>
    <tr>
      <td>Open Nav Data...</td>
      <td align="center">-</td>
      <td>手动打开星历数据。</td>
      <td></td>
    </tr>
    <tr>
      <td>Open Elev Mask...</td>
      <td align="center">-</td>
      <td>打开仰角限制数据。</td>
      <td></td>
    </tr>
    <tr>
      <td>Visibility Analysis...</td>
      <td align="center">-</td>
      <td>执行卫星可见性分析。</td>
      <td></td>
    </tr>
    <tr>
      <td>Save Image...</td>
      <td align="center">-</td>
      <td>将视图保存为图片。</td>
      <td></td>
    </tr>
    <tr>
      <td>Save # of Sats/DOP...</td>
      <td align="center">-</td>
      <td>将卫星数量和DOP保存到文本文件中。</td>
      <td></td>
    </tr>
    <tr>
      <td>Save SNR,MP and AZ/EL..</td>
      <td align="center">-</td>
      <td>将信噪比、多径效应以及方位角/仰角数据保存到文本文件中。</td>
      <td></td>
    </tr>
    <tr>
      <td>Connect...</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-131907.jpg"/></td>
      <td>连接到外部实时解算结果数据流。</td>
      <td></td>
    </tr>
    <tr>
      <td>Disconnect...</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-131939.jpg"/></td>
      <td>断开连接。</td>
      <td></td>
    </tr>
    <tr>
      <td>Connection Settings...</td>
      <td align="center">-</td>
      <td>显示“连接设置”对话框以配置连接选项。</td>
      <td></td>
    </tr>
    <tr>
      <td>Reload</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132038.jpg"/></td>
      <td>重新加载解算结果、观测和导航数据。</td>
      <td></td>
    </tr>
    <tr>
      <td>Clear</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132127.jpg"/></td>
      <td>清空解算结果、观测数据和导航数据。</td>
      <td></td>
    </tr>
    <tr>
      <td>Exit</td>
      <td align="center">-</td>
      <td>关闭并退出RTKPLOT。</td>
      <td></td>
    </tr>
    <tr>
      <td colspan="4" align="center"><strong>Edit</strong></td>
    </tr>
    <tr>
      <td>Time Span/Interval...</td>
      <td align="center">-</td>
      <td>设置“时间跨度/间隔”对话框以设置时间跨度和时间间隔。</td>
      <td></td>
    </tr>
    <tr>
      <td>Map Image...</td>
      <td align="center">-</td>
      <td>显示“地图图像”对话框以配置图像数据的大小、位置和比例。</td>
      <td></td>
    </tr>
    <tr>
      <td>Waypoints...</td>
      <td align="center">-</td>
      <td>显示“Waypoint”对话框用以增加或修改Waypoint</td>
      <td></td>
    </tr>
    <tr>
      <td>Solution Source...</td>
      <td align="center">-</td>
      <td>通过文本查看器显示解算结果数据的来源。</td>
      <td></td>
    </tr>
    <tr>
      <td>Obs Data Source...</td>
      <td align="center">-</td>
      <td>通过文本查看器显示观测数据的来源。</td>
      <td></td>
    </tr>
    <tr>
      <td>Obs Data QC...</td>
      <td align="center">-</td>
      <td>执行观测数据的质量控制（QC），并使用文本查看器显示结果。</td>
      <td></td>
    </tr>
    <tr>
      <td>Copy To Clipboard</td>
      <td align="center">-</td>
      <td>将图像拷贝到粘贴板</td>
      <td></td>
    </tr>
    <tr> 
      <td>Options</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132227.jpg"/></td>
      <td>设置对话框。</td>
      <td></td>
    </tr>
    <!-- View -->
    <tr>
      <td colspan="4" align="center"><strong>View</strong></td>
    </tr>
    <tr>
      <td>Show Tool Bar</td>
      <td align="center">-</td>
      <td>显示或隐藏工具栏</td>
      <td></td>
    </tr>
    <tr>
      <td>Show Status Bar</td>
      <td align="center">-</td>
      <td>显示或隐藏状态栏</td>
      <td></td>
    </tr>
    <tr>
      <td>Google Earth View...</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132323.jpg"/></td>
      <td>显示谷歌地球视图</td>
      <td></td>
    </tr>
    <tr>
      <td>Google Map View...</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132344.jpg"/></td>
      <td>显示谷歌地图视图</td>
      <td></td>
    </tr>
    <tr>
      <td>Input Monitor 1...</td>
      <td align="center">-</td>
      <td>显示“输入监视器”窗口以查看实时输入流1号。</td>
      <td></td>
    </tr>
    <tr>
      <td>Input Monitor 2...</td>
      <td align="center">-</td>
      <td>显示“输入监视器”窗口以查看实时输入流2号。</td>
      <td></td>
    </tr>
    <tr>
      <td>Center Origin</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132436.jpg"/></td>
      <td>将坐标原点移动到绘图区域的中心。</td>
      <td></td>
    </tr>
    <tr>
      <td>Fit Horizontal</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132459.jpg"/></td>
      <td>在绘图中调整解算结果或观测数据的水平范围以适应视图。</td>
      <td></td>
    </tr>
    <tr>
      <td>Fit Vertical</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132555.jpg"/></td>
      <td>在绘图中调整解算结果数据的垂直范围以适应视图。</td>
      <td></td>
    </tr>
    <tr>
      <td>Show Track Point</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132621.jpg"/></td>
      <td>在绘图中显示或隐藏解算结果或观测数据的轨迹点。</td>
      <td></td>
    </tr>
    <tr>
      <td>Fix Track Center</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132655.jpg"/></td>
      <td>将轨迹点固定在绘图区域的中心。</td>
      <td></td>
    </tr>
    <tr>
      <td>Fix Track Horizontal</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-132954.jpg"/></td>
      <td>在绘图中水平固定轨迹点。</td>
      <td></td>
    </tr>
    <tr>
      <td>Fix Track Vertical</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-133021.jpg"/></td>
      <td>在绘图中垂直固定轨迹点。</td>
      <td></td>
    </tr>
    <tr>
      <td>Show Map Image</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-133049.jpg"/></td>
      <td>显示或隐藏地图图像。</td>
      <td></td>
    </tr>
    <tr>
      <td>Show Path/Waypoints</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-133116.jpg"/></td>
      <td>显示或隐藏地图路径数据。</td>
      <td></td>
    </tr>
    <tr>
      <td>Animation Start</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-133141.jpg"/></td>
      <td>开始绘图的动画效果。</td>
      <td></td>
    </tr>
    <tr>
      <td>Animation Stop</td>
      <td align="center"><img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-133201.jpg"/></td>
      <td>停止绘图的动画效果。</td>
      <td></td>
    </tr>
    <!-- Help -->
    <tr>
      <td colspan="4" align="center"><strong>Help</strong></td>
    </tr>
    <tr>
      <td>About...</td>
      <td align="center">-</td>
      <td>显示“About...”对话框</td>
      <td></td>
    </tr>
  </tbody>
</table>
