题目描述：  
![image](/basical/array/image/image16.png)  
解题过程：  
1. 排序
2. 固定左指针，右指针设置为最后一个元素，中间元素从左指针下一个元素开始循环
3. 每次计算三个指针的元素之和sum，如果sum大于target就往左移右指针
4. 左指针的选定和每次进行sum和的判断之前对于中间元素的选定需要跳过重复元素
* 亮点：
1. 排序：对于这些寻找target值之类的题目，升序数组更有力量
2. 选定左指针，对于中间指针进行循环。算法中只有sum之和大于target的时候才右移右指针，如果sum小于target，则直接进入下一次循环，即中间指针往后取下一个值
代码：  
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>> result;
        int length = nums.size();
        for (int i = 0; i < length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int right = length - 1;
            for (int left = i + 1; left < length - 1; left++) {
                if (left > i + 1 && nums[left] == nums[left - 1]) {
                    continue;
                }
                int sum = nums[i] + nums[left] + nums[right];
                while (left < right && sum > 0) {
                    sum = nums[i] + nums[left] + nums[--right];                    
                }
                if (right == left) break;
                else if (sum == 0) {
                    result.push_back({nums[i],nums[left],nums[right]});
                }
            }
        }
        return result;
    }
};
```