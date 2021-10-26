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
    const midIdx = Math.floor((high + low) / 2)
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
            int version = (right + left) / 2;
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

// 这种更简洁，巧妙
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length -1;
        // 超出边界的情况
        if (target < nums[left]) return 0;
        if (target > nums[right]) return right + 1;
        // left == right 时进行最后一次运算，
        // 倒数第二次运算时， left = right - 1
        while(left <= right) {
            int idx = (left + right) / 2;
            int num = nums[idx];

            if (target > num) {
               left = idx + 1;
            } else {
                right = idx - 1;
            }
        }
      // 只能返回left，巧妙
       return left;
    }
}
```

双指针查找：有序数组的平方

> 一个递增的整数数组，返回每个数字的平方组成的递增数组

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
       int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0, j = n - 1, pos = n - 1; i <= j; ) {
            int _i = nums[i] * nums[i];
            int _j = nums[j] * nums[j];
            if (_i > _j) {
                ans[pos] = _i;
                i++;
            } else {
                ans[pos] = _j;
                j--;
            }
            pos--;
        }
        return ans;
    }
}
```

旋转数组

> java 版，提交测试的时间消耗居然是 0

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int len = nums.length;
        k %= len; // 取余数，以适应超出数组长度的情况
        if (k == 0) return;
        int[] start = Arrays.copyOfRange(nums, len - k, len);
        int[] end = Arrays.copyOf(nums, len - k);
        // 拼接
        System.arraycopy(start, 0, nums, 0, start.length);
        System.arraycopy(end, 0, nums, start.length, end.length);
    }
}
```

> js 版，和 java 版内存消耗差不多，但耗时 100ms 左右

```js
var rotate = function (nums, k) {
  k = k % nums.length
  const temp = nums.slice(nums.length - k).concat(nums.slice(0, nums.length - k))
  nums.splice(0, nums.length, ...temp)
}

// 数组翻转
let reverse = function (nums, start, end) {
  while (start < end) {
    ;[nums[start++], nums[end--]] = [nums[end], nums[start]]
  }
}
let rotate = function (nums, k) {
  k %= nums.length
  reverse(nums, 0, nums.length - 1)
  reverse(nums, 0, k - 1)
  reverse(nums, k, nums.length - 1)
  return nums
}
```

双指针：移动零

```js
// 第一个版本，性能较低300ms以上，应该是数组方法的原因
// 删除0，并补零
var moveZeroes = function (nums) {
  const n = nums.length
  for (let i = 0, j = n - 1; i <= j; i++) {
    if (nums[i] === 0) {
      nums.splice(i, 1)
      nums.push(0)
      i--
      j++
    }
  }
}
// 88ms
var moveZeroes = function (nums) {
  const n = nums.length
  let left = 0
  for (let right = 0; right < n; right++) {
    // left 指向处理完的列最后
    if (nums[right] !== 0) {
      ;[nums[left], nums[right]] = [nums[right], num[left]]
      left++
    }
  }
}
```

双指针版两数之和

> 这种方法不会漏掉的可能的值，应该是因为最大和最小的值分别在结尾和开头的两个数

```js
var twoSum = function (numbers, target) {
  let low = 0
  let high = numbers.length - 1
  while (low < high) {
    const sum = numbers[low] + numbers[high]
    if (sum === target) {
      return [low + 1, high + 1]
    }
    if (sum < target) {
      low++
    } else {
      high--
    }
  }
  return [-1, -1]
}
```

双指针：反转字符串

```js
var reverseString = function (s) {
  let i = 0
  let j = s.length - 1
  while (i < j) {
    // 1.用temp暂存
    const temp = s[i]
    s[i] = s[j]
    s[j] = temp
    // 2.数组结构复制
    // [s[i], s[j]] = [s[j], s[i]]
    i++
    j--
  }
}
```

反转字符串中的单词

```js
var reverseWords = function (s) {
  return s
    .split(' ')
    .map(i => i.split('').reverse().join(''))
    .join(' ')
}

// todo 。。。
var reverseWords = function (s) {
  const ret = []
  const length = s.length
  let i = 0
  while (i < length) {
    let start = i
    // 找到单词边界
    while (i < length && s[i] != ' ') {
      i++
    }
    // 反向填充找到的单词
    for (let p = start; p < i; p++) {
      ret.push(s[start + i - 1 - p])
    }
    // 判断边界，添加空格
    while (i < length && s[i] == ' ') {
      i++
      ret.push(' ')
    }
  }
  return ret.join('')
}
```

链表的中间结点 ？

双指针：无重复字符的最长子串

```js
// 460ms
var lengthOfLongestSubstring = function (s) {
  let n = 0
  let t = 0
  let arr = []
  let max = 0
  while (t < s.length) {
    if (!arr.includes(s[t])) {
      arr.push(s[t++])
    } else {
      n++
      t = n + 1
      max = Math.max(arr.length, max)
      arr = [s[n]]
    }
  }
  return Math.max(arr.length, max)
}
// 230ms 性能更好
var lengthOfLongestSubstring = function (s) {
  let n = 0
  let t = 0
  let arr = new Set()
  let max = 0
  while (t < s.length) {
    if (!arr.has(s[t])) {
      arr.add(s[t++])
    } else {
      n++
      t = n + 1
      max = Math.max(arr.size, max)
      arr.clear()
      arr.add(s[n])
    }
  }
  return Math.max(arr.size, max)
}
// 100ms 滑动窗口
var lengthOfLongestSubstring = function (s) {
  // 哈希集合，记录每个字符是否出现过
  const occ = new Set()
  const n = s.length
  // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
  let rk = 0
  let ans = 0
  for (let i = 0; i < n; i++) {
    if (i != 0) {
      // 左指针向右移动一格，移除一个字符
      occ.delete(s[i - 1])
    }
    while (rk < n && !occ.has(s[rk])) {
      // 不断地移动右指针
      occ.add(s[rk])
      rk++
    }
    // 第 i 到 rk 个字符是一个极长的无重复字符子串
    ans = Math.max(ans, rk - i)
  }
  return ans
}
```

双指针，滑动窗口：字符串的排列

```js
// 性能极差 7505ms
var checkInclusion = function (s1, s2) {
  const target = Array.from(s1).sort().toString()
  let n2 = s2.length
  let n1 = s1.length
  let r = n1
  let temp = Array.from(s2.slice(0, n1))
  while (r <= n2) {
    if (temp.slice().sort().toString() === target) {
      return true
    }
    temp.shift()
    temp.push(s2[r++])
  }
  return false
}
// 160ms，统计字母个数
var checkInclusion = function (s1, s2) {
  const n = s1.length,
    m = s2.length
  if (n > m) {
    return false
  }
  const cnt1 = new Array(26).fill(0)
  const cnt2 = new Array(26).fill(0)
  const aCharCode = 'a'.charCodeAt()
  for (let i = 0; i < n; ++i) {
    ++cnt1[s1[i].charCodeAt() - aCharCode]
    ++cnt2[s2[i].charCodeAt() - aCharCode]
  }
  if (cnt1.toString() === cnt2.toString()) {
    return true
  }
  // 滑动窗口，每次比较
  for (let i = n; i < m; ++i) {
    ++cnt2[s2[i].charCodeAt() - aCharCode]
    --cnt2[s2[i - n].charCodeAt() - aCharCode]
    if (cnt1.toString() === cnt2.toString()) {
      return true
    }
  }
  return false
}

// 80ms 性能好到令人胆寒
var checkInclusion = function checkInclusion(s1, s2) {
  const n = s1.length
  const m = s2.length
  const aCharCode = 'a'.charCodeAt()
  const getIndex = s => s.charCodeAt() - aCharCode
  if (n > m) {
    return false
  }
  // 用一个长度26的数组记录每个字母的差异，都为零则存在相等排列
  const cnt = new Array(26).fill(0)
  // 记录不同位的个数，首次挖坑填坑
  let diff = 0
  for (let i = 0; i < n; i++) {
    // 挖
    --cnt[getIndex(s1[i])]
    // 补
    ++cnt[getIndex(s2[i])]
  }
  for (const n of cnt) {
    if (n !== 0) {
      diff++
    }
  }
  if (diff === 0) {
    return true
  }
  // 移动窗口，尝试补坑
  for (let i = n; i < m; i++) {
    // 出字母位置
    const x = getIndex(s2[i - n])
    // 进字母位置
    const y = getIndex(s2[i])
    // 如果进出相同，跳过
    if (x === y) {
      continue
    }
    // 之前相等
    if (cnt[x] === 0) {
      diff++
    }
    // 移出窗口
    --cnt[x]
    // 之后相等
    if (cnt[x] === 0) {
      diff--
    }

    // y同理
    if (cnt[y] === 0) {
      diff++
    }
    // 进入窗口
    ++cnt[y]
    // 之后相等
    if (cnt[y] === 0) {
      diff--
    }

    // 如果坑被填完
    if (diff === 0) {
      return true
    }
  }
  return false
}
```

深度优先，广度优先：岛屿最大面积

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int ans = 0;
        // 嵌套循环，遍历每项
        for (int i = 0; i < grid.length; ++i) {
            for (int j = 0; j != grid[0].length; ++j) {
                ans = Math.max(ans, dfs(grid, i, j));
            }
        }
        return ans;
    }

    public int dfs(int[][] grid, int cur_i, int cur_j) {
        // 判断边界，是否土地
        if (cur_i < 0 || cur_j < 0 || cur_i == grid.length || cur_j == grid[0].length || grid[cur_i][cur_j] != 1) {
            return 0;
        }
        grid[cur_i][cur_j] = 0;
        int[] di = {0, 0, 1, -1};
        int[] dj = {1, -1, 0, 0};
        int ans = 1;
        // 递归四个方向
        for (int index = 0; index < 4; ++index) {
            int next_i = cur_i + di[index];
            int next_j = cur_j + dj[index];
            ans += dfs(grid, next_i, next_j);
        }
        return ans;
    }
}
```

```js
var maxAreaOfIsland = function (grid) {
  let ans = 0
  const row = [1, -1, 0, 0]
  const col = [0, 0, -1, 1]
  const dfs = (grid, i, j) => {
    // 判断边界以及是否为土地
    if (i < 0 || i === grid.length || j < 0 || j === grid[0].length || grid[i][j] !== 1) {
      return 0
    }
    // 保证每个点只查一次
    grid[i][j] = 0
    // 记录每个点的相邻土地节点
    let ans = 1
    // 递归查找四个方向
    for (let n = 0; n < 4; n++) {
      ans += dfs(grid, i + row[n], j + col[n])
    }
    return ans
  }
  grid.forEach((_, i) => {
    grid[i].forEach((_, j) => {
      ans = Math.max(ans, dfs(grid, i, j))
    })
  })
  return ans
}
```

深度优先：图像渲染

```js
// 80ms
var floodFill = function (image, sr, sc, newColor) {
  const current = image[sr][sc]
  const row = [1, -1, 0, 0]
  const col = [0, 0, -1, 1]
  const rn = image.length
  const cn = image[0].length
  const dfs = (image, r, c) => {
    if (r >= 0 && c >= 0 && r < rn && c < cn) {
      if (image[r][c] === current) {
        image[r][c] = newColor
        for (let i = 0; i < 4; i++) {
          dfs(image, r + row[i], c + col[i])
        }
      }
    }
  }
  if (current !== newColor) {
    // 从中心点开始查询
    dfs(image, sr, sc)
  }
  return image
}
```

```java
// 1ms
class Solution {

    int[] row = {1, -1, 0, 0};
    int[] col = {0, 0, 1, -1};

    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        int color = image[sr][sc];
        if (color != newColor) {
            dfs(image, sr, sc, color, newColor);
        }
        return image;
    }

    public void dfs(int[][] image, int sr, int sc, int color, int newColor) {
        if (sr >= 0 && sc >= 0 && sr < image.length && sc < image[0].length) {
            if (image[sr][sc] == color) {
                image[sr][sc] = newColor;
                for (int i = 0; i < 4; i++) {
                    dfs(image, sr + row[i], sc + col[i], color, newColor);
                }
            }
        }
    }
}
```

深度优先：合并二叉树

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
      if(root1 == null) {
          return root2;
      }
      if(root2 == null) {
          return root1;
      }
      TreeNode merged = new TreeNode(root1.val + root2.val);
      merged.left = mergeTrees(root1.left, root2.left);
      merged.right = mergeTrees(root1.right, root2.right);
      return merged;
    }
}
```

广度优先：腐烂的橘子

```js
var orangesRotting = function (grid) {
  // 队列，广度优先搜索
  const rn = grid.length
  const cn = grid[0].length
  const row = [1, -1, 0, 0]
  const col = [0, 0, 1, -1]
  // 遍历队列
  const qune = []
  // 轮数
  let round = 0
  // 新鲜数
  let fresh = 0
  for (let r = 0; r < rn; r++) {
    for (let c = 0; c < cn; c++) {
      const current = grid[r][c]
      if (current === 1) {
        fresh++
      } else if (current === 2) {
        // 第一轮
        qune.push([r, c])
      }
    }
  }
  // 存在新鲜且队列不为空时继续遍历
  while (fresh > 0 && qune.length) {
    round++
    const n = qune.length
    for (let i = 0; i < n; i++) {
      // 当push和pop或 unshift 和 shift的时候，结果错误
      // 上述原因是，后进后出的方式是错的，导致这一轮用到下一轮的数据，
      const [r, c] = qune.shift()
      for (let j = 0; j < 4; j++) {
        const _r = r + row[j]
        const _c = c + col[j]
        if (_r >= 0 && _c >= 0 && _r < rn && _c < cn) {
          if (grid[_r][_c] === 1) {
            fresh--
            grid[_r][_c] = 2
            qune.push([_r, _c])
          }
        }
      }
    }
  }
  if (fresh !== 0) {
    return -1
  }
  return round
}
```

递归：合并两个有序列表

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
//  0ms ? 递归， 时间空间复杂杜都是O(m + n)
// 判断 l1 和 l2 哪一个链表的头节点的值更小，然后递归地决定下一个添加到结果里的节点
// 为空的时候退出递归
// not clearly
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }
        if(l1.val < l2.val) {
          l1.next = mergeTwoLists(l1.next, l2);
          return l1;
        } else {
          l2.next = mergeTwoLists(l2.next, l1);
          return l2;
        }
    }
}
// 迭代的方式比较好东懂些
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode prehead = new ListNode(-1);
        // 指针
        ListNode pre = prehead;
        while(l1 != null && l2 != null){
           if(l1.val <= l2.val) {
               pre.next = l1;
               // 移动指针
               l1 = l1.next;
           } else {
               pre.next = l2;
               // 移动指针
               l2 = l2.next;
           }
           // 移动指针
           pre = pre.next;
        }

        // 相当长度迭代完成后剩下的部分
        pre.next = l1 == null ? l2 : l1;

        return prehead.next;
    }
}
```

反转链表

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
      // 把当前节点的指针指向上一个节点
      ListNode current = head;
      ListNode prev = null;
      while(current != null) {
          // 记录当前的next
          ListNode next = current.next;
          current.next = prev;
          // 移动指针
          prev = current;
          current = next;
      }
      return prev;
    }
}
```

最大子序列

```java
// 贪心算法
class Solution {
    public int maxSubArray(int[] nums) {
        //子序列逐渐增加，每次增加找出最大和
        int pre = 0, max = nums[0];
        for (int num : nums) {
             // pre代表之前序列的和，如果之前和小于零，则丢弃之前序列
             pre = Math.max(pre+num,num);
             max = Math.max(pre,max);
        }
        return max;
    }
}
// 动态规划
class Solution {
    public int maxSubArray(int[] nums) {
        // 动态规划的重点在于，如果前一项大于零有增益效果，则后一项+=前一项
        int max = nums[0];
        for (int i = 0; i < nums.length - 1; i++) {
            if(nums[i] > 0) {
                nums[i+1] += nums[i];
            }
            max = Math.max(max,nums[i+1]);
        }
        return max;
    }
}
```

合并两个有序数组

```js
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function (nums1, m, nums2, n) {
  // 拼接然后排序，
  nums1.splice(m, nums1.length - m, ...nums2)
  return nums1.sort((a, b) => a - b)
}

// 最终，合并后数组不应由函数返回，而是存储在数组 l1 中。
// 为了应对这种情况，nums1 的初始长度为 m + n
var merge = function (l1, m, l2, n) {
  // 创建两个各自的指针
  // 创建目标位数空数组
  // 循环填充空数组
  //    边界：两个指针到头
  //    一个数组添加完时，则添加另一个数组
  // 最后赋值nums1
  let p1 = 0,
    p2 = 0
  const sorted = new Array(m + n).fill(0)
  let cur
  while (p1 < m || p2 < n) {
    if (p1 === m) {
      cur = l2[p2++]
    } else if (p2 === n) {
      cur = l1[p1++]
    } else if (l1[p1] < l2[p2]) {
      cur = l1[p1++]
    } else {
      cur = l2[p2++]
    }
    sorted[p1 + p2 - 1] = cur
  }
  for (let i = 0; i < m + n; ++i) {
    l1[i] = sorted[i]
  }
}
// 逆向双指针，向后填充，可以理解为三指针
// p1 会不会被覆盖？
// 只需要比较已经填充的位置个数 和 p1后的位置个数
// p1 后位置 m+n-p1-1
// 每次填充的个数 n-p2-1 + m-p1-1
// 假设 m+n-p1-1 > n-p2-1 + m-p1-1
// 可得p2 > -1，恒成立
var merge = function (l1, m, l2, n) {
  let p1 = m - 1
  let p2 = n - 1
  let tail = m + n - 1 // 尾指针
  let cur
  while (p1 >= 0 || p2 >= 0) {
    // 边界，即是数组的范围之外
    if (p1 === -1) {
      cur = l2[p2--]
    } else if (p2 === -1) {
      cur = l1[p1--]
    } else if (l1[p1] > l2[p2]) {
      cur = l1[p1--]
    } else {
      cur = l2[p2--]
    }
    // 向尾部添加
    l1[tail--] = cur
  }
  return l1
}
```

哈希表：求两个数组的交集

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersect = function (nums1, nums2) {
  // 遍历较短的数组
  if (nums2.length < nums1.length) {
    return intersect(nums2, nums1)
  }
  const ans = []
  const map = {}
  let p = 0
  for (const i of nums1) {
    map[i] ? map[i]++ : (map[i] = 1)
  }
  for (const i of nums2) {
    if (map[i] > 0) {
      map[i]--
      ans.push(i)
    }
  }
  return ans
}
```

动态规划：买股票的最佳时机

```js
class Solution {
    public int maxProfit(int[] prices) {
        // 只需要找到最小价格
        // 遍历每一项，如果小于最小价格，替换之
        // 否则，计算利润
        // 这里的设最小值为最大值，仿佛几何题做延长线
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;
        for (int i = 0; i <prices.length; i++){
            if(prices[i] < minPrice) {
                minPrice = prices[i];
            } else if(prices[i] - minPrice > maxProfit){
                maxProfit = prices[i] - minPrice;
            }
        }
        return maxProfit;
    }
}
```

重塑矩阵

```js
var matrixReshape = function (mat, r, c) {
  const row = mat.length
  const col = mat[0].length
  // 如果不可行，返回原矩阵
  if (row * col !== r * c) {
    return mat
  }
  const ans = []
  let currentRow = 0
  let count = 1
  for (const n of mat) {
    for (const i of n) {
      ;(ans[currentRow] || (ans[currentRow] = [])).push(i)
      if (count++ % c === 0) {
        currentRow++
      }
    }
  }
  return ans
}
```

动态规划：杨辉三角

```js
// 80ms
var generate = function (numRows) {
  const ans = [[]]
  for (let i = 0; i < numRows; i++) {
    const temp = []
    temp.push(1)
    for (let j = 0; j < ans[i].length - 1; j++) {
      temp.push(ans[i][j] + ans[i][j + 1])
    }
    if (i > 0) {
      temp.push(1)
    }
    ans.push(temp)
  }
  ans.unshift()
  return ans
}

var generate = function (numRows) {
  const ans = []
  for (let i = 0; i < numRows; i++) {
    const row = []
    for (let j = 0; j <= i; j++) {
      if (j === 0 || j === i) {
        row.push(1)
      } else {
        row.push(ans[i - 1][j - 1] + ans[i - 1][j])
      }
    }
    ans.push(row)
  }
  return ans
  // 简化
var generate = function (numRows) {
  const ans = []
  for (let i = 0; i < numRows; i++) {
    const row = new Array(i + 1).fill(1)
    for (let j = 1; j < i - 1; j++) {
      row[j] = ans[i - 1][j - 1] + ans[i - 1][j]
    }
    ans.push(row)
  }
  return ans
}
```

矩阵：有效数独

```java
// 2ms
class Solution {
    public boolean isValidSudoku(char[][] board) {
        // 计数每行
        int[][] rows = new int[9][9];
        // 计数每列
        int[][] cols = new int[9][9];
        // 计数每个九宫格
        int[][][] subboxs = new int[3][3][9];
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char n = board[i][j];
                if (n != '.') {
                    // 数独数字为1-9，对应的index为0-8
                    int index = n - '0' - 1;
                    // 记录对应行，列，九宫格
                    int rowCount = ++rows[i][index];
                    int colsCount = ++cols[j][index];
                    int subboxsCount = ++subboxs[i/3][j/3][index];
                    // 每轮记录完成，检查是否有计数
                    if(rowCount > 1 || colsCount > 1 || subboxsCount > 1) {
                        return false;
                    }
                }
            }
        }
        return true;
    }
}
```

```js
/**
 * @param {character[][]} board
 * @return {boolean}
 */
// 82ms
var isValidSudoku = function (board) {
  const rows = new Array(9).fill(0).map(() => new Array(9).fill(0))
  const cols = new Array(9).fill(0).map(() => new Array(9).fill(0))
  const subboxs = new Array(3).fill(0).map(() => new Array(3).fill(0).map(() => new Array(9).fill(0)))
  for (let i = 0; i < 9; i++) {
    for (let j = 0; j < 9; j++) {
      const n = board[i][j]
      if (n !== '.') {
        //    const index = n - 1;
        const index = n.charCodeAt() - '0'.charCodeAt() - 1
        const rowCount = ++rows[i][index]
        const colCount = ++cols[j][index]
        const subboxCount = ++subboxs[Math.floor(i / 3)][Math.floor(j / 3)][index]
        if (rowCount > 1 || colCount > 1 || subboxCount > 1) {
          return false
        }
      }
    }
  }
  return true
}
```

矩阵置零：标记的方式的不同

```java
// 使用标记数组
class Solution {
    public void setZeroes(int[][] matrix) {
        int r = matrix.length;
        int c = matrix[0].length;
        boolean[] row = new boolean[r];
        boolean[] col = new boolean[c];
        // 记录有零的行和列
        for (int i = 0; i < r; i++) {
            for(int j = 0; j < c; j++) {
                if(matrix[i][j] == 0) {
                    row[i] = true;
                    col[j] = true;
                }
            }
        }
        // 设置行和列的单元格为零
        for (int i = 0; i < r; i++) {
            for(int j = 0; j < c; j++) {
                if(row[i] || col[j]) {
                     matrix[i][j] = 0;
                }
            }
        }
    }
}
// 使用两个标记变量分别标记首行和首列是否有零，
// 用首行和首列标记需要更改的行和列
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean flagCol0 = false, flagRow0 = false;
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                flagCol0 = true;
                break;
            }
        }
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                flagRow0 = true;
                break;
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (flagCol0) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
        if (flagRow0) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
    }
}

```

```js
// 三个方法都是利用数组标记的方式处理，
// 记录需要修改的行和列并修改，修改时需要注意
// 防止修改标记数组导致的结果错误，

// 比如，使用一个标记变量，但从最后更改标记首列，
/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
var setZeroes = function (matrix) {
  const m = matrix.length
  const n = matrix[0].length
  // 标记第一列是否出现过0
  // 这样第一行的第一个元素就可以用来记录第一行是否出现过0
  let flagCol0 = false
  for (let i = 0; i < m; i++) {
    // 记录首列
    if (matrix[i][0] === 0) {
      flagCol0 = true
    }
    // 记录首列之外的单元格
    for (let j = 1; j < n; j++) {
      if (matrix[i][j] === 0) {
        matrix[i][0] = matrix[0][j] = 0
      }
    }
  }
  // 为了防止每一列的第一个元素被提前更新，需要从最后一行开始遍历
  for (let i = m - 1; i >= 0; i--) {
    for (let j = 1; j < n; j++) {
      if (matrix[i][0] === 0 || matrix[0][j] === 0) {
        matrix[i][j] = 0
      }
    }
    if (flagCol0) {
      matrix[i][0] = 0
    }
  }
}
```

哈希表：字符串中的第一个唯一字符

```js
// 储存字母频次
var firstUniqChar = function (s) {
  const map = {}
  for (const n of s) {
    if (map[n]) {
      // 设置成布尔值也可以，主要为了记录
      map[n]++
    } else {
      map[n] = 1
    }
  }
  for (const n of s) {
    if (map[n] === 1) {
      return n
    }
  }
  return -1
}
```

哈希表：赎金信：

> 判断 ransomNote 能否由 magazine 构成，magazine 中的字符只能使用一次

```js
var canConstruct = function (ransomNote, magazine) {
  const map = {}
  for (const n of ransomNote) {
    map[n] ? map[n]++ : (map[n] = 1)
  }
  for (const n of magazine) {
    if (map[n]) {
      map[n]--
    }
  }
  for (const key in map) {
    if (map[key] !== 0) {
      return false
    }
  }
  return true
}
```

哈希表：有效的字母异位词

```js
// 哈希表
var isAnagram = function (s, t) {
  const map = {}
  for (const n of s) {
    map[n] ? map[n]++ : (map[n] = 1)
  }
  for (const n of t) {
    if (map[n]) map[n]--
    else return false
  }
  for (const key in map) {
    if (map[key] !== 0) {
      return false
    }
  }
  return true
}
// 数组
var isAnagram = function (s, t) {
  if (s.length !== t.length) {
    return false
  }
  // 用上个数组记录字母频次
  const table = new Array(26).fill(0)
  const aCode = 'a'.codePointAt()
  const getIndex = n => n.codePointAt() - aCode
  for (const n of s) {
    table[getIndex(n)]++
  }
  for (const n of t) {
    const num = --table[getIndex(n)]
    if (num < 0) return false
  }
  return true
}
```

哈希表：环形链表

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
 // 用哈希表记录是否存在就行
public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> seen = new HashSet<ListNode>();
        while(head != null){
            if(!seen.add(head)){
                return true;
            }
            head = head.next;
        }
        return false;
    }
}
```

js 版

```js
var hasCycle = function (head) {
  const map = new Map()
  while (head) {
    if (map.has(head)) {
      return true
    }
    map.set(head, true)
    head = head.next
  }
  return false
}
```

链表：合并两个有序链表

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode prehead = new ListNode(-1);
        // 指针
        ListNode pre = prehead;
        while(l1 != null && l2 != null) {
           if(l1.val <= l2.val) {
               pre.next = l1;
               // 移动l1指针
               l1 = l1.next;
           } else {
               pre.next = l2;
               // 移动l2指针
               l2 = l2.next;
           }
           // 移动pre指针
           pre = pre.next;
        }

        // 连接迭代完成后剩下的部分
        pre.next = l1 == null ? l2 : l1;

        return prehead.next;
    }
}
```

链表，递归：移除链表元素

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */
var removeElements = function (head, val) {
  if (head === null) {
    return head
  }
  head.next = removeElements(head.next, val)
  return head.val === val ? head.next : head
}
```

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
      // 创建一个节点
      ListNode dummyHead = new ListNode(0);
      dummyHead.next = head;
      // 创建一个指针节点
      ListNode cur = dummyHead;
      while(cur.next != null) {
          // 如果下一个节点是需要删除的，跳过
          if(cur.next.val == val) {
              cur.next = cur.next.next;
          } else {
            // 移动指针
              cur = cur.next;
          }
      }
      return dummyHead.next;
    }
}
```

双指针：反转链表

```js
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {
  // 双指针思路
  let prev = null
  let cur = head
  while (cur !== null) {
    const next = cur.next
    cur.next = prev
    prev = cur
    cur = next
  }
  return prev
}
```

删除排序链表中的重复元素，和删除指定值节点思路一样

```js
var deleteDuplicates = function (head) {
  if (!head) return head
  let cur = head
  while (cur.next) {
    if (cur.val === cur.next.val) {
      cur.next = cur.next.next
    } else {
      cur = cur.next
    }
  }
  return head
}
```

栈：有效的括号

```js
var isValid = function (s) {
  // 非对称时，停止遍历
  if (s.length % 2 === 1) {
    return false
  }
  const pairs = {
    ')': '(',
    ']': '[',
    '}': '{'
  }
  const stk = []
  // 先遇到的右括号先闭合，后遇到的左括号先闭合
  // 根据这个特点，将最新的左括号放到栈首，待验证
  // 验证最新的右括号于栈首左括号能够配对
  // 失败：或待验证列表为空，没有对应的L
  for (const ch of s) {
    // 遇到右括号，判断配对
    if (pairs[ch]) {
      // 没有左括号，或者左括号不对应时，则配对失败
      if (!stk.length || stk[stk.length - 1] !== pairs[ch]) {
        return false
      }
      // 配对成功
      stk.pop()
    } else {
      // 记录最新的左括号到栈首
      stk.push(ch)
    }
  }
  // 待验证列表是否为空
  return !stk.length
}
```

java 版

```java
class Solution {
    public boolean isValid(String s) {
        if(s.length() % 2 == 1) {
            return false;
        }
        Map<Character, Character> pairs = new HashMap<Character, Character>() {{
            put(')', '(');
            put(']', '[');
            put('}', '{');
        }};
        Deque<Character> stack = new LinkedList<Character>();
        for (int i = 0; i < s.length(); i++) {
            Character ch = s.charAt(i);
            // 右括号，验证匹配
            if (pairs.containsKey(ch)) {
                // 没有左括号，或者不匹配
                if (stack.isEmpty() || stack.peek() != pairs.get(ch)) {
                    return false;
                } else {
                   // 匹配成功，出栈
                   stack.pop();
                }
            } else {
                 // 添加左括号到栈首
                 stack.push(ch);
            }

    }
    return stack.isEmpty();
}
}
```

用栈实现队列

```js
/**
 * Your MyQueue object will be instantiated and called as such:
 * var obj = new MyQueue()
 * obj.push(x)  添加到队末
 * var param_2 = obj.pop()  取出并返回队末元素
 * var param_3 = obj.peek() 返回队末元素
 * var param_4 = obj.empty()
 */
// 队列：先进先出
// 栈：后进先出
// 即，用后进先出的方式实现先进先出
// 具体实现：两个栈，一个进栈，一个出栈，
// push时，直接添加到进栈
// pop时从出栈弹出先进的元素
// 既然栈是后进先出，那么将一个栈的所有元素弹出并压入另一个栈后，
// 再出栈就实现了先进先出的效果，
// 具体出栈时，如果出栈为空，将入栈【全部】压入到出栈，出栈弹出元素
var MyQueue = function () {
  this.inStack = []
  this.outStack = []
}

MyQueue.prototype.push = function (x) {
  this.inStack.push(x)
}

MyQueue.prototype.pop = function () {
  if (!this.outStack.length) {
    this.in2out()
  }
  return this.outStack.pop()
}

MyQueue.prototype.peek = function () {
  if (!this.outStack.length) {
    this.in2out()
  }
  return this.outStack[this.outStack.length - 1]
}

MyQueue.prototype.empty = function () {
  return this.outStack.length === 0 && this.inStack.length === 0
}

MyQueue.prototype.in2out = function () {
  while (this.inStack.length) {
    this.outStack.push(this.inStack.pop())
  }
}
```

递归：二叉树的前序排列

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
       List<Integer> result = new ArrayList();
       preorder(root, result);
       return result;
    }
    public void preorder(TreeNode head, List<Integer> res) {
        if(head == null) return;
        res.add(head.val);
        preorder(head.left,res);
        preorder(head.right,res);
    }
}

class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if (root == null) {
            return res;
        }

        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        TreeNode node = root;
        while (!stack.isEmpty() || node != null) {
            while (node != null) {
                res.add(node.val);
                stack.push(node);
                node = node.left;
            }
            node = stack.pop();
            node = node.right;
        }
        return res;
    }
}

```
