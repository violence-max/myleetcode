题目描述：  
![image](/basical/IQ/image/image12.png)  
解题过程：  
直接手动模拟，然后发现最后一个测试用例超时。而且，在做的过程中忽略了会遇到相同值的情况，但是又使用了哈希表。  
看了题解，评论区直接有个巨牛逼无敌的解法，c++优雅哥  
在code友的题解里面，我发现一个小tip：因为需要动态维护答案，然而答案是对一个比较大的数进行求余，若在某个时刻，求余得到一个非常小的数，然后接下来需要减去一个比较大的数，这时候就会发生得到负数的情况，然而这是不被允许的，所以需要在减去之前加上mod  
代码：（我的（超时）→ code友）  
```cpp
class Solution {
public:
    int GetMIN(unordered_map<int,int> map) {
        int MIN = INT_MAX;
        for (auto m : map) {
            if (m.first < MIN) {
                MIN = m.first;
            }
        }
        return MIN;
    }

    int GetMAX(unordered_map<int,int> map) {
        int MAX = INT_MIN;
        for (auto m : map) {
            if (m.first > MAX) {
                MAX = m.first;
            }
        }
        return MAX;
    }

    int getNumberOfBacklogOrders(vector<vector<int>>& orders) {
        int mod = 1000000000 + 7;
        unordered_map<int,int> buy,sell;
        for (auto &v : orders) {
            if (v[2] == 0) {
                while (v[1] > 0) {
                    int MIN = GetMIN(sell);
                    if (MIN <= v[0]) {
                        if (sell[MIN] >= v[1]) {
                            sell[MIN] -= v[1];
                            v[1] = 0;
                        } else {
                            v[1] -= sell[MIN];
                            sell.erase(MIN);
                        }
                    } else {
                        buy[v[0]] += v[1];
                        break;
                    }
                }
            } else {
                while (v[1] > 0) {
                    int MAX = GetMAX(buy);
                    if (MAX >= v[0]) {
                        if (buy[MAX] >= v[1]) {
                            buy[MAX] -= v[1];
                            v[1] = 0;
                        } else {
                            v[1] -= buy[MAX];
                            buy.erase(MAX);
                        }
                    } else {
                        sell[v[0]] += v[1];
                        break;
                    }
                }
            }
        }
        int ans = 0;
        for (auto se : sell) {
            ans = (ans + se.second) % mod;
        }
        for (auto bu : buy) {
            ans = (ans + bu.second) % mod;
        }
        return ans;
    }
};
```
```cpp
class Solution {
public:
    int getNumberOfBacklogOrders(vector<vector<int>>& orders) {
        priority_queue<pair<int,int>> buy;
        priority_queue sell {greater<>{},vector<pair<int,int>>{} };
        int mod = 1e9 + 7;
        return accumulate(orders.begin(),orders.end(),0,[&](int a,auto &v) {
            auto func = [&](auto &q1,auto &q2,auto &&op) {
                while (v[1] && !q1.empty() && op(q1.top().first,v[0])) {
                    auto [price,cnt] = q1.top();
                    q1.pop();
                    auto tmp = min(cnt,v[1]);
                    v[1] -= tmp;
                    cnt -= tmp;
                    a = (a + mod - tmp) % mod;
                    if (cnt) q1.emplace(price,cnt);
                }
                if (v[1]) q2.emplace(v[0],v[1]);
                return (a + v[1]) % mod;
            };
            return v[2] ? func(buy,sell,greater_equal<>{}) : func(sell,buy,less_equal<>{});
        });
    }
};
```