---
title: Debounce, throttle原理及实现
date: 2020-12-04 09:43:10
tags: javascript
banner_img: /img/wallhaven-rd3pjw.jpg
categories: 笔记
---
``` js
/**
* @param fn {Function}   实际要执行的函数
* @param delay {Number}  延迟时间，也就是阈值，单位是毫秒（ms）
* @return {Function}     返回一个“去弹跳”了的函数
*/
function debounce(fn, delay) {
  var timer
  // 返回一个函数，这个函数会在一个时间区间结束后的 delay 毫秒时执行 fn 函数
  return function () {
    // 保存函数调用时的上下文和参数，传递给 fn
    var context = this
    var args = arguments
    // 每次这个返回的函数被调用，就清除定时器，以保证不执行 fn    clearTimeout(timer)
    if(timer) clearTimeout(timer)
    // 当返回的函数被最后一次调用后（也就是用户停止了某个连续的操作），
    // 再过 delay 毫秒就执行 fn
    timer = setTimeout(function () {
      fn.apply(context, args)
    }, delay)
  }
}
```
debounce 返回了一个闭包，这个闭包依然会被连续频繁地调用，但是在闭包内部，却限制了原始函数 fn 的执行，强制 fn 只在连续操作停止后只执行一次。

``` js
// 节流throttle代码（定时器）：
var throttle = function(func, delay) {            
    var timer = null;            
    return function() {                
        var context = this;               
        var args = arguments;                
        if (!timer) {                    
            timer = setTimeout(function() {                        
                func.apply(context, args);                        
                timer = null;                    
            }, delay);                
        }            
    }        
}
```
