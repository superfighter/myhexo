---
title: Jest-1
date: 2017-08-06 15:02:01
tags: jest
---
## 前言
---
本系列内容基本上按照官网的示例来讲解如何入门。
## 匹配器
---
首先讲匹配器，是单元测试最基础的部分，一般称为断言，如：assert，tap，chai，should等断言库。Jest内置了断言库，正式场景中不需引入断言库。
我们从基本类型讲起，
### 字符串
---
```javascript
    it('"" toBe ""', () => {
        expect('').toBe('')
    })
    it('"" toEqual ""', () => {
        expect('').toEqual('')
    })
    it('"1" not toBe ""', () => {
        // 逆向断言
        expect('1').not.toBe('')
    })
    it('"1" not toEqual ""', () => {
        expect('1').not.toEqual('')
    })
    it('"1" not toEqual 1', () => {
        expect('1').not.toEqual(1)
    })
    it('"1" not toBe 1', () => {
        expect('1').not.toBe(1)
    })
    it('"hello world" toMatch "/world/"', () => {
        // 正则匹配器
        expect('hello world').toMatch(/world/)
    })
    it('"hello world" not toMatch "/world~/"', () => {
        expect('hello world').not.toMatch(/world~/)
    })
    it('"hello world" toContain "world"', () => {
        // 交集
        expect('hello world').toContain('world')
    })
    it('"hello world" is String', () => {
        expect('hello world').toEqual(expect.any(String))
    })
    it('"hello world" is not Number', () => {
        // 类型判断
        expect('hello world').not.toEqual(expect.any(Number))
    })
    it('"hello wordl" toHaveLength 11', () => {
        // 长度判断
        expect('hello wordl').toHaveLength(11)
    })
```
### 数字
---
```javascript
    it('12 toBe 12', () => {
        expect(12).toBe(12)
    })
    it('0.1+0.2 not toBe 0.3', () => {
        expect(0.1+0.2).not.toBe(0.3)
    })
    it('0.1+0.2 toBeCloseTo 0.3', () => {
        // 无限接近
        expect(0.1+0.2).toBeCloseTo(0.3)
    })
    it('0.1+0.2 toBeGreaterThan 0.3', () => {
        expect(0.1+0.2).toBeGreaterThan(0.3)
    })
    it('0.1+0.2 not toBeLessThan 0.3', () => {
        expect(0.1+0.2).not.toBeLessThan(0.3)
    })
    it('1+2 toBeGreaterThanOrEqual 3', () => {
        expect(1+2).toBeGreaterThanOrEqual(3)
    })
    it('1+2 toBeLessThanOrEqual 3', () => {
        expect(1+2).toBeLessThanOrEqual(3)
    })
```
### 数组
---
```javascript
     test('[1,2,3,4,500] not toBe [1,2,3,4,500]', () => {
         expect([1,2,3,4,500]).not.toBe([1,2,3,4,500])
     })
     test('[1,2,3,4,500] toEqual [1,2,3,4,500]', () => {
         expect([1,2,3,4,500]).toEqual([1,2,3,4,500])
     })
     test('[1,2,3,4,500] toContain 500', () => {
         expect([1,2,3,4,500]).toContain(500)
     })
     test('[1,2,3,4,500] not toContain "500"', () => {
         expect([1,2,3,4,500]).not.toContain('500')
     })
     test('[1,[2],3,4,500] not toContain 2', () => {
         expect([1,[2],3,4,500]).not.toContain(2)
     })
     test('[1,[2],3,4,500] toContain [2]', () => {
         // 多维数组交集多个
         expect([1,[2],3,4,500]).toEqual(expect.arrayContaining([[2]]))
     })
     test('[1,3,4,500] arrayContanin [3,4]', () => {
         // 一维数组交集多个
         expect([1,[2],3,4,500]).toEqual(expect.arrayContaining([3,4]))
     })
     it('["hello wordl"] toHaveLength 1', () => {
         expect(['hello wordl']).toHaveLength(1)
     })
     it('stringMatching', () => {
         const helloworld = [
             'hello',
             'world'
         ]
         const sm = [
             expect.stringMatching(/^hell/),
             expect.stringMatching(/^wo/),
         ]
         // 遍历交集
         expect(helloworld).toEqual(expect.arrayContaining(sm))
     })
```
### 布尔
---
```javascript
     it('false toBeFalsy', () => {
         expect(false).toBeFalsy()
     })
     it('"" toBeFalsy', () => {
         expect("").toBeFalsy()
     })
     it('0 toBeFalsy', () => {
         expect(0).toBeFalsy()
     })
     it('null toBeFalsy', () => {
         expect(null).toBeFalsy()
     })
     it('undefined toBeFalsy', () => {
         expect(undefined).toBeFalsy()
     })
     it('NaN toBeFalsy', () => {
         expect(NaN).toBeFalsy()
     })
     it('everything toBeTruthy', () => {
         expect(1).toBeTruthy()
     })
```
### 函数
---
```javascript
     function sum(a) {
         return a
     }
     // 基于mock fn
     const func = jest.fn((a, b)=>(a + b))
     const func2 = jest.fn((a, b)=>(a + b))
     it('func toHaveBeenCalledWith 3, 2', () => {
         func(3, 2)
         // 调用时的参数3，2
         expect(func).toHaveBeenCalledWith(3, 2)
     })
     it('func toHaveBeenCalled', () => {
         // 已被调用
         expect(func).toHaveBeenCalled()
     })
     it('func toHaveBeenLastCalled', () => {
         func(9, 9)
         // 最后一次被调用
         expect(func).toHaveBeenLastCalledWith(9, 9)
     })
     it('func2 toHaveBeenCalled', () => {
         expect(func2).not.toHaveBeenCalled()
     })
     it('func toHaveBeenCalledTimes 2', () => {
         // 被调用两次
         expect(func).toHaveBeenCalledTimes(2)
     })
     it('func toThrowError ', () => {
         const throwFunc = function (data){
             throw new Error('...error')
         }
         // 抛出错误
         expect(throwFunc).toThrowError('...error')
     })
     it('func toThrow', () => {
         expect(() => {
             throw new Error()
         }).toThrow()
     })
```
### 对象
---
```javascript
     it('{hello: "world"} toHaveProperty hello', () => {
         const helloworld = {hello: 'world'}
         // 属性检测
         expect(helloworld).toHaveProperty('hello')
     })
     it('{hello: "world"} not toHaveProperty hello:"google"', () => {
         const helloworld = {hello: 'world'}
         expect(helloworld).not.toHaveProperty('hello', 'google')
     })
     it('{hello: {world: "one"} deep referencing hello.world', () => {
         const helloworld = {hello: {world: 'one'}}
         // deep属性
         expect(helloworld).toHaveProperty('hello.world', 'one')
     })
     it('[{hello: 1, world: 2, every: 3}] toContainEqual {world: 2, every: 3}', () => {
         // 数组或者是可被数组化（Array.from）的类数组对象
         const helloworldevery = [{
             world: 2,
             every: 3
         },{
             hello: 1,
             world: 2
         },{
             every: 3
         }]
         const worldevery = {
             world: 2,
             every: 3
         }
         // 部分对象检测
         expect(helloworldevery).toContainEqual(worldevery)
     })
     it('现有的对象满足我期望的对象', () => {
         const feature = {
             height: 177,
             width: 90,
             padding: {
                 top: 20,
                 bottom: 20
             },
             content: ['h','e','l','l','o']
         }
         const current = {
             height: 177,
             width: 90,
             padding: {
                 top: 20,
                 bottom: 20,
                 left: 20,
                 right: 20
             },
             content: ['h','e','l','l','o'],
             checked: false
         }
         // 整个对象集合检测
         expect(current).toMatchObject(feature)
     })
     it('A toBeInstanceOf Class A', () => {
         function A() {}
         function B() {}
         const newA = new A()
         // 实例检测
         expect(newA).toBeInstanceOf(A)
     })
```
### 其他
---
```javascript
    it('undefined toBeUndefined', () => {
        expect(undefined).toBeUndefined()
    })

    it('null toBeUndefined', () => {
        expect(null).toBeNull()
    })
```
### 小结
---
以上总的讲述了Expect基本的使用API，接下来讲一讲另一个重要的方法mock。