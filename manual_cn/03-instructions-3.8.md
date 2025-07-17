# 3. 操作指南

## 3.8 RTKPLOT：观测数据可视化

RTKPLOT还用于查看和绘制RINEX观测数据，以及进行卫星可见性分析。关于RTKPLOT的更多选项，请参考3.7节。

### 3.8.1 文件输入

要绘制RINEX观测数据，执行RTKPLOT菜单中的“File”-“Open Obs Data”，并选择RINEX观测文件和导航文件。您可以选择多个RINEX文件。如果您没有选择RINEX导航信息文件，RTKPLOT会读取观测数据文件路径，并将扩展名替换为.*nav（.obs obs）或. yyN、. yyG、. yyH、. yyQ、. yyL、. yyP（. yyO yyO），分别作为GPS、GLONASS、SBAS、QZSS、Galileo或组合导航数据。如果您想单独读取RINEX导航信息文件，请执行菜单“File”-“Open Nav Messages”。

如果输入文件能够正确读取，您将看到一个卫星可见性图。您可以使用左键或右键拖动来改变时间跨度。您也可以使用一些工具栏按钮，就像使用解算结果绘制图一样。

![Satellite Visibility Plot of RTKPLOT](https://i.ibb.co/jgrkv0q/image.png)
<p style="text-align: center;">图3.8-1 RTKPLOT的卫星可见性图</p> 

### 3.8.2 绘图功能

通过选择工具栏中的下拉菜单，您可以将绘图切换为：

- Sat Vis：观测数据质量
- SkyPlot：星空图（天空视图）
- DOP/NSat：可见卫星情况及DOP
- SNR/MP/EL：信噪比、多径和仰角图信息
- SNR/MP-EL：信噪比、多路径和仰角信息

要显示信噪比（SNR）图，观测数据必须包含信噪比（C/N₀）信息。

要正确显示多路径图，观测数据必须包含双频（GPS、GLONASS和QZSS的L1-L2，Galileo的L1-L5，北斗的L2-L7）伪距和载波相位。RTKPLOT会利用这些观测值内部生成MP LC（线性组合）以绘制多路径图。

为了计算卫星的仰角，RTKPLOT还需要卫星的位置信息。在输入RINEX导航文件的情况下，卫星位置是通过RINEX导航文件中的卫星星历计算得出的。如果无法获取RINEX导航文件，RTKPLOT将使用NORAD TLE（两行元素）数据集作为卫星位置的替代方案。在这种情况下，您必须在“Options”对话框中以纬度、经度和高度的形式指定接收机的位置。

您可以通过工具栏上方的下拉菜单选择信号和卫星。在卫星选择中，“G”、“R”、“E”、“J”、“C”和“S”分别代表GPS、GLONASS、Galileo、QZSS、北斗和SBAS。

![Skyplot by RTKPLOT](https://i.ibb.co/cyCFYNY/image.png)
<p style="text-align: center;">图3.8-2 Skyplot</p> 

![# of Visible Satellites and DOP Plot by RTKPLOT](https://i.ibb.co/YT8HxdL/image.png)
<p style="text-align: center;">图3.8-3 可见卫星与DOP</p> 

![SNR/Multipath Plot by RTKPLOT](https://i.ibb.co/nR8Qr1S/image.png)
<p style="text-align: center;">图3.8-4 SNR/Multipath</p> 

![SNR/Multipath - Elevation Plot by RTKPLOT](https://i.ibb.co/Qn5x38G/image.png)
<p style="text-align: center;">图3.8-5 SNR/Multipath - Elevation</p> 

### 3.8.3 其他操作

您可以通过菜单“文件” - “打开仰角掩码”读取仰角掩码数据，并设置选项“仰角掩码模式”为“开”来应用仰角掩码。关于仰角掩码数据的格式，请参考位于`<install dir>\rtklib_<ver>\data\elmask_sample.txt`的示例文件。

![Elevation Mask in Skyplot by RTKPLOT](https://i.ibb.co/27pW7sh/image.png)
<p style="text-align: center;">图3.8-6 仰角MASK</p> 


通过执行菜单“编辑” - “观测数据源”或“观测数据质量控制”，您可以以文本形式查看解决方案的来源或质量控制结果。

![QC Result View by RTKPLOT](https://i.ibb.co/tHP90RV/image.png)
<p style="text-align: center;">图3.8-7 质量控制结果</p> 


### 3.8.4 卫星可见性分析

在2.4.2版本中，新增了卫星可见性分析功能。借助北美航空航天防务司令部（NORAD）的两行元素（TLE）数据集，您可以使用RTKPLOT随时随地预测GNSS卫星的可见性。要启用此功能，请通过“选项”对话框设置TLE数据和卫星编号文件的路径。以下图表展示了这些文件的示例。

![Examples of TLE Data (upper) and Satellite Number File (lower)](https://i.ibb.co/68DBK50/image.png)
<p style="text-align: center;">图3.8-8-1 TLE数据示例</p> 

![Examples of TLE Data (upper) and Satellite Number File (lower)](https://i.ibb.co/Ln5rcwn/image.png)
<p style="text-align: center;">图3.8-8-2 卫星编号文件示例</p> 

NORAD TLE数据文件可以在CelesTrak（<http://celestrack.com>）或SpaceTrack（<http://www.space-track.org>）上免费获取。您需要下载包含目标GNSS卫星轨道要素的TLE数据。对于所有GNSS卫星，建议使用SpaceTrack提供的“完整目录”（full catalog），其中包含所有可用卫星的信息。为了获得更准确的卫星位置，建议使用更新的TLE数据。

另一个卫星编号文件用于将TLE卫星编号转换为GNSS卫星编号，例如G23、R03、139等。一个示例卫星编号文件可以在安装目录下的`rtklib_<ver>/data/TLE_GNSS_SATNO.txt`找到。请注意，GNSS卫星编号与卫星之间的对应关系有时会因系统星座变化而改变。在这种情况下，您需要自行编辑和修改卫星编号文件。

为了预测GNSS卫星的可见性，请通过“选项”对话框设置用户位置的纬度、经度和高度。您需要将“接收机位置”设置为“纬度/经度/高度（Lat/Lon/Hgt）”。然后执行菜单“文件”-“可见性分析...”，您将看到“时间跨度/间隔”对话框。

![Time Span/Interval Dialog of RTKPLOT](https://i.ibb.co/d42mhVk/image.png)
<p style="text-align: center;">图3.8-9 可见性分析时间跨度/间隔对话框</p> 

通过该对话框，设置“Time Start”和“Time End”（以GPS时间表示）、“Interval”用于分析，然后点击“OK”按钮。您可以在指定位置获得一份卫星可见性图表。在该图表中，颜色默认表示不同的卫星系统，包括GPS、GLONASS、Galileo、QZSS、北斗和SBAS。您还可以像处理观测数据图表一样，将图表切换为天空图（Skyplot）、DOP/NSat或SNR/MP/EL。

![Satellite Visibility (Predicted) Plot by RTKPLOT](https://i.ibb.co/KKS1L3s/image.png)
<p style="text-align: center;">图3.8-10 卫星可见性 (Predicted)</p> 

![Skyplot (Predicted) by RTKPLOT](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250702-144110.jpg)
<p style="text-align: center;">图3.8-11 Skyplot (Predicted)</p> 

<GiscusTalk />
