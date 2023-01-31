题目描述：  
![image](/basical/array/image/image48.png)  
解决过程：  
10分钟ac。一开始想的是二分查找，空间复杂度满足要求。看了题解，我去，双指针强无敌，而且还外加了证明，很厉害  
代码：（二分查找→双指针）  
```cpp
class Solution {
public:
    int search(const vector<int>& numbers, int left, int right, int target) {
        while (left <= right) {
            int mid = (left + right) >> 1;
            if (numbers[mid] == target) return mid;
            else if (numbers[mid] > target) right = mid - 1;
            else left = mid + 1;
        }
        return -1;
    }

    vector<int> twoSum(vector<int>& numbers, int target) {
        int n = numbers.size();
        for (int i = 0; i < n; i++) {
            int _target = target - numbers[i];
            int mid = search(numbers,i+1,n-1,_target);
            if (mid != -1) return {i+1,mid+1};
        }
        return {-1,-1};
    }
};
```
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int low = 0, high = numbers.size() - 1;
        while (low < high) {
            int sum = numbers[low] + numbers[high];
            if (sum == target) {
                return {low + 1, high + 1};
            } else if (sum < target) {
                ++low;
            } else {
                --high;
            }
        }
        return {-1, -1};
    }
};
```