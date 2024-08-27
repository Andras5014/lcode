# [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)





**给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。**

**子数组是数组中元素的连续非空序列。**

```markdown
示例 1：

输入：nums = [1,1,1], k = 2
输出：2
```



思路

```markdown
引入前缀和
前缀和：nums 的第 0 项到 当前项 的和。
定义 prefixSum 数组，prefixSum[x]：第 0 项到 第 x 项 的和。
prefixSum[x]=nums[0]+nums[1]+…+nums[x]
nums 的某项 = 两个相邻前缀和的差：
nums[x]=prefixSum[x]−prefixSum[x−1]
nums 的 第 i 到 j 项 的和，有：
nums[i]+…+nums[j]=prefixSum[j]−prefixSum[i−1]
当 i 为 0，此时 i-1 为 -1，我们故意让 prefixSum[-1] 为 0，使得通式在i=0时也成立：
nums[0]+…+nums[j]=prefixSum[j]
```

根据题意 我们需要nums[i]+…+nums[j]=prefixSum[j]−prefixSum[i−1]，

prefixSum[j]−prefixSum[i−1]==k

我们利用一个map结构，key:前缀和，val：出现次数来存储

边存边查看 map，如果 map 中存在 key 为「当前前缀和 - k」，说明这个之前出现的前缀和，满足「当前前缀和 - 该前缀和 == k」，它出现的次数，累加给 count。



```go
func subarraySum(nums []int, k int) int {
    count := 0
    hashMap := make(map[int]int,0)
    preSum :=0
    hashMap[0]=1
    for _,num:=range nums {
        preSum += num
        if hashMap[preSum-k]>0{
            count +=hashMap[preSum-k]
        }
        hashMap[preSum]++
    }
    return count
}
```

