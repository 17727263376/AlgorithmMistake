![image](http://tva1.sinaimg.cn/large/007X8olVly1g7cx3ke7e4j30u00z776b.jpg)

<font size = "4">

- 倒水问题：

（一）原题大意：

你有两个杯子，A、B；你只有三种操作，

（1）清空任何一个杯子 

（2）当被子是空的时候可以填满任何一个杯子 

（3）将某一杯的水倒到另一杯中（不会溢出计算）

**最后直到B杯达到目标数Aim。**

（输入的A、B升数有要求，一定需要相邻互质。并且A小于B，且目标数Aim 要小于B即可。）

- - -
本题需要知道，不断的循环下面三个步骤

（1）装满A

（2）从A倒B

（3）如果B满了清空

基本以上三个步骤都能找打准确的结果。

以上步骤相当于B = (i*A)%B;
- - -
如果目标数Aim小于A，则每次都装满A，往B中倒的步数更少。如果目标数大于A，则每次都应该装满B，往A中倒，这样的步数更少
- - -
倒水定理则是在这个基础上提出的，如果多个杯子判断是否能够倒出指定升的水，则需要求出杯子的最大公约数，并判断目标数是否可以整除这个最大公约数，因为最大公约数是可以通过以上两个杯子的方法倒出来，目标数只需要不断重复倒出最大公约数并倒入即可
```C++
#include<iostream>
using namespace std;

int A, B, N;
//true表示对A操作,false表示对B操作
int conA, conB;

void fill(bool b){
	if(b){
		conA = A;
		cout<<"fill A"<<endl;
	}else{
		conB = B;
		cout<<"fill B"<<endl;
	}
} 

void empty(bool b){
	if(b){
		conA = 0;
		cout<<"empty A"<<endl;
	}else{
		conB = 0;
		cout<<"empty B"<<endl;
	}
}

void pull(bool b){
	if(b){
		//从A倒入B
		if(conA + conB <= B){
			conB = conA + conB;
			conA = 0;
		}else{
			conA = conA - (B - conB);
			conB = B;
		}
		cout<<"pour A B"<<endl;
 	}else{
		//从B倒入A
		if(conA + conB <= A){
			conA = conA + conB;
			conB = 0;
		}else{
			conB = conB - (A - conA);
			conA = A;
		}
		cout<<"pour B A"<<endl;
	}
}
//三个操作的函数 

int main (){
	
	while(cin >> A >> B >> N){
		
		if(N == 0){
			cout<<"success"<<endl;
			continue;
		}
		if(A == N){
			cout<<"fill A"<<endl;
			cout<<"pour A B"<<endl;
			cout<<"success"<<endl;
			continue;
		}
		if(B == N){
			cout<<"fill B"<<endl;
			cout<<"success"<<endl;
			continue;
		}
		//如果目标数刚好满足A和B的容量，则直接判断。否则继续循环有可能会进入死循环 
		conA = 0; conB = 0;
		//初始化两个杯都没水 
		while(true){
			if(A >= N){
				if(conA == 0){
					fill(true);
				}
				pull(true);
				if(conB == B){
					empty(false);
				}
				//如果目标数小于A，则优先倒满A可得到最优解 
			}else{
				if(conB == 0){
					fill(false);
				}
				pull(false);
				if(conA == A){
					empty(true);
				}
				//如果目标数大于A，则优先倒满B可得到最优解 
			}
			
			if(conA == N){
				if(conB != 0)	cout<<"empty B"<<endl;
				cout<<"pour A B"<<endl;
				cout<<"success"<<endl;
				break;
			}
			if(conB == N){
				cout<<"success"<<endl;
				break;
			}
			//一旦A和B倒出了目标数目，即可结束 
		}
	}
	return 0;
}
```
</font>