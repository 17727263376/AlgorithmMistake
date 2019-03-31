**题目描述：**

给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

**例如1：**
- - -
nums1 = [1, 3]

nums2 = [2]

则中位数是 2.0

**例如2：**
- - -
nums1 = [1, 2]

nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
- - -
**思路1：**
求两个有序数组的中位数，因为两个数组是有序的，只需要把两个数组合并成一个有序的数组，然后求中位数就可以直接求出
```
static const auto _=[](){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    return nullptr;
}();
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector<int> s(100000);
        if(nums1.size() == 0 && nums2.size() == 0) return 0;
        int a = 0, b = 0, c = 0;
        while(a < nums1.size() && b < nums2.size()){
            if(nums1[a] < nums2[b]){
                s[c++] = nums1[a];
                a++;
            }
            else{
                s[c++] = nums2[b];
                b++;
            }
        }
        while(a < nums1.size()){
            s[c++] = nums1[a];
            a++;
        }
        while(b < nums2.size()){
            s[c++] = nums2[b];
            b++;
        }
        if(c%2 != 0)    return s[c/2]*1.0;
        else            return (s[(c-1)/2]+s[c/2])/2.0;
    }
};
```

**但是**，后来看到了别的大佬用了短短几行就写出来了
```
vector<int> nums3(n3z); 
merge(nums1.begin(),nums1.end(),nums2.begin(),nums2.end(),nums3.begin());
```
于是我去查了merge函数，发现它可以实现将两个有序数组合并为一个新的有序数组，牛逼！！

**思路2：**
递归法：待补充！！