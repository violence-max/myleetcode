题目描述：

![image](/basical/array/code/maximumtastniess.md)

解题过程：

没做出来

- 二分查找

代码：（二分查找）

```cpp
class Solution {
public:
    // 检查是否存在甜蜜度大于等于tastiness的可能
    bool check(const vector<int>& price, int k, int tastiness) {
        // 排序后的price数组的第一个数字一定是最小价格，因此这里
        // prev:之前选中的最小价格取int的最小值使得price数组的第一个
        // 价格可以被选中
        int prev = INT_MIN >> 1;

        // 记录选中的糖果的种类数，用于结果判断(即相邻之差大于等于tastiness)
        int cnt = 0;

        for (int i : price) {
            if (i - prev >= tastiness) {
                cnt++;
                prev = i;
            }
        }

        return cnt >= k; /*恰好存在k个相邻差大于等于tastiness的糖果种类*/
    }

    int maximumTastiness(vector<int>& price, int k) {
         int n = price.size();
         sort(price.begin(), price.end());
         // 甜蜜度的取值范围
         int left = 0, right = price[n-1] - price[0];
         while (left < right) {
             // 使用left记录答案，如果left == right，说明甜蜜度为left
             
             // 计算中点更加准确（对于n为奇数而言）
             int mid = (left + right + 1) >> 1;
             if (check(price, k, mid)) {
                 left = mid;
             } else {
                 right = mid - 1;
             }
         }
         return left;
    }
};
```