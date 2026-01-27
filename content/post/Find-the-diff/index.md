---
title: "Find the difference"
date: 2026-01-27
draft: false
categories: ["LeetCode"]
tags: ["String", "hash", "sort"]
leetcode_id: 28
---

## Description: 
[Leetcode 28: Find the difference](https://leetcode.cn/problems/find-the-difference/description/?envType=study-plan-v2&envId=programming-skills)

## 从 `in` 到 `remove` 到哈希表：我把这道字符串题彻底想明白的全过程

> 这篇是我把**自己问过的问题**、**写错过的代码**、**你给我的解释**整理成一份可复用的笔记。只写我真正卡住的点和对应解决方式，不写空话。


## 0. 题目（我问的那道）

给定两个字符串 `s` 和 `t`（只包含小写字母）。

* `t` 是由 `s` 随机重排
* 然后在随机位置**多加了一个字母**

要求：找出 `t` 中**被添加的字母**。

关键点：

* `t` 比 `s` **多一个字符**
* 但这个字符**可能在 s 里本来就存在**（比如多出来一个 `'a'`）


## 1. 我最开始写的“过滤法”为什么一定错

### 1.1 我一开始的代码（从 s 里挑不在 t 的）

```python
res = []
for i in s:
    if i not in t:
        res.append(i)
return ''.join(res)
```

为什么错：

* 题目保证 `t` 包含 `s` 的全部字符（只是重排 + 多一个）
* 所以 `s` 里的每个字符都在 `t` 里出现过
* `if i not in t` 基本永远不成立
* 最后会返回空字符串 `""`

### 1.2 我把遍历改成 t 也还是错（只判断“有没有”）

```python
res = []
for i in t:
    if i not in s:
        res.append(i)
return ''.join(res)
```

为什么错：

* 我在问：`i` **有没有**出现在 `s`
* 但这题要问的是：`i` 在 `t` 里是不是**多出现了 1 次**

反例（我当时就卡在这里）：

* `s = "aab"`
* `t = "aaab"`

多出来的是 `'a'`，但 `'a' in s` 为 True。
所以这段代码依旧得不到答案。

结论：

> 这题核心不是“有没有”，而是“出现次数（计数）”。


## 2. 我问的：为什么 `if x:` / `if i not in ...` 这些判断老是出现？

当时我在 `zip_longest` 里也见过这种：

```python
for x, y in zip_longest(word1, word2):
    if x:
        ans.append(x)
    if y:
        ans.append(y)
```

解释（和这题的“计数/消耗”思路是一致的）：

* `zip_longest` 会用 `None` 补齐
* `if x:` 是在说：x 不是 None（也不是空字符串）时才加入
* 这是“状态判断”：当前这个位置有没有有效字符

这道找差异字母的题也一样：

* 只用 `in` 判断不到“多了一次”
* 我需要“状态”（次数）或“消耗”（配对后剩谁）


## 3. 我问的：能不能用 `remove`？可以，但要用对

### 3.1 只在我原代码基础上的最小改法（关键：消耗 s）

我原来是：

```python
res = []
for i in t:
    if i not in s:
        res.append(i)
return ''.join(res)
```

你告诉我：要在这个结构上修正，核心是：

> **不是判断“在不在”，而是“能不能配对”。**

所以要把 `s` 变成可删除的 list，并且每匹配一次就消耗掉：

```python
s = list(s)
res = []
for i in t:
    if i in s:
        s.remove(i)   # 消耗掉一个
    else:
        res.append(i) # 不能配对的就是多出来的
return ''.join(res)
```

这段为什么正确：

* 题目保证 t = s 的字符 + 1
* 每次看到 t 的字符，就从 s 里删掉一个对应字符
* 删不掉的那个字符就是“多出来的”

用反例验证：

* s = "aab" -> ['a','a','b']
* t = "aaab"

  * 前两个 'a' 能删
  * 第三个 'a' 删不掉 -> 答案 'a'

### 3.2 但 `remove` 的限制（我需要知道的真实点）

* `list.remove(x)` 是 O(n)
* 循环里 remove 会导致整体最坏 O(n^2)
* 数据大时会慢（LeetCode 可能会 TLE）

所以 remove 是：

* ✅ 思路直观、适合我理解“配对/消耗”
* ❌ 性能不稳，不是最推荐的终版

## 4. 哈希表（dict）到底是什么思路？

我问过一句：

> “哈希表也是一种编程思路，就是计数思路？”

你给的结论我现在的理解是：

* 哈希表不是某道题的专用技巧
* 它是：把“对象 -> 状态”存起来
* 在计数题里：对象是字符，状态就是出现次数

这题用哈希表的核心逻辑就是：

> `t` 的某个字符次数比 `s` 多 1。

## 5. 用普通 dict 写计数（我必须写对的版本）

我当时写错过（写成了 `s[i] += 1`），问题是我把“原始数据 s”和“状态表 counter”混了。

正确写法：

```python
counter = {}
for i in s:
    if i in counter:
        counter[i] += 1
    else:
        counter[i] = 1
```

这段代码的含义（我自己的话）：

* 如果 i 之前出现过（在 counter 里），次数 +1
* 如果 i 第一次出现，就新建 counter[i]=1
* 后面再出现就继续 +1


## 6. 用哈希表解这题（计数 + 消耗）

我理解这题最稳的哈希表做法是：

* 先统计 s 的每个字符次数
* 再遍历 t，每出现一次就把次数 -1
* 哪个字符减到负数，就是多出来的

```python
class Solution:
    def findTheDifference(self, s: str, t: str) -> str:
        counter = {}

        # 1) 统计 s
        for ch in s:
            if ch in counter:
                counter[ch] += 1
            else:
                counter[ch] = 1

        # 2) 消耗 t
        for ch in t:
            if ch not in counter:
                return ch
            counter[ch] -= 1
            if counter[ch] < 0:
                return ch
```

为什么一定对：

* 对于来自 s 的那些字符，counter 会从正数减到 0
* 多出来的字符会让某个计数从 0 变成 -1（或者根本不在 counter）

这个方法和 remove 的关系：

* remove 是“动元素本身（袋子里拿走）”
* counter 是“动次数状态（还能用几次）”
* 本质都是配对/消耗，只是数据结构不同

## 7. 我问的：`if i not in counter: counter[i]=0; counter[i]+=1` 是什么思路？

这是“默认值 + 累加”的写法（你说它等价于 if/else）。

写法 B：

```python
if i not in counter:
    counter[i] = 0
counter[i] += 1
```

我现在能用一句话解释：

* 元素没出现过时，默认次数是 0
* 每出现一次就 +1

它和 if/else 的关系：

写法 A：

```python
if i in counter:
    counter[i] += 1
else:
    counter[i] = 1
```

两者结果一样，只是：

* A 在区分“第一次 vs 不是第一次”
* B 把第一次也当成：先给 0 再 +1


## 8.  `defaultdict` 而且就是写法 B 的自动版

```python
from collections import defaultdict

counter = defaultdict(int)
for ch in s:
    counter[ch] += 1
```

它等价于：

```python
if ch not in counter:
    counter[ch] = 0
counter[ch] += 1
```

所以用 defaultdict 解这题可以写成：

```python
from collections import defaultdict

class Solution:
    def findTheDifference(self, s: str, t: str) -> str:
        counter = defaultdict(int)

        for ch in s:
            counter[ch] += 1

        for ch in t:
            counter[ch] -= 1
            if counter[ch] < 0:
                return ch
```

这里为什么能直接 `counter[ch] -= 1`：

* defaultdict(int) 的默认值是 0
* 没见过的字符一减就变 -1，立刻被识别出来

## 9. 从普通哈希到 defaultdict 到 Counter

* 先会普通 dict（搞清楚状态表是什么）
* 再用 defaultdict（省掉初始化 0）
* 最后用 Counter（把计数封装成成品）

但它们不是“等级”，只是抽象层级不同：

* dict：我手动维护状态
* defaultdict：默认状态交给 Python
* Counter：直接得到“频率模型”
> **而hashtable的本质也就是字典的计数作用**

## 10. 读文件算词频也是同一套模型

词频统计就是：

* key = 单词
* value = 出现次数

写法（普通 dict）：

```python
freq = {}
for word in words:
    if word in freq:
        freq[word] += 1
    else:
        freq[word] = 1
```

写法（defaultdict）：

```python
from collections import defaultdict
freq = defaultdict(int)
for word in words:
    freq[word] += 1
```

写法（Counter）：

```python
from collections import Counter
freq = Counter(words)
```

---

## 11. 核心要点

1. 这道题不能只用 `in`，因为 `in` 只回答“有没有”，不回答“出现几次”。

2. `remove` 能做对，因为它在做“配对/消耗”，但可能慢。

3. 哈希表（dict/defaultdict/Counter）是把“次数”显式化：

* 先统计
* 再消耗
* 计数变负数（或缺失）就是答案

4. 写哈希表时要分清：

* `s`/`t` 是原始数据
* `counter` 才是状态表
* 增减的是 `counter[ch]`，不是 `s[ch]`
