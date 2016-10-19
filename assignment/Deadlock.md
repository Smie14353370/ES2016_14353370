
# 死锁

@[实例分析|实验步骤|死锁条件|]

##实例分析
###Deadlock.java
  `class A{
	synchronized void methodA(B b){
		b.last();
	}
	synchronized void last(){
		System.out.println("Inside A.last()");
	}
}
class B{
	synchronized void methodB(A a){
		a.last();
	}
	synchronized void last(){
		System.out.println("Inside B.last()");
	}
} `
这里有两个类分别为AB，其中定义同步函数分别为methodA和methodB，分别在里面调用last函数以打印结果。下面解释synchronized：
- **当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。** 
- **当一个线程访问object的一个synchronized同步代码块或同步方法时，其他线程对object中所有其它synchronized同步代码块或同步方法的访问将被阻塞。**

 `class Deadlock implements Runnable{
	A a=new A();
	B b=new B();
	Deadlock(){
		Thread t = new Thread(this);
		int count = 10000;
				t.start();
		while(count-->0);
		a.methodA(b);
	} `
	这里是主函数的时间轴，当t.start(),之后，线程t就被插入到调度队列里，当调度到他的时候，就跑run()里面的代码

 `	public void run(){
		b.methodB(a);
	}
		public static void main(String args[]){
		new Deadlock();
	}
}  `
在t.start()调用后，count自减造成延时，增大a和b在同一时间点调用method函数的概率，由于synchronized的作用下若同时调用method函数即造成死锁。

## 实验步骤
>把上面的代码抄到 Deadlock.java里面,保存。注意此处我加了在method函数中加了延时，也改变了count值，以便增大死锁概率。
![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8xg9sv33dj20ap03s0sw.jpg)![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8xg9temflj209h0480sz.jpg)
>终端编译java文件， javac Deadlock.java	
![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8xg9vrmbhj20hi00h3yo.jpg)

> linux 系统把下面这段到记事本里，然后保存为.sh，然将批处理文件放在java程序（Deadlock.class）目录下，双击运行，观察结果
>![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8xg9svowwj207d05g0t6.jpg)
![enter image description here](http://ww3.sinaimg.cn/mw690/82c0590fgw1f8xg9ymz1vj20j100zt93.jpg)
![enter image description here](http://ww3.sinaimg.cn/mw690/82c0590fgw1f8xg9z7gbbj20fq00lglq.jpg)

>结果在第九次发生死锁
![enter image description here](http://ww4.sinaimg.cn/mw690/82c0590fgw1f8xg9s5vwbj20g00ctq4v.jpg)


##死锁条件
**互斥使用（资源独占）**
一个资源每次只能给一个进程使用 
**不可抢占（不可剥夺） **
资源申请者不能强行的从资源占有者手中夺取资源，资源只能由占有者自愿释放 
**请求和保持（部分分配，占有申请） **
一个进程在申请新的资源的同时保持对原有资源的占有（只有这样才是动态申请，动态分配） 
**循环等待 **
存在一个进程等待队列 
{P1 , P2 , … , Pn}, 
其中P1等待P2占有的资源，P2等待P3占有的资源，…，Pn等待P1占有的资源，形成一个进程等待环路
