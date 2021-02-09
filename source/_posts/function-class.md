---
title: 面向对象 + 继承
categories:
- 面向对象
tags:
- 面向对象
---
## 一、什么是面向对象: 
1.是一种编程思想，JS本身就是基于面向对象构建出来的。
2.特征:拥有属性与方法的数据
3.生活例子：
        一个人有:名字、年龄、会吃饭、会看电视、洗碗、拖地等。
        那么人的属性：名字和年龄
               方法：吃饭、看电视、洗碗、拖地
4.代码例子：
```
  function People(name, age) {
        //属性
        this.name = name;
        this.age = age;
        //方法
        this.havingDinner = function() {
            alert("吃饭")
        };
        this.watchTv = function() {
            alert("看电视")
        }
    }
```
5.注意：每个对象都有一个prototype(原型对象) , 里面包含着constructor与\__proto__。
     constructor属性：指向创建函数
       \__proto__属性：继承上一级父类对象，里面也包含着constructor与\__proto__一直的往上继承直接到object

              

## 二、继承：
1.子类继承父类的属性和方法
2.依靠prototype属性来继承


#### 1.原型继承
```
// 创建一个函数
    function Dog(name, age) {
        this.name = name;
        this.age = age;
    }
    Dog.prototype.nameLog = function() {
        console.log("名字:", this.name);
    }
    Dog.prototype.ageLog = function() {
        console.log("年龄:", this.age);
    }

    // 创建一个新函数
    function newDog(addr) {
        this.addr = addr; //新增属性
    }

    newDog.prototype = new Dog("名字" , "年龄"); //继承Dog(指向Dog实例含（\__proto__）) , 但缺少constructor
    newDog.prototype.constructor = newDog; //添加constructor属性保证原型的完整
    // 新增方法
    newDog.prototype.addrLog = function() {
        console.log("地址:", this.addr);
    }

    // 调用继承与自身
    var extendsDog = new newDog("二哈");
    extendsDog.nameLog();
    extendsDog.ageLog();
    extendsDog.addrLog();
```
缺点：继承所有的方法与属性都为"公有"

#### 2.call与apply继承
```
// 创建一个函数
    function Dog(name, age) {
        this.name = name;
        this.age = age;
    }
    // 创建一个新函数
    function newDog(name, age, addr) {
        call与apply的区别：传参方式
        Dog.call(this, name, age) 或者 Dog.call(this, [name, age]) //相当于this.name = name\this.age = age
        this.addr = addr; //新增属性
    }
    // 调用继承与自身
    var extendsDog = new newDog("testName", "testAge", "外国");
    console.log(extendsDog);
```
缺点：只能继承“私有”

#### 3.寄生组合继承(call + Object.create)
1.Object.create(obj)方法：创建一个新对象，使用现有的对象来提供新创建的对象的\__proto__。
```
 // 创建一个对象
    function Dog(name, age) {
        this.name = name;
        this.age = age;
    }
    Dog.prototype.nameLog = function() {
        console.log("名字:", this.name);
    }
    Dog.prototype.ageLog = function() {
        console.log("年龄:", this.age);
    }

    // 创建一个新对象
    function newDog(name, age, addr) {
        Dog.call(this, name, age)
        this.addr = addr; //添加新属性
    }

    // 创建一个空对象{} , 里面包含__proto__属性，指向参数对象
    newDog.prototype = Object.create(Dog.prototype);
    //在空对象中添加constructor属性指向，保证原型的完整
    newDog.prototype.constructor = newDog;
    newDog.prototype.addrLog = function() {
        console.log("地址:", this.addr);
    }

    // 调用继承与自身
    var extendsDog = new newDog("二哈", 9, "外国");
    extendsDog.nameLog();
    extendsDog.ageLog();
    extendsDog.addrLog();
```
好处：父类私有和公有的分别是子类实例的私有和公有属性方法

#### 4.原型链继承 + call继承
```
// 创建一个对象
    function Dog(name, age) {
        this.name = name;
        this.age = age;
    }
    Dog.prototype.nameLog = function() {
        console.log("名字:", this.name);
    }
    Dog.prototype.ageLog = function() {
        console.log("年龄:", this.age);
    }

    // 创建一个新对象
    function newDog(name, age, addr) {
        Dog.call(this, name, age) //相当于 this.name = name\this.age = age
        this.addr = addr; //为自身添加属性
    }
    newDog.prototype = new Dog(); //继承Dog(指向Dog实例含（__proto__）) , 但缺少constructor
    newDog.prototype.constructor = newDog; //把constructor指向自己(更正constructor)

    // 新增方法
    newDog.prototype.addrLog = function() {
        console.log("地址:", this.addr);
    }

    // 调用继承与自身
    var extendsDog = new newDog("二哈", 9, "外国");
    extendsDog.nameLog();
    extendsDog.ageLog();
    extendsDog.addrLog();
```
好处：父类私有和公有的分别是子类实例的私有和公有属性方法


#### Class继承（ES6）
在没有出现Class之前，Function既是构造函数同时也是类。
所以Class出现好处之一 ， 把我们之前的function“归为一类”。
```
    // 创建一个类
    class People {
        constructor(name, age) {
            this.name = name;
            this.age = age;
        }
        nameLog() {
            console.log("姓名：", this.name);
        }
        ageLog() {
            console.log("年龄:", this.age);
        }
    }

    // 创建一个新类
    class newPeople extends People {
        constructor(name, age, addr, salary) {
                super(name, age);
                // 新增属性
                this.addr = addr;
                this.salary = salary;
            }
            // 新增方法
        addrLog() {
            console.log("地址：", this.addr);
        }
        salaryLog() {
            console.log("薪资:", this.salary);
        }
    }
    var extendsPeople = new newPeople("zqy", 23, "湛江", 0.00);
    // 调用继承与自身
    extendsPeople.nameLog();
    extendsPeople.ageLog();
    extendsPeople.addrLog();
    extendsPeople.salaryLog();
```
如图：

![Image text](https://s1.ax1x.com/2020/04/28/Jh7Xo6.png)



以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~





