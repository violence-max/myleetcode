题目描述：  
![image](/basical/array/image/image20.png)  
解决过程：  
模拟就可以解决—出现了一个严重错误：在每次循环的时候忘记对MAX值进行初始化了，导致每次处理最大值时会沿用上一次的最大值 
代码：  
```cpp
class Solution {
public:
    int maxElement(vector<int> v) {
        int MAX = v[0];
        int ans = 0;
        for (int i = 1; i < v.size(); i++) {
            if (v[i] > MAX) {
                ans = i;
                MAX = v[i];
            }
        }
        return ans;
    }

    int deleteGreatestValue(vector<vector<int>>& grid) {
        int ans = 0;
        while (grid[0].size() >= 1) {
            int MAX = INT_MIN;
            for (auto &v : grid) {
                int index = maxElement(v);
                MAX = max(MAX,v[index]);
                v.erase(v.begin()+index);
            }
            ans += MAX;
        }
        return ans;
    }
};
```