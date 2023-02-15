题目描述：  
![image](/basical/IQ/image/image24.png)  
由于写过水壶问题，因此这个问题对于我来说相当简单，5分钟之内ac  
代码：  
```cpp
class Solution {
public:
    bool isGoodArray(vector<int>& nums) {
        int n = nums.size();
        int firstnum = nums[0];
        for (int i = 1; i < n; i++) {
            int next = gcd(firstnum,nums[i]);
            if (next == 1) return true;
            else {
                firstnum = next;
            }
        }
        return firstnum == 1;
    }
};
```