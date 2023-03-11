题目描述：  
![image](/basical/IQ/image/image30.png)  
解决过程：  
没做出来  
看了题目，首先要了解[余数定理](/taca/math/%E4%BD%99%E6%95%B0%E5%AE%9A%E7%90%86.md)，然后就很好做了  
* 关键是要删除的是子数组而不是随便一个子集
代码:  
```cpp
class Solution {
public:
    int minSubarray(vector<int>& nums, int p) {
        int x = 0;
        for (auto num : nums) {
            x = (x + num) % p;
        }
        if (x == 0) {
            return 0;
        }
        unordered_map<int, int> index;
        int y = 0, res = nums.size();
        for (int i = 0; i < nums.size(); i++) {
            index[y] = i; // f[i] mod p = y，因此哈希表记录 y 对应的下标为 i
            y = (y + nums[i]) % p;
            if (index.count((y - x + p) % p) > 0) {
                res = min(res, i - index[(y - x + p) % p] + 1);
            }
        }
        return res == nums.size() ? -1 : res;
    }
};
```