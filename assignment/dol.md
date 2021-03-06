
# DOL实例分析&编程

@[实例分析|编程修改|实验感想]

##实例分析
###example1
- **.c代码** 
定义generator、square、consumer进程。每个定义中包含两个函数，包括xxx.fire和xxx.init。consumer.c代码为例：
`void consumer_init(DOLProcess *p) {
    sprintf(p->local->name, "consumer");
    p->local->index = 0;
    p->local->len = LENGTH;
}
int consumer_fire(DOLProcess *p) {
    float c;
    if (p->local->index < p->local->len) {
        DOL_read((void*)PORT_IN, &c, sizeof(float), p);
        printf("%s: %f\n", p->local->name, c);
        p->local->index++;
    }
    if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
    }
    return 0;
}
`consumer_initgenerator_init 是初始化函数。这里代码的意思将当前位置为 0，设置生产者长度。这里的 local locallocal 指针向的 指针向的 是.h 文件的 _local_states结构。
 iregenerator_fire consumer_firegenerator_fire是信号产生函数。这里的代码：如果当前位置小于生产长度，则将 当前位置小于生产长度，则将 x（这里是当前下标）写入到输出端，否则销毁进程。所以说就是让这个序 被发射、开火执行 length ength 次之后停下来。 次之后停下来。

- **.xml代码** 
包括process以定义进程，sw_channel以定义通道，connection以定义各个模块之间的连接，一条线会对应两个connection，每条线有两个connection。
以square为例
` <process name="square"> 
    <port type="input" name="1"/>
    <port type="output" name="2"/>
    <source type="c" location="square.c"/>
  </process>`
  表示实现模块为square，这里有两个端口，分别是input的1和output的2,。
  `<sw_channel type="fifo" size="10" name="C1">
    <port type="input" name="0"/>
    <port type="output" name="1"/>
  </sw_channel>`
  定义这个通道名为C1,缓冲区大小为10。
  ` <connection name="s-c">
    <origin name="square">
      <port name="2"/>
    </origin>
    <target name="C2">
      <port name="0"/>
    </target>
  </connection>`
  这条connection叫“s-c”，他从“gennerator”这个模块的“2”端口连接到“c2”的“0”端口。
  - **结果** 
  > example1 xample1 xample1 运行结果如下图所示
  > generator enerator enerator enerator 产生 0-19 的整数（ 的整数（ length length length 为20 ，初始值为 ，初始值为 0）
  >  square quare quare 对输入进行平方操作
  >   consumer onsumer

###example2

和example1相比其.xml文件有迭代器iterator使用
- **通过迭代定义三个square模块** 
 `<variable value="3" name="N"/>
  <!-- instantiate resources -->
  <process name="generator">
    <port type="output" name="10"/>
    <source type="c" location="generator.c"/>
  </process>
  <iterator variable="i" range="N">
    <process name="square">
      <append function="i"/>
      <port type="input" name="0"/>
      <port type="output" name="1"/>
      <source type="c" location="square.c"/>
    </process>
  </iterator>
  <process name="consumer">
    <port type="input" name="100"/>
    <source type="c" location="consumer.c"/>
  </process>
  <iterator variable="i" range="N + 1">
    <sw_channel type="fifo" size="10" name="C2">
      <append function="i"/>
      <port type="input" name="0"/>
      <port type="output" name="1"/>
    </sw_channel>
  </iterator>`
  
 - **迭代生成连接connection** 
`<iterator variable="i" range="N">
    <connection name="to_square">
      <append function="i"/>
      <origin name="C2">
        <append function="i"/>
        <port name="1"/>
      </origin>
      <target name="square">
        <append function="i"/>
        <port name="0"/>
      </target>
    </connection>
    <connection name="from_square">
        <append function="i"/>
        <origin name="square">
          <append function="i"/>
          <port name="1"/>
        </origin>
        <target name="C2">
          <append function="i + 1"/>
          <port name="0"/>
        </target>
    </connection>
  </iterator> `

## 编程修改
###example1
> 修改example1使其输出 3次方数
> 只要修改i=i*i
> 改为i=i*i*i

修改前结果
![enter image description here](http://ww4.sinaimg.cn/mw690/82c0590fgw1f8ofwwk3sbj20h50czaek.jpg)

![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8ofwx2w2cj20e50ewjry.jpg)
修改后结果
![enter image description here](http://ww4.sinaimg.cn/mw690/82c0590fgw1f8ofwxfiytj20k00cn7a2.jpg)
###example2
> 修改example2，让 3个square square模块变成 2个
> 只要修改迭代器的迭代次数
> variable value="3" 

修改前结果
![enter image description here](http://ww3.sinaimg.cn/mw690/82c0590fgw1f8ofwygej0j20e70evdgf.jpg)
修改后结果
![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8ofwyg18zj20gk0codkf.jpg)

![enter image description here](http://ww3.sinaimg.cn/mw690/82c0590fgw1f8ofwz1ej6j20ge0dbmxm.jpg)


##实验感想
**DOL框架**
- 在example中各文件的含义：
src 文件夹：各进程（生产者，消费者，处理模块等）的功能定义
example.xml:系统架构即模块连接方式定义
> 注意
> dol目录下examples的文件是example的源代码，dol/build/bin/main里面的是运行的结果。
 

- **代码架构**
-/src文件夹内包含 2种文件：.c与对应的 .h ，就是 实现的模块*.dot 的 框的功能描述。（每个模块要实现 2个接口， xxx_init和xxx_fire两个函数， 分别是初始化这个模块干了什么，以及开的时候做）
 ./example*.xml 里面定义了模块与之间是怎么连接的， 就有哪些框哪些线
 ?.xml是这样的：process 就是那些框 , sw_channel那些线 ,  connection就是把线的这头连到框的那头
 
**实验过程问题**
遇到修改代码后运行却没有改变结果的情况，根据TA的说法先到dol/build/bin/main里面找到运行结果然后删除就可以了。
打开.dol时可能原来选了默认方式，然后没有出现显示框图的界面，解决方法是安装好那个程序并改变默认方式即可。
