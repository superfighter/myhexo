---
title: Jest-3
date: 2017-08-06 19:59:33
tags: jest
---
## 前言
---
[Jest-1]()、[Jest-2]()分别讲了断言基础和Mock基础，满足了大部分同步测试的需求。这节主要描述异步测试的情况，主要场景是回调、Promise以及async/await：
### 测试代码
---
```javascript
    /** __mocks__/requestfks.js **/
    const users = {
        4: {name: 'Mark'},
        5: {name: 'Paul'}
    }
    export default function request(url) {
        return new Promise((resolve, reject) => {
            const userId = parseInt(url.substr('/users/'.length), 10)
            process.nextTick(
                () => users[userId]
                    ? resolve(users[userId])
                    : reject({
                        error: 'User with '+ userId +' not found.' 
                    })
            )
        })
    }
```
```javascript
    /** requestfks.js **/
    const http = require('http')

    export default function request(url){
        return new Promise(resolve => {
            http.get({path: url}, response => {
                let data = ''
                response.on('data', _data => (data += _data))
                response.on('end', () => resolve(data))
            })
        })
    }
```
```javascript
    /** user.js **/
    import request from './requestfks'

    export function getUserName (userId) {
        return request('/users/' + userId).then(user => user.name)
    }
```
### 异步回调
---
这里引用官网的例子，重点是要引入内置参数done：
```javascript
    test('the data is peanut butter', done => {
      function callback(data) {
        expect(data).toBe('peanut butter');
        // 在回掉执行完成时调用done方法通知Jest异步过程结束
        done();
      }
    
      fetchData(callback);
    });
```
### Promise
---
```javascript
    const resolveFunc = jest.fn((d) => {
        return new Promise((resolve, reject) => {
            resolve(d)
        })
    })
    const rejectFunc = jest.fn((d) => {
        return new Promise((resolve, reject) => {
            reject(d)
        })
    })
    const catchFunc = jest.fn((d) => {
        return new Promise((resolve, reject) => {
            resolve(d+o)
        }).catch((d) => {
            throw new Error(d.message)
        })
    })
    it('callback', function () {
        console.time('done time')
        setTimeout(function (){
            expect(1).toBeTruthy()
            expect(2).toBe(2)
            expect(1).toBeFalsy()
            console.timeEnd('done time')
        }, 2000)
        jest.runOnlyPendingTimers()
    })
    it('promise resolve', () => {
        return expect(resolveFunc(1)).resolves.toBe(1)
    })
    it('promise reject', () => {
        return expect(rejectFunc(1)).rejects.toBe(1)
    })
    it('promise catch', () => {
        return expect(catchFunc(2)).rejects.toEqual(new Error('o is not defined'))
    })
    var states = {
        top: false,
        bottom: false,
        right: false
    }
    function prepareState(cb) {
        // 异步处理多个state
        setTimeout(function(){
            states = {
                top: true,
                right: true,
                bottom: true
            }
            cb()
        },3000)
        jest.runAllTimers()
    }

    function validateState() {
        return states.top && states.bottom && states.right
    }
    function waitOnState() {
        expect(states).toHaveProperty('top', true)
        return new Promise((resolve, reject) => {
            setInterval(() => {
                console.log('...waiting')
                if (states.top && states.bottom && states.right) {
                    resolve()
                }
            }, 1000)
            jest.runAllTimers()
        }).then(() => {
            console.log(states)
        })
    }
    it('hasAssertions', () => {
        expect.hasAssertions()
        prepareState(function(){
            expect(validateState(states)).toBeTruthy()
        })
        return waitOnState()
    })
    it('assertions 2', () => {
        expect.assertions(2)
        prepareState(function(){
            expect(validateState(states)).toBeTruthy()
        })
        return waitOnState()
    })
```
### async/await
---
```javascript
    jest.mock('../requestfks')
    import * as user from '../user'
    it('async/await', () => {
        expect.assertions(1)
        return user.getUserName(15).catch((data) => {
            expect(data).toEqual({"error": "User with 15 not found."})
        })
    })
    it('async/await', async () => {
        expect.assertions(1)
        await expect(user.getUserName(5)).resolves.toEqual('Paul')
    })
    it('async/await rejects', async () => {
        expect.assertions(1)
        await expect(user.getUserName(3)).rejects.toHaveProperty('error')
    })
    it('async/await resolves', async () => {
        expect.assertions(1)
        const data = user.getUserName(4)
        await expect(data).resolves.toEqual('Mark')
    })
    it('Promise.catch', async () => {
        expect.assertions(1)
        return user.getUserName(2).catch(e => {
            expect(e).toHaveProperty('error')
        })
    })
    it('try/catch', async () => {
        expect.assertions(1)
        try {
            await user.getUserName(12)
        } catch (e) {
            expect(e).toHaveProperty('error')
        }
    })
```
### 小结
---
本节介绍了异步测试的代码，是不是可以开始实际业务情景测试了？当然可以，但是Jest并不止这些内容。下节先介绍Jquery业务场景的登录测试。