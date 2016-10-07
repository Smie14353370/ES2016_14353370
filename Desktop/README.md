[TOC]
# DOL

@[框架介绍|配置过程|实验感想]

##框架介绍
**distributed operation layer (DOL)**基于网络的虚拟环境，在这个环境中，位于不同物理环境位置的多个用户或多个虚拟环境通过网络相连接，或者多个用户同时参加一个虚拟现实环境，通过计算机与其他用户进行交互，并共享信息。系统中，多个用户可通过网络对同一虚拟世界进行观察和操作，以达到协同工作的目的。简单的说是指一个支持多人实时通过网络进行交互的软件系统，每个用户在一个虚拟现实环境中，通过计算机与其它用户进行交互，并共享信息。特点概述：
 
- **共享的虚拟工作空间** 
- **伪实体的行为真实感** 
- **支持实时交互，共享时钟** 
- **多个用户以多种方式相互通信** 
- **资源信息共享以及允许用户自然操作环境中对象** 




## 配置过程
###安装必要的环境
- 	$sudo apt-get update
- $sudo apt-get install ant
- $sudo apt-get install openjdk-7-jdk
-$ sudo apt-get install unzip
> 输入ant或unzip 可看是否安装成功
> ![enter image description here](http://ww3.sinaimg.cn/mw690/82c0590fgw1f8jug70xoaj209d01yq3e.jpg)
> 此处即为ant下载成功


### 下载DOL安装包并解压
- **新建dol的文件夹 **
$	mkdir dol
- **将dolethz.zip解压到 dol文件夹中**
$	unzip dol_ethz.zip -d dol
- **解压systemc**
$	tar -zxvf systemc-2.3.1.tgz
![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8ju3c5ehjj20m90gptfc.jpg)

###编译systemc
- **解压后进入systemc-2.3.1的目录下**
$	cd systemc-2.3.1
- **新建一个临时文件夹objdir**
$	mkdir objdir
- **进入该文件夹objdir**
$	cd objdir
- **运行configure(能根据系统的环境设置一下参数，用于编译)**
$	../configure CXX=g++ --disable-async-updates
> 下图为运行configure之后的截图
> ![enter image description here](http://ww4.sinaimg.cn/mw690/82c0590fgw1f8ju3ctprvj20m90gp44z.jpg)

- **编译**
$	sudo make install
>编译完后文件目录如下![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8ju3f521oj20m90gpajh.jpg)

- **记录当前的工作路径**
$	pwd
>当前我的目录为/home/yinxiaolin/Desktop/install/systemc-2.3.1
>![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8ju3frp14j20m90gpgv4.jpg)

### 编译dol

- **进入刚刚dol的文件夹 **
$	cd ../dol
- **修改build_zip.xml文件 **
找到下面这段话，就是说上面编译的systemc位置在哪里，
property name="systemc.inc" value="/home/yinxiaolin/Desktop/install/systemc-2.3.1/include"/>
property name="systemc.lib" value="/home/yinxiaolin/Desktop/install/systemc-2.3.1/lib-linux/libsystemc.a"/>
>![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8ju3g7ve5j20m90gpn3s.jpg)
- **编译**
$	ant -f build_zip.xml all
>若成功会显示build successful
>![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8ju3h1tbyj20lv0gpwka.jpg)

- **进入build/bin/mian路径下**
$	cd build/bin/main
-**然后运行第一个例子**
>Run example1：
 > cd build/bin/main
 > ant -f runexample.xml -Dnumber=1
>成功结果如图
>![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8ju3h49fpj20jo0fg0ye.jpg)
>可视化
>![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8ju3hqvbsj20ib0fg764.jpg)

##实验感想
**DOL环境配置**
- **注意命令行权限问题**
 sudo是linux系统管理指令，是允许系统管理员让普通用户执行一些或者全部的root命令的一个工具，如halt，reboot，su等等。这样不仅减少了root用户的登录 和管理时间，同样也提高了安全性。sudo不是对shell的一个代替，它是面向每个命令的。
> 常用命令
 sudo apt-getinstall#(安装软件包)
sudo apt-getremove#（卸载软件包）
sudo apt-getremove--purge#（卸载软件包和配置文件）
sudo apt-getautoremove--purge#（卸载软件包及其依赖软件包配置文件，只对6.10有效）
sudo apt-getupdate（更新源）

- **注意JAVA环境变量**
修改环境变量注意事项
 安装jdk-8u40-linux-x64(http://www.oracle.com/technetwork/java/javase/downloads/index.html)
   a. 将安装包解压到 /usr/lib/java
      cd /usr/lib
      sudo mkdir java
      进入安装包下载所在路径(在终端打开)
      sudo tar zxvf ./jdk-8u40-linux-x64.gz -C /usr/lib/java
     重命名为jdk8
      cd /usr/lib/java
      sudo mv jdk1.8.0_40/ jdk8
      
   b.配置环境变量
     gedit ~/.bashrc
     在打开的文件的末尾添加JDK所在路径
 保存退出，然后输入下面的命令来使之生效
    source ~/.bashrc
    
   c.配置默认JDK
     /usr/lib/java/jdk8/为JDK所在路径
    sudo update-alternatives --install /usr/bin/java java /usr/lib/java/jdk8/bin/java 300
    sudo update-alternatives --install /usr/bin/javac javac /usr/lib/java/jdk8/bin/javac 300
 查看当前各种JDK版本和配置: 链接组 java (提供 /usr/bin/java)中只有一个候选项：/usr/lib/java/jdk8/bin/java无需配置。
    sudo update-alternatives --config java	

   d. 通过一下命令验证配置是否成功
    java -version	
    java
    javac