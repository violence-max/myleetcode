题目描述：  
![image](/basical/array/image/image42.png)  
解题过程：  
自己是不会写了，没想出来，一直在想同或运算和异或运算，没有想到数组求和和单独的位数上进行0和1的判断之类的  
代码：  
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (int i = 0; i < 32; i++) {
            int total = 0;
            for (int& num : nums) {
                total += (num >> i) & 1;
            }
            if (total % 3) {
                ans = ans | (1 << i);
            }
        }
        return ans;
    }
};
```