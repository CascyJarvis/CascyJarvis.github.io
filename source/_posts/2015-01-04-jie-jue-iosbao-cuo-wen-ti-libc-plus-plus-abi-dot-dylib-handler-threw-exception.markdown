---
layout: post
title: "解决ios报错问题:libc++abi.dylib handler threw exception"
date: 2015-01-04 14:34:47 +0800
comments: true
categories: 
---
在iOS开发时，有时候遇到`libc++abi.dylib handler threw exception`这样的异常，但是没打印出相对应的日志，实际上不是这段代码的问题。因此不知道什么地方出错了。这时候可以用捕获异常的方式来打印异常log：

```
@try{
 	//Error code here.
    }
    @catch(NSException *exception) {
        NSLog(@"exception:%@", exception);
    }
    @finally {
        
    }
```