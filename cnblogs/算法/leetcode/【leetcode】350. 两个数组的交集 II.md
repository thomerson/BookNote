## 两个数组的交集 II

[leetcode题目传送门](https://leetcode.cn/problems/intersection-of-two-arrays-ii/)

给你两个整数数组 nums1 和 nums2 ，请你以数组形式返回两数组的交集。返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序。

 

示例 1：

    输入：nums1 = [1,2,2,1], nums2 = [2,2]
    输出：[2,2]

示例 2:

    输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
    输出：[4,9]


### 实现

和349不同的是可以出现重复元素，并且出现重复次数一致

例如

    in :[1,1,1,2,3],[1,1,2,2]
    out:[1,1,2]

双指针遍历时，相等时两个指针都移动，不需要判断结果数组重复

```c#
public class Solution {
    public int[] Intersect(int[] nums1, int[] nums2) {
        Array.Sort(nums1);
        Array.Sort(nums2);

        int i=0,j=0;

        List<int> result = new List<int>();

        int[] temp=new int[nums1.Length];
        while(i<nums1.Length&&j<nums2.Length){
            if(nums1[i]>nums2[j]){
                j++;
            }else if(nums1[i]<nums2[j]){
                i++;
            }else{
                result.Add(nums1[i]);
                i++;
                j++;
            }
        }

       
        return result.ToArray();

    }
}

```

![image](https://img2023.cnblogs.com/blog/999484/202303/999484-20230317104252234-1707316492.png)


参考实例代码

```c#
public class Solution {
    public int[] Intersect(int[] nums1, int[] nums2) {
        Array.Sort(nums1);
        Array.Sort(nums2);
        int upper = 0;
        int lower = 0;
        List<int> hs = new List<int>();
        while (upper < nums1.Length && lower < nums2.Length) {
            if (nums1[upper] < nums2[lower]) {
                upper++;
            } else if (nums1[upper] > nums2[lower]) {
                lower++;
            } else {
                hs.Add(nums1[upper]);
                upper++;
                lower++;
            }
        }
        return hs.ToArray();
    }
}

```

