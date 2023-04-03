题目描述：  
![image](/basical/array/image/image66.png)  
解决过程：  
没做出来  
- 贪心算法
1. 从后往前遍历，寻找第一个降序的位置，记为元素a
2. 找到降序的位置之后，从后往前遍历，跳过所有大于等于a的元素和与前一个位置相等的元素，然后交换  
代码：  
```cpp
class Solution {
public:
    vector<int> prevPermOpt1(vector<int>& arr) {
        int n = arr.size();
        for (int i = n - 2; i >= 0; i--) {
            if (arr[i] > arr[i+1]) {
                int j = n - 1;
                while (arr[j] >= arr[i] || arr[j] == arr[j-1]) {
                    j--;
                }
                swap(arr[i], arr[j]);
                break;
            }
        }
        return arr;
    }
};
```