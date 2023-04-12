题目描述：  
![image](/basical/array/image/image68.png)  
解题过程：  
没做出来  
- 双指针：
1. j表示数组arr从后往前遍历到的最后一个大于等于前一个数字的索引
2. i从0开始，先找到使得`arr[j] ≥ arr[i]` 的位置j，然后尝试删除中间的子数组获取最小长度，如果i当前的字符大小大于下一个（i+1）字符的大小，那么就可以直接退出了
3. 关键思想在于：因为删除的是一个连续的子数组，所以可能删除三个部分：头部、尾部和中间的部分  
代码：  
```cpp
class Solution {
public:
    int findLengthOfShortestSubarray(vector<int>& arr) {
        int n = arr.size();
        int j = n - 1;
        while (j > 0 && arr[j-1] <= arr[j]) {
            j--;
        }
        if (j == 0) return 0;
        int res = j;
        for (int i = 0; i < n; i++) {
            while (j < n && arr[j] < arr[i]) {
                j++;
            }
            res = min(res, j - i - 1);
            if (i + 1 < n && arr[i] > arr[i+1]) {
                break;
            }
        }
        return res;
    }
};
```