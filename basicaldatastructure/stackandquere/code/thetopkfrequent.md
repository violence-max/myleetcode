题目描述：  
![image](/basicaldatastructure/stackandquere/image/image9.png)  
解题过程：  
这个题，一开始以为可以用暴力法，但是他说进阶版的时间复杂度必须优于O(nlog(n))，我想的暴力法是先获取一个一一对应的哈希表，然后根据出现的频次进行一个排序，获取前k个高频元素，这里的排序就要O(nlog(n))的时间复杂度了。问题在于，就算获取了前k个频次，也需要一个由频次映射到元素的哈希表来把这些元素重新装入数组里面，但是！！！！频次是可能一样的！！！！也就是说，如果由两个不同的数的频次相同并且被排序到了前k个频次里，那么根本就没有一个哈希表可以得到由频次到数据的一个对应，这个对应关系会在赋值的时候因为集合的互异性被覆盖掉。  
看了题解，发现了堆的方法。总体思想是在堆的大小还没有到k的时候直接把元素到频次的键值对装入堆中，堆根据元素的频次进行排序，频次最小的元素在堆顶。当堆的大小达到k时，先比较堆顶元素的频次和当前元素的频次，如果当前元素频次更大，那么就把堆顶元素弹出，然后加入当前元素。最后获得大小为k的堆，这个堆里的元素即为前k个高频元素。  
这个题解又用到了优先队列，因为优先队列的本质其实就是堆。但是这里为了根据元素的频次进行排序，自己写了一个compare函数。默认情况下，优先队列的排序函数是less，也就是从大到小排序，而且如果优先队列中的数据是pair键值对，是按照pair的第一个元素进行排序。这个compare函数写得很精辟，可以多看几次。  
代码：  
```cpp
class Solution {
public:
    static bool cmp(pair<int,int> p1,pair<int,int> p2) {
        return p1.second > p2.second;
    }
    vector<int> topKFrequent(vector<int>& nums, int k) {
        if (k == nums.size()) return nums;
        unordered_map<int,int> map;
        for (auto i : nums) {
            ++map[i];
        }
        priority_queue<pair<int,int>,vector<pair<int,int>>,decltype(&cmp)> q(cmp);
        for (auto [num,count] : map) {
            if (q.size() == k) {
                if (q.top().second < count) {
                    q.pop();
                    q.emplace(num,count);
                }
            } else {
                q.emplace(num,count);
            }
        }

        vector<int> res;
        while (!q.empty()) {
            res.push_back(q.top().first);
            q.pop();
        }
        return res;
    } 
};
```