题目描述：  
![image](/basicaldatastructure/setandmap/image/image11.png)  
7分钟解决  
代码：  
```cpp
class Solution {
public:
    int mostFrequentEven(vector<int>& nums) {
        unordered_map<int,int> map;
        int res = -1, max = 0;
        for (int i = 0; i < nums.size(); i++) {
            if ((nums[i] & 1) == 0) {
                int cnt = ++map[nums[i]];
                if (cnt > max || (cnt == max && res > nums[i])) {
                    res = nums[i];
                    max = cnt;
                }
                // cout << i << ' ' << cnt << ' ' << nums[i] << ' ' << res << endl;
            }
        }   
        return res;
    }
};
```