# algorithms records of algorithms learning

伟人曾经说过，概念是知识的基石。

### 第一天

算法复杂度

> 1. 时间复杂度：指输入数据大小为 NN 时，算法运行*所需计算操作数量*
>    > 从小到大排序，常数，对数，线性，平方阶，指数，阶乘 O(1)<O(logN)<O(N)<O(NlogN)<O($N^2$)<O($2^N$)<O(N!)
>    > 分别对应，恒定输出，二分法，单轮 for 循环，两层循环独立，递归。
> 2. 空间复杂度：

两数之和

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target 的那 两个 整数，并返回它们的数组下标。

```js
// 官方map方案，约耗时60
var twoSum = function (nums, target) {
  const hashMap = {}
  for (const idx in nums) {
    const current = nums[idx]
    const tag = target - current
    const index = hashMap[tag]
    if (index) {
      return [index, idx]
    } else {
      hashMap[current] = idx
    }
  }
}
// 约耗时160ms
var twoSum = function (nums, target) {
  let result = []
  for (let idx = 0; idx < nums.length; idx++) {
    const index = nums.indexOf(target - nums[idx])
    if (index !== -1 && index !== idx) {
      result = [index, idx]
      break
    }
  }
  return result
}
```

经典二分查找

```js
var search = function (nums, target) {
  let low = 0
  let high = nums.length - 1
  while (low <= high) {
    const midIdx = Math.floor((high - low) / 2) + low
    const num = nums[midIdx]
    if (num === target) {
      return midIdx
    } else if (num > target) {
      high = midIdx - 1
    } else {
      low = midIdx + 1
    }
  }
  return -1
}
```

二分法的应用：第一个错误版本

```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int left = 1;
        int right = n;
        while(left < right) {
            int version = left + (right - left) / 2;
            if(isBadVersion(version)) {
               right = version; // 答案区间[left, version]
            } else {
                left = version + 1; // 答案区间[left + 1, version]
            }
        }
        return left;
    }
}
```

二分法应用：查找插入位置

```java

class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length -1;
        // 超出边界的情况
        if (target < nums[left]) return 0;
        if (target > nums[right]) return right + 1;

        while(left <= right) {
            int idx = (left + right) / 2;
            int num = nums[idx];
            if (target ==  num) {
                return idx;
            } else if (target > num) {
               left += 1;
               // 判断下次是否为最后一次查找
               if (left == right) return right;
            } else {
                right -= 1;
               // 判断下次是否为最后一次查找
                if (left == right) return right;
            }
        }

       return 0;
    }
}
```
