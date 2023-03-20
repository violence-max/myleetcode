题目描述：  
![image](/basical/string/image/image63.png)  
解决过程：  
没做出来  
- 数论（裴蜀定理）
1. 轮转次数和累加次数是相互独立的
2. 因为字符串s的长度是偶数，因此如果b是偶数，那么只有奇数位上的数可以进行累加
3. 先对轮转次数进行枚举，再对累加次数进行枚举
4. 对轮转次数进行枚举可以采用取余的技巧，因为第一个数再经过若干次轮转之后一定会回到第一个位置上，因此需要一个vis数组来记录当前的轮转状态，如果当前的轮转状态是重复的就可以结束循环了。而每次轮转的进行只需要`(i+b)%n`
5. 对于累加次数的枚举需要对奇数和偶数同时进行，由第2点，可以设置一个变量`k_limit` 来决定枚举的次数，如果b是偶数那么偶数位的枚举上限是0，即不需要枚举，否则为9（累加最多执行9次回到原点）
6. **字符串字典序的判断可以直接使用min函数**
7. 对于代码中使用累加b和vis数组记录轮转状态的方法，裴蜀定理给出了优化：$xb - yn = z$，这里的x指的是轮转次数，y指的是取模过程中减去n的次数，z是值到达的位置。因此for循环可以优化为：`for ( int i = 0; i < n; i += g)` ，这里的g是`gcd(b,n)` 
8. 和水壶问题类似的是，基于第7点，我们探讨的问题是对于长度为n的字符串，进行x次长度为b的轮转过后能否使得z作为字符串新的起点
9. **使用s + s作为新的s来进行对轮转后字符串的取子串的操作**  
$$
xb - yn = z
$$  
代码：（裴蜀定理→深度优先搜索）  
```cpp
class Solution {
public:
    string findLexSmallestString(string s, int a, int b) {
        int n = s.size();
        vector<int> vis(n);
        string res = s;
        // 将 s 延长一倍，方便截取轮转后的字符串 t
        s = s + s;
        for (int i = 0; vis[i] == 0; i = (i + b) % n) {
            vis[i] = 1;
            for (int j = 0; j < 10; j++) {
                int k_limit = b % 2 == 0 ? 0 : 9;
                for (int k = 0; k <= k_limit; k++) {
                    // 每次进行累加之前，重新截取 t
                    string t = s.substr(i, n);
                    for (int p = 1; p < n; p += 2) {
                        t[p] = '0' + (t[p] - '0' + j * a) % 10;
                    }
                    for (int p = 0; p < n; p += 2) {
                        t[p] = '0' + (t[p] - '0' + k * a) % 10;
                    }
                    res = min(res, t);
                }
            }
        }
        return res;
    }
};
```  
```cpp
class Solution {
public:
    unordered_set<string> mem;
    string ans;
    string findLexSmallestString(string s, int a, int b) {
        int n = s.size();
        ans = string(n, '9');
        dfs(s, a, b);
        return ans;
    }

    void dfs(string& s, int a, int b) {
        if(mem.find(s) != mem.end()) return;
        ans = min(ans, s);
        mem.insert(s);
        // 向右旋转b位
        string t = rotate(s, b);
        dfs(t, a, b);
        // 给奇数位加上a
        t = s;
        for(int i = 1; i < s.size(); i += 2) {
            int x = s[i] - '0';
            x = (x + a) % 10;
            t[i] = (char)('0' + x);
        }
        dfs(t, a, b);
    }

    string rotate(string& cur, int right) {
        int n = cur.size();
        string t = cur;
        int index = 0;
        for(int i = n - right; i < n; i++) {
            t[index++] = cur[i];
        }
        for(int i = 0; i < n - right; i++) {
            t[index++] = cur[i];
        }
        return t;
    }
};
```