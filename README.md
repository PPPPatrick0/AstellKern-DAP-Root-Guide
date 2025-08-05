# AstellKern-DAP-Root-Guide

Rooting the Astell&Kern DAP with Apatch  
通过Apatch来获取艾利和数字播放器的Root权限
<img width="1080" height="1920" alt="Screenshot-trinket 1234567 -20250804145857" src="https://github.com/user-attachments/assets/98c6a9e1-ac68-4b4c-ad84-e974af0d0f84" />

## [Chinese Version](https://github.com/PPPPatrick0/AstellKern-DAP-Root-Guide?tab=readme-ov-file#%E4%B8%AD%E6%96%87%E7%89%88chinese-version)
## [English Version](https://github.com/PPPPatrick0/AstellKern-DAP-Root-Guide?tab=readme-ov-file#english-version-1)

## 中文版（Chinese Version）

### 简短说明

* 本指南旨在尝试为艾利和 (Astell&Kern) 播放器提供一套Root方案。本方案最初基于 SP3000 （无印版）、系统版本 v1.51 创建。

* 现在，艾利和的机器（UI4以及UI4.5），如 SP3000系列（v1.51得到验证），SE300 ...（UI4），SP4000（UI4.5）（理论可以，暂未得到验证）等机型，可参照本指南进行Root。

### 重要提示

***风险警告:***
* Root过程可能将伴随着失去保修、数据丢失甚至设备永久损坏（变砖）的风险。请您在充分理解所有步骤和潜在风险后再进行操作。所有操作均由您本人负责。

**APK包名确认:**
* 由于AK操作系统采用白名单制度来限制APK文件的安装，本指南中的所有APK文件均通过了修改Package Name的方式来绕过AK的限制。  
因此，每一个修改后的APK文件格式均为xxx_Modded(package_name).apk的格式上传，以方便你自行查找是否存在包名冲突的Application。  
（仅限UI4的机型）

### 操作流程概览
**本指南将引导您完成以下三个核心步骤：**
* 准备工作1: 在设备上启用开发者选项，并打开USB调试权限。
* 准备工作2：获取适用于设备的解密后的系统镜像文件。  
* 获取Root: 刷写Apatch patch后的镜像，并安装APatch管理器。

### 1 准备工作
在设备上启用开发者选项，并打开USB调试权限。  
由于AK的设备没有在常规设置中提供开启ADB调试的入口，我们需要通过一个终端命令来调用其隐藏的工厂调试菜单。
#### 1) 安装终端模拟器
在Release中，下载 APK.zip。  
定位到 Term_Modded(uk.co.sevendigital.android).apk 这个文件。这是一个经过修改的终端模拟器应用。  
将此APK文件传输到您的设备内部存储中，并进行安装。

#### 2) 开启ADB调试
在终端模拟器应用中，输入以下命令，然后按回车键：  
```
am start --user 0 -n com.iriver.tester.factorytool/.DebugSettingActivity
```
执行后，您将直接进入Astell&Kern的工厂调试界面。  
在此界面中，找到 ADB Debug 选项，并将其开启。  

#### 3) 验证连接
现在，使用USB数据线将您的设备连接到主机。  
在主机的终端里，输入以下命令：
```
adb devices
```
如果您看到输出了您设备的序列号，并且状态为 device，则表示ADB连接已成功建立。

#### 4) （可选，但强烈推荐）永久开启ADB调试
默认情况下，每次重启设备后，ADB调试选项都会自动关闭。  
为了方便后续操作及日后使用，您可以将其设置为永久开启。  
在主机的终端里，输入以下命令：
```
adb shell am start --user 0 -n com.iriver.tester.factorytool/.UserDebugActivity
```
设备的屏幕上会再次出现一个工厂调试界面。  
点击此界面上的第一个选项（与ADB相关的选项）。  
完成此操作后，ADB调试模式将会被永久激活，即使设备重启也不会关闭。

### 2 获取镜像文件
#### 1) 获得解密后的update.zip
请下载自己机型的OTA文件，名称一般为  `版本号.hex` ，下载后放入机器的根目录。  
在机器上点击确认升级。  
在升级的同时在电脑端不断刷新机器的内部存储目录。  
一般在升级进度50%之后在根目录下能够看见update.zip。  
迅速将update.zip拷贝至主机。

#### 2) 解密文件
请将update.zip内的 payload.bin 解密。
（推荐使用 payload-dumper  
也可以使用Release里添付的payload-dumper.exe  
将payload.bin拖拽至exe即可解包）  

  
至此获得解密后的镜像文件。

### 3 通过APatch获取Root权限
由于目标设备上修改包名后的APatch管理器存在文件写入问题，我们需要借助一台普通的安卓设备来完成启动镜像的修补工作。
#### 1) 在“辅助设备”上制作APatch镜像
您需要一台普通的、无特殊限制的安卓设备（手机或平板均可）作为“辅助设备”。  
* 在下载的 APK.zip中，解压后得到 APatch_10763_original.apk。  
同时，找到你设备的官方原版 boot.img。  
* 安装并传输文件：  
在您的“辅助设备”上，安装 APatch_10763_original.apk。  
将官方原版的 boot.img 文件也一同拷贝到这台辅助设备的内部存储中（例如，“Download”文件夹）。  
* 使用APatch进行修补：  
在辅助设备上，打开APatch应用。  
点击主界面右上角的从左往右第一个图标（手机+向下的箭头）。  
选择 “Select a boot image to Patch” 选项。  
在文件选择器中，找到并选择您刚刚拷贝的那个boot.img文件。  
应用会提示您设置一个 SuperKey。这是一个用于在管理器中激活Root权限的“密码”，请务必设置一个并牢牢记住它。  
输入完SuperKey后，点击“开始”（Start Patching）。APatch将会开始修补boot.img。  
*获取最终镜像：  
修补成功后，在辅助设备的“下载”文件夹中，会生成一个名为 apatch_patched_....img 的新文件。这就是我们最终需要的启动镜像。  
将这个修补完成的镜像，从辅助设备传回您的主机。

#### 2) 为设备刷入镜像并重启

* 在主机的终端里，输入以下命令让设备重启到fastboot模式：  
```
adb reboot bootloader
```
* 刷写vbmeta镜像以禁用AVB  
我们需要刷写vbmeta和vbmeta_system两个镜像，以彻底关闭安卓验证启动（AVB, Android Verified Boot）。  
执行刷写：  
在fastboot模式下，执行以下两条命令：  
```
# 关闭主验证链
fastboot --disable-verity flash vbmeta vbmeta.img

# 关闭针对system分区的验证链
fastboot --disable-verity flash vbmeta_system vbmeta_system.img
```
* 刷写最终镜像  
在主机的终端里，导航到包含最终APatch镜像的目录，执行刷写命令：  
```
# 将 "Your/File/Path" 替换为您实际的文件路径和名称
fastboot flash boot "Your/File/Path/apatch_patched_....img"

# 镜像刷写完成后，执行以下命令重启您的设备：
fastboot reboot
```

#### 3) 激活Root
等待设备进入操作系统。  
* 安装修改版APatch管理器：  
在 APK.zip 中，找到 APatch_10763_Modded(com.appgeneration.itunerfree).apk。  
将这个修改了包名的版本，安装到您的设备上。  
* 获取Root权限：  
在设备上打开刚刚安装的APatch管理器。  
在应用主界面的输入框中，输入您之前设定好的那个SuperKey。  
点击确认。如果一切顺利，应用将显示Root已激活。您可以通过 adb shell 并执行 su 命令来验证是否已获得#权限。


**至此，您的Astell&Kern DAP已成功获取Root权限。**  
  
请发挥你的想象力，没有什么是在拥有Root后做不到的！  
关于解除Iriver操作系统的APP白名单限制，敬请期待！[@7dollars](https://github.com/7dollars)  

**Enjoy It!**




## English Version

Oops: 404: English Translation Not Found  
My translator is currently compiling (or maybe I'm just procrastinating). An English version of this epic saga is on its way. In the meantime, I hope the code snippets and command lines speak for themselves. 

## Shout-Outs!
This success wouldn't have been possible without these legends:  
The APatch Team: Your kernel-patching approach was the skeleton key that bypassed this device's final defenses. Absolute genius!  
The AKBox dev [@7dollars](https://github.com/7dollars) and community: Thanks for all the encouragement and support. You were the motivation that kept me going!  
Google Gemini: My AI partner, who provided tireless technical support and brainstorming sessions. AI-assisted reverse engineering is incredibly cool.  
