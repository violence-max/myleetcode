题目描述：  
![image](/basical/array/image/image32.png)  
解题过程：  
不太会，看了题解，居然是二分查找，逐一判断中间值作为最大值是否ok，首先这个思想很6，其次判断三个部分和的做法也很秀（尤其是在处理整数越界的部分）  
代码：  
```cpp
class Solution {
public:
    bool valid(int mid,int n,int index,int maxsum) {
        int left = index;
        int right = n - index - 1;
        return mid + cal(mid,left) + cal(mid,right) <= maxsum;
    }

    long cal(int big,int length) {
        if (length <= big - 1) {
            int small = big - length;
            return (long)(big - 1 + small) * length / 2;
        } else {
            int ones = length - (big - 1);
            return (long)(big) * (big - 1) / 2 + ones;
        }
    }

    int maxValue(int n, int index, int maxSum) {
        int left = 1,right = maxSum;
        int ans = 1;
        while (left <= right) {
            int mid = (left + right) >> 1;
            if (valid(mid,n,index,maxSum)) {
                ans = mid;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return ans;
    }
};
```