题目描述：
![image3](/basicaldatastructure/array/image/image3.png)
解题过程：  
在做这个题的时候我一直在想，之前在做二分查找的时候搜索到的到底都是对应元素的哪一个位置，因为很明显，一个排序数组中如果有重复元素，那些重复元素是连在一起的，如果是简单的二分查找，找到对应的元素的随机性太大了，包括插入索引位置那一题，应该是寻找同一个元素在数组中最后一个位置的下一个位置的索引，这都是我之前做的两个题没有考虑到的地方。  
因为本题要求一个开始索引一个结尾索引嘛，我一开始想到的还是从开始遍历和从后面遍历（笑了），但是这样用不到二分法，可是二分法还要确定一开始位置和结束位置的，这样的东西我打不出来啊，所以我就看了题解。  
看了题解我发现太巧妙了：  
1.  查找元素在数组中第一个位置的索引（如果存在）：
只需要在中间值大于等于target目标值的时候更改右指针就好了，然后将结果记为此时的中间值（`mid`）。直到左右指针相等的时候，右指针又一次发生改变，下次挑出循环，但是此时结果已经为中间值而且被记录下来了  
2.  查找元素在数组中最后一个位置的索引（如果存在）：
只需要在中间值小于等于target目标值的时候更改左指针就可以了，跟前一种情况类似，始终保持结果在目标值的右侧，查找的结果为想要的索引值再加一  
**注意：**  
对于空数组的情况特殊处理的方法：再binarySearch函数中将result值设为数组的大小（返回之后再减一是-1，小于left（0），所以直接返回{-1,-1}）  
代码：  
```cpp
class Solution {
public:
    int binarySearch(vector<int>& nums,int target,bool lower) {
        int result = nums.size(),left = 0,right = nums.size() - 1;
        int mid;
        while (left <= right) {
            mid = ((right - left) >> 1) + left;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
                result = mid;
            } else {
                left = mid + 1;
            }
        }
        return result;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        int leftIdx = binarySearch(nums,target,true);
        int rightIdx = binarySearch(nums,target,false) - 1;
        if (leftIdx <= rightIdx && nums[leftIdx] == target && nums[rightIdx] == target)
            return vector<int> {leftIdx,rightIdx};
        return vector<int> {-1,-1};
    }
};
```