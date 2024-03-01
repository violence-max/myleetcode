题目描述：

![image](/basical/IQ/image/image60.png)

解决过程：

没做出来，比较难的一道智商模拟题

代码：

```cpp
class Solution {
public:
    bool canConvert(string str1, string str2) {
        // 不同的case:
        // 1) 一一映射
        // 2) 一对多映射：如果str1中有某个字符需要转换到str2中两个或者更多字符，则无法完成转换
        // 3) 链表映射（重叠映射）需要将str1中的每个字符转换为权值高于未来使用的字符（不会被二次转换）a->b->c->d->e->f
        // 4) 循环链表：需要引入一个不存在于str1中的临时字符来实现转换 a->b->c c->a
        // 5) 多个循环链表：可以只使用一个临时字符为每个环单独实现转换
        // 6) 带26个字母的循环链表：无法实现转换
        // 7) 带有26个字母和一个环的链表：只要str2中独特字符个数小于26就可以完成转换

        // 转换条件：
        // 1) str1中无字符映射到str2中的多个字符
        // 2) 如果str1中不同字符数是26，那么str2中不同字符数应该小于26，否则应该满足str1==str2
        if (str1 == str2) return true;
        unordered_map<char,char> m;
        unordered_set<char> s;
        int n = str1.size();
        for (int i = 0; i < n; ++i) {
            if (m.find(str1[i]) == m.end()) {
                m[str1[i]] = str2[i];
                s.insert(str2[i]);
            } else if (m[str1[i]] != str2[i]) {
                return false;
            }
        }
        if (s.size() < 26) return true;
        return false;
    }
};
```