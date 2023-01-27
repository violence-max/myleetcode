题目描述：

![image](/basical/string/image/image51.png)  
解决过程：  
封了三种情况去讨论，两个字符串长度绝对值相差为1的情况借用了句子的相似性的写法，但是官解只在乎从头开始记录相似字符个数的写法，我觉得也可以  
代码：  
```cpp
class Solution {
public:
    bool issimilar(string& s,string& t) {
        int ls = s.length(), lt = t.length();
        int is = 0, it = 0, js = 0, jt = 0;
        while (is < ls && it < lt && s[is] == t[it]) {
            is++;
            it++;
        }
        while (ls-1-js >= is && lt-1-jt >= it && s[ls-js-1] == t[lt-jt-1]) {
            js++;
            jt++;
        }
        return is + js == ls;
    }
    bool isOneEditDistance(string s, string t) {
        int ls = s.length(), lt = t.length();
        int diff = abs(ls - lt);
        if (diff > 1) return false;
        else if (diff == 0) {
            int op = 0;
            for (int i = 0; i < ls; i++) {
                if (s[i] != t[i]) {
                    op++;
                    if (op > 1) return false;
                }
            }
            return op == 1;
        } else {
            return (ls > lt) ? issimilar(t,s) : issimilar(s,t);
        }
    }
};
```