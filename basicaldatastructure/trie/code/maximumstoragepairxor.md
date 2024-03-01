题目描述：

![image](/basicaldatastructure/trie/image/image5.png)

解决过程：

第371场周赛t4，没做出来

排序+字典树+滑动窗口解决问题

代码：（字典树→哈希表）

```cpp
class Node {
public:
    // 异或字典树节点
    array<Node*, 2> child {};
    int cnt; // 子树的个数
};

const int u = 19;

class Trie {
    Node* root = nullptr;
public:
    Trie() {
        root = new Node();
    }

    void insert(int val) {
        Node* cur = root;
        for (int i = u; i >= 0; i--) {
            int bit = (val >> i) & 1;
            if (cur->child[bit] == nullptr) {
                cur->child[bit] = new Node();
            }
            cur = cur->child[bit];
            cur->cnt++;
        }
    }

    void remove(int val) {
        // 移除val
        Node* cur = root;
        for (int i = u; i >= 0; i--) {
            int bit = (val >> i) & 1;
            cur = cur->child[bit];
            cur->cnt--; // 即使子树个数减为0也不销毁节点
        }
    }

    int max_xor(int val) {
        Node* cur = root;
        int ans = 0;
        for (int i = u; i >= 0; i--) {
            int bit = (val >> i) & 1;
            if (cur->child[bit ^ 1] && cur->child[bit ^ 1]->cnt) {
                ans = ans | (1 << i);
                bit = bit ^ 1;
            }
            cur = cur->child[bit];
        }
        return ans;
    }
};

class Solution {
public:
    int maximumStrongPairXor(vector<int>& nums) {
        sort(nums.begin(), nums.end()); // nums的顺序对结果无影响，排序后可以对于不等式消除绝对值
        Trie t = Trie();
        int ans = INT_MIN, left = 0;
        for (int y : nums) {
            t.insert(y); // 可以与自己组成强数对
            while (nums[left] * 2 < y) {
                // 滑动窗口清楚字典树中不符合强书堆构造规则的元素
                t.remove(nums[left++]);
            }
            ans = max(ans, t.max_xor(y));
        }
        return ans;
    }
};
```

```cpp
class Solution {
public:
    int maximumStrongPairXor(vector<int> &nums) {
        sort(nums.begin(), nums.end());
        int high_bit = 31 - __builtin_clz(nums.back());

        int ans = 0, mask = 0;
        unordered_map<int, int> mp;
        for (int i = high_bit; i >= 0; i--) { // 从最高位开始枚举
            mp.clear();
            mask |= 1 << i;
            int new_ans = ans | (1 << i); // 这个比特位可以是 1 吗？
            for (int y: nums) {
                int mask_y = y & mask; // 低于 i 的比特位置为 0
                auto it = mp.find(new_ans ^ mask_y);
                if (it != mp.end() && it->second * 2 >= y) {
                    ans = new_ans; // 这个比特位可以是 1
                    break;
                }
                mp[mask_y] = y;
            }
        }
        return ans;
    }
};
```