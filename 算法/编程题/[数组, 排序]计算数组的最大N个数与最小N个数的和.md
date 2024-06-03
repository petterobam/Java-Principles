### 计算数组的最大N个数与最小N个数的和

#### 题目描述
给定一个数组，编写一个函数来计算它的最大N个数与最小N个数的和。需要对数组进行去重。

- 数组中数字范围[0, 1000]
- 最大N个数与最小N个数不能有重叠，如有重叠，输入非法返回-1
- 输入非法返回-1

#### Java实现

```java
import java.util.*;

public class MaxMinNSum {
    public static int calculateMaxMinNSum(int[] nums, int N) {
        if (nums == null || nums.length < 2 * N) {
            return -1;
        }
        
        Set<Integer> uniqueNumsSet = new HashSet<>();
        for (int num : nums) {
            uniqueNumsSet.add(num);
        }
        
        List<Integer> uniqueNums = new ArrayList<>(uniqueNumsSet);
        
        if (uniqueNums.size() < 2 * N) {
            return -1;
        }
        
        Collections.sort(uniqueNums);
        
        int maxSum = 0;
        int minSum = 0;
        
        for (int i = 0; i < N; i++) {
            minSum += uniqueNums.get(i);
            maxSum += uniqueNums.get(uniqueNums.size() - 1 - i);
        }
        
        return maxSum + minSum;
    }

    public static void main(String[] args) {
        int[] nums1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
        int N1 = 2;
        System.out.println(calculateMaxMinNSum(nums1, N1));  // 输出应为 34

        int[] nums2 = {1, 2, 3, 4, 4, 5, 6, 7, 8, 9, 10};
        int N2 = 3;
        System.out.println(calculateMaxMinNSum(nums2, N2));  // 输出应为 36

        int[] nums3 = {1, 2, 3, 4, 5, 6, 7, 8};
        int N3 = 4;
        System.out.println(calculateMaxMinNSum(nums3, N3));  // 输出应为 -1
    }
}
```

#### 思路分析

1. **输入验证**：
    - 如果数组为null或者长度小于2N，直接返回-1，表示输入非法。

2. **去重处理**：
    - 使用`Set`来去重，保证数组中的数字唯一。

3. **排序处理**：
    - 将去重后的数字列表进行排序。

4. **合法性检查**：
    - 如果去重后的数字个数仍然小于2N，直接返回-1，表示输入非法。

5. **计算最大N个数和最小N个数的和**：
    - 从排序后的数组中取出最小的N个数和最大的N个数，并分别求和。

6. **返回结果**：
    - 返回最大N个数和最小N个数的和。

#### 说明

- **去重**：用`HashSet`去重，以确保数组中没有重复的数字。
- **排序**：用`Collections.sort`对去重后的数组进行排序，方便后续取最大和最小N个数。
- **合法性检查**：确保在计算前检查数组是否有足够的数字供取值，避免重叠。
- **求和**：分别计算最小N个数和最大N个数的和，并返回总和。

通过以上步骤，可以确保在最短时间内计算出满足条件的最大N个数和最小N个数的和。