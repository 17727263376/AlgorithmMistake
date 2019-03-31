**题目描述：**

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

**例如1：**
- - -
输入: "babad"

输出: "bab"

注意: "aba" 也是一个有效答案。

**例如2：**
- - -
输入: "cbbd"

输出: "bb"
- - -
**思路一：**

暴力法：枚举所有字串，依次判断，时间复杂度为n^3；

**思路二：**

动态规划：如果s[i][j]是一个回文串，那么s[i+1][j-1]肯定也是一个回文串。即如果枚举一个长度为3的字串，那么只要头尾相同，头尾相内靠近也是相同的。
```
class Solution {
public:
    string longestPalindrome(string s) {
        ios::sync_with_stdio(false);
        cin.tie(nullptr);
        
        int len = s.length();
        if(len == 0)    return "";
        if(len == 1)    return s;
        
        vector<vector<int>> dp (len, vector<int>(len));
        int start = 0, maxLen = 1;
        
        for(int i = 0; i<len; ++i){
            dp[i][i] = 1;
            if(i < len-1){
                if(s[i] == s[i+1]){
                    dp[i][i+1] = 1;
                    start = i;
                    maxLen = 2;
                }
            }
        }
        
        for(int l = 3; l<=len; l++){
            for(int i = 0; l+i-1<len; ++i){
                int j = l+i-1;
                if(s[i] == s[j] && dp[i+1][j-1] == 1){
                    dp[i][j] = 1;
                    start = i;
                    maxLen = l;
                }
            }
        }
        
        return s.substr(start, maxLen);
    }
};
```
**思路三：**

Manacher算法：
1. 在每个字符中插入一个不影响全局的字符如#（首尾也要插入）

例如：abc---->#a#b#c#;

2. 使用一个辅助数组，表示以i元素为中心，最大回文半径。

3. 增加两个辅助变量，一个表示当前最大回文的最右侧边界maxR。一个为该最大回文边界的中心p，即对称轴

4. 对于每一次遍历i，
 
如果i比maxR还要大，那么就直接从i开始向左右开始判断是否回文，也就是直接扩张。
 
如果i比maxR要小，那么就判断i关于p的对称点j，因为j肯定比p小，所以j已经判断过了。首先让i的最大回文半径等于j的，然后判断这个半径最右侧，是否超过了maxR，如果没有超过说明，i和j都在p的这个回文的范围内，并且关于p对称，就直接让i的最大回文半径等于j，如果超过或者等于maxR，那么i所在的回文有可能超过p，所以在这个基础上要继续扩宽。
