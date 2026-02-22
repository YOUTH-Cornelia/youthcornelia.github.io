---
title: "Find the Index of the First Occurrence in a String"
date: 2026-02-22
draft: false
categories: ["9021"]
tags: ["Array"]
---


# ⭐ 这题需要掌握的全部知识（COMP9021版）

我按重要程度排序。

# ⭐⭐⭐⭐⭐ ① 读题 → 转换成步骤（最核心能力）

## 你要学会：

```text
英文规则 → 人话 → 算法步骤
```

这题训练：

* last occurrence（最后一次出现）
* followed by（后面发生什么）
* before（位置范围）
* remove all occurrences（删除所有）

## 你应该会：

```text
① 找参考值 e
② 找关键位置（分界点）
③ 根据位置删除
```

## 老师真正考：

```text
能不能把规则变成程序逻辑
```

不是 Python。

# ⭐⭐⭐⭐⭐ ② 从右往左扫描（suffix 思维）

## 你要学会：

```python
for i in range(len(L)-1, -1, -1)
```

## 为什么这题要用？

因为：

```text
找最后一个 ≥ e
```

= 从右往左第一个。

## 你应该形成条件反射：

看到：

```text
last / 最后 / rightmost
```

就反着扫。

# ⭐⭐⭐⭐ ③ 找分界点（boundary thinking）

## 这题核心结构：

```text
找 e 前最后一个 ≥ e
```

这个叫：

```text
分界点
```

## 你要会：

```python
boundary = -1
for i in ...:
    if condition:
        boundary = i
        break
```

## 为什么重要？

COMP9021 很爱出：

* pivot
* split position
* boundary
* prefix/suffix题

# ⭐⭐⭐⭐ ④ suffix（右侧信息）思维

## 这题本质：

```text
看某位置右边发生什么
```

## 你要理解：

```text
L[i+1:]
= 右侧
```

## 以后会看到：

* 后面有没有更大
* 后面是否满足条件
* suffix max

# ⭐⭐⭐ ⑤ list 过滤 / 删除元素（Python基础）

## 必会模板：

```python
L[:] = [x for x in L if condition]
```

## 你要知道：

* 删除多个元素 → 不用 remove
* 用 list comprehension
* 原地修改用 `L[:]`

# ⭐⭐⭐ ⑥ 值 vs index（你刚踩的坑）

你刚犯的：

```python
new.append(i)  # index
```

但删除时：

```python
x not in new  # value
```

## 必须区分：

```text
L[i] → 值
i → 位置
```

COMP9021超常考。


# ⭐⭐⭐ ⑦ break 的使用

## 什么时候用：

```text
找一个 → break
找全部 → 不 break
```

这题：

```text
找最后一个 ≥ e
```

所以 break。

# ⭐⭐ ⑧ IndexError 防御（工程意识）

你学到：

```python
if not L:
    return
```


## 你要形成习惯：

访问：

```python
L[-1]
```

先检查空列表。

# ⭐⭐ ⑨ range 三参数

## 必须理解：

```python
range(start, stop, step)
```

这题：

```python
range(len(L)-2, -1, -1)
```

= 从右往左。


# ⭐⭐ ⑩ set 优化（进阶）

```python
remove_set = set()
```

避免：

```text
O(n²)
```

这是效率意识。

# ⭐ 老师为什么出这题（真实目的）

这题同时考：

```text
✔ 读题能力
✔ 位置关系
✔ suffix思维
✔ Python list操作
✔ 循环控制
✔ 边界处理
```

不是简单题。

# ⭐ 你通过这题应该获得的能力

如果这题真的学懂，你应该能：

## ✅ 一看到：

```text
last / after / before
```

自动想：

```text
位置关系
```

## ✅ 一看到：

```text
最后一个
```

自动：

```text
从右扫
```

## ✅ 一看到：

```text
删除多个
```

自动：

```python
[x for x in L if ...]
```

# ⭐ 给你一个终极总结（必须记）

这题本质训练：

```text
数组题三大思维：
① 扫描方向
② 位置关系
③ 过滤结果
```

你现在三样都练到了。


# ⭐ 我对你现在能力的真实评价（认真）

你现在：

```text
✔ Python语法 OK
✔ 能理解算法
✔ 缺 → 结构识别速度
```

再练 10–20 题会明显提升。