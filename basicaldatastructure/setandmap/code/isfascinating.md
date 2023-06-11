题目描述：

![image](/basicaldatastructure/setandmap/image/image13.png)

解决过程：
第107场双周赛t1，5分钟，错了一次，忽略了不包含0这个条件

代码：

```cpp
class Solution {
public:
    bool isFascinating(int n) {
        int n2 = n * 2;
        int n3 = n * 3;
        unordered_set<int> cnt;
        function<bool(int)> get = [&](int num) -> bool {
            while (num > 0) {
                int val = num % 10;
                if (cnt.count(val) || val == 0) {
                    return false;
                }
                cnt.insert(val);
                num = num / 10;
            }  
            return true;
        };
        return get(n) && get(n2) && get(n3);
    }
};
```