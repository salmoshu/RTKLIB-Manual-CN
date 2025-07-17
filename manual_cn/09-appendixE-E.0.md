---
sidebarDepth: 2
---

# 附录E. 模型与算法

附录中使用的常见模型或方程的缩写如下所示：

<style>
  .align-colon {
    display: inline-block;
    width: 100px; /* 根据最长内容调整宽度 */
  }
</style>
<span class="align-colon">$c$</span>
: 光速 (m/s)<br>
<span class="align-colon">$P_{r,i}^s$</span>
: $L_i$ 频段伪距 (m)<br>
<span class="align-colon">$D_{r,i}^s$</span>
: $L_i$ 频段多普勒频率 (Hz)<br> 
<span class="align-colon">$\phi_{r,i}^s$</span>
: $L_i$ 频段载波相位 (cycle)<br>
<span class="align-colon">$\Phi_{r,i}^s$</span>
: $L_i$ 频段载波相位 (m)<br>
<span class="align-colon">$t_r$</span>
: 导航信号发送至接收机时间，即接收机收到信号的时间 (s)<br>
<span class="align-colon">$t_s$</span>
: 卫星发送导航信号时间 (s)<br>
<span class="align-colon">$\rho_r^s$</span>
: 卫星和接收机天线之间的几何距离 (m)<br>
<span class="align-colon">$r_r^s$</span>
: 卫星和接收机天线之间的伪距率 (m/s)<br>
<span class="align-colon">$\boldsymbol{r}^s(t)$</span>
：t时刻ECEF下卫星位置（m）<br>
<span class="align-colon">$\boldsymbol{v}^s(t)$</span>
：t时刻ECEF下卫星速度(m/s)<br>
<span class="align-colon">$\boldsymbol{r}_r(t)$</span>
：t时刻ECEF下接收机天线位置（m）<br>
<span class="align-colon">$\boldsymbol{v}_r(t)$</span>
：t时刻ECEF下接收机天线速度（m/s）<br>
<span class="align-colon">$\boldsymbol{e}^s_r$</span>
：ECEF下，接收机到卫星的LOS单位向量<br>
<span class="align-colon">$\boldsymbol{e}^s_{r,enu}$</span>
：当地坐标系下，接收机到卫星的单位向量<br>
<span class="align-colon">$\boldsymbol{E}_r$</span>
：ECEF坐标系到大地坐标系的旋转变换矩阵<br>
<span class="align-colon">$\boldsymbol{E}^s$</span>
：ECEF坐标系到卫星载体坐标系的旋转变换矩阵<br>
<span class="align-colon">$\varphi_r$</span>
：接收机天线位置的纬度/大地维度<br>
<span class="align-colon">$\lambda_r$</span>
：接收机天线位置的经度/大地经度<br>
<span class="align-colon">$Az^s_r$</span>
：当前位置观测，卫星的方位角（rad）<br>
<span class="align-colon">$El_r^s$</span>
：当前位置观测，卫星的高度角（rad）<br>
<span class="align-colon">$dt_r(t)$</span>
：t时刻接收机钟差（s）<br>
<span class="align-colon">$d\dot{t}_r(t)$</span>
：t时刻接收机钟漂——频差（s/s）<br>
<span class="align-colon">$dT^s(t)$</span>
：t时刻卫星钟差（s）<br>
<span class="align-colon">$d\dot{T}^s(t)$</span>
：t时刻卫星钟漂——频差（s/s）<br>
<span class="align-colon">$T_r^s$</span>
：对流层延迟（m）<br>
<span class="align-colon">$I_{r,i}^s$</span>
：L频段电离层延迟（m）<br>
<span class="align-colon">$f_i$</span>
：$L_i$频段载波频率（Hz）<br>
<span class="align-colon">$\lambda_i$</span>
：$L_i$频段波长（m）<br>
<span class="align-colon">$B_{r,i}^s$</span>
：L频段载波偏差（cycle）<br>
<span class="align-colon">$N_{r,i}^s$</span>
：L频段载波整周模糊度（cycle）<br>
<span class="align-colon">$\varepsilon_P$</span>
：伪距测量误差（m）<br>
<span class="align-colon">$\varepsilon_\phi$</span>
：载波相位测量误差（cycle）<br>
<span class="align-colon">$\varepsilon_\Phi$</span>
：载波相位测量误差（m）<br>
<span class="align-colon">$\omega_e$</span>
：地球自转角速度（rad/s）<br>
<span class="align-colon">$\boldsymbol{d}_{r,pco,i}$</span>
：当地坐标系下，L频段接收机天线相位中心偏差(PCO)（m）<br>
<span class="align-colon">$\boldsymbol{d}_{pco,i}^s$</span>
：卫星载体坐标系下，卫星天线L波段相位中心偏差(PCO)（m）<br>
<span class="align-colon">$d_{r,pcv,i}(El)$</span>
：$L_i$频段接收机天线相位中心变化PCV（m）<br>
<span class="align-colon">$d_{pcv,i}^s(\theta)$</span>
：$L_i$频段卫星天线相位中心变化PCV（m）<br>
<span class="align-colon">$\alpha$</span>
：接收机天线到卫星的天底角（$\alpha$）<br>
<span class="align-colon">$\boldsymbol{d}_{r,disp}$</span>
：当地坐标系下，接收机天线位置处的地球潮偏移<br>
<span class="align-colon">$\phi_{pw}$</span>
：相位缠绕效应（cycle）<br>
<span class="align-colon">$dT_{rel}^s$</span>
：卫星时钟的相对论改正<br>
<span class="align-colon">$\boldsymbol{U}(t)$</span>
：t时刻ECI和ECEF的坐标系转换矩阵<br>
<span class="align-colon">$\boldsymbol{R}_x(\theta)$</span>
：绕X轴旋转θ得到的坐标旋转矩阵<br>
<span class="align-colon">$\boldsymbol{R}_y(\theta)$</span>
：绕Y轴旋转θ得到的坐标旋转矩阵<br>
<span class="align-colon">$\boldsymbol{R}_z(\theta)$</span>
：绕Z轴旋转θ得到的坐标旋转矩阵<br>
<span class="align-colon">$()^{jk}$</span>
：卫星端，卫星j和k之间的单差<br>
<span class="align-colon">$()_{rb}$</span>
：地面端，基准站b和移动站r之间的单差<br>
<span class="align-colon">$Z_{T,r}$</span>
：对流层天顶总延迟（m）<br>
<span class="align-colon">$Z_{H,r}$</span>
：对流层天顶干延迟（m）<br>
<span class="align-colon">$G_{E,r}$</span>
：对流层东向梯度分量<br>
<span class="align-colon">$G_{N,r}$</span>
：对流层北向梯度分量 <br>
<span class="align-colon">$m_{H,r}^s$</span>
：天顶对流层干延迟映射函数<br>
<span class="align-colon">$m_{W,r}^s$</span>
：天顶对流层湿延迟映射函数<br>
<span class="align-colon">$m_{I,r}^s$</span>
：电离层延迟映射函数<br>

<GiscusTalk />
