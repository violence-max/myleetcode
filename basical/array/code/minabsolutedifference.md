题目描述：

![image](/basical/array/image/image99.png)

解决过程：

第358场周赛t3，ac

思考的过程多了冗余的一步：不需要考虑right[i]，因为从左往右遍历的过程中左边的数字就会和遍历到的数字做计算

- lower_bound()表示的是第一个不小于target的数
- set自带的lower_bound和algorithm头文件里的用法不一样

代码：（我的→灵神）

```
class Solution {
public:
    int right[100020], left[100020];
    int minAbsoluteDifference(vector<int>& nums, int x) {
        set<int> s;
        int n = nums.size(), ptr = n - 1;
        for (int i = n - 1; i >= 0; --i) {
            if (n - 1 - i < x) {
                right[i] = INT_MAX;
                continue;
            }
            
            // cout << "prepare to insert:" << ' ' << nums[ptr] << endl;
            s.insert(nums[ptr--]);
            auto high = s.upper_bound(nums[i]);
            // auto low = s.lower_bound(nums[i]);
            int h = high == s.end() ? INT_MAX : *high;
            int l = high == s.end() ? *s.rbegin(): high == s.begin() ? INT_MIN / 2 : *(--high);
            // cout << i << ' ' << h << ' ' << l << endl;
            right[i] = min(h - nums[i], nums[i] - l);
            // cout << i << ' ' << nums[i] << ' ' << right[i] <<endl;
        }
        s.clear();
        ptr = 0;
        int ret = INT_MAX;
        for (int i = 0; i < n; ++i) {
            if (i < x) {
                left[i] = INT_MAX;
            } else {
                s.insert(nums[ptr++]);
                auto high = s.upper_bound(nums[i]);
                // auto low = s.lower_bound(nums[i]);
                int h = high == s.end() ? INT_MAX : *high;
                int l = high == s.end() ? *s.rbegin(): high == s.begin() ? INT_MIN / 2 : *(--high);
                left[i] = min(h - nums[i], nums[i] - l);
            }
            
            ret = min(ret, min(left[i], right[i]));
        }
        return ret;
    }
};
```

```cpp
class Solution {
public:
    int minAbsoluteDifference(vector<int> &nums, int x) {
        int ans = INT_MAX, n = nums.size();
        set<int> s = {INT_MIN / 2, INT_MAX}; // 哨兵
        for (int i = x; i < n; i++) {
            s.insert(nums[i - x]);
            int y = nums[i];
            auto it = s.lower_bound(y); // 注意用 set 自带的 lower_bound，具体见视频中的解析
            ans = min(ans, min(*it - y, y - *prev(it))); // 注意不能写 *--it，这是未定义行为：万一先执行了 --it，前面的 *it-y 就错了
        }
        return ans;
    }
};
```