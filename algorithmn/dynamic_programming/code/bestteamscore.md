题目描述：
![image](/algorithmn/dynamic_programming/image/image56.png)  
```cpp
class Solution {
public:
    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        int n = scores.size();
        vector<pair<int, int>> people;
        for (int i = 0; i < n; ++i) {
            people.push_back({scores[i], ages[i]});
        }
        sort(people.begin(), people.end());
        vector<int> dp(n, 0);
        int res = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i - 1; j >= 0; --j) {
                if (people[j].second <= people[i].second) {
                    dp[i] = max(dp[i], dp[j]);
                }
            }
            dp[i] += people[i].first;
            res = max(res, dp[i]);
        }
        return res;
    }
};
```  
解决过程：  
没做出来：（这里的动态规划是以遍历到的球员为最终的球员队列中的最后一个球员，而我做的是第一个球员）  
代码：（动态规划→线段树）  
```cpp
class Solution {
public:
    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        int n = scores.size();
        vector<pair<int, int>> people;
        for (int i = 0; i < n; ++i) {
            people.push_back({scores[i], ages[i]});
        }
        sort(people.begin(), people.end());
        vector<int> dp(n, 0);
        int res = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i - 1; j >= 0; --j) {
                if (people[j].second <= people[i].second) {
                    dp[i] = max(dp[i], dp[j]);
                }
            }
            dp[i] += people[i].first;
            res = max(res, dp[i]);
        }
        return res;
    }
};
```  
```cpp
class Solution {
public:
    int max_age;
    vector<int> t;
    vector<pair<int, int>> people;
    int lowbit(int x) {
        return x & (-x);
    }

    void update(int i, int val) {
        for (; i <= max_age; i += lowbit(i)) {
            t[i] = max(t[i], val);
        }
    }

    int query(int i) {
        int ret = 0;
        for (; i > 0; i -= lowbit(i)) {
            ret = max(ret, t[i]);
        }
        return ret;
    }

    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        max_age = *max_element(ages.begin(), ages.end());
        t = vector<int>(max_age + 1, 0);
        int res = 0;
        for (int i = 0; i < scores.size(); ++i) {
            people.push_back({scores[i], ages[i]});
        }
        sort(people.begin(), people.end());

        for (int i = 0; i < people.size(); ++i) {
            int score = people[i].first, age = people[i].second, cur = score + query(age);
            update(age, cur);
            res = max(res, cur);
        }
        return res;
    }
};
```