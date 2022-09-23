---
title: "golang context"
date: 2022-09-23T15:32:18+08:00
draft: true
---


![](/static/golang/context.png)
``` golang
type Context interface {
  // 返回截止时间和是否设置的bool值
	Deadline() (deadline time.Time, ok bool)

  // 返回一个被关闭的管道，关闭的管道仍然是可读的，据此goroutine可以收到关闭请求； 当context还未关闭时，Done()返回nil。
	Done() <-chan struct{}

  // 关闭原因,未关闭返回nil
	Err() error

  // 用于传参 withValue
	Value(key interface{}) interface{}
}
```

* Context仅仅是一个接口定义，根据实现的不同，可以衍生出不同的context类型；
* cancelCtx实现了Context接口，通过WithCancel()创建cancelCtx实例； WithCancel() 设置可以退出的子节点
* timerCtx实现了Context接口，通过WithDeadline()和WithTimeout()创建timerCtx实例；WithDeadline() 设置截止时间、WithTimeout()设置超时时间
* valueCtx实现了Context接口，通过WithValue()创建valueCtx实例； 
* 三种context实例可互为父节点，从而可以组合成不同的应用形式；
---
* deadline: 指定最后期限，比如context将2018.10.20 00:00:00之时自动结束
* timeout: 指定最长存活时间，比如context将在30s后结束。

---

CancelFunc 是主动让下游结束，而 Done 是被上游通知结束。


### 锁


* sync.Mutex 互斥锁
  
  *Lock()加锁，Unlock()解锁

* sync.RWMutex 读写分离锁
  
  不限制并发读，只限制并发写和并发读写

* sync.WaitGroup

  等待一组 goroutine 返回 Add Done Wait

* sync.Once

  保证某段代码只执行一次

* sync.Cond

  让一组 goroutine 在满足特定条件时被唤醒

