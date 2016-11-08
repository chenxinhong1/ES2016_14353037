###Lab3 DOL实例分析&编程
---
latest updated: 2016-10-15

> 之前配置好了DOL环境，这次是要对代码进行分析并且进行代码的更改以便更好地理解<br />
> 本次实验的主要步骤：<br /><b>
> 
> - 修改example2，让3个square模块变为2个；
> - 修改example1，让其输出三次方数
> - 写在最前面的一个步骤：改文件是在 `.../dol/examples/examplex` 里面改，而不是在 `dol/build/bin/main/examplex` 里面改，在 `main` 里面的 `example` 文件夹已经是运行过一次的文件，所有的配置已经完成，改代码是不会有任何效果的，这一点要注意；并且在改完代码后，要把原来在 `main` 文件夹下的 `examplex` 文件夹删除，可以用命令 `sudo rm -rf xxx` 来删除，`xxx` 为文件夹名字
</b>

<b>步骤一：<br/>
1、改完代码之后运行程序得到的结果：</b><br />
![result1](http://i1.piimg.com/567571/d7ad5aa90141d848.png)

可以看到上面的结果已经从二次方改为三次方的结果：<br/>
<b>1 -> 1 ; 2 -> 8 ; 3 -> 27 ; ... </b>

<b>2、再来看改完代码之后的dot截图：</b>

![result1dot](http://p1.bpimg.com/567571/623cd34cbd73c40c.png)

&emsp;&emsp;可以看到dot并没有发生改变，依旧是一个generator，一个consumer，以及中间一个square；只是代码发生了改变，来看更改的地方：<br/>
![code1](http://p1.bpimg.com/567571/9d7b3311b5b6315e.png)

&emsp;&emsp;从上图可以看到，更改的地方就是中间的 `i=i*i` 改为 `i=i*i*i` 就可以达到效果。

<b>步骤二：<br/>
1、改完代码之后运行程序得到的结果：</b><br />
![result2](http://p1.bqimg.com/567571/98a18ceb16ae7b18.png)

可以看到上面的结果已经从<b>跑三次square改为跑两次square，也就是四次方</b>的结果：<br/>
<b>1 -> 1 ; 2 -> 16 ; 3 -> 81 ; ... </b>

<b>2、再来看改完代码之后的dot截图：</b>

![result2dot](http://p1.bqimg.com/567571/6adfe988e922c652.png)

&emsp;&emsp;可以看到dot图已经发生了改变，从原来的三个square process变为现在的两个process，如果我们打开flattered之后的xml文件，也可以看到只有两个square process了：<br/>
&emsp;&emsp;![code2](http://p1.bqimg.com/567571/cc4d59461b4f1b7e.png)

&emsp;&emsp;而更改的代码如下，只更改了一个数字就完成了任务：<br/>
&emsp;&emsp;![code3](http://p1.bqimg.com/567571/4e367892fefea959.png)

####实验感想：
1. 这次的任务是对代码进行理解并且进行修改，首先就是对代码的理解，`DOL` 的代码并不仅仅是 `C代码`，更多的其实是 `xml` 文件，其中有 `process` 的声明，并且有连接的 `channel` 的声明，`channel` 的入口，出口等信息其实都非常地容易看懂，然后 `C代码` 是针对每个 `process` 而言的，所以整体来说理解起来并不是很大难度；
2. 这次遇到的困难主要是一开始修改了代码之后，怎么重新编译运行都没有生效，之后是转念一想会不会是因为编译运行过一次之后，整个程序的框架就定了，并且用于 `process` 执行的代码也已经生成成了可执行程序，那么在这种情况下，就算是改了原来的代码，程序只要还在 `main` 目录下，就会被直接调用，而不是重新编译运行，所以在每次修改完代码后，要把 `main` 文件夹中的 `example` 文件夹先删去，这一点在最前面已经说了。

以上为本次实验报告内容，有任何问题请随时联系2461392385@qq.com