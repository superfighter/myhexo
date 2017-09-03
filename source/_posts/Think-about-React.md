---
title: Think about React
date: 2017-08-27 09:24:27
tags:
	- react
---
## 前言
---
近一个月开始着手用React重构项目，从0开始到最终成型，从无头绪到摸到套路还是挺坎坷的，有必要把所有的思考在这里记录。
### 业务场景
---
原先的项目是用jquery写的，业务逻辑不复杂，总的就是取数据做展示。抽象出了若干组件：

* 导航
* 表单
    * 日期
    * input
    * select
    * check
    * autoComplete
* 列表
    * table
    * page

### 注意点
<!-- more -->
---
* 组件通信。`这一点是贯穿全局的，必须对此运筹帷幄。`一句话概括就是props和state的串联整个信息传递链。

````
	// app.js
    constructor() {
        this.state = {
            props1: 1,
            props2: 2,
            props3: 3,
            props4: 4
        }
    }
    render() {
        const {props1, props2, props3, props4} = this.state;
        return (
            /* ...other component*/
            <parent props1={props1} props2={props2}/>
            /* ...other component*/
        )
    }
````
````
	// parent.js
    constructor(props) {
        super(props);
        this.state = {
            props1: 1,
            props2: 2,
            clicked: false
        }
    }
    handleChange(e) {
        // 这里处理联动，改变state值，触发render继而改变props值，向下传递信息
        this.setState({
            props1: e.target.value
        })
    }
    handleClick() {
        // 可继续向上传递，也可直接处理信息终止传递
        this.setState({
            clicked: true
        })
    }
    render() {
        const {props1, props2, clicked} = this.state;
        const {handleChange} = this;
        return (
            <div>
            {clicked &&
                (<span>小的被点了！</span>)
            }
            <input onChange={handleChange} type="text">
            // 传两个props给子组件
            <child props1={props1} props2={props2} onClick={handleClick}/>
            </div>
        )
    }
````
````
	// child.js
    constructor(props) {
        super(props);
        this.state = {
            props1: props.props1,
            props2: props.props2
        }
    }
    componentWillReceiveProps(nextPros) {
        // 这里收到父组件改变了props，也同步改变自身state，触发render
        if (nextPros.props1 !== this.props.props1) {
            this.setState({
                props1: nextProps.props1
            })
        }
    }
    handleClick() {
        // 这里通过props向上传递信息
        this.props.onClick();
    }
    render() {
        const {props1, props2} = this.state;
        const {handleClick} = this;
        return (
            <div>
                <span onClick={handleClick}>from parent props1: {props1}</span>
                <span>from parent props2: {props2}</span>
            </div>
        )
    }
````

### 小结
---
先总结组件通信，表现在父子通信和兄弟通信。其他方面主要表现在组件抽象层次上，抽象的好有利于props和state无缝串联，抽象不当就会处理特别扭，这里要在再思考思考，开发组件前打好底稿非常重要。
