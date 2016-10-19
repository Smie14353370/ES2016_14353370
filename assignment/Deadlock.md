
# ����

@[ʵ������|ʵ�鲽��|��������|]

##ʵ������
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
������������ֱ�ΪAB�����ж���ͬ�������ֱ�ΪmethodA��methodB���ֱ����������last�����Դ�ӡ������������synchronized��
- **������������һ����������һ��������ʱ���ܹ���֤��ͬһʱ�����ֻ��һ���߳�ִ�иöδ��롣** 
- **��һ���̷߳���object��һ��synchronizedͬ��������ͬ������ʱ�������̶߳�object����������synchronizedͬ��������ͬ�������ķ��ʽ���������**

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
	��������������ʱ���ᣬ��t.start(),֮���߳�t�ͱ����뵽���ȶ���������ȵ�����ʱ�򣬾���run()����Ĵ���

 `	public void run(){
		b.methodB(a);
	}
		public static void main(String args[]){
		new Deadlock();
	}
}  `
��t.start()���ú�count�Լ������ʱ������a��b��ͬһʱ������method�����ĸ��ʣ�����synchronized����������ͬʱ����method���������������

## ʵ�鲽��
>������Ĵ��볭�� Deadlock.java����,���档ע��˴��Ҽ�����method�����м�����ʱ��Ҳ�ı���countֵ���Ա������������ʡ�
![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8xg9sv33dj20ap03s0sw.jpg)![enter image description here](http://ww1.sinaimg.cn/mw690/82c0590fgw1f8xg9temflj209h0480sz.jpg)
>�ն˱���java�ļ��� javac Deadlock.java	
![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8xg9vrmbhj20hi00h3yo.jpg)

> linux ϵͳ��������ε����±��Ȼ�󱣴�Ϊ.sh��Ȼ���������ļ�����java����Deadlock.class��Ŀ¼�£�˫�����У��۲���
>![enter image description here](http://ww2.sinaimg.cn/mw690/82c0590fgw1f8xg9svowwj207d05g0t6.jpg)
![enter image description here](http://ww3.sinaimg.cn/mw690/82c0590fgw1f8xg9ymz1vj20j100zt93.jpg)
![enter image description here](http://ww3.sinaimg.cn/mw690/82c0590fgw1f8xg9z7gbbj20fq00lglq.jpg)

>����ڵھŴη�������
![enter image description here](http://ww4.sinaimg.cn/mw690/82c0590fgw1f8xg9s5vwbj20g00ctq4v.jpg)


##��������
**����ʹ�ã���Դ��ռ��**
һ����Դÿ��ֻ�ܸ�һ������ʹ�� 
**������ռ�����ɰ��ᣩ **
��Դ�����߲���ǿ�еĴ���Դռ�������ж�ȡ��Դ����Դֻ����ռ������Ը�ͷ� 
**����ͱ��֣����ַ��䣬ռ�����룩 **
һ�������������µ���Դ��ͬʱ���ֶ�ԭ����Դ��ռ�У�ֻ���������Ƕ�̬���룬��̬���䣩 
**ѭ���ȴ� **
����һ�����̵ȴ����� 
{P1 , P2 , �� , Pn}, 
����P1�ȴ�P2ռ�е���Դ��P2�ȴ�P3ռ�е���Դ������Pn�ȴ�P1ռ�е���Դ���γ�һ�����̵ȴ���·
