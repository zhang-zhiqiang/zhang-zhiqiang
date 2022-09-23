---
title: "排序"
draft: false
tags: ["排序"]
categories: ["算法"]
weight: 3
---

## 排序

### 冒泡

``` golang
func Bubbling(data []int) ([]int, error) {
    if len(data) == 0 {
        return []int{}, nil
    }
    length := len(data)
    var flag bool
    for i := 0; i < length; i++ {
        flag = false
        for j := 0; j < length-i-1; j++ {
            if data[j] > data[j+1] {
                data[j], data[j+1] = data[j+1], data[j]
                flag = true
            }
        }
        if !flag {
            break
        }
    }
    return data, nil
}
```



### 插入
插入排序 分已排序区间 （data[0]） 和 未排序区间

每次取未排序区间内的第一个值，

循环对比 已排序区间 找到它要插入的位置

往后移动已排序区间进行插入

``` golang

func InsertSort(data []int) ([]int, error) {
    if len(data) == 0 {
        return []int{}, nil
    }
    count := len(data)
    for i := 0; i < count; i++ {
        value, j := data[i], i-1
        for ; j >= 0; j-- {
            if data[j] > value {
                data[j+1] = data[j]
            } else {
                break
            }
        }
        data[j+1] = value
    }
    return data, nil
}

```

### 选择

选择排序 分 已排序区间 [] - 未排序区间

循环每次在未排序区间内找最小的值加入到已排序区间的末尾
``` golang
func SelectionSort(data []int) ([]int, error) {
    if len(data) == 0 {
        return []int{}, nil
    }
    length := len(data)
    for i := 0; i < length-1; i++ {
        min := i
        for j := i; j < length; j++ {
            if data[min] > data[j] {
                min = j
            }
        }
        data[i], data[min] = data[min], data[i]
    }
    return data, nil
}
```


### 归并

``` golang 
func MergeSort(data []int)  {
   merge_sort_c(data, 0, len(data)-1)
}

func merge_sort_c(data []int, start, end int) {
    if start >= end {
        return
    }
    mid := (start + end) / 2
    merge_sort_c(data, start, mid)
    merge_sort_c(data, mid+1, end)

    merge_mutli(data, start, mid, end)
}

func merge_mutli(data []int, start, mid, end int){
    var left, right = start, mid + 1
    tmp := []int{}
    for left <= mid && right <= end {
        if data[left] <= data[right] {
            tmp = append(tmp, data[left])
            left++
        } else {
            tmp = append(tmp, data[right])
            right++
        }
    }
    if left <= mid {
        tmp = append(tmp, data[left:mid+1]...)
    }
    if right <= end {
        tmp = append(tmp, data[right:]...)
    }
    for pos, item := range tmp {
        data[start + pos] = item
    }
}
```


### 快排

``` golang
func QuickSortR(data []int, first, last int) {
    if first >= last {
        return
    }

    mid := partition(data, first, last)
    fmt.Printf("mid %d   value %d \n", mid, data[mid])
    QuickSortR(data, first, mid-1)
    QuickSortR(data, mid+1, last)
}

func partition(data []int, first int, last int) int {
    fmt.Printf("参数 first %d val  %d  last %d    value %d \n", first, data[first], last, data[last])
    i := first-1
    for j := first; j < last; j++ {
        if data[j] < data[last] {
            data[j], data[i+1] = data[i+1], data[j]
            i++
        }
    }
    data[i+1], data[last] = data[last], data[i+1]
    fmt.Printf("data %v \n", data)
    return i+1
}

```
