---
layout: post
title: 请把闭包介绍给我
categories: jekyll update
---

# 闭包 Closure

## 1、什么是闭包
最简单的闭包 demo code：
```
function A(){
    function B(){
    console.log("Hello Closure");
    }
    return B;
}
var c = A();
c(); //Hello Closure
```

用一句话概括这段code：**函数A的内部函数B被函数A外的一个变量c引用**。
再来看看闭包的定义：
**当一个内部函数被其外部函数之外的变量引用时，就形成了一个闭包。**
所以，这就是一个最简单的闭包。

## 2、闭包的作用
了解一下JavaScript的回收机制：如果一个对象不再被引用，就会被GC回收，否则会一直保存在内存中。
上面的一段code，B定义在A中，所以B依赖A而存在。变量c引用了B，则间接引用了A。所以，A不会被回收，将会一直保存在内存中。我想，最根本的原因是什么：**它还在被引用，它还被需要着，它还不能死**。

而这么做的目的是什么呢？很明显，因为写下代码的我不想它死。

有时候，我们需要在一个模块中定义这样一个变量：**希望这个变量一直保存在内存中，但又不会“污染”全局变量**，这是我们就可以用闭包来定义这个模块。

## 3、高端局怎么玩

这都是看别人写的，我不确定他玩的这个是什么分段，至少黄金以上。
code：
```
(function(document){
    var viewport;
    var obj = {
        init: function(id){
            viewport = document.querySelector("#"+id);
        },
        addChild: function(child){
            viewport.appendChild(child);
        },
        removeChild: function(child){
            viewport.removeChild(child);
        }
    }
    window.jView = obj; //很关键
})(document);
```
首先这是一个立即执行函数，分成两部分看：
`(function(){ })` 和 `( )`
第一个`( )`是一个表达式，这个表达式本身是一个匿名函数。
第二个`( )`表示立即执行这个函数。

假如匿名函数为f，obj是在f中定义的一个对象，这个对象中定义了一系列方法，执行`window.jView`就是在window全局变量定义了一个变量jView，并将这个变量只想obj对象。即
**全局变量jView引用了obj，而obj对象中的函数又引用了f中的变量viewport，因此f中的viewport不会被GC回收，一直保存在内存中，构成了闭包**。
# 我想 
其实他这么说我还是有一丝丝困惑的。我现在的理解是，闭包就是为了让一个你需要的对象活下来，又不会“污染”全局变量，其本质是与GC回收做抗争。在上例中，我们想让viewport活在内存里，又不能直接写入全局变量，于是把他写在一个函数里，开始搞猫腻。他存活下来只有一条理由，它还被需要着，于是要有一个变量去引用它。可是，若是直接引用它，引用过后就gg了。所以，只好间接的去引用它：**全局变量jView直接引用了obj对象，obj依赖f而存活，同样依赖viewport而存活，于是我们成功间接引用到了viewport变量，击败了GC回收。**

有不对的地方，以后会来修改。