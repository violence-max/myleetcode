题目描述：  
![image1](/sort/image/image1.png)
解题过程：  
简单的归并排序，注意递归语句的调用，以及用额外空间来保存排好序的值的思想  
代码：  
```cpp
class Solution {
    vector<int> temp;
    void mergeSort(vector<int>& nums,int left,int right) {
        if (left >= right)
            return;
        int mid = (left + right) >> 1;
        mergeSort(nums,left,mid);
        mergeSort(nums,mid + 1,right);
        int i = left,j = mid + 1;
        int count = 0;
        while (i <= mid && j <= right) {
            if (nums[i] <= nums[j]) 
                temp[count++] = nums[i++];
            else
                temp[count++] = nums[j++];
        }
        while (i <= mid) 
            temp[count++] = nums[i++];
        while (j <= right)
            temp[count++] = nums[j++];
        for (int k = 0; k < right - left + 1; k++)
            nums[k + left] = temp[k];
    }
public:
    vector<int> sortArray(vector<int>& nums) {
        temp.resize(nums.size(),0);
        mergeSort(nums,0,nums.size()-1);
        return nums;
    }
};
```