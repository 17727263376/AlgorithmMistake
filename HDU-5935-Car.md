<font size = 4>

**题目描述：**

Ruins is driving a car to participating in a programming ==contest==. As on a very tight schedule, he will drive the car without any slow down, so the speed of the car is non-decrease ==real number==. 

Of course, his speeding ==caught the attention of== the traffic police. Police record NN positions of Ruins without time mark, the only thing they know is every position is recorded at an integer time point and Ruins started at 0. 

Now they want to know the minimum time that Ruins used to pass the last position.

---

contest--[ˈkɑ:ntest]:竞赛

real number:实数

caught the attention of:吸引交警的注意

---

**Input:**

First line contains an integer T, which indicates the number of test cases. 

Every test case begins with an integers N, which is the number of the recorded positions. 

The second line contains NN numbers a1, a2, ⋯⋯, aNa indicating the recorded positions. 

Limits 

1≤T≤100 

1≤N≤10^5 

0<ai≤10^9 

ai<ai+1

</font>

**Output:**

For every test case, you should output 'Case #x: y', where x indicates the case number and counts from 1 and y is the minimum time.

**样例:**
---
1

3

6 11 21

Case #1: 4

---

**题意：**

一个人不减速开车，被交警整点记录了一些位置，求最短时间通过最后一个记录点

**注意：**

一开始读完这题就想着，哇和蓝桥杯一样。后来才意识到，是英语太捞了，很多条件都没有看懂，主要有

1. 车速**并不是匀速**，是递增或者匀速
2. 车速不一定是**整数**
 
搞清楚这两点才算是读懂了题目

**解法：**

第一次读完题目想着是按照蓝桥杯的等差数列来写，然后就在思考如何把蓝桥杯那题解法更加优化一点。

第二次读完题，知道了他是均速或者递增，很明显，最后一段距离一定是全场的最大速度。但是没有考虑速度为小数的情况

最后一次，也是百度以后，看了别人书写的答案，才算知道怎么解了。

已知最大速度为最后一段，那么我们就从后往前（也只能从后往前），计算每一段的距离，用它除以上一段的速度得到该段的时间，判断该距离除时间是否能够得到速度。并更新速度和时间。

```
核心代码为：
int len = num[n]-num[n-1], t = 1, count = 1;
double v = len*1.0;
for(int i = n-1; i > 0; i--){
    len = num[i]-num[i-1];
    t = len/v;
    while(1.0*len/t > v)	t++;
    count += t;
    v = 1.0*len/t;
}
printf("Case #%d: %d\n", index, count);
index++;
```