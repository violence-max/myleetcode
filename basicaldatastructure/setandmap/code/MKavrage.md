题目描述：  
![image](/basicaldatastructure/setandmap/image/image1.png)  
解决过程：  
做了三个小时，包括看题解的时间。一开始想的使用双向链表和优先队列解决问题，然后超时，不管怎么优化都是超时。因为当m比较大时，用优先队列计算平均值的时间复杂度是O(m)，而当需要计算比较多次平均值时就无了。但是没有想到怎么用双向链表和优先队列维护k个最小值和最大值。题解给的答案是用multiset，我的红黑树啊，我居然没想到，绝了。一切细节见代码  
代码：  
```cpp
class MKAverage {
private:
    int m,k;
    long long sum;
    multiset<int> s1,s2,s3;
    queue<int> q;
public:
    MKAverage(int m, int k) : m(m),k(k) {
        sum = 0;
    }
    
    void addElement(int num) {
        q.push(num);
        if (q.size() <= m) {
            s2.insert(num);
            sum += num;
            if (q.size() == m) {
                for (int i = 0; i < k; i++) {
                    s1.insert(*s2.begin());
                    s3.insert(*s2.rbegin());
                    sum -= *s2.begin() + *s2.rbegin();
                    s2.erase(s2.begin());
                    s2.erase(prev(s2.end()));
                }
            }
            return;
        }

        if (num < *s1.rbegin()) {
            s1.insert(num);
            s2.insert(*s1.rbegin());
            sum += *s1.rbegin();
            s1.erase(prev(s1.end()));
        } else if (num > *s3.begin()) {
            s3.insert(num);
            s2.insert(*s3.begin());
            sum += *s3.begin();
            s3.erase(s3.begin());
        } else {
            s2.insert(num);
            sum += num;
        }

        int x = q.front();
        q.pop();
        if (s1.count(x) > 0) {
            s1.erase(s1.find(x));
            s1.insert(*s2.begin());
            sum -= *s2.begin();
            s2.erase(s2.begin());
        } else if (s3.count(x) > 0) {
            s3.erase(s3.find(x));
            s3.insert(*s2.rbegin());
            sum -= *s2.rbegin();
            s2.erase(prev(s2.end()));
        } else {
            sum -= x;
            s2.erase(s2.find(x));
        }
    }
    
    int calculateMKAverage() {
        if (q.size() < m) return -1;
        return sum / (m - 2 * k);
    }
};

/**
 * Your MKAverage object will be instantiated and called as such:
 * MKAverage* obj = new MKAverage(m, k);
 * obj->addElement(num);
 * int param_2 = obj->calculateMKAverage();
 */
```