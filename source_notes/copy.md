
# 1. 相对定位

### 11.1.9 卫星系统的表示

- 表示卫星系统的字母：G：GPS、R：GLONASS、E：GALILEO、C：BDS、J：QZSS，I：IRNSS、S：SBAS

- 7位二进制码表示，对应位写1表示有对应的系统，做与或算可加系统。

  ```c
  static const int navsys[]={             /* satellite systems */
      SYS_GPS,SYS_GLO,SYS_GAL,SYS_QZS,SYS_SBS,SYS_CMP,SYS_IRN,0
  };
  ```

  ```
  #define SYS_NONE    0x00                /* navigation system: none */
  #define SYS_GPS     0x01                /* navigation system: GPS */
  #define SYS_SBS     0x02                /* navigation system: SBAS */
  #define SYS_GLO     0x04                /* navigation system: GLONASS */
  #define SYS_GAL     0x08                /* navigation system: Galileo */
  #define SYS_QZS     0x10                /* navigation system: QZSS */
  #define SYS_CMP     0x20                /* navigation system: BeiDou */
  #define SYS_IRN     0x40                /* navigation system: IRNS */
  #define SYS_LEO     0x80                /* navigation system: LEO */
  #define SYS_ALL     0xFF                /* navigation system: all */
  ```

### 11.1.11 卫星的表示

可以表示为各系统的卫星ID（系统缩写+PRN）：B02、C21，也可表示为连续的satellite number ，各种转换函数如下：

- **satno()**：传入卫星系统(SYS_GPS,SYS_GLO,...) ，和PRN码，转换为连续的satellite number。
- **satsys()**：传入satellite number ，返回卫星系统(SYS_GPS,SYS_GLO,...) ，通过传入的指针prn传出PRN值。
- **satid2no()**：传入卫星ID，返回satellite number。
- **satno2id()**：传入卫星系统，和PRN，返回卫星ID(Gxx,Cxx)
- **sat2code()**：传入satellite number，返回卫星ID(Gxx,Cxx)
- **code2sys()**：传入卫星系统缩写，返回系统二进制码SYS_XXX。
- **satexclude()**：检测某颗卫星在定位时是否需要将其排除

### 11.1.12 观测值类型的表示

- C：伪距、D：多普勒、L：载波相位、S：载噪比

```c
  static const char obscodes[]="CLDS";    /* observation type codes */
```

- sigind_t：表示每种卫星系统的载波类型和观测值类型 ，每种类型的系统其实对应的就是一个sigind_t结构体，也就是说只需要建立七个结构体就够了。 

  ```c
  typedef struct {                        /* signal index type */
      int n;                              /* number of index */   //n代表这个卫星系统总的观测值类型，对应的卫星系统标识符后面的数字
      int idx[MAXOBSTYPE];                /* signal freq-index */
      int pos[MAXOBSTYPE];                /* signal index in obs data (-1:no) */
      uint8_t pri [MAXOBSTYPE];           /* signal priority (15-0) */
      uint8_t type[MAXOBSTYPE];           /* type (0:C,1:L,2:D,3:S) */
      uint8_t code[MAXOBSTYPE];           /* obs-code (CODE_L??) */
      double shift[MAXOBSTYPE];           /* phase shift (cycle) */
  } sigind_t;
  
  ```

- **CODE_XXX**：观测值类型定义，用一串连续的数字表示。

  ```c
  #define CODE_NONE   0                   /* obs code: none or unknown */
  #define CODE_L1C    1                   /* obs code: L1C/A,G1C/A,E1C (GPS,GLO,GAL,QZS,SBS) */
  #define CODE_L1P    2                   /* obs code: L1P,G1P,B1P (GPS,GLO,BDS) */
  ......
  #define CODE_L4B    67                  /* obs code: G1aL1OCd   (GLO) */
  #define CODE_L4X    68                  /* obs code: G1al1OCd+p (GLO) */
  #define MAXCODE     68                  /* max number of obs code */
  ```

- **code2obs**()：传入obs code (CODE_???) ，返回code string ("1C","1P","1Y",...)

- **obs2code**()：传入code string ("1C","1P","1Y",...)，返回obs code (CODE_???) 

  ```c
  static char *obscodes[]={       /* observation code strings */
      
      ""  ,"1C","1P","1W","1Y", "1M","1N","1S","1L","1E", /*  0- 9 */
      "1A","1B","1X","1Z","2C", "2D","2S","2L","2X","2P", /* 10-19 */
      "2W","2Y","2M","2N","5I", "5Q","5X","7I","7Q","7X", /* 20-29 */
      "6A","6B","6C","6X","6Z", "6S","6L","8L","8Q","8X", /* 30-39 */
      "2I","2Q","6I","6Q","3I", "3Q","3X","1I","1Q","5A", /* 40-49 */
      "5B","5C","9A","9B","9C", "9X","1D","5D","5P","5Z", /* 50-59 */
      "6E","7D","7P","7Z","8D", "8P","4A","4B","4X",""    /* 60-69 */
  };
  ```

- **code2idx**()：传入obs code (CODE_???) 和卫星系统(SYS_???) ，返回载波频率的下标

  ```c
  frequency index (-1: error)
                        0     1     2     3     4 
             --------------------------------------
              GPS       L1    L2    L5     -     - 
              GLONASS   G1    G2    G3     -     -  (G1=G1,G1a,G2=G2,G2a)
              Galileo   E1    E5b   E5a   E6   E5ab
              QZSS      L1    L2    L5    L6     - 
              SBAS      L1     -    L5     -     -
              BDS       B1    B2    B2a   B3   B2ab (B1=B1I,B1C,B2=B2I,B2b)
              NavIC     L5     S     -     -     - 
  ```

- **code2freq**()：传入obs code (CODE???))和卫星系统(SYS_???)，以及GLONASS的信道，调用code2freq_GPS()、code2freq_GLO()、code2freq_GAL()、code2freq_QZS()、code2freq_SBS()、code2freq_BDS()、code2freq_IRN()，返回对应的载波频率(Hz) 

- **sat2freq()**：传入satellite number和obs code ，返回对应的载波频率(Hz) 

- **setcodepri()**、**getcodepri()**：设置和获取信号优先级。如果输入的观测数据在同一频率内包含多个信号，RTKLIB将按照以下默认优先级选择一个信号进行处理。 

  ```c
  static char codepris[7][MAXFREQ][16]={  /* code priority for each freq-index */
     /*    0         1          2          3         4         5     */
      {"CPYWMNSL","PYWCMNDLSX","IQX"     ,""       ,""       ,""      ,""}, /* GPS */
      {"CPABX"   ,"PCABX"     ,"IQX"     ,""       ,""       ,""      ,""}, /* GLO */
      {"CABXZ"   ,"IQX"       ,"IQX"     ,"ABCXZ"  ,"IQX"    ,""      ,""}, /* GAL */
      {"CLSXZ"   ,"LSX"       ,"IQXDPZ"  ,"LSXEZ"  ,""       ,""      ,""}, /* QZS */
      {"C"       ,"IQX"       ,""        ,""       ,""       ,""      ,""}, /* SBS */
      {"IQXDPAN" ,"IQXDPZ"    ,"DPX"     ,"IQXA"   ,"DPX"    ,""      ,""}, /* BDS */
      {"ABCX"    ,"ABCX"      ,""        ,""       ,""       ,""      ,""}  /* IRN */
  };
  ```

### 后处理处理流程

![](https://pic-bed-1316053657.cos.ap-nanjing.myqcloud.com/img/ce409d244fbd4dc0a3da7c4cb5c93c85.png)