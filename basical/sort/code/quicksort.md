题目描述：  
![image](/basical/sort/image/image1.png)
解题过程：  
经典快速排序：先随机数分成左右两边排好序，然后排左边和右边  
代码：  
```cpp
class Solution {
public:
    void swap(int& a,int& b) {
        int temp = a;
        a = b;
        b = temp;
    }

    void quickSort(vector<int>& nums,int left,int right) {
        if (left >= right) return;
        int q = randomPartition(nums,left,right);
        quickSort(nums,left,q - 1);
        quickSort(nums,q + 1,right);
    }

    int randomPartition(vector<int>& nums,int left,int right) {
        int q = rand() % (right - left + 1) + left;
        swap(nums[right],nums[q]);
        return partition(nums,left,right);
    }

    int partition(vector<int>&nums,int left,int right) {
        int x = right;
        int i = left - 1, j = left;
        for (; j < right; j++) {
            if (nums[j] <= nums[x]) 
                swap(nums[++i],nums[j]);
        }
        swap(nums[++i],nums[x]);
        return i;
    }

    vector<int> sortArray(vector<int>& nums) {
        quickSort(nums,0,nums.size()-1);
        return nums;
    }
};
```