
## ✅ 一、文章头部（Front Matter｜Stack 友好）

```yaml
---
title: "Merge Strings Alternately"
date: 2026-01-25
draft: false
categories: ["LeetCode"]
tags: ["String", "Two Pointers"]
leetcode_id: 1768
---
```

### 🔍 说明（你心里有数就行）

* `categories`：**固定 LeetCode**
* `tags`：**算法/数据结构维度**
* `leetcode_id`：你后面如果做自动索引/统计会很有用（Stack 不冲突）

---

## ✅ 二、正文模板（这是精华）

### 🔗 题目链接

[LeetCode 1768 – Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately/)


## 一、这道题在干嘛（一句话版）

给两个字符串，从左到右 **交替取字符拼接**；
如果其中一个先用完，把另一个的剩余部分直接接在后面。


## 二、我一开始是怎么想的（最自然的思路）

> 不考虑什么算法，就是模拟人手动拼字符串的过程。

我的直觉是：

1. 用两个指针 `i`、`j` 指向 `word1` 和 `word2`
2. 轮流取字符加入结果
3. 谁还有剩的，就继续加

👉 这是一个**典型的模拟 + 状态判断题**。


## 三、容易卡住 / 写炸的点

### ❌ 1. 只用 `while i < m and j < n`

这样只会处理 **最短字符串长度**，
**剩余字符会被直接忽略**。


### ❌ 2. 用 `zip` 忘记处理尾巴

```python
for a, b in zip(word1, word2):
    ...
```

`zip` 在**最短的那个字符串结束时就停了**，
如果不额外处理，尾巴会丢。


## 四、解法一：双指针 + 状态判断（最推荐）

### 💡 思路关键词

* 状态判断代替分类讨论
* 覆盖：等长 / word1 长 / word2 长 三种情况

### ✅ 代码

```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        i = j = 0
        m, n = len(word1), len(word2)
        ans = []

        while i < m or j < n:
            if i < m:
                ans.append(word1[i])
                i += 1
            if j < n:
                ans.append(word2[j])
                j += 1

        return "".join(ans)
```


## 五、解法二：`zip_longest`（更 Pythonic，但要懂原理）

### 💡 为什么一定要写 `if x`？

`zip_longest` 会用 `None` 补齐短的那个字符串，
如果你不判断，`None` 会被加入结果，程序直接炸。

### ✅ 代码

```python
from itertools import zip_longest

class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        ans = []
        for x, y in zip_longest(word1, word2):
            if x:
                ans.append(x)
            if y:
                ans.append(y)
        return "".join(ans)
```


## 六、这道题真正训练的是什么？

✅ **用状态判断，覆盖多种情况**
而不是先拆成：

* 等长
* A 长
* B 长

👉 这是很多字符串 / 数组题的通用套路。


## 七、可以复用到哪些题？

* 交替合并数组 / 字符串
* 两个指针不等长遍历
* 流式拼接 / 模拟类题目


## 八、我的总结（偏个人）

> 这道题不难，但很适合作为
> **“从分类讨论 → 状态驱动思维” 的过渡题**。

如果你刚开始刷题，
**理解这道题的 while + if 结构，非常值。**


## ✅ 三、你可以长期固定下来的「写作骨架」

以后你每篇 LeetCode 都只需要填内容：

```md
## 一句话题意
## 第一反应
## 卡住的点
## 解法一（主）
## 解法二（可选）
## 这题在练什么
## 可以复用到哪
```

👉 **非常 Stack、非常工程、非常适合你**

