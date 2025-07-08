# 3. 操作指南

## 3.11 RTKLIB CUI程序

RTKLIB包含了以下命令行界面（CUI）应用程序。这些CUI应用程序仅使用标准ANSI C（C89）函数以及少量标准C库，以确保其可移植性。因此，您可以在非Windows环境（如Linux、Unix、Mac OS X等）上构建这些CUI应用程序。您还可以在嵌入式CPU（如ARM）上构建并运行这些应用程序。关于这些应用程序（包括命令行选项）的详细信息，请参阅附录A中的CUI命令参考。有关构建这些应用程序的步骤，请参考RTKLIB的第4.2节“应用构建”。请注意，您可能需要修改makefile以适应构建环境，但在大多数情况下，程序本身无需修改即可在不同环境中移植。

**RTKRCV**<br>
实时定位。RTKNAVI的CUI版本。

**RNX2RTKPV**<br>
后处理分析。RTKPOST的CUI版本。

**POS2KMLV**<br>
用于将RTKLIB解算结果文件转换为Google Earth KML文件。

**CONVBINV**<br>
接收机原始数据的RINEX转换器。RTKCONV的CUI版本。

**STR2STRV**<br>
数据流服务器。STRSVR的CUI版本。
