题目描述：  
![image](/basical/array/image/image55.png)  
解决过程：  
没想出来时间复杂度和空间复杂度满足进阶题目要求的解法  
看了题解，有好几种方法：  
1. 二分查找：
    1. 关注到如果一个数重复出现两次及以上，那么其两侧的元素必然满足出现次数均为1的条件。借此可以设置cnt[i]表示小于等于i的元素的个数，若cnt[i] ≤ i ，则说明在target左侧，否则在target右侧
2. 二进制
    1. 用x表示数组nums中第i位上为1的数的个数，y表示1到n第i位上为1的数的个数。如果target出现次数在两次及以上，那么会满足x > y，此时该ans上的该位可以设置为1
    2. 在对二进制的位数进行判断时，从0开始计数比较合理，题解里面这部分写得不错
3. 快慢指针
    1. 使用索引到值建立了一个映射，因为存在重复的数字，因此根据这个映射构建图之后一定会存在环，根据这个环就可以利用快慢指针来得到环的入口的元素，即重复元素  
代码:（二分查找(超时)→二进制→快慢指针）  
```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n = nums.size();
        int left = 1, right = n - 1, ans = -1;
        while (left <= right) {
            int mid = (left + right) >> 1;
            int cnt = 0;
            for (int i = 0; i < n; i++) {
                cnt += nums[i] <= mid;
            }
            if (cnt <= mid) {
                left++;
            } else {
                ans = mid;
                right--;
            }
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n = nums.size();
        int bit_max = 31;
        while (!((n-1) >> bit_max)) {
            bit_max--;
        }
        int ans = 0;
        for (int bit = 0; bit <= bit_max; bit++) {
            int x = 0, y = 0;
            for (int i = 0; i < n; i++) {
                if (nums[i] & (1 << bit)) x++;
                if (i >= 1 && (i & (1 << bit))) y++;
            }
            if (x > y) ans |= 1 << bit;
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = 0, fast = 0;
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
};
```