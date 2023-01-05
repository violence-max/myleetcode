题目描述：  
![image](/basical/array/image/image31.png)  
非常经典的一道题目，和移动零类似，但是需要进行两次遍历。  
题解给出非常牛逼的常数空间一次遍历解法，都是使用了双指针技术，分别直接排0和1还有0和2  
代码：
```cpp
class Solution {
public:
    int colorswap(vector<int> &nums,int index,int target) {
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == target) {
                swap(nums[index],nums[i]);
                index++;
            }
        }
        return index;
    }

    void sortColors(vector<int>& nums) {
        int len = nums.size();
        int left = colorswap(nums,0,0);
        left = colorswap(nums,left,1);
        left = colorswap(nums,left,2);
    }
};
```
```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n = nums.size();
        int p0 = 0, p1 = 0;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 1) {
                swap(nums[i], nums[p1]);
                ++p1;
            } else if (nums[i] == 0) {
                swap(nums[i], nums[p0]);
                if (p0 < p1) {
                    swap(nums[i], nums[p1]);
                }
                ++p0;
                ++p1;
            }
        }
    }
};
```
```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n = nums.size();
        int p0 = 0, p2 = n - 1;
        for (int i = 0; i <= p2; ++i) {
            while (i <= p2 && nums[i] == 2) {
                swap(nums[i], nums[p2]);
                --p2;
            }
            if (nums[i] == 0) {
                swap(nums[i], nums[p0]);
                ++p0;
            }
        }
    }
};
```