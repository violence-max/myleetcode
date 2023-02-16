题目描述:  
![image](/basical/array/image/image52.png)  
解决过程:  
三分钟ac,题解写得很精髓  
代码:(我的→题解)  
```cpp
class Solution {
public:
    vector<int> numberOfPairs(vector<int>& nums) {
        unordered_set<int> s;
        int n = nums.size(), cnt = 0;
        for (int i = 0; i < n; i++) {
            if (s.count(nums[i])) {
                cnt++;
                s.erase(nums[i]);
            } else s.insert(nums[i]);
        }
        return {cnt,n - 2 * cnt};
    }
};
```  
```cpp
class Solution {
public:
    vector<int> numberOfPairs(vector<int>& nums) {
        unordered_map<int, bool> cnt;
        int res = 0;
        for (int num : nums) {
            if (cnt.count(num)) {
                cnt[num] = !cnt[num];
            } else {
                cnt[num] = true;
            }
            if (!cnt[num]) {
                res++;
            }
        }
        return {res, (int)nums.size() - 2 * res};
    }
};
```