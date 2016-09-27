##关于DOL的框架描述与安装教程
edited latest in 2016.09.27 
> **DOL(Distributed Operation Layer)**,是一个可以让半自动/自动程序在多处理器的结构平台上筹划工作的框架，拥有以下三个部分：
>
* **DOL 程序编程界面**，允许用户在不知道底层结构详情的情况下进行程序编程
* **DOL 函数模拟**，允许用户测试各自的程序，常用的功能有验证函数，获得应用层的相关参数 
* **DOL 筹划最优化**，允许在抽象层对应用程序和结构进行描述，但是要保证必要信息的完整
* [**点击下载 DOL Package**](http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip)

####配置DOL的过程中，以下环境是必须的：

- C/C++ environment
- Java environment:java, javac
- Build environment:ant, make
- SystemC environment

准备好了以上的文件后，便可开始配置：<br>

- **先解压缩相关文件**<br>
 `unzip dol_ethz.zip -d [choice_of_directory]`　将压缩包解压缩到目录下<br>


- **C/C++ environment：**<br>
 `sudo ape-get update`　更新源信息<br>
 `sudo ape-get install g++`　安装编译器以及运行环境<br>
 `g++`　可以利用该命令查看是否安装成功<br><br>
 安装成功之后的终端窗口应如下：<br>
![](https://d17oy1vhnax1f7.cloudfront.net/items/3F2S1l3c012t0G0m2m2p/step12.2B2a3m2z1T0u.png)
<br>

- **Java environment：**<br>
 1. `cd /usr/lib` <br>
 `sudo mkdir java`　# 创建用于放java相关文件的文件夹<br>
 2. `cd [choice_of_directory_of_unzipping]`　# 进入到刚刚解压缩的文件夹路径里<br>
 `sudo tar zxvf ./jdk-8u40-linux-x64.gz -C /usr/lib/java` <br># 将相关文件下载到指定文件夹，即刚才创建的文件夹<br>
 `cd /usr/lib/java`<br>
 `sudo mv jdk1.8.0_40/ jdk8`　# 重命名（随意）相关程序，在后续过程中更方便表达<br>
 3. `gedit ~/.bashrc`　# 配置环境变量<br>
 4. 在终端执行上面这行代码之后会出现一个文件，在文件的末尾添加以下代码，表示JDK的所在路径：<br>
 `export JAVA_HOME=/usr/lib/java/jdk8`<br>
 `export JRE_HOME=${JAVA_HOME}/jre`<br>
 `export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib`<br>
 `export PATH=${JAVA_HOME}/bin:$PATH`<br>
 5. `source ~/.bashrc`　# 保存文件并执行<br>
 6. 配置默认选择的JDK
  `sudo update-alternatives --install /usr/bin/java java /usr/lib/java/jdk8/bin/java 300`　# 其中的路径为之前第2步时指定的文件夹，我这里是/usr/lib/java <br>
 `sudo update-alternatives --install /usr/bin/javac javac /usr/lib/java/jdk8/bin/javac 300`　# 对javac同样配置默认的jdk <br>
 7. `sudo update-alternatives --config java`　# 查看当前所装的各种JDK版本与配置，特别需要注意，java和javac均要选择在之前几步中安装的这种，可以看路径来区分版本
 8. `java -version`　# 查看JDK版本<br>
 9. 配置完成后结果如下图<br>
![](https://d17oy1vhnax1f7.cloudfront.net/items/3f042m1K2X1u2R3G0n31/step22.38092W2n0d1S.png)
<br><br>

- **Build environment：**<br>
 `sudo ape-get install ant`　# 安装ant工具<br>
 `ant`　# 利用ant指令验证是否安装成功<br>
 
- **Systemc environment**<br>
 1. `sudo mkdir /Documents/qianrushi`　# 创建文件夹以用于放置解压后文件<br>
 2. `cd [choice_of_direcory_of_unzipping]`　# 进入到最开始解压缩的文件夹路径中<br>
 `sudo tar -zxvf systemc-2.3.1.tgz [-C /Documents/qianrushi/]`　# 后面的文件夹路径为上面刚创建的文件夹的路径<br>
 3. `cd /home/durant35/apps/libs/systemc-2.3.1`<br>
 `sudo mkdir objdir`<br>
 `cd objdir`　# 进入到刚创建的文件夹里来，检测systemc的运行是否正常<br>
 `../configure CXX=g++ --disable-async-updates`　# 运行configure，设置参数
 4. 以上的步骤是为了编译systemc做准备，到这一步应该出现以下结果：
 ![](https://d17oy1vhnax1f7.cloudfront.net/items/362O0s3u3N3J35354117/step4.2s1q1i2s3u3l.png)<br><br>
 5. `sudo make` <br>
 `sudo make check` <br>
 `sudo make install`　# 编译systemc<br>
 一切正常的话，结果应该如下图（All 11 tests passed）：<br>
![](https://d17oy1vhnax1f7.cloudfront.net/items/2Q0G1o0t031W143r2k0j/step5.2N1S0Y0g3T1d.png)<br>
 `pwd`　# 记录当前工作路径，将该路径记下来，**编译DOL**时会用到<br><br>


- **编译DOL**
 1. ` cd [choidce_of_directory_of_unzipping]`　# 进入dol的解压文件夹中
 `sudo gedit build_zip.xml`　# 修改这个xml文件，打开之后会很容易看到以下两行：<br>
 (1) `<property name="systemc.inc" value="....../include"/>`<br>
 (2) `<property name="systemc.lib" value="....../lib-linux/libsystemc.a"/>`<br>
 将上述两行中的“......”换成在**systemc编译过程中第5步**通过pwd指令得到的工作路径，保存后关闭；<br> 
 注：如果是64位系统，(2)中的linux要改成linux64；<br>
 2. `ant -f build_zip.xml all`　# 编译dol文件<br>
 正常结果应为：<br>
![](https://d17oy1vhnax1f7.cloudfront.net/items/003I0b0n0s151d153D38/stepn5.0B0Q352E1t1N.png)
 3. 假设2的结果显示 **"build successfully"** ，运行以下代码试跑example 1：<br>
 `cd build/bin/main`<br>
 `ant -f runexample.xml -Dnumber=1`<br>
 一切正常的话，结果应该如下图（All 11 tests passed）：<br>
![](https://d17oy1vhnax1f7.cloudfront.net/items/2I411i1z172Y3j0m2G2K/stepn6.2p3H0R0s170F.png)<br><br>

- **配置过程中可能遇到的问题**：<br>
 1. 可能会提示 java 编译器无法使用，这时候要利用之前查看JDK版本与配置的语句，重新选择编译器，或者重新解压 java 压缩包进行再次安装：<br>
 `sudo update-alternatives --config java`<br>
 （如果是因为有多个 java 编译器的问题，在这里应该会让选择使用哪一个，这个时候选择自己安装目录下的那个即可）<br>
 2. 要注意在设置默认JDK的时候，不要打错路径名；
 3. 所有遇到的 **build-fail** 问题都可以通过仔细看问题提示，然后修改对应文件或者采取提示的代码尝试解决，而不是遇到问题就直接问人，查百度。
 
###实验感想与心得：
1. 不要死板地跟着ppt和txt的教程来，要根据终端的提示自行进行代码编写，有许多细节的问题是没有办法仅通过实验文档就能解决的。
2. 如果是 **permission denied** 类型的问题，在命令前加上 **sudo** 即可。
3. 虽然对嵌入式这门实验课还是没有什么概念，配置环境已经是一个入门的难题，花了比较多的时间找错误并且发现问题解决问题，收获还是蛮丰富的，希望以后的实验也可以顺利进行。

> 以上均为本人亲自尝试以及截图，若有问题可联系 **chenxh86@mail2.sysu.edu.cn**

