# 附录A: 延伸阅读

## A.1 北斗信号解析过程

北斗信号解析过程在RTKLIB中主要在rtcm3.c和rinex.c中，主要关注引用obs2code和code2idx函数的地方，其他星座信号解析方法相同，具体的：

```c
/* 1. 实时解析：save_msm_obs (rtcm3.c) */
/* signal to rinex obs type */
code[i]=obs2code(sig[i]);
idx[i]=code2idx(sys,code[i]);
…
/* get signal index */
sigindex(sys,code,h->nsig,rtcm->opt,idx);
/* 2. 后处理解析：readrnxobsb/set_index (rinex.c) */
/* set signal index */
for (i=n=0;*tobs[i];i++,n++) {
    ind->code[i]=obs2code(tobs[i]+1);
    ind->type[i]=(p=strchr(obscodes,tobs[i][0]))?(int)(p-obscodes):0;
    ind->idx[i]=code2idx(sys,ind->code[i]);
    ind->pri[i]=getcodepri(sys,ind->code[i],opt);
    ind->pos[i]=-1;
}
```

以下是解析流程的概述：

**1. 设置索引**

初始化信号标识的索引，将频率带（如B1-2、B2a、B3）及其调制类型（I、Q、I+Q、数据、导频等）映射到内部数据结构。

**2. obs2code**

```c
static char *obscodes[]={ /* observation code strings */
    "" ,"1C","1P","1W","1Y", "1M","1N","1S","1L","1E", /* 0- 9 */
    "1A","1B","1X","1Z","2C", "2D","2S","2L","2X","2P", /* 10-19 */
    "2W","2Y","2M","2N","5I", "5Q","5X","7I","7Q","7X", /* 20-29 */
    "6A","6B","6C","6X","6Z", "6S","6L","8L","8Q","8X", /* 30-39 */
    "2I","2Q","6I","6Q","3I", "3Q","3X","1I","1Q","5A", /* 40-49 */
    "5B","5C","9A","9B","9C", "9X","1D","5D","5P","5Z", /* 50-59 */
    "6E","7D","7P","7Z","8D", "8P","4A","4B","4X","" /* 60-69 */
};
```

此函数将观测码标识（如"2I"、"QX"）转换为obscodes数组中的索引（对应RTKLIB中的CODE_*常量）。例如，B1-2/1561.098的"2I"可能映射为CODE_L1I，表示L1频率的同相分量。其中obscodes数组如上所示。

**3. code2idx**

确定每个频率数据的存储位置。例如，根据优先级分配位置（如L1在0号位），确保观测数据一致性。其中调用的code2freq_BDS函数，从而最终决定了北斗频点的使用规则。

**4. getcodepri**

```c
static char codepris[7][MAXFREQ][16]={ /* code priority for each freq-index */
    /* L1/E1/B1I L2/E5b/B2I L5/E5a/B2a E6/LEX/B3I E5(a+b)/B2(a+b) */
    {"CPYWMNSL","CPYWMNDLSX","IQX" ,"" ,"" }, /* GPS */
    {"CPABX" ,"CPABX" ,"IQX" ,"" ,"" }, /* GLO */
    {"CABXZ" ,"XIQ" ,"XIQ" ,"ABCXZ" ,"IQX" }, /* GAL */
    {"CLSXZ" ,"LSX" ,"IQXDPZ" ,"LSXEZ" ,"" }, /* QZS */
    {"C" ,"IQX" ,"" ,"" ,"" }, /* SBS */
    {"IQXDPAN" ,"IQXDPZ" ,"DPX" ,"IQXA" ,"DPX" }, /* BDS */
    {"ABCX" ,"ABCX" ,"" ,"" ,"" } /* IRN */
};
```

评估同一频率下不同码（如I、Q、I+Q）的优先级，使用rtkcmn.c中的codepris数组。

<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">表A-1 RINEX 3.04 北斗观测码</p>

![](https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250707-235455.jpg)

基于预定义优先级选择最高优先级的码，例如I优于I+Q。

## A.2 自适应MASK阈值

可以将mask的判断逻辑放到了rescode的内部for循环中，也可以在外面构建，然后循环执行rescode函数，然后传入拷贝的opt变量，这样代码会更清晰一些。

## A.3 MGEX-AGNSS服务

服务网址：[mgex.igs-ip.net GNSS Streaming Server](https://mgex.igs-ip.net/home)
<!-- 
width: 300px; /* 设置显示区域的宽度 */
height: 200px; /* 设置显示区域的高度 */
overflow: hidden; /* 隐藏超出部分 */
object-fit: cover; /* 裁剪图片以适应容器 */
object-position: center; /* 裁剪图片的中心部分 */ -->

<div style="width: 700px; height: 420px; overflow: hidden; margin: 0 auto;">
  <img src="https://raw.githubusercontent.com/salmoshu/Winchell-ImgBed/main/img/20250616-174914.jpg" alt="Cropped Image" style="width: 100%; height: 100%; object-fit: cover; object-position: top;">
</div>

<p style="text-align: center; font-family: 'Microsoft YaHei', SimSun, Arial, sans-serif; font-size: 14px;">图A-1 RTKLIB NTRIP Browser</p>

使用RTKLIB的NTRIP服务工具连接星历AGNSS服务，选择RTCM3EPH-MGEX挂载点。可以直接挂着该服务让它每天源源不断接收RTCM格式的星历数据即可，由于并不是流媒体格式，因此并不会占据很多内存。长期后台录制主要是为了避免有时候做实验忘记记录星历了。

后面可以使用RTKCONV将RTCM星历数据转换为RINEX格式，并用在算法优化上。

## A.4 批处理程序

::: details 点击查看代码
```python
import os
import subprocess
import time

################## USER INPUT BEGIN #######################
"""
    Referring: https://rtklibexplorer.wordpress.com/2022/01/05/batch-processing-rtklib-solutions-with-rnx2rtkp-and-python/
    Note     :
        该脚本为后处理批处理脚本，能同时跑不同配置的多组数组，具体的：
        会运行CONF_PATH配置文件夹下所有的配置，以及DATA_PATH文件夹下所有的RINEX数据，
        结果会以配置文件的名称（CONF_PATH）作为分类文件夹，存储多组结果数据（DATA_PATH内所有RINEX文件）。
    TBD      :
        1）需要解决DATA_DIR下属直接包含RINEX文件但不能处理的BUG
"""
BIN_DIR  = "../app/rnx2rtkp/msc/Release" # 可执行文件所在目录
DATA_DIR = "../data"                     # 输入文件目录。输入文件要求在data目录内部，且名称必须为rover.obs、base.obs、rover.nav
CONF_DIR = "../app/rnx2rtkp/conf"        # 配置文件夹。配置文件必须以.conf结尾
OUTS_DIR = "./output"                    # 输出文件
TRACE_LEVEL = 0                          # trace等级
##################  USER INPUT END  #######################

# set maximum number of simultaneous occurences of rnx2rtkp to run
max_windows = 14
poll_list = [None] * max_windows
slash_type = "\\" if os.name == "nt" else "/"

def ExecPostProc(bin, src, out, cfg):
    # create command to run solution
    if os.name == "nt":
        rtk_cmd = r'%s\rnx2rtkp.exe -ti 1.0 -x %d -y 0 -k %s -o %s.pos %s\rover.obs %s\base.obs %s\rover.nav' % \
                  (bin, TRACE_LEVEL, cfg, out, src, src, src)
    else:
        rtk_cmd = r'%s/rnx2rtkp -x %d -y 0 -k %s -o %s.pos %s/rover.obs %s/base.obs %s/rover.nav' % \
                  (bin, TRACE_LEVEL, cfg, out, src, src, src)
    # run command
    return subprocess.Popen(rtk_cmd, shell=True)

def GetConfigs(cfg_path):
    cfgs = []
    paths = os.walk(cfg_path)
    for _, _, files in paths:
        for f in files:
            if ".conf" in f:
                cfgs.append(f)
    return cfgs

def GenPostProcParas(data_path):
    input_dirs = []
    output_files = []
    
    paths = os.walk(data_path)
    for path, dir_lst, _ in paths:
        for dir_name in dir_lst:
            if (os.path.exists(os.path.join(path, dir_name)+"/rover.obs")):
                target_dir = os.path.join(path, dir_name)
                target_arr = target_dir.split(slash_type+"data"+slash_type)[-1].split(slash_type)
                pos_name = "_".join(target_arr)
                input_dirs.append(target_dir)
                output_files.append(pos_name)
    return input_dirs, output_files

def GetPollStat(polls):
    stats = []
    for p in polls:
        if p is not None and (p.poll() is None):
            stats.append(1)
        else:
            stats.append(0)
    return stats

def CheckIsFinished(polls):
    is_finished = True
    for p in polls:
        if p is not None and (p.poll() is None):
            is_finished = False
    return is_finished

# 删除output中所有的event文件
def DeleteFilesWithName(folder_path, keyword):
    for root, _, files in os.walk(folder_path):
        for file in files:
            if keyword in file:
                file_path = os.path.join(root, file)
                os.remove(file_path)

if __name__ == "__main__":
    start_time = time.time()
    cur_path = os.path.abspath(os.path.dirname(__file__))+"/"
    bin_path = os.path.abspath(cur_path + BIN_DIR)
    data_path = os.path.abspath(cur_path + DATA_DIR)
    conf_path = os.path.abspath(cur_path + CONF_DIR)
    output_path = os.path.abspath(cur_path + OUTS_DIR)

    input_paths, output_files = GenPostProcParas(data_path)
    cfgs = GetConfigs(conf_path)

    # loop through date folders
    ntasks = 0
    for src, out in zip(input_paths, output_files):
        for cfg in cfgs:
            is_idle = 0
            # if max windows open, wait for one to close
            while is_idle == 0:
                for j, p in enumerate(poll_list):
                    if p is None:
                        is_idle = 1
                    elif p.poll() is not None:
                        poll_list[j] = None
                        is_idle = 1
                time.sleep(1)  # wait here for existing window to close

            for j, p in enumerate(poll_list):
                if p is None:
                    is_idle = 1
                    out_path = os.path.join(output_path, cfg+slash_type+out)
                    poll_list[j] = ExecPostProc(bin_path, src, out_path, conf_path+slash_type+cfg)
                    ntasks += 1
                    print('Process: {:0>3d}/{:0>3d} '.format(ntasks, len(output_files)*len(cfgs)))
                    break            

    # Wait for all solutions to finish
    print('Waiting for solutions to complete ...')
    poll_sum = 0
    while CheckIsFinished(poll_list) == False:
        poll_stat = GetPollStat(poll_list)
        if (poll_sum != sum(poll_stat)):
            poll_sum = sum(poll_stat)
            print('Poll busy status({:0>3d}/{:0>3d}): '.format(poll_sum, max_windows), poll_stat)
        time.sleep(1)
    end_time = time.time()
    DeleteFilesWithName(output_path, "_events.pos")
    print('Batch processing completed in {0:.2f}s'.format(end_time-start_time))
```
:::

这是一个 Python 脚本，用于批量运行 RTKLIB 的 `rnx2rtkp` 程序，处理 RINEX 格式的 GNSS 数据，可以结合多个配置文件（`.conf`），生成定位结果（`.pos` 文件）。

**1. 核心功能**
- **输入**：
  - 可执行文件目录（`BIN_DIR`）：`rnx2rtkp` 程序路径。
  - 数据目录（`DATA_DIR`）：包含 RINEX 文件的子目录。
  - 配置目录（`CONF_DIR`）：`.conf` 配置文件。
  - 输出目录（`OUTS_DIR`）：结果按配置文件名分类存储。
- **输出**：结果存储在 `OUTS_DIR/<配置文件名>/<数据子目录>.pos`。

**2. 特点**
- 支持 Windows/Linux（动态路径分隔符）。
- 限制并行进程数以优化性能（`max_windows`）。
