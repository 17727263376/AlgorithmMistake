![image](http://tva1.sinaimg.cn/large/007X8olVly1g79nurlf21j30u00u7mzo.jpg)

<font size = "4">

- - -
有一个游戏,规则是这样,有一堆气球100个,标号1->100,有两个人参与.一但开始,两个人就疯狂的踩气球,时间到就结束了(也许就10s),把他们各自踩破的球上的编号乘起来,分别是M,N,那么排名自然揭晓了

可是分数低的人不服气,想申诉.现在问题来了,怎么申诉呢?因为每个标号的球只有一个,所以加入B踩破的话,A就没办法踩了,申诉想要产生的矛盾就在这儿.现在假如分数是 343 49 ,343可以是踩了 7和49,49只能是踩49,他们两都同时必须要踩这个49,那么就产生矛盾了.所以49赢了.还有要是有人的分数,不能由1->100的数的成绩的出,算说假话,如果两个人都说假话,还是分高的赢.

```C++
#include<iostream>
using namespace std;
int isA, isB;
void DFS(int m, int n, int kk){//kk代表当前因子 
	if(m == 1 && n == 1){
		isA = 1;
		return ;
		//如果找出了一组m和n都不相同的因子，则说明max方是合法的。只要有一组就可以返回 
	}
	if(n == 1){//n == 1 说明min值的所有因子都找完了，如果这个时候m还找不完所有因子，说明一定需要重复的元素才能重新乘回max 
		isB = 1;
		//这里不能return，只能说明这一种方案有重复的方案，不代表就没有了无重复的方案 
	}
	int k = kk;
	//k记录当前因子 
	while((k < m || k < n) && (k < 100)){
		k++;
		if(m%k == 0){
			DFS(m/k, n, k);
			if(isA)	return ;
		}
		if(n%k == 0){
			DFS(m, n/k, k);
			if(isA)	return ;
		}
	}
}

int main ()
{
	int max, min;
	while(cin>>max>>min){
		if(max < min){
			max = max^min;
			min = max^min;
			max = max^min;
		}
		//max获取最大的输入 
		isA = 0;
		isB = 0;
		//isA代表max方是否正确，isB代表min方是否正确 
		DFS(max, min, 1);
		int result = max;
		if(isB == 1 && isA == 0){
			result = min;
		}
		//只有B也就是min方是对的，max是错的情况才能算min获胜 
		cout<<result<<endl;
	}
	return 0;
}
```