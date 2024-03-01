题目描述：

![image](/basicaldatastructure/stackandquere/image/image23.png)

解决过程：

想用回溯但是超时了没做出来

- 贪心+优先队列

代码：

```cpp
class Solution {
public:
    vector<int> rearrangeBarcodes(vector<int>& barcodes) {
        unordered_map<int, int> cnt;
        for (auto i : barcodes) {
            cnt[i]++;
        }
        priority_queue<pair<int, int>> pq; // 最大堆，将频率最高的元素排在堆顶，优先选用
        for (const auto& [code, fre] : cnt) {
            pq.push({fre, code});
        }
        vector<int> res;
        while (!pq.empty()) {
            auto [xfre, xcode] = pq.top();
            pq.pop();
            if (res.empty() || res.back() != xcode) {
                res.push_back(xcode);
                if (xfre > 1) {
                    pq.push({xfre - 1, xcode});
                }
            } else {
                auto [yfre, ycode] = pq.top();
                pq.pop();
                res.push_back(ycode);
                if (yfre > 1) {
                    pq.push({yfre - 1, ycode});
                }
                pq.push({xfre, xcode});
            }
        }
        return res;
    }
};
```