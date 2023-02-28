题目描述：  
![image](/basical/IQ/image/image8.png)  
解题过程：  
很考验智商的一个题目，我觉得如果空间想象能力不够好这个题目根本做不来，因为不能发现这个题目的规律。首先，根据贪心的思想，要想把接触地面的立方体的个数最小化，只能把整个立方体组合起来的物体依靠某个角落放置。  
这个题目的规律有以下几个要点：  
1. 第i层最多能增加i个接触地面的立方体
2. 在前i-1层放满的情况下，第i层每增加一个接触地面的立方体就可以多放一个额外的立方体，比如：第3层，前2层放满后总的立方体个数为4，若增加一个接触地面的立方体，则可以多放一个，总数加1变成5，再加一个，又可以多放一个，总数加2，变成7，再加一个，又可以多放一个，总数加3，变成10。  
因此，这个题目可以通过找规律的方式解出：  
1. n逐渐减去每一层能够添加的立方体总数，来确定当需要放置的立方体总数为n时可以放置到哪一层
2. 剩余的n逐渐减去最高层每放置一个接触地面的立方体所带来的能够放置的立方体的总的个数，来确定最高层可以放置多少个接触地面的立方体  
总的能够接触地面的立方体的个数为前i-1层放满能够放置的接触地面的立方体的个数（1~i-1的累加）和第2步计算出来的答案的和。  
代码：  
找规律：  
```cpp
class Solution {
public:
    int minimumBoxes(int n) {
        int cur = 1,i = 1,j = 1;
        while (n > cur) {
            n -= cur;
            i++;
            cur += i;
        }
        cur = 1;
        while (n > cur) {
            n -= cur;
            j++;
            cur++;
        }
        return (i - 1) * i / 2 + j;
    }
};
```  
二分查找：（关于第i层放满总共能放多少个盒子的方程不太熟悉具体的证明过程）  
```cpp
class Solution {
public:
    int minimumBoxes(int n) {
        int i = 0, j = 0;
        int low = 1, high = min(n, 100000);
        while (low < high) {
            int mid = (low + high) >> 1;
            long long sum = (long long) mid * (mid + 1) * (mid + 2) / 6;
            if (sum >= n) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        i = low;
        n -= (long long) (i - 1) * i * (i + 1) / 6;
        low = 1, high = i;
        while (low < high) {
            int mid = (low + high) >> 1;
            long long sum = (long long) mid * (mid + 1) / 2;
            if (sum >= n) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        j = low;
        return (i - 1) * i / 2 + j;
    }
};
```  
解方程：（把前两个方法的最小的i和j都通过方程给计算出来了）  
```cpp
class Solution {
public:
    int g(int x) {
        return (long long) x * (x + 1) * (x + 2) / 6;
    }

    int minimumBoxes(int n) {
        int i = pow(6.0 * n, 1.0 / 3);
        if (g(i) < n) {
            i++;
        }
        n -= g(i - 1);
        int j = ceil(1.0 * (sqrt((long long) 8 * n + 1) - 1) / 2);
        return (i - 1) * i / 2 + j;
    }
};
```