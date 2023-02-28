题目描述：  
![image](/basical/array/image/image34.png)  
解决过程：  
我是直接移动数组，看了题解的快慢指针，我真的是弟弟  
题解的快慢指针：  
1. 若数组长度小于等于2，则无需处理，直接返回数组长度即可
2. 满指针slow和快指针fast起初赋值为2，若nums[slow-2] ≠ nums[fast]，则说明当前检查的元素可以被添加道数组中，直接将快指针指向的值赋值给慢指针的位置上，然后向前移动慢指针，每次都要向前移动快指针  
代码（我的→题解）：  
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int left = 0,i = 0,n = nums.size();
        while (i < n) {
            int cnt = 1;
            int j = i + 1;
            while (j < n && nums[j] == nums[i]) {
                cnt++;
                if (cnt <= 2) left++;
                j++;
            }
            if (j == n) return (left + 1);
            if (cnt > 2) {
                left++;
                int k = left;
                while (j < n) {
                    swap(nums[k++],nums[j++]);
                }
                n = k;
                i = left;
            } else {
                i = j;
                left = j;
            }
        }
        return left + 1;
    }
};
```  
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if (n <= 2) return n;
        int slow = 2, fast = 2;
        while (fast < n) {
            if (nums[slow-2] != nums[fast]) {
                nums[slow] = nums[fast];
                slow++;
            }
            fast++;
        }
        return slow;
    }
};
```