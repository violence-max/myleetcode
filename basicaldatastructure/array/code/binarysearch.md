题目描述：  
![image1](/basicaldatastructure/array/image/image1.png)
解题过程：经典思路，不想说了  
代码：（改用c++了）  
```cpp
class Solution {
public:
    int conquer(vector<int>& nums,int target,int left,int right){
        if (left > right){
            return -1;
        }
        int mid = (left + right) / 2;
        if (nums[mid] == target){
            return mid;
        } else if(nums[mid] < target){
            return conquer(nums,target,mid+1,right);
        } else{
            return conquer(nums,target,left,mid-1);
        }
    }

    int search(vector<int>& nums, int target) {
        int length = nums.size();
        return conquer(nums,target,0,length-1);
    }
};
```