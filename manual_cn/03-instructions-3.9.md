# 3. 操作指南

## 3.9 RTKGET：GNSS产品和数据下载

对于PPP（精密单点定位）分析，您通常需要下载IGS（国际GNSS服务）提供的精密GNSS产品，例如卫星轨道和钟差信息。在其他情况下，您可能需要通过互联网从GNSS数据档案中下载CORS（连续运行参考站）网络的观测数据。为了下载这些GNSS相关的产品和数据，RTKLIB提供了一个有用的图形界面下载工具RTKGET。RTKGET是在2.4.2版本中新增的功能。

### 3.9.1 软件执行

打开可执行文件文件`<install dir>\rtklib_<ver>\bin\rtkget.exe`。您将看到RTKGET的主窗口。

![Main Window of RTKGET](https://i.ibb.co/vsg9qfN/image.png)

### 3.9.2 URL列表

首先，您需要配置GNSS数据的URL列表文件。在主窗口中点击 <span style="border: 1px solid black; padding: 3px;">Options...</span> 按钮。您将看到“Options”对话框。

![Options Dialog of RTKGET](https://i.ibb.co/VwqNysJ/image.png)

填写“URL List File for GNSS Data”的文件路径，或者通过点击 <span style="border: 1px solid black; padding: 3px;">...</span> 来选择文件。关于文件格式，请参考附录B.5。GNSS数据的URL列表文件示例可以在`<install dir>\rtklib_<ver>\data\URL_LIST.txt`找到。如果您留空该字段，RTKGET将使用默认的URL列表文件`<install dir>\rtklib_<ver>\data\URL_LIST.txt`。

### 3.9.3 下载设置

在主窗口中通过左侧的列表框选择下载数据类型。您可以在列表中选择多种数据类型。使用列表框上方的组合框，可以根据数据类型类别过滤列表。下载数据的URL地址显示在第一个消息区域的中央。本地目录也显示在第二个消息区域。

![Selection of Download Data Type by RTKGET](https://i.ibb.co/LJ3yzDL/image.png)

**1. 时间设置**

使用“开始”、“结束”和“间隔”字段以GPS时间指定时间跨度。下载数据URL地址中的时间关键字将被指定的时间跨度中的时间替换。如果URL地址包含关键字%N，则该关键字将被“编号”字段中的数字替换。要指定一系列数字，可以在“编号”字段中使用“-”来表示范围，例如“1-99”。

**2. 站点名称**

如果URL地址包含关键字%s、%S或%r，则这些关键字将被右侧列表框“站点”中选定的站点名称替换。要选择站点名称，请点击主窗口右上角的 <span style="border: 1px solid black; padding: 3px;">...</span> 按钮。您将看到“站点”对话框。您可以在该对话框中输入或编辑站点名称列表。站点名称列表中的每一行表示一个将用于替换URL地址中关键字的站点名称。要从外部文本文件加载站点名称列表，请点击 <span style="border: 1px solid black; padding: 3px;">Load</span> 按钮来选择文件。您可以通过点击 <span style="border: 1px solid black; padding: 3px;">Save</span> 按钮将站点名称列表保存到外部文本文件。外部文本文件是一个包含站点名称文本行的简单文本文件。在输入或编辑站点名称列表后，点击 <span style="border: 1px solid black; padding: 3px;">OK</span> 按钮关闭对话框，并将站点名称列表设置到主窗口。

![Stations Dialog of RTKGET](https://i.ibb.co/7rk2GMZ/image.png)

在“站点”列表框中选择站点名称。点击 <span style="border: 1px solid black; padding: 3px;">A</span> 按钮以选择所有站点，点击 <span style="border: 1px solid black; padding: 3px;">D</span> 按钮以取消选择所有站点。

**3. FTP使用**

如果是通过FTP下载，需要设置“FTP登录”和“密码”以登录FTP服务器。对于大多数匿名FTP服务器，您可以使用“anonymous”和您的电子邮件地址来填写这些字段。然后检查或取消检查以下下载选项：

- 跳过现有文件：设置如果本地文件已存在，是否跳过下载。
- 解压/解包文件：设置如果下载的文件是压缩的，是否对其进行解压或解包。
- 本地目录：检查并设置下载文件的本地目录路径。点击 <span style="border: 1px solid black; padding: 3px;">...</span> 通过“目录选择”对话框来选择目录。如果未勾选，则使用URL列表文件中的默认下载目录。目录路径可以包含关键字，这些关键字将被日期、时间、站点名称和环境变量替换，就像URL列表中的替换一样。点击 <span style="border: 1px solid black; padding: 3px;">?</span> 按钮以显示目录路径中详细的替换内容。

![Keyword Replacement Dialog of RTKGET](https://i.ibb.co/qjgV1g4/image.png)

### 3.9.4 下载操作

点击 <span style="border: 1px solid black; padding: 3px;">Download</span> 按钮开始下载。第三个消息区域显示每个文件的下载状态指示器。指示器“_”表示正在进行，“o”表示下载成功，“.”表示已跳过，“x”表示无数据，“X”表示下载出错。若想中途终止下载，点击 <span style="border: 1px solid black; padding: 3px;">Abort</span> 按钮。即使点击了该按钮，正在进行的最后一次下载也无法取消。请稍等片刻，待最后一次下载完成后再终止。

![Data Downloading by RTKGET](https://i.ibb.co/kMwqqVd/image.png)

完成所有下载或终止下载后，点击主窗口中的 <span style="border: 1px solid black; padding: 3px;">Files...</span> 按钮，以执行显示已下载本地目录的Windows资源管理器。要查看下载日志，点击 <span style="border: 1px solid black; padding: 3px;">Log...</span> 按钮。您可以在“文本查看器”窗口中看到下载日志。要启用下载日志功能，您需要在“选项”对话框中设置“下载日志文件”字段。如果为下载日志勾选了“追加”，则日志将添加到现有的日志文件中（如果存在）。如果没有勾选，则每次下载尝试时都会新生成下载日志。

![Download Log View by RTKGET](https://i.ibb.co/crxnF20/image.png)

### 3.9.5 可用性测试

在实际下载数据之前，测试本地文件中GNSS数据的存在性，设置完成后（与上述实际下载设置相同），点击 <span style="border: 1px solid black; padding: 3px;">Test...</span> 按钮。您可以在“文本查看器”窗口中看到本地数据可用性报告。通过点击 <span style="border: 1px solid black; padding: 3px;">Save...</span> 按钮并指定文件保存对话框中的文件路径，您可以保存该报告。本地数据可用性报告的列数和日期格式可以通过“选项”对话框进行修改。

![Local File Test View by RTKGET](https://i.ibb.co/BP85z6L/image.png)

### 3.9.6 日志信息

默认情况下，下载进度和错误的详细日志会自动删除。若要保留此类错误日志，请在“选项”对话框中勾选“下载错误时保留错误信息”。每个下载文件的详细错误日志将被保存为 `<local dir>\<file>.err` 文件。为了便于分析问题，也可以勾选“保留远程目录列表”。远程文件列表将被保存为当前目录下的 `.listing` 文件。

<GiscusTalk />
