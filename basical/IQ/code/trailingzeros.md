题目描述：  
![image](/basical/IQ/image/image21.png)  
解题过程：  
本来以为不会，玩了会手机就会了，不过我统计的是10、5、2的个数。看了题解，确实只需要统计5的个数，然后就出现了进阶算法：质因数个数算法，代码很很简当，就不盘逻辑了，要点是质因数5的个数一定小于质因数2的个数  
代码：  
```cpp
class Solution {
public:
    void excute(int& cnt_2, int& cnt_5, int& cnt_10, int target) {
        while (target) {
            if (target % 10 == 0) {
                cnt_10++;
                target /= 10;
            } else if (target % 5 == 0) {
                cnt_5++;
                target /= 5;
            } else if (target % 2 == 0) {
                cnt_2++;
                target /= 2;
            } else return;
        }
    }

    int trailingZeroes(int n) {
        int cnt_2 = 0, cnt_5 = 0, cnt_10 = 0;
        while (n) {
            excute(cnt_2,cnt_5,cnt_10,n);
            n--;
        }
        return cnt_10 + min(cnt_2,cnt_5);
    }
};
```