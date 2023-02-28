题目描述：  
![image](/basical/array/image/image46.png)  
解决过程：  
一开始coding出了一坨屎山，然后开始铲屎，虽然最后代码简洁了，但是时间变久了。感觉自己写这种普通题目的逻辑性逐渐变强了。有个code友写得非常简洁，不过是在头部和尾部加入了lower-1和upper+1，然后从下标为1的元素开始考虑和前一个元素的差值，有0，1，2三种情况  
代码：（我的→code友）  
```cpp
class Solution {
public:
    vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
        int n = nums.size();
        if (n == 0) {
            string tmp = (upper - lower > 0) ? to_string(lower) + "->" + to_string(upper) : to_string(lower);
            return {tmp};
        }

        auto get = [&](int left,int right) -> string {
            if (left == right) return to_string(left);
            else {
                return to_string(left) + "->" + to_string(right);
            }            
        };

        vector<string> ans;
        for (int i = 0; i <= n; i++) {
            int diff = (i == 0) ? nums[i] - lower : (i == n) ? upper - nums[i-1] : nums[i] - nums[i-1];
            if (diff > 1 || (i == 0 && diff >= 1) || (i == n && diff >= 1)) {
                int left = (i == 0) ? lower : nums[i-1] + 1;
                int right = (i == n) ? upper : nums[i] - 1;
                string tmp = get(left,right);
                ans.push_back(move(tmp));
            }
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
        nums.insert(nums.begin(),lower-1);
        nums.push_back(upper+1);
        vector<string> ans;
        for(int i = 1;i<nums.size();i++)
            if(nums[i]-nums[i-1]==1) continue;
            else if(nums[i]-nums[i-1]==2) ans.push_back(to_string(nums[i]-1));
            else ans.push_back(to_string(nums[i-1]+1)+"->"+to_string(nums[i]-1));
        return ans;
    }
};
```