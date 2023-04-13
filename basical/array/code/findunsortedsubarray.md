题目描述：  
![image](/basical/array/image/image58.png)  
解题过程：  
没做出来，看了题解：  
- 排序：将数组分成numsA,numsB,numsC三个部分，其中numsB是需要进行升序排序的最短子数组。因此可以将排序过后的数组nums和原来的nums做比较得到前缀数组numsA和后缀数组numsC。
    - 学到了is_sorted()的用法
- 一次遍历：基本思想基于无序数组中从左到右一次遍历拿到的最大值可以确定其有序数组的右边界，从右到左一次遍历拿到的最小值可以确定其有序数组的左边界  
代码：（排序→一次遍历）  
```cpp
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        if (is_sorted(nums.begin(), nums.end())) {
            return 0;
        }
        vector<int> numsSorted(nums);
        sort(numsSorted.begin(), numsSorted.end());
        int left = 0;
        while (nums[left] == numsSorted[left]) {
            left++;
        }
        int right = nums.size() - 1;
        while (nums[right] == numsSorted[right]) {
            right--;
        }
        return right - left + 1;
    }
};
```  
```cpp
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        int n = nums.size();
        int maxn = INT_MIN, right = -1;
        int minn = INT_MAX, left = n;
        for (int i = 0; i < n; i++) {
            if (maxn > nums[i]) {
                right = i;
            } else {
                maxn = nums[i];
            }
            if (minn < nums[n -i - 1]) {
                left = n - i -1;
            } else {
                minn = nums[n - i - 1];
            }
        }
        return right == -1 ? 0 : right - left + 1;
    }
};
```