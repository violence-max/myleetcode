题目描述：  
![image](/basical/array/image/image12.png)
解题过程：  
一开始完全看不懂题目和例子的意思，看了题解以后发现，原来就是寻找一个连续的子序列，这个子序列里面只能有两种元素。结果又是滑动窗口，右边指针找，左边指针在子序列元素个数大于2时不断移除元素，同时求子序列长度。  
代码：   
```cpp
class Solution {
public:
    int totalFruit(vector<int>& fruits) {
        int len = fruits.size();
        if (len == 0) return 0;
        unordered_map<int,int> cnt;
        int left = 0,ans = 0;
        for (int right = 0; right < len; right++) {
            cnt[fruits[right]]++;
            while (cnt.size() > 2) {
                auto it = cnt.find(fruits[left]);
                it->second -= 1;
                if (it->second == 0) {
                    cnt.erase(fruits[left]);
                }
                left++;
            }
            ans = max(ans,right - left + 1);
        }
        return ans;
    }
};
```