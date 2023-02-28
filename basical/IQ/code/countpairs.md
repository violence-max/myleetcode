题目描述：  
![image](/basical/IQ/image/image10.png)  
解题过程：题解以一种非常秀的方式解决了这道题目，我真的是被秀得头皮发麻，字典树！！！我实在是非常难描述这个字典树算法，但是我还是尽力去描述一下吧：  
1. 字典树来描述01字符串，从根节点开始，向左走表示遇到0，向右走表示遇到1
2. 面对数组中序号为i的数（从1开始计数），当进行判断有多少个数和这个序号为i的数进行异或在low和high的范围内时必须先把其之前的一个数加入字典树中
3. 把一个数加入字典树中需要将该数经历过的路径的标记加1
4. 在进行枚举判断一个数之前有几个数能与其相异或在范围内时，若x正在判断的第k位二进制数为1，则说明该数能与当前节点上的数进行相异或，其值小于x；若为0，则该数继续向前走；若不能走，则返回答案，否则，循环结束后遇到异或相等的数  
代码：  
暴力：  
```cpp
class Solution {
public:
    int countPairs(vector<int>& nums, int low, int high) {
        int count = 0,m = nums.size();
        for (int i = 0; i < m; i++) {
            for (int j = i + 1; j < m; j++) {
                int beauti = nums[i] ^ nums[j];
                if (low <= beauti && beauti <= high) {
                    count++;
                }
            }
        }
        return count;
    }
};
```
```cpp
struct Trie {
    array<Trie*, 2> son{nullptr, nullptr};
    int sum;
    Trie():sum(0) {}
};

class Solution {
private:
    Trie* root = nullptr;
    static constexpr int HIGH_BIT = 14;

public:
    void add(int num) {
        Trie* cur = root;
        for (int k = HIGH_BIT; k >= 0; k--) {
            int bit = (num >> k) & 1;
            if (cur->son[bit] == nullptr) {
                cur->son[bit] = new Trie();
            }
            cur = cur->son[bit];
            cur->sum++;
        }
    }

    int get(int num, int x) {
        Trie* cur = root;
        int sum = 0;
        for (int k = HIGH_BIT; k >= 0; k--) {
            int r = (num >> k) & 1;
            if ((x >> k) & 1) {
                if (cur->son[r] != nullptr) { 
                    sum += cur->son[r]->sum;
                }
                if (cur->son[r ^ 1] == nullptr) {
                    return sum;
                }
                cur = cur->son[r ^ 1];
            } else {
                if (cur->son[r] == nullptr) {
                    return sum;
                }
                cur = cur->son[r];
            }
        }
        sum += cur->sum;
        return sum;
    }

    int f(vector<int>& nums, int x) {
        root = new Trie();
        int res = 0;
        for (int i = 1; i < nums.size(); i++) {
            add(nums[i - 1]);
            res += get(nums[i], x);
        }
        return res;
    }

    int countPairs(vector<int>& nums, int low, int high) {
        return f(nums, high) - f(nums, low - 1);
    }
};
```