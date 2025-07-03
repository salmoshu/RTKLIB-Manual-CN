---
sidebarDepth: 2
---

# 附录E. 模型与算法

## E.2 坐标系统

在RTKLIB中，接收机和卫星的位置以ECEF（地心地固坐标系）的X、Y、Z分量表示。ECEF框架的选择取决于卫星星历和基准站位置，并未明确固定。例如，使用GPS信号时，单点定位结果通常基于WGS84坐标系。在基线分析中，移动站的位置会被转换到与基准站相同的ECEF框架中。实际上，常用的ECEF框架（如WGS84、PZ90.02和ITRF）在广播星历或单点定位精度范围内是等效的。然而，对于基线分析或PPP（精密单点定位），需要更严格地处理坐标系，使用统一的坐标系可以有效减少处理误差。

### E.2.1 从大地坐标转换为ECEF XYZ坐标

大地坐标是基于参考椭球体定义的，如图E.2-1所示。大地纬度$\phi_r$、经度$\lambda_r$和椭球高$h$可以转换为ECEF XYZ坐标$rr = (x, y, z)^T$，转换公式如下：

$\begin{equation}
e^2 = f(2-f) \tag{E.2.1}
\end{equation}$

$\begin{equation}
v = \frac{a}{\sqrt{1-e^2sin{\phi_r}^2}} \tag{E.2.2}
\end{equation}$

$\begin{equation}
r_r=\begin{pmatrix}
(v+h)cos\phi_r cos\lambda_r  \\
(v+h)cos\phi_r sin\lambda_r  \\
v(1-e^2)sin\phi_r
\end{pmatrix}\tag{E.2.3}
\end{equation}$

其中，<br>
$a$：地球参考椭球体的长半轴（米）<br>
$f$：地球参考椭球体的扁率

当前版本的RTKLIB始终使用WGS84坐标系提供的参考椭球体的$a$和$f$值如下：

$\begin{equation}
a = 6378137.0 (m)
\end{equation}$ <br>
$\begin{equation}
f = 1.0/298.257223563
\end{equation}$

![Reference Ellipsoid](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250218-224823.jpg)
<p style="text-align: center;">图E.2-1  参考椭球体</p> 

### E.2.2 将ECEF坐标转换为大地坐标

将ECEF中的XYZ位置 $r_r ​= (x,y,z)^T$ 转换为大地坐标，需采用以下步骤。在此过程中，大地纬度是通过迭代方法计算得出的。

$\begin{equation}
r = \sqrt{x^2 + y^2} \tag{E.2.4}
\end{equation}$

$\begin{equation}
\Phi_{r,0} = 0       \tag{E.2.5}
\end{equation}$

$\begin{equation}
\Phi_{r,i+1} = arctan(\frac{z}{r} - \frac{ae^2tan\phi_{r,i}}{r\sqrt{1+(1-e^2)tan^2\phi_{r,i}}}) \tag{E.2.6}
\end{equation}$

$\begin{equation}
\Phi_{r,0} = \lim\limits_{x \to \infty} \phi_{r,i} \tag{E.2.7}
\end{equation}$

$\begin{equation}
\lambda = ATAN2(y,x) \tag{E.2.8}
\end{equation}$

$\begin{equation}
h = \frac{r}{cos\phi_r} - \frac{a}{\sqrt{(1-e^2)sin^2\phi_r}} \tag{E.2.9}
\end{equation}$

### E.2.3本地坐标与ECEF坐标之间的转换

接收机位置的局部坐标（也称为ENU坐标）在GNSS导航处理中经常被使用。从ECEF坐标到局部坐标的旋转矩阵 $E_r$​ 表示为：

$\begin{equation}
E_r=\begin{pmatrix}
-sin\lambda_r            & cos\lambda_r            & 0                 \\
-sin{\phi_r}cos_{\lambda_r} & -sin{\phi_r}sin{\lambda_r} & cos{\phi_r} \\
cos{\phi_r}cos{\lambda_r}   & cos{\phi_r}sin{\lambda_r}  & sin\phi_r
\end{pmatrix}\tag{E.2.10}
\end{equation}$

其中，$\phi_r$和$\lambda_r$分别是接收机位置的大地纬度和经度。通过使用旋转矩阵$E_r$以及接收机在ECEF坐标系中的位置$r_r$，可以将ECEF坐标系中的位置$r_{ECEF}$转换为局部坐标系中的位置$r_{local}$，转换公式如下：

$\begin{equation}
r_{local} = E_r(r_{ECEF} - r_r) \tag{E.2.11}
\end{equation}$

### E.2.4 大地水准面模型

为了从椭球高得到大地高，我们需要考虑似大地水准面高。大地高 hgeod​（单位：米）表示为：

$\begin{equation}
h_{geod} = h - geod(\phi_r, \lambda_r) \tag{E.2.12}
\end{equation}$

其中，$geoid{\phi_r, \lambda_r}$ 是在纬度 $\phi_r$ 和经度 $\lambda_r$​ 处的似大地水准面高（单位：米）。RTKLIB支持通过处理选项“似大地水准面模型”选择以下几种似大地水准面模型。所有这些模型均以纬度和经度网格点上的似大地水准面高提供。RTKLIB使用双线性插值来获取适用于特定纬度和经度位置的值。除了内部模型外，使用这些似大地水准面模型时，需要下载相应的似大地水准面数据文件，并将其文件路径设置为处理选项“似大地水准面数据文件”。

- **内置模型：** 基于EGM96的1度×1度网格似大地水准面模型
- **EGM96：** 15" x 15" 网格的EGM96似大地水准面模型
- **EGM2008：** 2.5" x 2.5" 网格或1弧秒×1弧秒网格的EGM2008似大地水准面模型
- **GSI 2000：** 1" x 1.5" 弧秒网格的GSI 2000似大地水准面模型（仅适用于日本列岛周边）
