---
title: Jest-2
date: 2017-08-06 15:02:01
tags: jest
---
### 前言
---
[Jest-1]()主要讲述了Jest冢对Expect的用法，这篇[Jest-2]()将讲述重要的mock部分
### Mock
---
单元测试的主要做的事情给予我们想给的，得到我们期望的。现在的业务代码中都是由各个模块串联而成，你包我，我包他，一个包可能很大很长，但最终会抛出公共接口。我们在测试引入包时，不需关心内部具体实现，这时可以使用Mock功能，直接给出期望的结果。
除主测之外的代码，都可以Mock。
### Function Mock
---
在上一篇中的Function里也提到jest.fn，这里细说：
```javascript
    it('mockFn.mock.calls', () => {
        const f = jest.fn()
        f(1, 2)
        f(2, 3)
        // 调用过程存入集合
        expect(f.mock.calls).toEqual(expect.arrayContaining([[1, 2], [2, 3]]))
    })
    it('mockFn.mock.instances', () => {
        const f = jest.fn()
        const a = new f()
        const b = new f()
        // 引用计数
        expect(f.mock.instances[0]).toBe(a)
    })
    it('mockFn.mockClear', () => {
        function f() {
            return 3
        }
        f = jest.fn(()=>4)
        const a = new f()
        const b = new f()
        f(2)
        expect(f.mock.calls[2]).toEqual([2])
        // 清空执行轨迹
        f.mockClear()
        expect(f.mock.instances).toEqual([])
        expect(f.mock.calls).toEqual([])
        // f.mockRestore()
        // 重置mock.fn
        f.mockReset()
        expect(f()).toBeUndefined()
    })
    it('mockFn.mockImplementationOnce', () => {
        var myMockFn = jest.fn()
            .mockImplementationOnce(cb => cb(null, true))
            .mockImplementationOnce(cb => cb(null, false))
        // 可逐一调用
        expect(myMockFn((err, val) => val)).toBeTruthy()
        expect(myMockFn((err, val) => val)).toBeFalsy()
        expect(myMockFn()).toBeUndefined()
    })
    it('mockFn.mockReturnValueOnce', () => {
        const myMockFn = jest.fn()
        // 只执行一次，后重置
        myMockFn.mockReturnValueOnce(90)
        expect(myMockFn()).toBe(90)
        expect(myMockFn()).toBeUndefined()
    })
```

### Class Mock
---
jest.fn是单个函数的模拟，一个具体的Module可能会抛出多个接口，同样可以模拟：
```javascript
    jest.mock('../request', (url) => {
         let users = {
             4: {name: 'Mark'},
             5: {name: 'Paul'}
         }
         return new Promise((resolve, reject) => {
             if (url) {
                 const userId = url.substr('/users/'.length)
                 process.nextTick(
                     () => users[userId]
                         ? resolve(users[userId])
                         : reject({
                             error: 'User with '+ userId +' not found.' 
                         })
                 )
             }
         })
    })
```
还有一种形式：
```javascript
    jest.mock('../request')
    // 接下来针对需要自定义的方法单独jest.fn
    jest.default = jest.fn()
```
### 计时器Mock
---
计时器经常在业务中用到，`setTimeout`,`setInterval`,`clearTimeout`,`clearInterval`;要是在测试中按真实的设置等下去那就疯啦，所以facebook的天才们提供了模拟方案，即做了延迟处理又不用等多时：
```javascript
    // 启用timer mock
    jest.useFakeTimers()
    // 调用全部timer mock
    jest.runAllTimers()
    // 运行pending中的timers，如一个定时器（1000）的回调触发了另一个定时器，这个后触发的定时器（10000ms）处于pending
    jest.runOnlyPendingTimers()
    // 快进
    jest.runTimersToTime(1000)
    // 清空pending的timers
    jest.clearTimers()
```
### 小结
---
以上讲了function mock、class mock以及timers mock的种种api，在实际测试环境中出场率还是挺高的。接下一节将介绍异步检测，非常重要哟。