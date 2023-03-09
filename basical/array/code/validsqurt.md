题目描述：  
![image1](/basical/array/image/image6.png)
解题过程：
* 二分法
    * 注意两数相乘的时候加long进行类型强转
```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {
        int left = 0,right = num;
        int mid;
        while (left <= right) {
            mid = (left + right) >> 1;
            if ((long)mid * mid == num) {
                return true;
            } else if ((long)mid * mid < num) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return false;
    }
};
```