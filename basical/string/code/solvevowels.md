题目描述：

![image](/basical/string/image/image83.png)

解决过程：

第109场双周赛t2，ac

代码：

```cpp
class Solution {
public:
    const unordered_set<char> ss = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'};
    string sortVowels(string s) {
        priority_queue<int> idx;
        priority_queue<char> q;
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            char x = s[i];
            if (ss.count(x)) {
                // cout << i << ' ' << x << endl;
                idx.push(i);
                q.push(x);
            }
        }
        while (!idx.empty()) {
            int id = idx.top();
            char c = q.top();
            idx.pop();
            q.pop();
            s[id] = c;
        }
        return s;
    }
};
```