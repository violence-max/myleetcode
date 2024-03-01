题目描述：

![image](/algorithmn/greed/image/image23.png)

解决过程：

写了一会

代码：

```cpp
class Solution {
public:
    vector<long long> maximumEvenSplit(long long finalSum) {
        if (finalSum & 1) {
            return {};
        }

        unordered_set<long long> cnt;
        vector<long long> ans;
        int s = 2;
        function<bool(long long, int)> check = [&](long long sum, int split) -> bool {
            if ((sum - split) > split && !cnt.count(sum - split) && !cnt.count(split)) {
                cnt.insert(split);
                return true;
            } else {
                return false;
            }
        };
        while (check(finalSum, s)) {
            ans.push_back(s);
            finalSum -= s;
            // cout << s << ' ' << finalSum << endl;
            s += 2;
        }
        ans.push_back(finalSum);
        return ans;
    }
};
```