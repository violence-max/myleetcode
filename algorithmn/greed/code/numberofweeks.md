题目描述：

![image](/algorithmn/greed/image/image22.png)

解决过程：

就这种题目居然还想了30分钟，跟某次周赛排AABB是一样的啊

代码：

```cpp
long long sum;
long long mxx;
int n;
multiset<long long> s;
class Solution {
public:
    long long numberOfWeeks(vector<int>& milestones) {
        s.clear();
        n = milestones.size();
        for (auto i : milestones) {
            s.insert((long long)i);
        }
        while (true) {
            sum = 0;
            for (auto it = s.begin(); it != s.end(); ++it) {
                sum += *it;
            }
            cout << sum << endl;
            mxx = *s.rbegin();
            if (mxx > sum - mxx + 1) {
                long long next = sum - mxx + 1;
                s.erase(prev(s.end()));
                s.insert(next);
            } else {
                break;
            }
        }
        return sum;
    }
};
```

```cpp
class Solution {
public:
    long long numberOfWeeks(vector<int>& milestones) {
        // 耗时最长工作所需周数
        long long longest = *max_element(milestones.begin(), milestones.end());
        // 其余工作共计所需周数
        long long rest = accumulate(milestones.begin(), milestones.end(), 0LL) - longest;
        if (longest > rest + 1){
            // 此时无法完成所耗时最长的工作
            return rest * 2 + 1;
        }
        else {
            // 此时可以完成所有工作
            return longest + rest;
        }
    }
};
```