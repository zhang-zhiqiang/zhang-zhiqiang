---
title: "二分查找"
date: 2022-09-23T15:18:26+08:00
draft: false
tags: ["二分查找"]
categories: ["算法"]
weight: 10
---

二分

[33](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
``` go
func search(nums []int, target int) int {
    l, r := 0, len(nums)-1
    for l <= r {
        mid := l + (r-l)/2
        if nums[mid] == target {
            return mid
        }

        if nums[mid] >= nums[l] {
            if nums[mid] > target && target >= nums[l] {
                r = mid - 1
            } else {
                l = mid + 1
            }
        } else {
            if nums[mid] < target && target <= nums[r] {
                l = mid + 1
            } else {
                r = mid - 1
            }
        }
    }
    return -1
}
```

[153](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)
```go
func findMin(nums []int) int {
    l, r := 0, len(nums) -1
    for l < r {
        mid := l + (r - l) / 2
        if nums[mid] < nums[r] {
            r = mid
        } else {
            l = mid + 1
        }
    }
    return nums[l]
}
```

