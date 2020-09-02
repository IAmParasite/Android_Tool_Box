# Android Tool Box 开发文档 (BUAA1921小学期作业)

##提示：如需使用投屏控制功能，请下载[依赖](https://github.com/Genymobile/scrcpy/releases/download/v1.16/scrcpy-win64-v1.16.zip)并解压到程序目录！

## 一. 项目概述
本项目旨在构建一个简易、高效、可靠的高集成度安卓工具箱。项目以Python语言为基础，调用ftplib、Tkinter等库，辅以Java进行Android端软件开发，并通过调用Windows和Android Shell的方式执行ADB相关命令,实现获取设备信息、投屏控制、全功能通用刷机、高速文件传输、微信/QQ文件提取备份、软件批量安装、预装应用卸载等功能

将复杂的操作简单化、自动化，使安卓高级操作上手更加容易

## 二. 环境及工具
### 开发&测试环境：
- Windows10 2004 & Ubuntu 20.04 LTS(WSL)
- Python 3.8 
- VSCode with Python Extension
- Android Studio with SDK
### 打包环境：
- Windows7 SP1 
- Python 3.5
### 测试机型：
- Xiaomi Gemini (Mi5) MIUI9 & MIUI10
- Redmi Raphael (K20 Pro) MIUI12
- Huawei ELE-AL00 (P30) EMUI10.1
- Redmi Nikel (Note4 MTK) MIUI7



## 三. 功能设计
## Python部分：

### 1.Fastboot刷机实用工具

#### 	卡刷

#### 	线刷

### 2.高速文件传输（FTP&ADB）

#### 	高速上传

#### 	下载

### 3.屏幕投射（基于Scrcpy）

#### 	本体

#### 	操作控件和快捷键映射

### 4.文件备份

#### 	微信文件/图片一键备份

####  QQ文件/图片一键备份

#### 	系统图片一键备份

#### 	应用&数据一键备份

### 5.软件管理和自动化配置

#### 	软件安装

#### 	免root软件卸载

#### 	软件激活工具

##### 冰箱icebox

##### 炼妖壶Island

##### 黑阈Brevent

## Android部分：

####  FTP微型服务器

## 四. 编写目的



现在的一键式刷机工具（如刷机精灵、线刷宝等）虽然能够提供简单易用的交互界面，但是存在不能保存配置以便批量刷机、刷机过程中植入广告程序和后门等缺陷；各大安卓论坛中提供的工具包分化过强，功能过于单一且与品牌、机型、版本等存在强绑定关系，无法做到工具的通用性；各厂家提供的刷机工具则高度定制化，无法修改部分预设，且各个厂家工具不互通，不能做到随心所欲；而直接调用Android SDK进行操作对于零编程基础的玩家更是非常不友好。

因此本项目结合作者八年的刷机经验，编写了基于Python的命令行程序，用户只需简单输入Y/N等操作就可以完成刷机流程，大大降低了刷机门槛，让每个用户都能充分体验到刷机的乐趣



## 四. 项目或功能背景

Android SDK 提供了相当丰富的扩展功能，基于ADB可以实现对系统底层的修改和配置，提高Android设备的可玩性。但是基于ADB的大多应用需要有较高的命令行操作和编程语言基础，对于希望深度优化安卓设备而缺乏相关经验的普通用户非常不友好。

本项目利用Python语言极强的扩展性，粘合了Linux Shell、Android Debug Bridge、

因此，本项目利用Python语言通过调用Shell脚本的方式实现了简易的交互式ADB界面，同时整合并重写部分优秀的开源安卓实用工具，自行编写了小米机型的全自动刷机工具（MTK和高通方案），以及基于Scrcpy的非华为设备多屏协同解决方案、



## 五. 模块与关系

1. #### FastbootFlash.py

   刷机模块，提供线刷和卡刷功能

   需要调用Methods

2. #### FileBackUp.py

   备份模块，可备份文件/应用

   提供Recovery模式整机镜像备份和文件备份功能

3. #### FileTrans.py

   多线程文件高速传输模块

   可用于快速在电脑与安卓设备之间传输文件

4. #### main.py

   程序主入口

   显示菜单并且按照用户操作调用各个模块的主方法

5. #### Methods.py	

   程序核心功能模块

   通过系统命令行调用ADBShell与设备进行各种交互
   用于与系统Shell交互(调用命令行、获取系统路径等)

   后期可能用pyADB进行替换，便于高级调试
7. #### Screen.py

   投屏模块

   调用Scrcpy项目，实现屏幕实时共享控制功能

8. #### SoftScript.py

   软件管理模块

   提供特殊软件激活、软件批量安装、软件管理等




## 六. 算法设计&接口注释

1. ### Methods.py	

   (str)建立管道，调用系统命令行执行command并返回stdout内容

   ##### cmd(*command*):

   (null)解压tar压缩文件，参数分别为文件名和解压路径

   ##### unziptar(*filename*, *dirs*):

   (bool/int)选项输入限定，非法选项要求重新输入，参数为选项个数

   ##### InputJudge(*num*):

   (弃用)(null)清屏并启动新的python文件

   ##### NewProgram(*pyFileName*):

   (str)引导文件拖拽并获取文件绝对路径，返回文件绝对路径

   ##### dragtoWindowGetName():

   (null)程序出口引导

   ##### exitProgram(*num*):
   (*str*)当前连接设备的wlan0适配器的IP地址

   ##### IP = ""

   (*dict*)手机设备信息

   ##### MobileInfo = {}

   (*str*)Fastboot返回的设备连接信息 格式：SN \*t fastboot*

   ##### getFastbootConnection():

   (*bool*)有线连接状态，开启USB调试且处于正常开机状态返回True

   ##### USBConnected():

   (*bool*)BootLoader连接状态，处于Bootloader模式返回True

   ##### BLConnected():

   (*bool*)无线连接状态，远程调试已连接则返回True

   ##### WLANConnected():

   (null)重启到系统

   ##### rebootUI():

   (null)重启到Bootloader

   ##### rebootBL():

   (null)重启到Recovery

   ##### rebootRec():

   (*dict*)获取build.prop记载的手机详细信息

   ##### getInfo():

   (*bool*)获取Bootloader加锁状态

   ##### lockStatus():

   (*str*)获取电池电量

   ##### getBattery():

   (*str*)获取当前连接设备的wlan0适配器的IPV4地址

   ##### getIPaddress():

   (*bool*)获取设备开机状态

   ##### powerStatus():

   (null)传送sourcedir所指的文件或目录到设备rmpath目录下

   ##### pushFile(*sourcedir*, *rmpath*):

   (null)传送设备rmpath所指的文件或目录到本地destdir下

   ##### pullFile(*rmpath*, *destdir*):

   (null)将拖拽到窗口的文件传输到设备rmpath所指的目录下

   ##### dragtoPush(*rmpath*):

   (null)模拟点击坐标(x,y)

   ##### touchPoint(*x*, *y*):

   (null)模拟滑动,从(x1,y1)到(x2,y2)

   ##### slidePoint(*x1*, *y1*, *x2*, *y2*):

2. #### FastbootFlash.py

   (list)Tar线刷包列表

   ##### TAR = []

   (str)当前路径

   ##### path = os.getcwd()

   (list)当前路径下文件列表

   ##### list_dir = os.listdir(path+"\\images\\")

   (int)全局变量，tar个数

   ##### num_of_TARs = 0

   (null)寻找tar包并存入TAR[]

   ##### findTarPacks():

   (null)从fastboot重启到UI

   ##### fastbootReboot():

   (null)重启菜单

   ##### rebootList():

   (null)强制刷入指定文件 mode为设备目标分区，file为镜像文件

   ##### directFlash(*mode*, *file*):

   (null)双清

   ##### fastbootWipe():

   (null)主程序入口

   ##### Flashmain():

3. #### FileBackUp.py

   (str)微信图片路径

   ##### wechatimage

   (str)微信文件路径

   ##### wechatfiles

   (str)QQ图片路径

   ##### qqimage

   (str)QQ文件路径

   ##### qqfiles

   (str)系统图片路径

   ##### pictures

   (null)QQ备份

   ##### qqbackup():

   (null)微信备份

   ##### wechatbackup():

   (null)相册备份

   ##### photobackup():

   (null)备份恢复入口

   ##### appBackandRestore():

   (null)应用恢复

   ##### apprestore():

   (null)应用备份

   ##### appbackup():

   (null)主程序入口

   ##### FileBackupmain():

5. #### main.py

   (null)输出菜单

   ##### printmenu():

   (null)输出手机信息

   ##### printinfo():

   (null)输出欢迎界面和Logo

   ##### printHello():

   (null)输出关于我们

   ##### aboutProject():

   (null)主菜单模块

   ##### mainmenu():

   

6. #### Screen.py

   (str)当前路径

   ##### path = os.getcwd()

   (list)参数列表，格式如：["-m 1024","-f",...]

   ##### conf = []

   (str)全局变量 参数字符串

   ##### config = ''

   (null)配置存取模块，调用外部Scrcpy-Settings.txt存取配置字符串

   ##### initializeConfig():

   (str)参数生成模块，生成config字符串

   ##### configGenerator():

   (null)以有线连接方式启动，传入config字符串

   ##### USBconnect(config):

   (null)以无线连接(远程调试)方式启动，参数同USBconnect

   ##### WLANconnect(config):

   (null)主程序入口，可从外部调用

   ##### Screenmain()：

7. #### SoftScript.py

   (list)当前目录APK列表

   ##### Apklist = []

   (str)当前路径

   ##### path = os.getcwd()

   (list)当前目录文件列表

   ##### list_dir = os.listdir(path+"\\ApkFiles\\")

   (int)全局变量，APK数量

   ##### num_of_APKs = 0

   (null)免root卸载软件 参数为apk包名

   ##### unInstallwithoutRoot(*PackageName*):

   (list)返回目录下APK列表

   ##### listAPKFiles():

   (null)格式化输出文件列表

   ##### printAPKList(*num*):

   (bool)是否存在指定文件

   ##### inAPKList(*APKname*):

   (bool)询问是否安装

   ##### askforInst(*APKname*):

   (null)安装应用

   ##### pushApp(*APKname*):

   (null)软件安装子程序入口

   ##### installSoftware():

   (null)软件激活子程序入口

   ##### activeSofts():

   (null)冰箱激活组件

   ##### IceboxStart():

   (null)炼妖壶启动组件

   ##### IslandStart():

   (null)黑阈启动组件

   ##### BreventStart():

   (null)主程序入口

   ##### Softmain():


## 七. 部署与运行
   在安装有Python3环境的电脑上可以直接调用`python main.py`运行程序

   除Methods.py外，其余程序均可通过`python xxx.py`执行

   此外也可以通过执行已经打包好的Main.exe启动程序，二者效果完全相同

   手机端要求开启USB调试和USB安装，小米手机需额外开启USB调试（安全模式）

   如需使用刷机功能，请确定当前设备已解锁BootLoader

   手机端FTP客户端请授予读写内部存储和网络完全控制权限


## 八. 不足和改进
   - 界面太丑 使用powershell调用会好一点

   - 有些地方明显GUI效率更高（如Screen模块的配置选择）应当使用GUI而非命令行（文件选择部分已经改进了）

   - 由于直接调用打包好的项目，缺少Debug信息导致异常处理做的不够，在某些特殊情况下（如设备连接断开、网络连接断开等）程序会崩溃。正在逐步以Py库的形式进行替换

   - 当前仅支持单设备连接，后续添加批量刷机、机对机文件传输等多设备功能支持
   
   - 由于测试机型较为有限且基本都为小米手机，故没有对其他厂家的机型做适配 开学后会借其他手机进行测试和改进

## 九. 心得与展望
十天的时间对于开发这样一个功能比较复杂的软件来说实在是捉襟见肘了。原有的如手机遥控电脑、从手机重装电脑系统等很多计划之内的功能由于学习时间成本的问题，都没有得到最终实现，软件界面开发也不尽如人意。而且现在的软件也是站在巨人的肩膀上，调用了相当多的第三方程序。

在软件编写的过程中，我亲身参与到了小型软件开发的全过程中，逐渐体会到了写代码的酣畅淋漓的快感，测试时候遍地BUG的痛苦和绝望，在网上找到解决方法时的兴奋，半夜突然想到解决方案时跳起来记到纸上的激动和惊喜……

但是开发进行到后期的疲软也让我认识到不能总把自己封锁于自己的舒适空间，要大胆去尝试新的东西，了解新的方法，学习新的技能，而不能浅尝辄止。

另外写注释和读英文注释真的太让人头疼了（英语太烂

附上开发时间线如下：
- 8.25 听课 继续自学Python 开始编写SoftScript.py 学习引用第三方库相关方法
- 8.26 查阅资料 开始编写Sreen.py和ADBMethods.py 在RedmiK20Pro上通过Screen测试
- 8.27 借手机测试Screen.py 修复Screen.py参数调用的BUG 
- 8.28 编写FastbootFlash.py 从ADBMethods.py拆分出SysMethods.py 编写main.py 由于Pyinstaller不可用故编写Run.c和Run.exe 作为直接入口
- 8.29 使用AndroidStudio编写FTP客户端 安装虚拟机 搭建Python3.5环境用于使用PyInstaller 第一次打包测试 编写开发文档 在小米5上测试FastbootFlash失败，期间变砖数次 编写FileTrans.py
- 8.30 程序调用出现问题 在助教的指导下紧急修改模块划分 修正FastbootFlash的刷入Recovery后不引导导致被官方覆盖的BUG 修正FastbootFlash 修正ADB文件传输报错的BUG 优化界面交互,使用文件选择框代替拖入获取文件名 重写Main.py 准备交付
- 8.31 调整并完善程序 精简代码结构 删除冗余函数(1600->1200行) 规范格式化代码，梳理全局变量，调整函数和方法接口，调整交互界面 重写FileTrans.py并加入多线程支持 编写FileBackUp.py 制作答辩PPT
- 9.1 测试FileTrans.py 重写开发文档 上传项目到GitHub 调整交互界面细节(因去做核酸检测时医院信号不好而错过签到)