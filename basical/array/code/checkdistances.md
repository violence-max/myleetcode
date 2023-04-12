题目描述：  
![image](/basical/array/image/image69.png)  
解决过程：  
5分钟解决  
- 模拟：
1. 位运算来表示字母占位—已经计算过的字母进行掩码覆盖
2. 没有计算过的字母直接按照distance数组进行距离计算判断  
代码：  
```cpp
class Solution {
public:
    bool checkDistances(string s, vector<int>& distance) {
        int n = s.size();
        int mask = 0;
        for (int i = 0; i < n; i++) {
            int index = s[i] - 'a';
            if (mask & (1 << index)) continue;
            if (i + distance[index] + 1 >= n || s[i + distance[index] + 1] != s[i]) return false;
            mask |= (1 << index);
        }
        return true;
    }
};
```