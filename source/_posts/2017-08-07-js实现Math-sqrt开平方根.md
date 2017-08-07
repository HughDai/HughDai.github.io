---
title: js实现Math.sqrt开平方根
date: 2017-08-07 18:16:30
layout: post
comments: true
categories: js
tags: [算法,数学]
keywords: 算法,数学
---
## 前言
有一次刷脉脉,在匿名区看到一道题目:二分法用实现Math.sqrt函数开平方根。想当年1.414、1.732、2.236、2.449背的滚瓜烂熟,甚至手写过开方计算。
<!-- more -->
## 分析
看到二分法就不由得想到数组的二分查找,在我的这篇博客[二分法查找](http://hughdai.github.io/2017/07/21/%E4%B8%80%E4%BA%9Bjs%E5%9F%BA%E7%A1%80%E7%AE%97%E6%B3%95/#查找)有代码。
二分法实现开方也是一样的道理,拿到最小值和最大值,取中间值,判断中间值的平方是否等于目标值,等于则返回,否则继续折半查找一步一步逼近正确值。
```js
        function sqrtBisection(n) {
            if (isNaN(n)) return NaN;
            if (n === 0 || n === 1) return n;
            var low = 0,
                high = n,
                pivot = (low + high) / 2,
                lastPivot = pivot;
            // do while 保证执行一次
            do {
                console.log(low, high, pivot, lastPivot)
                if (Math.pow(pivot, 2) > n) {
                    high = pivot;
                } else if (Math.pow(pivot, 2) < n) {
                    low = pivot;
                } else {
                    return pivot;
                }
                lastPivot = pivot;
                pivot = (low + high) / 2;
            }
            while (Math.abs(pivot - lastPivot))

            return pivot;
        }
```
然后我又google了一下发现大部分的解决方案都是用的[牛顿迭代](https://zh.wikipedia.org/wiki/%E7%89%9B%E9%A1%BF%E6%B3%95), 即切线逼近。高中大学学的那点数学知识基本上忘光了,公式看的我眼花,我就不献丑了,反正最后可以得出的方程式是 x = (x + n / x) / 2。

```js
        function sqrtNewton(n) {
            if (n < 0) return NaN;
            if (n === 0 || n === 1) return n
            var val = n,
                last;
            do {
                console.log(val, last)
                last = val;
                val = (val + n / val) / 2;
            }
            while (Math.abs(val - last))
            return val
        }
```
比较两种方法的时间复杂度
二分法的时间复杂度固然是O(logN),牛顿法的复杂度我就不太明了了。看看两个函数对2000开平方所耗时间吧。
```bash
console.time('二分法耗时')
sqrtBisection(2000)
console.timeEnd('二分法耗时')

result: 二分法耗时: 7.697998046875ms

console.time('牛顿法耗时')
sqrtNewton(2000)
console.timeEnd('牛顿法耗时')

result: 牛顿法耗时: 1.44580078125ms
```




