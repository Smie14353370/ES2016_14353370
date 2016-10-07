[TOC]
# DOL

@[��ܽ���|���ù���|ʵ�����]

##��ܽ���
**distributed operation layer (DOL)**������������⻷��������������У�λ�ڲ�ͬ������λ�õĶ���û��������⻷��ͨ�����������ӣ����߶���û�ͬʱ�μ�һ��������ʵ������ͨ��������������û����н�������������Ϣ��ϵͳ�У�����û���ͨ�������ͬһ����������й۲�Ͳ������ԴﵽЭͬ������Ŀ�ġ��򵥵�˵��ָһ��֧�ֶ���ʵʱͨ��������н��������ϵͳ��ÿ���û���һ��������ʵ�����У�ͨ��������������û����н�������������Ϣ���ص������
 
- **��������⹤���ռ�** 
- **αʵ�����Ϊ��ʵ��** 
- **֧��ʵʱ����������ʱ��** 
- **����û��Զ��ַ�ʽ�໥ͨ��** 
- **��Դ��Ϣ�����Լ������û���Ȼ���������ж���** 




## ���ù���
###��װ��Ҫ�Ļ���
- 	$sudo apt-get update
- $sudo apt-get install ant
- $sudo apt-get install openjdk-7-jdk
-$ sudo apt-get install unzip
> ����ant��unzip �ɿ��Ƿ�װ�ɹ�
> ![enter image description here](http://ww3.sinaimg.cn/mw690/82c0590fgw1f8jug70xoaj209d01yq3e.jpg)
> �˴���Ϊant���سɹ�


### ����DOL��װ������ѹ
- **�½�dol���ļ��� **
$	mkdir dol
- **��dolethz.zip��ѹ�� dol�ļ�����**
$	unzip dol_ethz.zip -d dol
- **��ѹsystemc**
$	tar -zxvf systemc-2.3.1.tgz
![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8ju3c5ehjj20m90gptfc.jpg)

###����systemc
- **��ѹ�����systemc-2.3.1��Ŀ¼��**
$	cd systemc-2.3.1
- **�½�һ����ʱ�ļ���objdir**
$	mkdir objdir
- **������ļ���objdir**
$	cd objdir
- **����configure(�ܸ���ϵͳ�Ļ�������һ�²��������ڱ���)**
$	../configure CXX=g++ --disable-async-updates
> ��ͼΪ����configure֮��Ľ�ͼ
> ![enter image description here](http://ww4.sinaimg.cn/mw690/82c0590fgw1f8ju3ctprvj20m90gp44z.jpg)

- **����**
$	sudo make install
>��������ļ�Ŀ¼����![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8ju3f521oj20m90gpajh.jpg)

- **��¼��ǰ�Ĺ���·��**
$	pwd
>��ǰ�ҵ�Ŀ¼Ϊ/home/yinxiaolin/Desktop/install/systemc-2.3.1
>![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8ju3frp14j20m90gpgv4.jpg)

### ����dol

- **����ո�dol���ļ��� **
$	cd ../dol
- **�޸�build_zip.xml�ļ� **
�ҵ�������λ�������˵��������systemcλ�������
property name="systemc.inc" value="/home/yinxiaolin/Desktop/install/systemc-2.3.1/include"/>
property name="systemc.lib" value="/home/yinxiaolin/Desktop/install/systemc-2.3.1/lib-linux/libsystemc.a"/>
>![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8ju3g7ve5j20m90gpn3s.jpg)
- **����**
$	ant -f build_zip.xml all
>���ɹ�����ʾbuild successful
>![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8ju3h1tbyj20lv0gpwka.jpg)

- **����build/bin/mian·����**
$	cd build/bin/main
-**Ȼ�����е�һ������**
>Run example1��
 > cd build/bin/main
 > ant -f runexample.xml -Dnumber=1
>�ɹ������ͼ
>![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8ju3h49fpj20jo0fg0ye.jpg)
>���ӻ�
>![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8ju3hqvbsj20ib0fg764.jpg)

##ʵ�����
**DOL��������**
- **ע��������Ȩ������**
 sudo��linuxϵͳ����ָ�������ϵͳ����Ա����ͨ�û�ִ��һЩ����ȫ����root�����һ�����ߣ���halt��reboot��su�ȵȡ���������������root�û��ĵ�¼ �͹���ʱ�䣬ͬ��Ҳ����˰�ȫ�ԡ�sudo���Ƕ�shell��һ�����棬��������ÿ������ġ�
> ��������
 sudo apt-getinstall#(��װ�����)
sudo apt-getremove#��ж���������
sudo apt-getremove--purge#��ж��������������ļ���
sudo apt-getautoremove--purge#��ж�������������������������ļ���ֻ��6.10��Ч��
sudo apt-getupdate������Դ��

- **ע��JAVA��������**
�޸Ļ�������ע������
 ��װjdk-8u40-linux-x64(http://www.oracle.com/technetwork/java/javase/downloads/index.html)
   a. ����װ����ѹ�� /usr/lib/java
      cd /usr/lib
      sudo mkdir java
      ���밲װ����������·��(���ն˴�)
      sudo tar zxvf ./jdk-8u40-linux-x64.gz -C /usr/lib/java
     ������Ϊjdk8
      cd /usr/lib/java
      sudo mv jdk1.8.0_40/ jdk8
      
   b.���û�������
     gedit ~/.bashrc
     �ڴ򿪵��ļ���ĩβ���JDK����·��
 �����˳���Ȼ�����������������ʹ֮��Ч
    source ~/.bashrc
    
   c.����Ĭ��JDK
     /usr/lib/java/jdk8/ΪJDK����·��
    sudo update-alternatives --install /usr/bin/java java /usr/lib/java/jdk8/bin/java 300
    sudo update-alternatives --install /usr/bin/javac javac /usr/lib/java/jdk8/bin/javac 300
 �鿴��ǰ����JDK�汾������: ������ java (�ṩ /usr/bin/java)��ֻ��һ����ѡ�/usr/lib/java/jdk8/bin/java�������á�
    sudo update-alternatives --config java	

   d. ͨ��һ��������֤�����Ƿ�ɹ�
    java -version	
    java
    javac