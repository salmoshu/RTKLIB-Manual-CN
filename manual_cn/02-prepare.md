---
sidebarDepth: 2
---

# 2. 准备

## 2.1 系统要求
RTKLIB中包含的GUI和CUI可执行文件能在Windows<sup>[35]</sup>环境下工作，在其他操作系统或环境中（如Linux），则需要自行编译和构建CUI应用。

所有的库函数和API都是用ANSI C（C89）编写的。该库内部使用Windows的winsock和WIN32线程，带有编译器选项DWIN32，Linux/UNIX的标准套接字和pthread（POSIX线程）则没有任何选项。通过设置编译器选项-DLAPACK或-DMKL，则可以使用LAPACK/BLAS<sup>[36]</sup>或英特尔MKL<sup>[37]</sup>进行快速矩阵运算。

CUI应用也是用ANSI C编写的。核心库和CUI应用可以在许多环境中构建，如Linux（使用gcc）。GUI应用是用C++编写的，并使用Embarcadero/Borland VCL（可视化组件库）<sup>[38]</sup>作为GUI工具包。而在demo5维护的版本中开始使用QT来进行GUI应用开发。

## 2.2 License
RTKLIB根据以下BSD 2条款许可证<sup>[40]</sup>和另外两个独家条款分发。只要用户遵守许可证，他们就可以使用、链接或包含RTKLIB来开发、生产或销售自己的非商业或商业产品。

注：在2.4.1版本之前，RTKLIB的早期版本是在GPLv3<sup>[59]</sup>许可证下分发的。

```txt
The RTKLIB software package is distributed under the following BSD 2-clause
license. Users are permitted to develop, produce or sell their own non-
commercial or commercial products utilizing, linking or including RTKLIB as long
as they comply with the license.

--------------------------------------------------------------------------------

         Copyright (c) 2007-2020, T. Takasu, All rights reserved.

Redistribution and use in source and binary forms, with or without modification,
are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright notice, this list
of conditions and the following disclaimer. Redistributions in binary form must
reproduce the above copyright notice, this list of conditions and the following
disclaimer in the documentation and/or other materials provided with the
distribution.

The software package includes some companion executive binaries or shared
libraries necessary to execute APs on Windows. These licenses succeed to the
original ones of these software.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

--------------------------------------------------------------------------------

Notes:
Previous versions of RTKLIB until ver. 2.4.1 had been distributed under GPLv3
license.

```
<GiscusTalk />
