# [853. 车队](https://leetcode.cn/problems/car-fleet)

[English Version](/solution/0800-0899/0853.Car%20Fleet/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>在一条单行道上，有 <code>n</code> 辆车开往同一目的地。目的地是几英里以外的&nbsp;<code>target</code>&nbsp;。</p>

<p>给定两个整数数组&nbsp;<code>position</code>&nbsp;和&nbsp;<code>speed</code>&nbsp;，长度都是 <code>n</code> ，其中&nbsp;<code>position[i]</code>&nbsp;是第 <code>i</code> 辆车的位置，&nbsp;<code>speed[i]</code>&nbsp;是第 <code>i</code> 辆车的速度(单位是英里/小时)。</p>

<p>一辆车永远不会超过前面的另一辆车，但它可以追上去，并与前车 <strong>以相同的速度</strong> 紧接着行驶。此时，我们会忽略这两辆车之间的距离，也就是说，它们被假定处于相同的位置。</p>

<p><strong>车队</strong><em>&nbsp;</em>是一些由行驶在相同位置、具有相同速度的车组成的非空集合。注意，一辆车也可以是一个车队。</p>

<p>即便一辆车在目的地才赶上了一个车队，它们仍然会被视作是同一个车队。</p>

<p>返回到达目的地的 <strong>车队数量</strong> 。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre>
<strong>输入：</strong>target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]
<strong>输出：</strong>3
<strong>解释：</strong>
从 10 和 8 开始的车会组成一个车队，它们在 12 处相遇。
从 0 处开始的车无法追上其它车，所以它自己就是一个车队。
从 5 和 3 开始的车会组成一个车队，它们在 6 处相遇。
请注意，在到达目的地之前没有其它车会遇到这些车队，所以答案是 3。
</pre>

<p><strong>示例 2:</strong></p>

<pre>
<strong>输入:</strong> target = 10, position = [3], speed = [3]
<strong>输出:</strong> 1
<strong>解释:</strong> 只有一辆车，因此只有一个车队。
</pre>

<p><strong>示例 3:</strong></p>

<pre>
<strong>输入:</strong> target = 100, position = [0,2,4], speed = [4,2,1]
<strong>输出:</strong> 1
<strong>解释:</strong>
以0(速度4)和2(速度2)出发的车辆组成车队，在4点相遇。舰队以2的速度前进。
然后，车队(速度2)和以4(速度1)出发的汽车组成一个车队，在6点相遇。舰队以1的速度前进，直到到达目标。</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>n == position.length == speed.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt; target &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= position[i] &lt; target</code></li>
	<li><code>position</code>&nbsp;中每个值都 <strong>不同</strong></li>
	<li><code>0 &lt; speed[i] &lt;= 10<sup>6</sup></code></li>
</ul>

## 解法

### 方法一：排序

我们将车辆按照位置降序排序，这样我们只需要比较相邻两辆车的到达时间即可。

我们初始化一个变量 $pre$ 表示上一辆车到达终点的时间，如果当前车辆到达终点的时间大于 $pre$，说明当前车辆无法追上前面的车辆，因此需要另外开一个车队，否则当前车辆会与前面的车辆组成一个车队。

时间复杂度 $O(n)$，空间复杂度 $O(n)$。其中 $n$ 是车辆的数量。

<!-- tabs:start -->

```python
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        idx = sorted(range(len(position)), key=lambda i: position[i])
        ans = pre = 0
        for i in idx[::-1]:
            t = (target - position[i]) / speed[i]
            if t > pre:
                ans += 1
                pre = t
        return ans
```

```java
class Solution {
    public int carFleet(int target, int[] position, int[] speed) {
        int n = position.length;
        Integer[] idx = new Integer[n];
        for (int i = 0; i < n; ++i) {
            idx[i] = i;
        }
        Arrays.sort(idx, (i, j) -> position[j] - position[i]);
        int ans = 0;
        double pre = 0;
        for (int i : idx) {
            double t = 1.0 * (target - position[i]) / speed[i];
            if (t > pre) {
                ++ans;
                pre = t;
            }
        }
        return ans;
    }
}
```

```cpp
class Solution {
public:
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        int n = position.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return position[i] > position[j];
        });
        int ans = 0;
        double pre = 0;
        for (int i : idx) {
            double t = 1.0 * (target - position[i]) / speed[i];
            if (t > pre) {
                ++ans;
                pre = t;
            }
        }
        return ans;
    }
};
```

```go
func carFleet(target int, position []int, speed []int) (ans int) {
	n := len(position)
	idx := make([]int, n)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool { return position[idx[j]] < position[idx[i]] })
	var pre float64
	for _, i := range idx {
		t := float64(target-position[i]) / float64(speed[i])
		if t > pre {
			ans++
			pre = t
		}
	}
	return
}
```

```ts
function carFleet(target: number, position: number[], speed: number[]): number {
    const n = position.length;
    const idx = Array(n)
        .fill(0)
        .map((_, i) => i)
        .sort((i, j) => position[j] - position[i]);
    let ans = 0;
    let pre = 0;
    for (const i of idx) {
        const t = (target - position[i]) / speed[i];
        if (t > pre) {
            ++ans;
            pre = t;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- end -->
