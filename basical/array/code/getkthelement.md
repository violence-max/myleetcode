题目描述：  
![image](/basical/array/image/image18.png)  
解决过程：  
本来一直想分割数组来着，就是题解的第二种方法，但是没尝试出来，感觉应该是一个递归，但实际上，能用递归做出来的划分问题应该用循环也可以做出来，所以下次要注意了。  
用的方法是二分，寻找第k大的数，简直强无敌，包括特殊情况的处理的实现，因为特殊情况可能有好几种，而实现它的方法居然借助一个起始索引给组织起来了？太寂寞了。  
代码：  
```cpp
class Solution {
public:
    int getKthElement(vector<int> &nums1,vector<int> &nums2,int k) {
        int m = nums1.size(),n = nums2.size();
        int index1 = 0,index2 = 0;
        while (true) {
            if (index1 == m) {
                return nums2[index2+k-1];
            }
            if (index2 == n) {
                return nums1[index1+k-1];
            }
            if (k == 1) {
                return min(nums1[index1],nums2[index2]);
            }
            int newIndex1 = min(index1+k/2-1,m-1);
            int newIndex2 = min(index2+k/2-1,n-1);
            if (nums1[newIndex1] <= nums2[newIndex2]) {
                k -= newIndex1 - index1 + 1;
                index1 = newIndex1 + 1;
            } else {
                k -= newIndex2 - index2 + 1;
                index2 = newIndex2 + 1;
            }
        }
    }
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int length = nums1.size() + nums2.size();
        if (length & 1) {
            return getKthElement(nums1,nums2,(length+1)/2);
        } else {
            return (getKthElement(nums1,nums2,length/2) + getKthElement(nums1,nums2,length/2 + 1)) / 2.0;
        }
    }
};
```