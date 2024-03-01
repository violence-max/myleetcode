题目描述：

![image](/basical/array/image/image90.png)

解题过程：

不超过5分钟

- 二分查找
- 双指针

代码：（二分查找→双指针）

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int ans = -1;
        int n = numbers.size();
        int right = n - 1;
        for (int i = 0; i < n - 1; ++i) {
            int left = i + 1;
            int x = numbers[i];
            while (left <= right) {
                int mid = (left + right) >> 1;
                if (numbers[mid] <= target - x) {
                    left = mid + 1;
                    ans = mid;
                } else {
                    right = mid - 1;
                }
            }
            if (x + numbers[ans] == target) {
                return {i + 1, ans + 1};
            }
        }
        return {};
    }
};
```

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int ans = -1;
        int n = numbers.size();
        int i = 0, j = n - 1;
        while (numbers[i] + numbers[j] != target) {
            if (numbers[i] + numbers[j] > target) {
                --j;
            } else if (numbers[i] + numbers[j] < target) {
                ++i;
            }
        }
        return {i + 1, j + 1};
    }
};
```