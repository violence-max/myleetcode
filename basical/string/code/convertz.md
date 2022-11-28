题目描述：  
![Untitled](/basical/string/image/image13.png)    
解题过程：  
笑死，又是根本不会。 
看了题解，三种方法，矩阵模拟、压缩矩阵空间，直接构造，都来尝试一下吧。  
1. 矩阵模拟：  
    矩阵模拟讲究合理计算矩阵的横纵坐标。这个题类似周期的题目，因为是z字形，可以把z字除了最后一个字以外的其余部分视作一个周期，把整个字符串视作具有完整的周期，即即使有最后不满一个周期的部分也记作一个周期数。然后对字符串中的所有字符做遍历，若当前计数模周期小于要求的z的横向长度，则横坐标增一，否则横坐标减一且纵坐标加一。（应该叫行和列才对，小丑了）  
    复现这个代码的时候还遇到了三个困难：  
    1. 初始化矩阵，不知道怎么构造一个长度为n的字符串。其实直接构造就可以了，即定义变量之后加个括号
    2. 除零。sb地把`i % t` 写成了 `t % i`   
    3. 忽略初始条件。一开始忘记加上z字形长度等于1的判断条件了  
    学了一个向上取整处理情况：`len + t - 1 / t` 取的是len / t的向上取整部分  
    代码：  
    ```cpp
    class Solution {
    public:
        string convert(string s, int numRows) {
            int len = s.size();
            if (len < numRows || len < 2 || numRows == 1) return s;
            int t = 2 * numRows - 2;
            int c = (len + t - 1) / t * (numRows - 1);
            vector<string> matrix (numRows,string(c,0));
            for (int i = 0,x = 0,y = 0; i < len; i++) {
                matrix[x][y] = s[i];
                if (i % t < numRows - 1) x++;
                else {
                    x--; y++;
                }
            }
            string ans;
            for (auto str : matrix) {
                for (auto c : str) {
                    if (c) {
                        ans += c;
                    }
                }
            }
            return ans;
        }
    };
    ```
2. 压缩矩阵空间：  
    矩阵模拟浪费了很多存储空间，这个方法直接寂寞地对矩阵里面的每个字符串进行增长操作，忽略了列数。只需要字符串中的字符编号和周期模一下判断就可以决定下一个要处理的字符串的序号了，无敌。  
    代码：  
    ```cpp
    class Solution {
    public:
        string convert(string s, int numRows) {
            int len = s.size();
            if (len < 2 || numRows == 1 || len < numRows) return s;
            int t = 2 * numRows - 2;
            vector<string> matrix (numRows);
            for (int i = 0,x = 0; i < len; i++) {
                matrix[x] += s[i];
                x = (i % t < numRows - 1) ? x+1 : x-1;
            }
            string ans;
            for (auto str : matrix) {
                ans += str;
            }
            return ans;
        }
    };
    ```  
3. 直接构造：  
    这个方法直接把空间复杂度降到了O(1)，我都无语了，太寂寞了这个方法。  
    直接拿捏住字符串构造的规律，对每一行进行循环，每一行再对第一个字符开始做循环，每次自增一个周期，除了第0行和最后一行以外，一个周期里面可以增加两个数，而第一个数的序号模周期刚好就是序号本身，第二个数模周期得到周期减去序号，类似于反码吧，两个数之和刚好就是周期。  
    我觉得这种方法还是有点难想，下次遇到不一定会，毕竟是找规律，还是要花一点时间的，除非我是天才。尤其是在处理循环和判断条件上，这个写法真的太简洁了，而且很清楚。  
    代码：  
    ```cpp
    class Solution {
    public:
        string convert(string s, int numRows) {
            int len = s.size();
            if (len < 2 || numRows == 1 || len < numRows) return s;
            int t = 2 * numRows - 2;
            string ans;
            for (int i = 0; i < numRows; i++) {
                for (int j = 0; j + i < len; j += t) {
                    ans += s[j+i];
                    if (i > 0 && i < numRows - 1 && j + t - i < len) {
                        ans += s[j+t-i];
                    }
                }
            }
            return ans;
        }
    };
    ```