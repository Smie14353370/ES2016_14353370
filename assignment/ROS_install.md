
# 安装ROS

@[SLAM介绍|ROS介绍|安装ROS|]

##SLAM介绍
SLAM (simultaneous localization and mapping),也称为CML (Concurrent Mapping and Localization), 即时定位与地图构建，或并发建图与定位。 SLAM最早由Smith、Self和Cheeseman于1988年提出。 由于其重要的理论与应用价值，被很多学者认为是实现真正全自主移动机器人的关键。
###使用领域
- 室内机器人
扫地机要算机器人里最早用到SLAM技术这一批了，国内的科沃斯、塔米扫地机通过用SLAM算法结合激光雷达或者摄像头的方法，让扫地机可以高效绘制室内地图，智能分析和规划扫地环境，从而成功让自己步入了智能导航的阵列。
- AR
目前基于SLAM技术开发的代表性产品有微软的Hololens，谷歌的Project Tango以及同样有名的Magic Leap，后者4月20号公布它的新一代水母版demo后，国内的AR公司更加看到了这个趋势，比如进化动力近期就公布了他们的SLAM demo, 用一个小摄像头实现VR头显空间定位，而易瞳去年10月雷锋网去试用新品的时候，就发现已经整合SLAM技术了，国内其他公司虽然没有正式公布，但我们可以肯定，他们都在暗暗研发这项技术，只等一个成熟的时机就会展现给大家。很多VR应用需要用到SLAM技术，定位只是一个feature，路径记录、3D重构、地图构建都可以是SLAM技术的输出。
- 无人机
SLAM（即时定位与地图构建）：通过感知自身周围环境来构建3D增量式地图，从而实现自主定位和导航。
- 无人驾驶
因为Google无人驾驶车的科普，很多人都知道了基于激光雷达技术的Lidar Slam。Lidar Slam是指利用激光雷达作为外部传感器，获取地图数据，使机器人实现同步定位与地图构建。虽然成本高昂，但目前为止是最稳定、最可靠、高性能的SLAM方式。

##ROS介绍
ROS(Robot Operating System）是一个机器人软件平台，它能为异质计算机集群提供类似操作系统的功ROS图标能。ROS的前身是斯坦福人工智能实验室为了支持斯坦福智能机器人STAIR而建立的交换庭(switchyard）项目。到2008年，主要由威楼加拉吉继续该项目的研发。

ROS提供一些标准操作系统服务，例如硬件抽象，底层设备控制，常用功能实现，进程间消息以及数据包管理。ROS是基于一种图状架构，从而不同节点的进程能接受，发布，聚合各种信息（例如传感，控制，状态，规划等等）。目前ROS主要支持Ubuntu。

ROS可以分成两层，低层是上面描述的操作系统层，高层则是广大用户群贡献的实现不同功能的各种软件包，例如定位绘图，行动规划，感知，模拟等等。

ROS（低层）使用BSD许可证，所有是开源软件，并能免费用于研究和商业用途。而高层的用户提供的包则可以使用很多种不同的许可证。
## ROS安装
- Configure your Ubuntu repositories
Configure your Ubuntu repositories to allow "restricted," "universe," and "multiverse." You can follow the Ubuntu guidefor instructions on doing this.

- Setup your sources.list
Setup your computer to accept software from packages.ros.org. ROS Jade ONLY supports Trusty (14.04), Utopic (14.10) and Vivid (15.04) for debian packages.
>sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

- Set up your keys
> sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116

- Installation
First, make sure your Debian package index is up-to-date:
>sudo apt-get update

	Desktop-Full Install: (Recommended) : ROS, rqt, rviz, robot-generic libraries, 2D/3D simulators, navigation and 2D/3D perception
>sudo apt-get install ros-jade-desktop-full

	To find available packages, use:
>apt-cache search ros-jade

- Initialize rosdep
Before you can use ROS, you will need to initialize rosdep. rosdep enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS.
>sudo rosdep init
>rosdep update
- Environment setup
It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell is launched:
>echo "source /opt/ros/jade/setup.bash" >> ~/.bashrcsource ~/.bashrc

- Getting rosinstall
rosinstall is a frequently used command-line tool in ROS that is distributed separately. It enables you to easily download many source trees for ROS packages with one command.
To install this tool on Ubuntu, run:
>sudo apt-get install python-rosinstall

##实验感想
本次实验主要是环境的配置，所以重点我还是放在SLAM和ROS的认识上，为下星期的cartographer做准备。
SLAM的基本理论，是指它的数学建模数学模型影响着整个系统的性能，决定了其他问题的处理方法。在早先的研究中（86年提出[1]至21世纪前期[2]），是使用卡尔曼滤波器的数学模型的。那里的机器人，就是一个位姿的时间序列；而地图，就是一堆路标点的集合。用(x,y,z)表示每一个路标，然后在滤波器更新的过程中，让这三个数慢慢收敛。
　　这样的模型好处是可以直接套用滤波器的求解方法。卡尔曼滤波器是很成熟的理论，比较靠谱。
　　缺点在于滤波器。所以EKF的线性化假设啊，必须存储协方差矩阵带来的资源消耗啊，都成了缺点（之后的文章里会介绍）。然后呢，最直观的就是，用(x,y,z)表示路标？万一路标变了怎么办？平时我们不就把屋里的桌子椅子挪来挪去的吗？那时候滤波器就挂了。所以它也不适用于动态的场合。

