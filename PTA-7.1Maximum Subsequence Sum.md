<font size = "4">

**题目：**

Given a sequence of K integers { N1, N2, ..., NK}. A continuous subsequence is defined to be { Ni, Ni+1, ..., Nj} where 1≤i≤j≤K. The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements. For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.

**输入：**

Each input file contains one test case. Each case occupies two lines. The first line contains a positive integer K (≤10000). The second line contains K numbers, separated by a space.

**输出：**

For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line. In case that the maximum subsequence is not unique, output the one with the smallest indices i and j (as shown by the sample case). If all the K numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

**题意：**

这题就是求一个序列的最大子列和的问题。

**思路一：**

看到序列的问题我第一时间想的就是动规，dp[i][j]表示长度为i起点为j的子列合。可以得到结论

**dp[1][j] = num;**

**dp[i][j] = dp[i-1][j]+dp[1][j+i-1];**

作为第一次自己手推出的动规方程，心情那是一个激动，结果马上就着手写代码。期间遇到了及其多的问题。主要问题有：

1. 内存爆了，我就很奇怪为啥用vector都能爆内存，我也没有动态开辟空间。最过通过两个队友的提示解决了一部分的内存爆了的问题，第一个方法是申明dp为全局变量，这个方法也省去了new的时间。第二个方法是采用结构体分配空间，这样可以达到分配非连续空间的效果。这两个方法都让我大开眼界。作为每一个遇到大数组都用vector的我来说，得到了很大的帮助
2. 题意不明确，我的英语真是没救了，题目让我输出相应下标的具体value，结果我一直在输出下标，我还纳闷为啥一直错呢。真是沙雕了！

下面为还剩一个样例没通过的源码
```
#include<iostream>
#include<cstdio>
#include<vector>
#include<cstdlib>
using namespace std;
int dp[10001][10001];//使用全局变量
//struct Node{
//	int *data;
//	Node(){
//		data = new int[10001];
//	}
//};
//使用结构体申请
int main ()
{
	int n, temp, max1, index_begin, len, i, j;
	//vector< vector<int> > dp(10001, vector<int>(10001));
	
    //	int** dp = (int**)malloc(sizeof(int*)*10001);
    //	for(i = 0; i<10001; i++){
    //		dp[i] = (int*)malloc(sizeof(int)* 10001);
    //	}
    // 使用malloc申请控件

	//int dp[10001][10001];
	
	//Node dp[10001];//使用结构体
	
//	dp动规表示长度为i起点为j的字串sum大小 

	bool is_negative = true;//用来判断是否全部为负数
	scanf("%d", &n);
	index_begin = 0; len = 1;
	for(i = 0; i < n; ++i){
		scanf("%d", &temp);
		dp[1][i] = temp;    //一边输入一边设置dp的边界值
		if(i == 0)	max1 = temp;
		if(max1 < temp && i > 0){
			max1 = temp;
			index_begin = i;
		}                   //判断一个数就是最大的情况
		if(temp >= 0)	is_negative = false; 
	}
	if(is_negative){
		cout<<0<<" "<<dp[1][0]<<" "<<dp[1][n-1];
		return 0;
	}
	for(i = 2; i <= n; i++){
	    //从长度为2开始枚举
		for(j = 0; j < n; j++){
			if((j+i-1) < n){
				dp[i][j] = dp[i-1][j] + dp[1][j+i-1];
				if(dp[i][j] > max1){
					max1 = dp[i][j];
					len = i;
					index_begin = j;
				}
				if(dp[i][j] == max1){
					if(index_begin > j){
						len = i;
						index_begin = j;
					}
					//题目要求输出下标最小的情况
				}
			}else{
				break;
			}
		}
	}
	cout<<max1<<" "<<dp[1][index_begin]<<" "<<dp[1][index_begin+len-1];
	return 0;
}
```

**思路二：**
联机算法：在读取数据的时候计算累加情况，如果<0则重新开始

不看百度我铁定不会这种做法

```
#include <iostream>
using namespace std;
int main(){
	int k;
	cin>>k;
	int a[k], thisSum = 0, maxSum = -1, tag = 0, firstNum = 0, lastNum = 0;
	int temp_first = 0;
	for(int i = 0; i < k; i++){  
		cin>>a[i];
		thisSum += a[i];
		if(tag == 0){
			temp_first = a[i];
			tag = 1;
			//当thisSum归零以后将temp_first设置为新的下标对应的value 
		}
		if(thisSum > maxSum){
			maxSum = thisSum;
			firstNum = temp_first;	
			lastNum = a[i];
		}else if(thisSum < 0){
			thisSum = 0;
			tag = 0;
			//为什么要thisSum<0的时候才清零？
			//那是因为当thisSum为负数以后，必须在下一个数加上一个比
			//前面最大和的数还要大的数才可以由负数变为正数并且超过它，显然如果存在这样一个数，完全应该
			//从这个数开始重新计数，这样之后的累加和才会更加大。
			 
			//为什么thisSum<ansSum的时候不需要清零？ 
			//举个例子，如果在经过一次累加以后，ansSum变成了ansSum-5（大于0），这种情况，完全可能存在
			//一个数字使得ansSum再一次突破最大。 
		}
	}
	if(maxSum < 0){
		cout<<0<<" "<<a[0]<<" "<<a[k-1];
	}else{
		cout<<maxSum<<" "<<firstNum<<" "<<lastNum;
	}
}
```
**思路三：**

分治法：这个问题可以分为左一半的最大和，和右一半的最大和，还有跨越中间分界线的最大和。

这种方法的难点就在于怎么合并

假设左一半的最大子列和为LeftMax，右一半为RightMax。那么跨越中间的就是**包含左边最后一个元素的最大和加上右边包括第一个元素的最大和的和**

- - -

**注：**
百度有关于动态规划的解法，是一维动规，而我的是二维的，所以说还是太菜了。虽然想到了动规，但是却不能很好的实现！！
</font>