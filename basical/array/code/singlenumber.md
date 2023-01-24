题目描述：  
![image](/basical/array/image/image41.png)  
线性复杂度下空间复杂度还要求是常量的，除了哈希表我真的想不出来空间复杂度使用更小的数据结构或者方法了  
操，位运算，异或满足交换律和结合律，很强  
代码：（哈希表→位运算）  
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_set<int> set;
        for (int& i : nums) {
            if (set.count(i)) {
                set.erase(i);
            } else set.insert(i);
        }
        return *set.begin();
    }
};
```
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ret = 0;
        for (int& i : nums) ret ^= i;
        return ret;
    }
};
```