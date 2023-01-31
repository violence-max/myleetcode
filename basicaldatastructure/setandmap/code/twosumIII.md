题目描述：  
![image](/basicaldatastructure/setandmap/image/image6.png)  
解决过程：  
因为替补说了调用add和find的次数为10^4，因此如果有一个函数的时间复杂度达到了O(n)，那么当数据大起来时根本不行，因此我一直在寻找一种很快速的方法，到最后发现找不到，看了题解，题解也没有，还是传统的哈希和双指针+排序，很无聊。但是用哈希写这道题的时候很有趣，我忽略的两个相同的数加起来得到目标值的情况和溢出  
代码：  
```cpp
class TwoSum {
private:
    unordered_map<long,int> cnt;
public:
    TwoSum() {
    }
    
    void add(int number) {
        ++cnt[number];
    }
    
    bool find(int value) {
        for (auto& [_value, freq] : cnt) {
            long _another_value = value - _value;
            if (cnt.count(_another_value)) {
                if (_another_value != _value || (_another_value == _value && freq > 1))
                    return true;
            }
        }
        return false;
    }
};
```