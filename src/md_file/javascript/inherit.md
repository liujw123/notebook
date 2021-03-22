# 原型继承

> 凡事函数作为独立函数调用，无论它的位置在哪里，它的行为表现，都和直接在全局环境中调用无异

----------

### 寄生式继承

```js 

    function createAnother(original){
        var clone = Object.create(original);    // 通过调用函数创建一个新对象
        clone.sayHi = function(){               // 以某种方式来增强这个对象
            console.log( "Hi ! " + this.name );
        };
        
        return clone;                        // 返回这个对象
    }

    var person = {
        name: "Bob",
        friends: [ "Shelby", "Court", "Van" ]
    };
    var anotherPerson = createAnother(person);
    // anotherPerson.sayHi();

```

-----------------------


### 寄生组合式继承

```js

    function SuperType(name){
        this.name = name;
        this.colors = ["red", "blue", "green"];
    }

    SuperType.prototype.sayName = function(){
        console.log(this.name);
    }

    function SubType(name, age){
        SuperType.call(this, name);　　//第二次调用SuperType()
        this.age = age;
    }

    SubType.prototype = new SuperType();　　//第一次调用SuperType()

    SubType.prototype.sayAge = function(){
        console.log(this.age);
    }

    SubTypea = new SubType('L',2);
    SubTypea.sayName();
    SubTypea.sayAge();

```

---

### 组合继承

```js

    function SuperType(name){
        this.name = name;
        this.colors = ['red','blue','green'];   
    }

    SuperType.prototype.sayName = function(){
        alert(this.name);
    }

    function SubType(name,age){   
        SuperType.call(this,name);     //第二次调用SuperType() 
        this.age = age;
    }

    SubType.prototype = new SuperType();    //第一次调用SuperType()
    SubType.prototype.constructor = SubType;  
    SubType.prototype.sayAge=function(){
        alert(this.age);
    };

    // var instance = new SubType('Greg',39);     //调用SubType构造函数，重写原型属性
    // instance.colors.push('black');      //重写原型属性
    // console.warn(instance.colors)


```

--- 

### 寄生组合式继承

```js

    function Human(name){
        this.name = name;
        this.colors = ['white', 'yellow', 'black'];
    }

    Human.prototype.sayName = function(){
        return this.name;
    }
    
    function Person(name, job){
        //继承属性
        Human.call(this, name);
        //定义自己的属性
        this.job = job;
    }

    //继承方法
    Person.prototype = new Human();
    Person.prototype.constructor = Person;

    //定义自己的方法
    Person.prototype.sayJob = function(){
        return this.job;
    }
    
    // let p = new Person('bob', 'JavaScript');
    // console.log(Person.prototype);//bob

```


--- 


### 简化的 `mixin(..)` 示例：

```js 

    function mixin( sourceObj, targetObj ) {
        for (var key in sourceObj) {
            // 仅拷贝非既存内容
            if (!(key in targetObj)) {
                targetObj[key] = sourceObj[key];
            }
        }
        return targetObj;
    }

    var Vehicle = {
        engines: 1,
        ignition: function() {
            console.log( "Turning on my engine." );
        },
        drive: function() {
            this.ignition();
            console.log( "Steering and moving forward!" );
        }
    };

    var Car = mixin( Vehicle, {
        wheels: 4,
        drive: function() {
            Vehicle.drive.call( this );
            console.log( "Rolling on all " + this.wheels + " wheels!" );
        }
    } );

    // Car.drive();

```