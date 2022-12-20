题目描述：  
![image](/basical/IQ/image/image5.png) 
解题过程：  
直接把c++STL代码库的源码搬上来当题目也是牛逼，看了题解我才发现我居然对题目没有完全理解，我以为的是好像是逐渐把最大数往前挪的下一个排列，没有想到字典序居然是是这个样子的—永远看靠前的数字或者字符。  
单调栈是不行的，题解是寂寞的。从后往前查找第一个不单调减的数字，再从后往前找第一个比这个数字要大的数字，交换之后对较小数之后的数字进行一个升序排序，完美！  
代码：  
```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int n = nums.size();
        int i = n - 2;
        while (i >= 0 && nums[i] >= nums[i+1])
            i--;
        if (i >= 0) {
            int j = n - 1;
            while (nums[i] >= nums[j]) 
                j--;
            swap(nums[i],nums[j]);
        }
        reverse(nums.begin()+i+1,nums.end());
    }
};
```