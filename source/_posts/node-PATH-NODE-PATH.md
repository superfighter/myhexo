---
title: node $PATH NODE_PATH
date: 2017-06-10 08:23:23
tags:
---
* 设置全局变量文件夹
    ````
        Error: vue command not found
        export PATH=/user/local/bin:$PATH`
        echo $PATH
    ````
* 设置指定$NODE_PATH
    ````
        Error: Cannot find module 'inherits'
        echo $NODE_PATH
        export $NODE_PATH=/user/local/lib/node_modules
    ````
