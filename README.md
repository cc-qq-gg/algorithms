# algorithms records of algorithms learning

伟人曾经说过，概念是知识的基石。

### 第一天

算法复杂度

> 1. 时间复杂度：指输入数据大小为 NN 时，算法运行*所需计算操作数量*
>    > 从小到大排序，常数，对数，线性，平方阶，指数 `O(1)<O(logN)<O(N)<O(NlogN)<O(N 2)<O(2N)<O(N!)`
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
    const mid = Math.floor((high - low) / 2) + low
    const num = nums[mid]
    if (num === target) {
      return mid
    } else if (num > target) {
      high = mid - 1
    } else {
      low = mid + 1
    }
  }
  return -1
}
```
