题目描述：  
![image](/basical/string/image/image38.png)  
解决过程：  
一下子就ac了，没什么难度。不过一开始每次更新字符串的时候都是重新通过s拼接字符串，每次都是O(n)的时间复杂度，加上外层循环的while，就是O(n^2)的时间复杂度了，果不其然超时了。后来我直接用一个空字符串进行“接住”操作，ac  
代码：  
```cpp
class Solution {
public:
    string evaluate(string s, vector<vector<string>>& knowledge) {
         int ls = s.length();
         unordered_map<string ,string> map;
         int m = knowledge.size();
         if (m != 0) {
             for (int i = 0; i < m; i++) {
                map[knowledge[i][0]] = knowledge[i][1];                 
             }
         }
         int i = 0;
         string ans;
         while (i < ls) {
             if (s[i] == '(') {
                 i += 1;
                 int start = i,len = 0;
                 while (s[i] != ')')
                    i++;
                len = i - start;
                string tmp = s.substr(start,len);
                if (map.count(tmp)) {
                    ans += map[tmp];
                } else {
                    ans += '?';
                }
             } else {
                 ans += s[i];
             }
             i++;
         }
         return ans;
    }
};
```