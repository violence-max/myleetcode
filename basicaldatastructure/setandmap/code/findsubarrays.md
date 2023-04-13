题目描述：  
![image](/basicaldatastructure/setandmap/image/image10.png)  
解题过程：  
3秒钟写完  
- 哈希表  
代码：  
```cpp
class Solution {
public:
    bool findSubarrays(vector<int>& nums) {
        int sum = nums[0] + nums[1];
        unordered_set<int> s = {sum};
        for (int i = 1; i < nums.size() - 1; i++) {
            int _sum = nums[i] + nums[i+1];
            if (s.count(_sum)) return true;
            s.insert(_sum);
        }
        return false;
    }
};
```