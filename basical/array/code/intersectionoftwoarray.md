题目描述：  
![image](/basical/array/image/image13.png)  
解题过程：直接使用哈希表解决，先是对两个数组进行建表，然后对其中一个表进行循环，用第三个表存储元素，最后根据第三个表的大小来静态分配数组大小，最后进行一个数组的赋值。很慢这种方法，看了题解发现有各种各样的写法，但是源头来讲都是哈希表，有只使用一个哈希表的，有一行语句不用写内置函数就构建了一个哈希表的，有先排序然后用指针来写不用哈希表的等等。我用的是`unordered_map` ，但是这个题肯定是用`unordered_set` 好一点，因为不需要对元素技术，也就是说不需要键值对，只需要一个互异的集合。  
这个题还是学了点东西的，比如打破`unordered_map` 的舒适区进入`unordered_set` ，c++数组向量的直接赋值和从最后取值等。  
代码：  
```cpp
class Solution {
public:
    unordered_map<int,int> initMap(vector<int> &nums) {
        unordered_map<int,int> map;
        for (auto &n : nums) {
            map[n]++;
        }
        return map;
    }
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int,int> nums1Map = initMap(nums1),nums2Map = initMap(nums2),intersecMap;
        for (auto it : nums1Map) {
            if (nums2Map[it.first] > 0) {
                intersecMap[it.first]++;
            }
        }
        int size = intersecMap.size();
        vector<int> intersecVec(size,0);
        int cnt = 0;
        for (auto it : intersecMap) {
            intersecVec[cnt++] = it.first;
        }
        return intersecVec;
```