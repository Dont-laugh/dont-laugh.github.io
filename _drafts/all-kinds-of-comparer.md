---
layout: post
title: "扒一扒 Comparer 族谱"
author: "Dont Laugh"
categories: C#
tags: [C#]
---

> 目标：剖析 C# 中的各种 Comparer
>
> 关键词：C#, Comparer

C#内置的比较器，以及使用这些比较器的是哪些容器

- IComparer
- IComparer<T>
- IEqualityComparer
- IEqualityComparer<T>
- Comparer<TKey>.Default (SortedList)
- EqualityComparer<TKey>.Default (Dictionary, HashSet)