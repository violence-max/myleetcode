题目描述：

![image](/basical/string/image/image76.png)

```cpp
class Solution {
public:
    string ops(const string& s, int st, int e) {
        string ans = "";
        for (int i = st; i <= e; ++i) {
            ans += s[i] - 1;
        }
        return ans;
    }
    
    string smallestString(string s) {
        int n = s.size();
        int start = -1, end = -1;
        int i = 0;
        while (i < n && s[i] == 'a') {
            ++i;
        }
        if (i == n) {
            s[n-1] = 'z';
            return s;
        }
        start = i;
        while (i < n && s[i] != 'a') {
            ++i;
        }
        end = i - 1;
        return s.substr(0,start) + ops(s,start,end) + s.substr(end+1);
    }
};
```

解决过程：

第349场周赛t2，12分钟解决

代码：

```cpp
class Solution {
public:
    string ops(const string& s, int st, int e) {
        string ans = "";
        for (int i = st; i <= e; ++i) {
            ans += s[i] - 1;
        }
        return ans;
    }
    
    string smallestString(string s) {
        int n = s.size();
        int start = -1, end = -1;
        int i = 0;
        while (i < n && s[i] == 'a') {
            ++i;
        }
        if (i == n) {
            s[n-1] = 'z';
            return s;
        }
        start = i;
        while (i < n && s[i] != 'a') {
            ++i;
        }
        end = i - 1;
        return s.substr(0,start) + ops(s,start,end) + s.substr(end+1);
    }
};
```