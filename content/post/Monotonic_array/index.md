---
title: "Monotonic array"
date: 2026-02-18
draft: false
categories: ["LeetCode"]
tags: ["Array"]
leetcode_id: 896
---

## Description:
[Leetcode 896: Monotonic array](https://leetcode.cn/problems/monotonic-array/description/?envType=study-plan-v2&envId=programming-skills)


# 单调数组判断：从错误 return 到 ∀ 逻辑模型

## 1. 问题定义

判断数组 `nums` 是否单调：

```
(∀ i: nums[i] <= nums[i+1])
OR
(∀ i: nums[i] >= nums[i+1])
```

允许相等。

## 2. 本质逻辑结构

单调数组 =

```
(∀ 递增成立) OR (∀ 递减成立)
```

这是一个：

* 全称命题（∀）
* 外层 OR 结构

## 3. 关键逻辑规则

### 3.1 ∀ 型问题

如果题目是：

> 对所有 i 都成立

程序结构必须是：

* 找反例
* 发现反例 → return False
* 全部检查完 → return True

不能：

* 看到一个成立 → return True

### 3.2 ∃ 型问题（对比）

如果题目是：

> 存在一个 i 成立

程序结构是：

* 找正例
* 发现一个 → return True
* 全部检查完 → return False

## 4. 单调判断的正确状态模型

定义：

* increasing = 递增是否仍可能
* decreasing = 递减是否仍可能

初始化：

```
increasing = True
decreasing = True
```

含义：

> 在没有证据之前，两种方向都可能成立。

## 5. 相邻元素三种情况

设 a = nums[i], b = nums[i+1]

### 5.1 上升（a < b）

* 递减被否定
* increasing 不变

### 5.2 下降（a > b）

* 递增被否定
* decreasing 不变

### 5.3 相等（a == b）

* 两边都不否定


## 6. 提前终止条件

当：

```
increasing == False AND decreasing == False
```

说明：

* 既出现上升
* 又出现下降

方向翻转 → 不可能单调

可提前：

```
return False
```


## 7. 最终返回逻辑

遍历结束：

```
return increasing OR decreasing
```

只要有一个方向未被否定，即为单调。


## 8. 常见错误分析

### 错误 1：正例当反例

```python
if nums[i] <= nums[i+1]:
    increasing = False
```

错误原因：

* 递增的反例是 `>`
* 不是 `<=`

### 错误 2：循环内错误 return

```python
if nums[i] <= nums[i+1]:
    return False
```

问题：

* 把局部成立当作全局失败
* ∀ 问题不能因正例提前返回

### 错误 3：状态语义混乱

错误理解：

```
increasing = 是否看到递增
```

正确理解：

```
increasing = 递增是否还可能成立
```

## 9. 逻辑骨架抽象

单调数组属于：

```
(∀ 方向1) OR (∀ 方向2)
```

程序骨架：

```
初始化多个可能性为 True
遍历证据
    根据证据否定不可能方向
返回仍然存活的方向
```

## 10. 通用判断模板

### 判断类问题优先分类：

1. 是否“所有都满足”？ → ∀ → 找反例
2. 是否“存在一个满足”？ → ∃ → 找正例
3. 是否“统计数量”？ → 计数型
4. 是否“枚举起点 + 验证结构”？ → ∃ + ∀


## 11. 核心总结

单调数组不是数组题。

它是：

* 逻辑量词转换
* 局部证据对全局命题的影响
* 状态淘汰模型
* ∀ 型问题的标准实现

## 12. 技术口诀

* ∀ → 找反例
* ∃ → 找正例
* True 需要完整验证
* False 可以提前证伪
