题目描述：  
![image](/algorithmn/greed/image/image9.png)  
解题过程：  
我草了，这么简单的题目我竟然花了半个小时。主要是没搞清楚应该怎么找零，思路大概：只需要考虑总金额→居然还要找零？那用一个哈希表记录，这回稳了→不知道哪里来的报错，先把总金额去掉吧，只考虑找零就行→20的情况下居然有两种找零方式，无敌了我。  
这道题可以用空间复杂度更小的办法来做，可是哈希表代码更加短小精悍一点。不对，本来哈希表也没有用O(n)的空间复杂度啊，就存储三个值而已。  
代码：  
```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        unordered_map<int,int> map;
        for (int i = 0; i < bills.size(); i++) {
            if (bills[i] > 5) {
                if (bills[i] == 10) {
                    if (!map[5]) return false;
                    else map[5]--;
                } else {
                    if (!map[10]) {
                        if (map[5] < 3) return false;
                        else map[5] -= 3;
                    } else {
                        if (!map[5]) return false;
                        else {
                            map[5]--;
                            map[10]--;
                        }
                    }
                }
            }
            map[bills[i]]++;
        }
        return true;
    }
};
```