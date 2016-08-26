---
layout: post
title: "理解原型对象"
categories: JavaScript
---

# 理解原型对象

首先看一下创建对象的原型模式（注意Person要被玩坏了）：

```javascript
function Person() {} //空的构造函数Person()

Person.prototype.name = "Tom";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function() {
  alert(this.name);
}

var person1 = new Person();
var person2 = new Person();
```

这里要注意的一点是`alert(person1.sayName == person2.sayName)//true`

那，每个函数都有一个prototype属性，这个属性是一个指针，指向一个对象。这个对象的用途是**_包含可以由特定类型的所有实例共享的属性和方法_**。那这个对象其实就是原型对象。

### #1

Anytime，只要是创建了一个函数，就会根据一组特定的规则为该函数创建一个prototype属性，这个属性指向函数的原型对象。先看下图：

 ![](http://t1ree.github.io/images/prototype_1.png)

下面还有一部分，没有说到我就不看。那，Person的ptototype属性指向了Person.prototype这个原型对象，在这个原型对象里有我们刚才创建的属性和方法。

而，在函数建立之初，其原型对象会自动获得一个constructor属性，这个属性包含一个指针，指向prototype属性所在的函数。好了，下面是完整的图：


 ![](http://t1ree.github.io/images/prototype_2.png)



### #2

当创建了自定义的构造函数后，其原型对象默认只会取得constructor属性；至于其他方法，则是从Object继承而来的。

当调用构造函数创建一个新实例后，该实例的内容讲包含一个指针（内部属性），指向构造函数的原型对象（es5中该属性叫[[Prototype]、对于FF、Chrome、Safari在每个对象上支持一个属性\__proto__）。要明确的重要一点是：**这个连接存在于实例与原型对象之间而不是实例与构造函数之间**。

那，如何确定这种关系呢？  isPrototypeOf() 大法好。

`alert(Person.prototype.isPrototypeOf(person1));//true`

以及新方法Object.getPrototypeOf()，可以返回对象[[Prototype]]的值：

`alert(Object.getPrototypeOf(person1) == Person.prototype);//true`

### #3

当代码读取某个对象的某个属性时，都会执行一次搜索。首先从对象实例自身开始，然后搜索指针指向的原型对象。所以，实例中属性的值时优先使用的。

那，如何区分实例中的属性和原型对象中的属性呢？hasOwnProperty()大法好。

```javascript
function Person() {}
Person.prototype.name = "Nick";
var person1 = new Person();
alert(person1.hasOwnProperty("name")); //false

person1.name = "Greg";
alert(person1.hasOwnProperty("name")); //true 实例自身的属性才会返回true
```

