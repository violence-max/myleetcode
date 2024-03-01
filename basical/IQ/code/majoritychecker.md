题目描述：  
![image](/basical/IQ/image/image36.png)  
解题过程：  
没做出来  
- 随机化+二分查找
- 摩尔投票+线段树  
代码：（随机化+二分查找）  
```cpp
class MajorityChecker {
public:
    MajorityChecker(vector<int>& arr): arr(arr) {
        for (int i = 0; i < arr.size(); ++i) {
            // 记录数字到该数字在整个arr数组的相同数字中的位置的映射
            loc[arr[i]].push_back(i);
        }
    }
    
    int query(int left, int right, int threshold) {
        int length = right - left + 1;
        uniform_int_distribution<int> dis(left, right);

        // 对于绝对众数而言，我们认为每次选择会有1/2的概率挑选到（因为绝对众数的出现次数严格大于length / 2），因此在k次足够随机的选择里没有选择到绝对众数的概率只有1 - (1/2)^k
        for (int i = 0; i < k; ++i) {
            int x = arr[dis(gen)];
            vector<int>& pos = loc[x];
            // 类似于前缀和数组的用法
            int occ = upper_bound(pos.begin(), pos.end(), right) - lower_bound(pos.begin(), pos.end(), left);
            if (occ >= threshold) {
                return x;
            }
            else if (occ * 2 >= length) {
                // 众数没有超过threshold说明没有其它数可以超过了
                return -1;
            }
        }

        return -1;
    }

private:
    static constexpr int k = 20;

    const vector<int>& arr;
    unordered_map<int, vector<int>> loc;
    mt19937 gen{random_device{}()};
};
```