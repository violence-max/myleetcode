题目描述：  
![image](/basical/array/image/image56.png)  
解题思路：  
写得比较久，但是最后还是做出来了。主要思想是两个相同的数那道题，把每个数放到自己的位子上，最后遍历一次数组，如果索引和值对不上就可以找到缺失的数了  
题解的鸽笼原理：  
- 将遍历到的每个数num，使其对应位置上的值加上n，代表数组中有这个值。之后遍历一次数组，如果某个位置没有被遍历到，则其值不会大于n，就找到了缺失的值。值得注意的是，当遍历到数num时，其对应位置上的值有可能已经加上了n，因此需要用(num - 1) % n来得到对应的位置  
代码：（两数交换→鸽笼原理）  
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> ans;
        int n = nums.size();
        int i = 0;
        while (i < n) {
            if (nums[i] != i + 1) {
                while (nums[i] != i + 1) {
                    if (nums[nums[i] - 1] == nums[i]) {
                        break;
                    } else swap(nums[nums[i] - 1], nums[i]);
                }
            } 
            i++;
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
                ans.push_back(i + 1);
            }
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int n = nums.size();
        for (auto& num : nums) {
            int x = (num - 1) % n;
            nums[x] += n;
        }
        vector<int> ret;
        for (int i = 0; i < n; i++) {
            if (nums[i] <= n) {
                ret.push_back(i + 1);
            }
        }
        return ret;
    }
};
```