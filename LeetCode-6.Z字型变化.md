**题目描述：**
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

L   C   I   R

E T O E S I I G

E   D   H   N

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);

**例如：**
- - -
输入: s = "LEETCODEISHIRING", numRows = 3

输出: "LCIRETOESIIGEDHN"
- - -
**思路：**

一开始想的是通过找规律，肯定可以找到一个规律直接写。后来想了想，发现这个很像树的层序遍历，答案就出来了，不过这个需要补全字符串，使他刚好可以Z字变化
```
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows == 1)    return s;
        ios::sync_with_stdio(false);
        cin.tie(nullptr);
        int len = s.length();
        int i = 0;
        while(i < len - 1)    i = i+2*(numRows-1);
        if(i != len - 1) {
            while(len-1 != i){
                s += '*';
                len++;
            }
        }  
        string key = "";
        vector<int> p(len, 0);
        queue<int> qu;
        
        i = 0;
        while(i<len)    {
            if(s[i] != '*') key += s[i];
            p[i] = 1;
            if(i-1 >= 0 && p[i-1] == 0) {qu.push(i-1);  p[i-1] = 1;}
            if(i+1 <len && p[i+1] == 0) {qu.push(i+1);  p[i+1] = 1;}
            i = i+2*(numRows-1);
        }
        while(!qu.empty()){
            int temp = qu.front();
            if(s[temp] != '*')  key += s[temp];
            qu.pop();
            if(temp-1 >= 0 && p[temp-1] == 0) {qu.push(temp-1); p[temp-1] = 1;}
            if(temp+1 <len && p[temp+1] == 0) {qu.push(temp+1); p[temp+1] = 1;}
            
        }
        return key;
    }
};
```
**网上大佬8ms的代码：**

思路是通过一个向下还是向上的bool变量，每一行为一个字符串，最后汇总
```
class Solution {
public:
    string convert(string s, int numRows) {
             if (numRows == 1) return s;

        vector<string> rows(min(numRows, int(s.size())));
        int curRow = 0;
        bool goingDown = false;

        for (char c : s) {
            rows[curRow] += c;
            if (curRow == 0 || curRow == numRows - 1) goingDown = !goingDown;
            curRow += goingDown ? 1 : -1;
        }

        string ret;
        for (string row : rows) ret += row;
        return ret;
    }
};
```
**体会：**
大佬的代码非常清晰，思路也很严谨，而我的层序遍历的方法也可以，不过一开始没有考虑到字符串需要补全这一点。