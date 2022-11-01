题目描述：  
![image1](/basical/array/image/image4.png)
解题过程：  
这个是做过的题目，很简单，就是把每个位置上的元素放回到原来的位置，如果原来的位置已经存在了该元素则说明该元素是重复的，然后返回就行了  
代码：  
```cpp
class Solution {
public:
    void swap(int& a,int& b){
        int temp = a;
        a = b;
        b = temp;
    }

    int findRepeatNumber(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i++) {
            while (nums[i] != i) {
                if (nums[i] != nums[nums[i]]) 
                    swap(nums[i],nums[nums[i]]);
                else 
                    return nums[i];
            }
        }
        return -1;
    }
};
```