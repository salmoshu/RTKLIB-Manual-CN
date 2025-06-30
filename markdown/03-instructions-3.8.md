# 3. 操作指南

## 3.8 RTKPLOT：观测数据可视化

RTKPLOT 也用于查看和绘制 RINEX 观测数据以及进行卫星可见性分析。

（1）要绘制 RINEX 观测数据，请执行 RTKPLOT 的菜单“文件”--“打开观测数据”，并选择 RINEX 观测文件和导航电文文件。您可以选择多个 RINEX 文件。如果您没有选择 RINEX 导航电文文件，RTKPOST 会读取将扩展名替换为 .*nav（.obs）或 .yyN、.yyG、.yyH、.yyQ、.yyL、.yyP（.yyO）的观测数据文件路径，作为 GPS、GLONASS、SBAS、QZSS、Galileo 或组合导航数据。如果您想单独读取 RINEX 导航电文文件，请执行菜单“文件”--“打开导航电文”。如果输入文件能够正确读取，您可以看到一个卫星可见性图。您可以使用左键或右键拖动来改变时间跨度。您也可以使用一些工具栏按钮，就像在解决方案绘图中一样。

![Satellite Visibility Plot of RTKPLOT](https://i.ibb.co/jgrkv0q/image.png)

（2）通过选择工具栏中的下拉菜单，您可以将图形切换为天空图中的卫星可见性（SkyPlot）或可见卫星数量的图形，即DOP/NSat图形（DOP/NSat），信噪比/多径/仰角图形（SNR/MP/EL）以及信噪比/多径 - 仰角图形（SNR/MP-EL）。要显示信噪比图形，观测数据必须包含信噪比（C/N0）信息。要显示正确的多径图形，观测数据必须包含双频（GPS、GLONASS和QZSS为L1-L2，伽利略为L1-L5，北斗为L2-L7）的伪距和载波相位。RTKPLOT 内部使用这些观测值生成 MP LC（线性组合）以绘制多径图。为了计算卫星的仰角，RTKPLOT 还需要卫星的位置信息。当输入 RINEX 导航文件时，卫星位置是通过使用 RINEX 导航文件中的卫星星历来计算的。如果无法获取 RINEX 导航文件，RTKPLOT 会使用 NORAD TLE（两行元素）数据集来确定卫星位置。在这种情况下，您必须在“选项”对话框中以纬度、经度和高度的形式指定接收机位置。您可以通过工具栏上方的下拉菜单选择信号和卫星。在卫星选择中，“G”“R”“E”“J”“C”和“S”分别代表 GPS、GLONASS、伽利略、QZSS、北斗和 SBAS。

![Skyplot by RTKPLOT](https://i.ibb.co/cyCFYNY/image.png)

![# of Visible Satellites and DOP Plot by RTKPLOT](https://i.ibb.co/YT8HxdL/image.png)

![SNR/Multipath Plot by RTKPLOT](https://i.ibb.co/nR8Qr1S/image.png)

![SNR/Multipath - Elevation Plot by RTKPLOT](https://i.ibb.co/Qn5x38G/image.png)

（3）您可以通过菜单“文件” - “打开仰角掩码”读取仰角掩码数据，并设置选项“仰角掩码模式”为“开”来应用仰角掩码。关于仰角掩码数据的格式，请参考位于`<install dir>\rtklib_<ver>\data\elmask_sample.txt`的示例文件。

![Elevation Mask in Skyplot by RTKPLOT](https://i.ibb.co/27pW7sh/image.png)

（4）通过执行菜单“编辑” - “观测数据源”或“观测数据质量控制”，您可以以文本形式查看解决方案的来源或质量控制结果。

![QC Result View by RTKPLOT](https://i.ibb.co/tHP90RV/image.png)

（5）在2.4.2版本中，新增了卫星可见性分析功能。借助北美航空航天防御司令部（NORAD）的两行元素（TLE）数据集，您可以使用RTKPLOT随时随地预测全球导航卫星系统（GNSS）卫星的可见性。要启用此功能，请通过“选项”对话框设置TLE数据和卫星编号文件的路径。以下图表展示了这些文件的示例。

![Examples of TLE Data (upper) and Satellite Number File (lower)](https://i.ibb.co/68DBK50/image.png)
![Examples of TLE Data (upper) and Satellite Number File (lower)](https://i.ibb.co/Ln5rcwn/image.png)

NORAD TLE数据文件可以在CelesTrak（http://celestrack.com）或SpaceTrack（http://www.space-track.org）免费获取。由于网络原因，上述网页的解析并未成功，这可能与链接本身有关，也可能与网络连接有关。如果您需要访问这些网页，请检查链接的合法性，并尝试重新加载网页。如果您下载TLE数据时遇到问题，建议稍后再试或检查网络连接。
您需要下载包含目标GNSS卫星轨道元素的TLE数据。对于所有GNSS卫星，最好使用SpaceTrack提供的“完整目录”，其中包含了所有可用卫星的信息。为了获得更准确的卫星位置，建议使用更新的TLE数据。另一个卫星编号文件用于将TLE卫星编号转换为GNSS卫星编号，如G23、R03、139等。一个示例卫星编号文件可以在`<install dir>\rtklib_<ver>\data\TLE_GNSS_SATNO.txt`找到。请注意，由于系统星座变化，GNSS卫星编号与卫星的对应关系有时会发生变化。在这种情况下，您需要自行编辑和修改卫星编号文件。为了预测GNSS卫星的可见性，通过“选项”对话框设置用户位置，包括纬度、经度和高度。您需要将“接收机位置”设置为“纬度/经度/高度”。然后执行菜单“文件” - “可见性分析...”，您将看到“时间跨度/间隔”对话框。

![Time Span/Interval Dialog of RTKPLOT](https://i.ibb.co/d42mhVk/image.png)

通过该对话框，设置“开始时间”和“结束时间”为GPS时间，“间隔”用于分析，然后点击“确定”按钮。您将获得指定位置的卫星可见性图表。在图表中，默认情况下，颜色表示卫星系统，如GPS、GLONASS、伽利略、QZSS、北斗和SBAS。您可以像观测数据图表一样，将图表切换为天空图（Skyplot）、DOP/NSat或SNR/MP/EL。

![Satellite Visibility (Predicted) Plot by RTKPLOT](https://i.ibb.co/KKS1L3s/image.png)

![Skyplot (Predicted) by RTKPLOT](https://i.ibb.co/fp8S8Rj/image.png)

（6）关于RTKPLOT用于解算数据绘图的选项以及RTKPLOT提供的菜单，请参阅3.7节 使用RTKPLOT查看和绘制解算数据。
