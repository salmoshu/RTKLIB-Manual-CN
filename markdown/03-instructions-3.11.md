# 3. 操作指南

## 3.11 RTKLIB CUI程序

RTKLIB包含了以下命令行界面（CUI）应用程序。这些CUI应用程序仅使用标准ANSI C（C89）函数和少量标准C库，以确保其可移植性。因此，您可以在非Windows环境，如LINUX、UNIX、MAC OS X等上构建这些CUI应用程序。您也可以在嵌入式CPU，如ARM上构建和执行这些应用程序。关于这些应用程序（包括命令行选项）的详细信息，请参考附录A CUI命令参考。关于构建这些应用程序的步骤，请参考4.2节 RTKLIB的CUI应用程序构建。请注意，您可能需要修改makefile以适应构建环境，但在大多数情况下，程序本身无需修改即可移植。

(1) RTKRCV <br>
Real-time Positioning. The console AP version of RTKNAVI.

(2) RNX2RTKPV <br>
Post-Processing Analysis. The console AP version of RTKPOST.

(3) POS2KMLV <br>
Google Earth KML converter for solution files.

(4) CONVBINV <br>
RINEX Converter of receiver raw data. The console AP version of RTKCONV.

(5) STR2STRV <br>
Stream Server. Console AP version of STRSVR.
