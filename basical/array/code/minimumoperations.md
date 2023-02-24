题目描述：  
![image](/basical/array/image/image53.png)  
解决过程：  
模拟就直接解决了  
看了题解，发现有多少个非零数就需要多少次操作，因此直接用哈希表就可以解决了  
代码：（模拟→哈希表）  
```cpp
class Solution {
public:
    int getMIN (vector<int>& nums) {
        int res = INT_MAX;
        for (auto i : nums) {
            if (i != 0) res = min(res,i);
        }
        return res;
    }
    int minimumOperations(vector<int>& nums) {
        auto check = [&]() {
            return any_of(nums.begin(),nums.end(),[&](int a) {return a != 0;});
        };

        int cnt = 0;
        while (check()) {
            int MIN = getMIN(nums);
            cnt++;
            for (auto& i : nums) {
                i = max(0,i - MIN);
            }
        }

        return cnt;
    }
};
```  
```cpp
class Solution {
public:
    int minimumOperations(vector<int>& nums) {
        unordered_set<int> hashSet;
        for (int num : nums) {
            if (num > 0) {
                hashSet.emplace(num);
            }
        }
        return hashSet.size();
    }
};
```