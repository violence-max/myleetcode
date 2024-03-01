题目描述：

![image](/basical/simulation/image/image15.png)

解决过程：
没做出来

- 使用in_block表示当前待加入行是否在注释块内，每次只扫描一个字符

代码：

```cpp
class Solution {
public:
    vector<string> removeComments(vector<string>& source) {
        bool in_block = false;
        vector<string> res;
        string new_line = "";
        for (auto& str : source) {
            int l = str.size();
            for (int i = 0; i < l; ++i) {
                if (in_block) {
                    if (i + 1 < l && str[i] == '*' && str[i + 1] == '/') {
                        in_block = false;
                        ++i;
                    }
                } else {
                    if (i + 1 < l && str[i] == '/' && str[i + 1] == '/') {
                        break;
                    } else if (i + 1 < l && str[i] == '/' && str[i + 1] == '*') {
                        in_block = true;
                        ++i;
                    } else {
                        new_line += str[i];
                    }
                }
            }
            if (!in_block && !new_line.empty()) {
                res.push_back(new_line);
                new_line = "";
            }
        }
        return res;
    }
};
```