####Lab 5 ROS Installation & Cartographer Experiment
Updated in 2016-11-1<br/>
> &emsp;&emsp;本周的任务是安装 <b>ROS（Robot Operation System）</b>框架，利用其仿真激光雷达的功能，实现一下 <b>SLAM</b> 算法（Simultaneous Localization And Mapping），而 <b>Cartographer </b>就是 Google 上开源的 SLAM 算法，它基于激光雷达和 <b>IMU</b> （惯性处理单元）来运作。<br/>
> 
> 实验报告会包含以下两方面的内容：<br/>
> 
> - 关于 ROS 的安装过程
> - 配置好 Cartographer 后跑出的结果

#####关于 ROS 的安装过程：
根据助教提供的教程网址：`http://wiki.ros.org/kinetic/Installation/Ubuntu` ，对 ROS 进行了安装，下面是具体的安装过程：<br/>

1. 添加资源列表（Setup sources.list）<br/>
`sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`<br/>
这就是将获取资源的位置告诉程序，如果需要获取相关资源就到这个地方去找。

2. 设置密钥（Setup keys）<br/>
`sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116`<br/>

3. 安装 ROS：<br/>
&emsp;(1) 确保 Debian Package Index 是最新版本：<br/>
&emsp;&emsp;&emsp;`sudo apt-get update` <br/>
&emsp;(2) 安装 ROS 的完整 Package ：<br/>
&emsp;&emsp;&emsp;`sudo apt-get install ros-kinetic-desktop-full` <br/>
注：这一步会需要比较久的时间，可以不用一直等着；

4. 初始化 rosdep（对于安装需要的依赖项有帮助，并且是一些 ROS 核心构成部分运行的必需）：<br/>
`sudo rosdep init`<br/>
`rosdep update`<br/>
这一步应该要得到下图的效果：<br/>
![](http://ww2.sinaimg.cn/large/6a59a366gw1f9o1tyu5kmj20kb07qae9.jpg)

5. 配置 ROS 的运行环境<br/>
&emsp;(1) 将 bash 文件加入到 系统环境变量文件（bashrc） 中：<br/>
&emsp;&emsp;&emsp;`echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc`<br/>
&emsp;(2) 运行环境配置文件，让修改生效：<br/>
&emsp;&emsp;&emsp;`source ~/.bashrc` <br/>

6. 安装 rosinstall（一个经常使用的命令行工具，可以轻易地下载 ROS 包的资源树）<br/>
`sudo apt-get install python-rosinstall`<br/>

以上就安装好了 ROS，接下来先验证ROS是否成功安装。<br/>

#####关于 ROS 的验证
&emsp;&emsp;由于没有安装Cartographer，所以这个地方的验证我们要用另外的方法来实现：<br/>

1. 首先是再次尝试安装 ROS 的 full package，应该会出现下面的提示,版本已是最新的：<br/>
![](http://ww2.sinaimg.cn/large/6a59a366gw1f9o1vyx713j20kr03iwfj.jpg)<br/>

2. 根据助教所提示的另外的验证方法的教程，做了以下的尝试，以及显示的结果：<br/>
&emsp;①用以下代码尝试找到 ROS 的文件系统：<br/>
&emsp;`rospack find [package_name]`<br/>
&emsp;应该可以看到显示的是这个 package 的路径，如下图：<br/>
&emsp;![i](http://i1.piimg.com/567571/17d2a7dbe91473df.png)<br/>
<br/>
&emsp;②用以下代码尝试查看 ROS 的环境变量：<br/>
&emsp;`export | grep ROS`<br/>
&emsp;应该可以看到显示的是已经配置好了的 `ROS_PACKAGE_PATH`，如下图：<br/>
&emsp;![i](http://i1.piimg.com/567571/010aa068a80d87e1.png)<br/>
<br/>
&emsp;③用以下代码尝试查看一级依赖包的信息：<br/>
&emsp;`rospack depends1 [package_name]`<br/>
&emsp;应该可以看到显示的是该包的 packages.xml 的内容，表明运行该包会依赖于哪些包，如下图：<br/>
&emsp;![s](http://p1.bpimg.com/567571/a4efa236bba4daa4.png)<br/>
<br/>
&emsp;④用以下代码尝试检查当前 ROS 系统情况：<br/>
&emsp;`roswtf`<br/>
&emsp;如果没有运行 ROS 系统，就会看到显示的是系统无运行，如下图：<br/>
&emsp;![](http://ww2.sinaimg.cn/large/6a59a366gw1f9o1xnwv0ij20c0062jsj.jpg)<br/>

从验证的情况看，ROS的安装应该是成功了的，接下来的任务是安装Cartographer。

#####关于 Cartographer 的安装流程
&emsp;&emsp;因为助教提供的教程网址中，部分文件需要翻墙才可下载，所以使用的是助教提供的网络上一名大神的博客的文章教程进行安装：<br/>

1. 安装所有必需的依赖项：<br/>
`sudo apt-get install -y google-mock libboost-all-dev libeigen3-dev libgflags-dev libgoogle-glog-dev`<br/>
`liblua5.2-dev libprotobuf-dev libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler`<br/>
`python-sphinx ros-kinetic-tf2-eigen libatlas-base-dev libsuitesparse-dev liblapack-dev`<br/>
注：这里需要注意，原博文中依赖项是 ros-indigo-tf2-eigen，因为我们安装的 ROS 版本是 kinetic 的，所以要改这个依赖项；

2. 安装 ceres-solver，1.11.0版本，博主提供了一个他自己 github 上的代码，按照步骤来即可：<br/>
`git clone https://github.com/hitcm/ceres-solver-1.11.0.git`<br/>
`cd ceres-solver-1.11.0`<br/>
（博主是写着直接进入build文件夹，然而在clone下来的文件夹中是没有build的，所以自己创建一个）<br/>
`mkdir build`<br/>
`cmake ..`<br/>
`make -j`<br/>
`sudo make install`<br/>
注：如果在 `make` 步骤虚拟机卡住了，不要惊慌，只是卡住了而已，如果一直卡着，把虚拟机关了再打开，直接跑下一步就好。

3. 安装 Cartographer，同样使用 github，按照步骤来即可：<br/>
`git clone https://github.com/hitcm/cartographer.git`<br/>
`cd cartographer`<br/>
`mkdir build`<br/>
`cmake ..`<br/>
`make`<br/>
`sudo make install`<br/>
注：这里同样，如果卡住了，那就放着，耐心的等，实在卡死了那就关掉再来把，可以考虑把虚拟机的内存改大一些，否则容易死机。

4. 安装 Cartographer_ros，也就是在 `2D` 和 `3D` 提供 `SLAM` 的平台，作者同样提供了 github 下载：<br/>
`sudo apt-get update`&emsp;先更新当前的package<br/>
`sudo apt-get install python-wstool`&emsp;安装wstool，帮助安装ros<br/>
`mkdir catkin_ws` &emsp;创建文件夹用来放 ros 文件<br/>
`cd catkin_ws`<br/>
`wstool init src` &emsp;初始化 cartographer 在 ros下的运行安装环境
`git clone https://github.com/hitcm/cartographer_ros.git`<br/>
`cd catkin_ws`<br/>
`catkin_make`<br/>

以上便完成了 Cartographer_ros 的安装，接下来就是运行的阶段啦。

#####跑2D/3D的数据结果
1. 跑 2D 数据包得到的结果：<br/>
![](http://ww2.sinaimg.cn/large/6a59a366gw1f9o1wlfhttj211x0lbqdb.jpg)<br/>
<br/>

2. 跑 3D 数据包得到结果（还没有完全跑完）：<br/>
![](http://ww1.sinaimg.cn/large/6a59a366gw1f9o1wtpk3ej20we0iw7e4.jpg)<br/>
<br/>

###实验体会：
&emsp;&emsp;这次的实验主要是以体验为主，安装了 ros 之后，下载了 cartographer 的源码，然后运行其源码构建 2D 和 3D 的模型图；虽说是体验，但是也过足了安装以及配置环境的瘾，因为虚拟机的内存设置不够，导致一直安装过程卡死，后来也是看到群上的提醒，才发现可以这么改，本次实验历时还是比较长，而得到的结果也算是令人满意的，模型的构建以及自动识路的算法，有时间了一定好好研究一下。

> 本次实验报告如上，有任何问题请随时联系 2461392385@qq.com



