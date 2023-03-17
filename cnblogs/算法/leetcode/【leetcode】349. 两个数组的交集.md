## 349. 两个数组的交集

[leetcode题目传送门](https://leetcode.cn/problems/intersection-of-two-arrays/)



给定两个数组 nums1 和 nums2 ，返回 它们的交集 。输出结果中的每个元素一定是 唯一 的。我们可以 不考虑输出结果的顺序 。

示例 1：

    ```
    输入：nums1 = [1,2,2,1], nums2 = [2,2]
    输出：[2]
    ```

示例 2：

    ```
    输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
    输出：[9,4]
    解释：[4,9] 也是可通过的
    ```


### 实现

    分别遍历数组去重放入list（数组需要先知道长度）

* linq一行代码搞定

```c#
public class Solution {
    public int[] Intersection(int[] nums1, int[] nums2) {
        return nums1.Intersect(nums2).ToArray();
    }
}
```

提交之后发现

![image](https://img2023.cnblogs.com/blog/999484/202303/999484-20230316212117618-2140947451.png)


用时和内存都不太理想

自己来写数组遍历试试

```c#
public class Solution {
    public int[] Intersection(int[] nums1, int[] nums2) {
        List<int> result=new List<int>();
        for(int i=0;i<nums1.Count();i++){
            if(nums2.Contains(nums1[i])&&!result.Contains(nums1[i])){
                result.Add(nums1[i]);
            }
        }
        return result.ToArray();
    }
}
```

![image](https://img2023.cnblogs.com/blog/999484/202303/999484-20230316215826996-1775076851.png)



参考示例代码


```c#
public class Solution {
    public int[] Intersection(int[] nums1, int[] nums2) {

            Array.Sort(nums1);
            Array.Sort(nums2);
            int[] res = new int[nums1.Length];

            int i = 0, j = 0;
            int k = -1;
            while(i<nums1.Length && j < nums2.Length)
            {
                if (nums1[i] < nums2[j])
                    i++;
                else if (nums1[i] > nums2[j])
                    j++;
                else
                {
                    if ( (k>-1 && res[k] != nums1[i]) || k==-1)
                    {
                        k++;
                        res[k] = nums1[i];
                    }
                    i++;
                    j++;
                }
            }
            int[] ans = new int[k + 1];
            for (int h = 0; h < k + 1; h++)
                ans[h] = res[h];
            return ans;
    }
}
```

数组排序，然后用两个指针分别遍历数组，如果两个指针指向的元素相等 就是其中一个交集，否则比较两个指针指向的元素的大小，较小的向前移动

![image](https://img2023.cnblogs.com/blog/999484/202303/999484-20230316231739964-1204062421.png)

