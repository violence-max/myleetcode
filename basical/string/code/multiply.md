题目描述：  
![image](/basical/string/image/image25.png)  
解题过程： 
看了一遍根本不知道是怎么回事，又不能转换成字符串，就算转换成字符串直接相乘还有可能溢出，让我很难受。  
看了答案的解法，我感觉就像是模拟，但是其中有一些操作字符串的经典手法：比如说翻转，以及加入整数然后每一位加上**’0’**使得字符串变为整数字符串（当然直接凭借整数转换为字符串的字符串也可以）  
方法一：做加法 
通过模拟从低位开始乘，但是要在除了最后一位之外的所有位加上前导零，算出的字符串与答案相加  
方法二：通过数组存储结果（做乘法） 
直接从低位开始乘并且把结果存储于数组中，之后从后往前遍历数组解决进位问题，这里能够提前知道数组的最大长度是基于一个理论：m位整数和n位整数相乘的结果的位数位m+n-1位或者m+n位（当然我觉得用不断添加的方法也可以）  
代码： 
```cpp
class Solution {
    public String multiply(String num1, String num2) {
        if (num1.equals("0") || num2.equals("0")) {
            return "0";
        }
        String ans = "0";
        int m = num1.length(), n = num2.length();
        for (int i = n - 1; i >= 0; i--) {
            StringBuffer curr = new StringBuffer();
            int add = 0;
            for (int j = n - 1; j > i; j--) {
                curr.append(0);
            }
            int y = num2.charAt(i) - '0';
            for (int j = m - 1; j >= 0; j--) {
                int x = num1.charAt(j) - '0';
                int product = x * y + add;
                curr.append(product % 10);
                add = product / 10;
            }
            if (add != 0) {
                curr.append(add % 10);
            }
            ans = addStrings(ans, curr.reverse().toString());
        }
        return ans;
    }

    public String addStrings(String num1, String num2) {
        int i = num1.length() - 1, j = num2.length() - 1, add = 0;
        StringBuffer ans = new StringBuffer();
        while (i >= 0 || j >= 0 || add != 0) {
            int x = i >= 0 ? num1.charAt(i) - '0' : 0;
            int y = j >= 0 ? num2.charAt(j) - '0' : 0;
            int result = x + y + add;
            ans.append(result % 10);
            add = result / 10;
            i--;
            j--;
        }
        ans.reverse();
        return ans.toString();
    }
}
```  
```cpp
class Solution {
public:
    string multiply(string num1, string num2) {
        if (num1 == "0" || num2 == "0")
            return "0";
        int m = (int)num1.size(),n = (int)num2.size();
        vector<int> arr (m+n);
        for (int i = n - 1; i >= 0; i--) {
            int y = num2.at(i) - '0';
            for (int j = m - 1; j >= 0; j--) {
                int x = num1.at(j) - '0';
                int product = x * y;
                arr[i+j+1] += product;
            }
        }
        for (int i = m + n - 1; i > 0; i--) {
            arr[i-1] += arr[i] / 10;
            arr[i] = arr[i] % 10;
        }
        int index = arr[0] == 0 ? 1 : 0;
        string ans;
        while (index < m + n) {
            ans.push_back(arr[index++]);
        }   
        for (auto &c : ans) {
            c += '0';
        }
        return ans;
    }
};
```