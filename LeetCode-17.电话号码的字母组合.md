**题目描述：**

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。



**示例:**
- - -
输入："23"

输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].

**思路一：**

使用递归：发现规律s的字母组合可以化简为s[0]+substr(1)加在后面的过程，先把s[0]排列好，然后在substr(1)加在后面
```
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        int len = digits.length();
        vector<string> s;
        map<char, string> m;
        m['2'] = "abc";m['3'] = "def";m['4'] = "ghi";m['5'] = "jkl";
        m['6'] = "mno";m['7'] = "pqrs";m['8'] = "tuv"; m['9'] = "wxyz";
        if(len == 0)    return {};
        if(len == 1){
            string t = m[digits[0]];
            for(int i = 0; i<t.length(); ++i){
                s.push_back({t[i]});
            }
        }
        else{
            int c = 1;
            for(int i = 1; i<len; ++i){
                c *= m[digits[i]].length();
            }
            string t = m[digits[0]];
            for(int i = 0; i<t.length(); ++i){
                for(int j = 0; j<c; ++j){
                    s.push_back({t[i]});
                }
            }
            vector<string> temp = letterCombinations(digits.substr(1));
            for(int i = 0; i<s.size(); ++i){
                s[i] += temp[i%temp.size()];
            }
            //这个除法我觉得想的很好，差点就用两个循环才能实现拼接。
        }
        return s;
    }
};
```

**思路二：**

宽度搜索：使用队列，将每个数字对应的字母加入队列，下一次循环的时候将队列内的元素全部取出，在拼接当前的字母
```
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        int len = digits.length();
        vector<string> s;
        map<char, string> m;
        m['2'] = "abc";m['3'] = "def";m['4'] = "ghi";m['5'] = "jkl";
        m['6'] = "mno";m['7'] = "pqrs";m['8'] = "tuv"; m['9'] = "wxyz";
        if(len == 0)    return {};
        
        queue<string> que;
        que.push("");
        
        for(int i = 0; i<len; ++i){
            int ll = que.size();
            for(int j = 0; j<ll; ++j){
                string temp = que.front();
                que.pop();
                for(int k = 0; k<m[digits[i]].length(); ++k){
                    que.push(temp+m[digits[i]][k]);
                    //拼接后再入队列
                }
            }
        }
        
        while(!que.empty()){
            s.push_back(que.front());
            que.pop();
        }
        return s;
    }
};
```