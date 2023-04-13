题目描述：  
![image](/basical/array/image/image65.png)  
解决过程：  
2分钟ac  
- 哈希表：先构造，或者寻找比i - diff和i - 2 * diff都行
- 三指针：从数字i开始尝试找到满足题意的三元组(i,j,k)，逐渐尝试j和k的可行性  
代码：（哈希→三指针）  
```cpp
class Solution {
public:
    int arithmeticTriplets(vector<int>& nums, int diff) {
        unordered_set set (nums.begin(), nums.end());
        int res = 0;
        for (auto i : nums) {
            if (set.count(i + diff) && set.count(i + 2 * diff)) res++;
        }
        return res;
    }
};
```
```cpp
class Solution {
public:
    int arithmeticTriplets(vector<int>& nums, int diff) {
        int ans = 0;
        int n = nums.size();
        for (int i = 0, j = 1, k = 2; i < n - 2 && j < n - 1 && k < n; i++) {
            j = max(j, i + 1);
            while (j < n - 1 && nums[j] - nums[i] < diff) {
                j++;
            }
            if (j >= n - 1 || nums[j] - nums[i] > diff) {
                continue;
            }
            k = max(k, j + 1);
            while (k < n && nums[k] - nums[j] < diff) {
                k++;
            }
            if (k < n && nums[k] - nums[j] == diff) {
                ans++;
            }
        }
        return ans;
    }
};
```