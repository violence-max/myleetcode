题目描述：  
![image](/basical/array/image/image61.png)  
解决过程：  
排序+直接判断子数组是不是等差数列  
题解：  
1. max为子数组的最大值，min为子数组的最小值，l为子数组的长度，则公差为d = (max - min) / (l - 1)，故如果d不为整数则可以提前返回false
2. 得到公差后可以在不排序的情况下借用位置数组来进行子数组是否为等差数列的判断：对于遍历到的数字i，如果满足`(i - min) % d == 0` ，而且`seen[(i - min) / d]` 为0，则数字i可以被当作是等差数列中的一个数字  
代码：（排序→暴力）  
```cpp
class Solution {
public:
    bool check(vector<int> v) {
        if (v.size() == 2) return true;
        sort(v.begin(), v.end());
        for (int i = 1; i < v.size() - 1; i++) {
            if (v[i+1] - v[i] != v[i] - v[i-1]) {
                return false;
            }
        }
        return true;
    }

    vector<bool> checkArithmeticSubarrays(vector<int>& nums, vector<int>& l, vector<int>& r) {
        vector<bool> ans (l.size());    
        for (int i = 0; i < l.size(); i++) {
            if (check({nums.begin()+l[i], nums.begin()+r[i]+1})) {
                ans[i] = true;
            }
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    vector<bool> checkArithmeticSubarrays(vector<int>& nums, vector<int>& l, vector<int>& r) {
        int n = l.size();
        vector<bool> ans;
        for (int i = 0; i < n; ++i) {
            int left = l[i], right = r[i];
            int minv = *min_element(nums.begin() + left, nums.begin() + right + 1);
            int maxv = *max_element(nums.begin() + left, nums.begin() + right + 1);

            if (minv == maxv) {
                ans.push_back(true);
                continue;
            }
            if ((maxv - minv) % (right - left) != 0) {
                ans.push_back(false);
                continue;
            }

            int d = (maxv - minv) / (right - left);
            bool flag = true;
            vector<int> seen(right - left + 1);
            for (int j = left; j <= right; ++j) {
                if ((nums[j] - minv) % d != 0) {
                    flag = false;
                    break;
                }
                int t = (nums[j] - minv) / d;
                if (seen[t]) {
                    flag = false;
                    break;
                }
                seen[t] = true;
            }
            ans.push_back(flag);
        }
        return ans;
    }
};
```