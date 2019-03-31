**题目描述：**
给定一个字符串 (s) 和一个字符模式 (p)。实现支持 '.' 和 '*' 的正则表达式匹配。

'.' 匹配任意单个字符。

'*' 匹配零个或多个前面的元素。

匹配应该覆盖整个字符串 (s) ，而不是部分字符串。

说明:

s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。
- - -
**示例 1:**

输入:
s = "aa"

p = "a"

输出: false

解释: "a" 无法匹配 "aa" 整个字符串。
- - -
**示例 2:**

输入:
s = "aa"

p = "a*"

输出: true

解释: '*' 代表可匹配零个或多个前面的元素, 即可以匹配 'a' 。因此, 重复 'a' 一次, 字符串可变为 "aa"。
- - -
**示例 3:**

输入:

s = "ab"

p = ".*"

输出: true

解释: ".*" 表示可匹配零个或多个('*')任意字符('.')。
- - -
**示例 4:**

输入:
s = "aab"

p = "c*a*b"

输出: true

解释: 'c' 可以不被重复, 'a' 可以被重复一次。因此可以匹配字符串 "aab"。
- - -
**示例 5:**

输入:

s = "mississippi"

p = "mis*is*p*."

输出: false

**思路：**

一开始的想法：将s和p依次匹配，用新的string保存结果，如果最后的key和s相同这说明匹配成功。这种想法在多个*和.的时候会产生错误，这样是错误的。

**1.递归：**
通过线性递归的方法
```
class Solution {
public:
    bool isMatch(string s, string p) {
        ios::sync_with_stdio(false);
        cin.tie(nullptr);
        
        if(!s.length() && !p.length())      return true;
        if(!p.length() && s.length() > 0)   return false;
        //边界情况
        if(!s.length()){
            if(p.length()%2 != 0)   return false;
            int i = 1;
            while(i < p.length() && p[i] == '*'){
                i += 2;
            }
            if(i == p.length()+1)   return true;
            else                    return false;
        }
        //当s为空，p必须满足.*.*.*这样的形式才可以匹配，所以首先要是偶数的长度，其次偶数位是*
        int i = -1;
        
        if(p.length() >= 2 && p[1] == '*'){
            do{
                if(isMatch(s.substr(++i), p.substr(2))){
                    return true;
                }else if(i == s.length()){
                    return false;
                }
            }while(s[i] == p[0] || p[0] == '.');
            return false;
            //用p+2和s匹配
        }
        else{
            if(s[0] == p[0] || p[0] == '.') return isMatch(s.substr(1), p.substr(1));
            else                            return false;
        }
        //如果前两位不是.*形式，就判断字母是否相同或者为‘.’
    }
};
```
string.substr(**int x**):

当这个函数为一个参数时，表示截取string从x之后的字符串

例如s = “0123456789”;那么s.substr(2)等于“23456789”;

string.substr(**int x, int y**):

当它有两个参数的时候，表示截取x到y之间的字符串

例如s = “0123456789”;那么s.substr(0,2)等于“01”;

**2.动态规划：**

首先，设x为s的最后一字字符，z为p的最后一个字符，y为p的倒数第二字字符，剩余的记作S或者P；

则s = Sx;

p = Pyz;

那么当z = '*'，如果x != y那么只要s和P匹配即可；

如果x == y，那么s要和P匹配

```
class Solution {
public:
    bool isMatch(string s, string p) {
        ios::sync_with_stdio(false);
        cin.tie(nullptr);
        
        vector<vector<bool>> match(s.length()+3, vector<bool>(p.length()+3, false));
        
        //match[i][j]表示长度为i的s和长度为j的p是否匹配
        
        match[0][0] = true;
        match[1][1] = (s[0] == p[0] || p[0] == '.');
        for(int j = 2; j <= p.length(); j += 2){
            match[0][j] = (match[0][j-2] && p[j-1] == '*');
        }
        //边界情况
        for(int i = 1; i<=s.length(); i++){
            for(int j = 2; j<=p.length(); j++){
                if(p[j-1] != '*'){
                    match[i][j] = match[i-1][j-1] && (s[i-1] == p[j-1] || p[j-1] == '.');
                }
                else{
                    match[i][j] = match[i][j-2] || (match[i-1][j] && (s[i-1] == p[j-2] || p[j-2] == '.'));
                }
                //动态规划的递推关系
            }
        }
        
        return match[s.length()][p.length()];
    }
};
```

更优化简洁的递推
```
vector<vector<bool>> match(lenS+3, vector<bool>(lenP+3, false));
        
    match[0][0] = true;
    
    for(int i = 0; i <= lenS; i++){
        for(int j = 1; j <= lenP; j++){
            if(j > 1 && p[j-1] == '*'){
                match[i][j] = match[i][j-2] || (i>0 && match[i-1][j] && (s[i-1] == p[j-2] || p[j-2] == '.'));
            }else{
                match[i][j] = i>0 && match[i-1][j-1] && (s[i-1] == p[j-1] || p[j-1] == '.');
            }
        }
    }
    //巧妙的将s为空的情况一起考虑进去
```
**体会：**

通过这一道题目，我对于动态规划有了很深的理解，只要遇到解决一个问题可以通过他的小问题合并得到最终解的题目，都可以尝试使用动态规划，每一个的动态规划基本都设计一个二维数组，这个数字的i和j可能表示的意思不仅仅是坐标，它可能是一种标记，相应的value也是可能会是一种标记。