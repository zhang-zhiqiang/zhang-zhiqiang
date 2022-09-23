---
title: "堆"
date: 2022-09-23T15:17:46+08:00
draft: false
tags: ["堆"]
categories: ["算法"]
weight: 20
---

### 二叉堆

#### 第一个元素在数组中的索引为0的话，父节点和子节点的位置关系如下：
1. 索引为i的左孩子的索引是（2*i+1）
2. 索引为i的右孩子的索引是（2*i+2）
3. 索引为i的父节点的索引是 floor((i-1)/2)

#### 添加操作
1. 先插入到堆的尾部
2. 依次向上调整整个堆的结构（一直到根即可）

#### 删除操作
1. 将删除的元素替换为要删除的元素， 总长度减一
2. 依次向下调整整个堆的结构（一直到堆尾即可）

[golang heap](https://pkg.go.dev/container/heap)

minheap
``` golang
import (
    "container/heap"
)

type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

[40、最小的k个树](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)
``` golang
func getLeastNumbers(arr []int, k int) []int {
    l := len(arr)
    if l < k {
        return arr
    }
    h := &IntHeap{}
    heap.Init(h)
    for i := 0; i < l; i++ {
        heap.Push(h, arr[i])
    }
    ans := make([]int, 0, 0)
    for i := 0; i < k; i++ {
        ans = append(ans, heap.Pop(h).(int))
    }
    return ans
}
```


