题目描述：  
![image](/basical/array/image/image60.png)  
解决过程：  
做出来了  
- 前缀和+哈希：
1. 找到目标的k，标记其下标为found，以found为中点进行对数组nums的向两边扩展
2. 前缀数组和后缀数组的的定义：pre[i]表示从下标i到found大于k个元素的个数
3. ans初始化为1，因为可以单独拿k作为子数组
4. 在向两边进行扩展的过程中，分别以k作为子数组的结尾或者开头来判断是否存在以k为中点的子数组：考虑子数组分别为奇数或者偶数来决定子数组中大于k的元素的个数是0个还是1个
5. 对于found左半部分的元素，由于前缀和已经计算好了，可以直接对其进行遍历，根据3，即子数组的长度为奇数或者偶数在found的右边寻找是否存在对应的部分来进行对答案的计算
- 题解：
1. 只需要一次遍历，用sum维护前缀和，注意空前缀的前缀和为0
2. 对于found之后的右半部分，只需要查询found之前的前半部分是否存在sum或者sum-1，即在区间[left+1,right]内满足pre[left] == pre[right]或者pre[left] + 1 == pre[right]即可  
代码：（前缀和+哈希→题解）  
```cpp
class Solution {
public:
    int countSubarrays(vector<int>& nums, int k) {
        int found = -1, n = nums.size();
        for (int i = 0; i < n; i++) {
            if (nums[i] == k) {
                found = i;
                break;
            }
        }
        if (found == -1) return 0;
        unordered_map<int,int> pre, su;
        vector<int> _pre (found), _su (n - found - 1);
        int ans = 1;
        for (int i = found - 1; i >= 0; i--) {
            _pre[i] = (i == found - 1 ? 0 : _pre[i+1]) + ((nums[i] > k) ? 1 : (nums[i] == k) ? 0 : -1);
            ans += _pre[i] == 0 || _pre[i] == 1 ? 1 : 0;
            pre[_pre[i]]++;
        }
        for (int i = found + 1; i < n; i++) {
            int idx = i - found - 1;
            _su[idx] = (idx == 0 ? 0 : _su[idx-1]) + (nums[i] > k ? 1 : nums[i] == k ? 0 : -1);
            ans += _su[idx] == 0 || _su[idx] == 1 ? 1 : 0;

            su[_su[idx]]++;
        }
        for (auto [value, count] : pre) {
            if (su.count(-value)) {
                ans += count * su[-value];
            }
            if (su.count(-value+1)) {
                ans += count * su[-value+1];
            }
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    inline int sign(int num) {
        if (num == 0) {
            return 0;
        }
        return num > 0 ? 1 : -1;
    }

    int countSubarrays(vector<int>& nums, int k) {
        int n = nums.size();
        int kIndex = -1;
        for (int i = 0; i < n; i++) {
            if (nums[i] == k) {
                kIndex = i;
                break;
            }
        }
        int ans = 0;
        unordered_map<int, int> counts;
        counts[0] = 1;
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += sign(nums[i] - k);
            if (i < kIndex) {
                counts[sum]++;
            } else {
                int prev0 = counts[sum];
                int prev1 = counts[sum - 1];
                ans += prev0 + prev1;
            }
        }
        return ans;
    }
};
```