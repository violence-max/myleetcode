题目描述：  
![image1](/basical/sort/image/image1.png)
解题过程：  
经典堆排序：建堆、选择堆，调整堆  
代码：  
```cpp
class Solution {
public:
    void swap(int& a,int& b){
        int temp = a;
        a = b;
        b = temp;
    }

    void heapify(vector<int>& nums,int start,int end) {
        int dad = start,son = 2 * start + 1;
        while (son <= end) {
            son = ((son + 1) <= end && nums[son+1] > nums[son]) ? son + 1 : son;
            if (nums[dad] >= nums[son]) return;
            else {
                swap(nums[dad],nums[son]);
                dad = son;
                son = 2 * dad + 1;
            }
        }
        return;
    }

    void maxHeap(vector<int>& nums) {
        for (int i = (int)nums.size()/2-1; i >= 0; i--){
            heapify(nums,i,(int)nums.size()-1);
        }
    }

    void heapSort(vector<int>& nums) {
        for (int i = (int)nums.size() - 1; i > 0; i--) {
            swap(nums[i],nums[0]);
            heapify(nums,0,i - 1);
        }
    }

    vector<int> sortArray(vector<int>& nums) {
        maxHeap(nums);
        heapSort(nums);
        return nums;
    }
};
```