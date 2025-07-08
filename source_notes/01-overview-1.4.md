# 1. 初识RTKLIB

## 1.4 主程序应用

### 1.4.1 后处理程序

**1. 后处理程序执行流程（rnx2rtkp）**

<img src="https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231025155540386.png" alt="image-20231025155540386" style="zoom:80%;" />
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1-7 RTKLIB 后处理程序组织结构</p>

**2. 后处理函数调用关系（postpos）**

<img src="https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231012212121216.png" alt="image-20231012212121216" style="zoom:80%;" />
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1-8 RTKLIB 后处理函数调用关系</p>

### 1.4.2 实时处理程序

**1. 实时处理程序执行流程（rtkrcv）**

<img src="https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/image-20231024203128852.png" alt="image-20231024203128852" style="zoom:80%;" />
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1-9 RTKLIB 实时处理程序组织结构</p>

**2. 实时处理函数调用关系（rtkrcv）**

<img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250707-220645.png" alt="image-20250707220645051" style="zoom:80%;" />
<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图1-10 RTKLIB 实时处理函数调用关系</p>
