题目描述：  
![image](/basical/array/image/image57.png)  
解决过程:  
本来想用滑动窗口做出来，但是发现不太行，看了题解，发现提供了两种思路：  
- 暴力：对于以索引i的值结尾的和为k的连续子数组，可以从索引i开始倒回去逐一计算sum值，一旦sum值等于k则count加一
- 前缀和+哈希表优化：记pre[i]为0~i的值的总和。用变量pre来进行前缀和的计算：在从前往后遍历数组nums的过程中，pre加上遍历到的值x。如果在哈希表中存在pre-k，则存在map[pre-k]个和为k的连续子数组  
代码：（暴力→前缀和+哈希表优化）  
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int count = 0;
        for (int start = 0; start < nums.size(); ++start) {
            int sum = 0;
            for (int end = start; end >= 0; --end) {
                sum += nums[end];
                if (sum == k) {
                    count++;
                }
            }
        }
        return count;
    }
};
```  
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int,int> map;
        int pre = 0, ret = 0;
        map[0] = 1;
        for (auto x : nums) {
            pre += x;
            if (map.count(pre - k)) {
                ret += map[pre - k];
            }
            map[pre]++;
        }
        return ret;
    }
};
```