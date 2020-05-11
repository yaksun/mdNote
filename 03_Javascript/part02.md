## 01.数据类型

* 基本数据类型`string number boolean undefined null`

  ```js
  //使用typeof判断 
  var a
  console.log(typeof a)
  //typeof 可以区分string number boolean undefined Function
  //但不能区分 null Array Object(Function是对象但能判断)
  ```

* 引用数据类型`Object Function Array `

  Function和Array 也是Object,是特殊的对象

  ```js
  //主要使用instanceof判断
  var b={
      c:[],
      d:function(){
          return function(){
              
          }
      }
  }
  console.log(b instanceof Object)
  //当是函数类型时,加上()就是执行
  b.d()()
  ```
## 02.内存,变量和数据
内存:存储数据的临时空间
多个引用变量指向同一个对象,通过一个变量修改对象内部数据,其它所有变量看到的是修改之后的数据
2个引用变量指向同一个对象,让其中一个引用变量指向另一个对象,另一个引用变量依然指向前一个对象

```js
var a={age:12}

a={name:'yaksun',age:20}

//function fn2(obj){
//    obj.age=18
//}

//fn2(a)
//console.log(a.age) // 18

function fn(obj){
	obj = {age:15}
}

fn(a)

console.log(a.age) // 20
```

```js
var b = 3 
// 传参的时候相当于变量赋值
// 两个独立的变量,独立的操作
// 都是值(基础数据或引用地址)传递
function fn(b){
	b = b+1
}

fn(b)

console.log(b) //3
```

## 03.释放内存

* 局部变量:函数执行完自动释放

* 对象:成为垃圾对象=》垃圾回收器回收

* 内存的生命周期

  分配小内存空间,得到它的使用权

  存储数据,可以反复进行操作

  释放小内存空间

## 04.函数执行

* 声明方式

  ```js
  //直接声明
  function fn1(){
  console.log('fn1')
  }
  //表达式声明 变量提升
  var fn2 = function(){
      console.log('fn2')
  }
  ```
  
* 调用方式:直接调用.对象调用.new调用.call/apply调用

  ```js
  //临时成为某个对象的方法
  var obj={} 
  
  function test2(){
      this.xxx = 'hhhe'
  }
  //call/apply 使test2成为obj的方法
  test2.call(obj)
  
  console.log(obj.xxx)
  ```
## 05.匿名函数自调用  

```js
//作用 
//隐藏实现
// 不会污染外部(全局)命名空间
(function(){

})()
```

  

```js
(function(){
	var a=1 
    function test(){
        console.log(++a)
    }
    
    window.$ = function(){
        return{
            test:test
        }
    }
})()

$().test()
```

##  06.`this`指向问题

* this是什么
  * 任何函数本质上都是通过某个对象来调用的,如果没有直接指定就是window
  * 所有函数内部都有一个变量this
  * 它的值是调用函数的当前对象
* 如何确定this的值
  * test()：window
  * p. test() :  p
  * new test() : 新创建的对象实例
  * p. call(obj) : obj

## 07.原型对象

* 函数的prototype属性
  * 每个空函数都有一个prototype属性,它默认指向一个Object空对象(原型对象)
  * 原型对象中有一个属性constructor,它指向函数对象(相互引用)
  
* 给原型对象添加属性(一般都是方法)

  * 作用:函数的所有实例对象自动拥有原型中的属性(方法)

  * 实例对象可以访问

    ```js
    function Person(){
    
    }
    
    Person.prototype.test=function(){
        console.log('给原型对象添加方法')
    }
    
    var p1 = new Person()
    p1.test()
    ```
  
 * __proto__属性

   * 每个实例对象都有一个`__proto__`,可以称为隐式原型,实例化创建对象时自动添加的

   * 每个函数都有一个prototype,即显式原型,在定义函数时自动添加的

   * 两者都是引用类型数据,地址值指向同一个原型对象


## 08.原型链(也叫隐式原型链)

* 访问一个属性时,先在自身属性中查找,找到返回
* 如果没有,再沿着`__proto__`这条链向上查找,找到返回
* 如果最终没有找到,返回undefined
* 作用:查找对象的属性(方法)
* 实例对象的隐式原型是构造函数的显示原型
* Object,Function,Array都是全局变量

* 所有函数都是Function的实例(包括Function自身),所以才有`Function.prototype ==Function.__proto__`

* 读取对象的属性值时,会自动到原型链(隐式)中查找

* 设置对象的属性时,不会查找原型链,如果当前对象没有此属性,直接添加此属性并设置值

* 方法一般定义在原型中,属性一般通过构造函数定义在对象本身上
## 08.探索instanceof的本质

* 本质就是实例A的`__proto__`和构造函数B的portotype，指向的是同一个地址，也叫做A是B的实例

  `A instanceof B`为true

* Function.prototype是Object()的实例
* Object() 是 Function()的实例
* Function() 是 Function() 的实例
* 所有函数 是Function() 的实例

## 09.变量和函数提升

* 通过var定义的变量,在定义语句前就可以访问到值:undefined(打印window可以看到)
* 通过function声明的函数,在之前就可以直接访问(必须是直接声明的函数)
* 函数提升高于变量提升

```js
    var a=2
    function fn() 
        console.log(a);
       // 先找本作用域的
       var a=4
    }
	//相当于
     function fn() {
         var a
         console.log(a);
      	a=4
    }
    fn() // undefined
```

```js
fn()

// 以表达式变量形式声明,会造成变量提升而访问不到
var fn=function(){
console.log('fn')
}
```

## 10.全局执行上下文和函数执行上下文

* 全局执行上下文
  * var 定义的全局变量等于undefined,添加到window的属性
  *  function 声明的全局函数===》赋值(fun)，添加为window的方法
  * this ==> 赋值(window)
  
* 函数执行上下文

  * 在调用函数,准备执行函数体之前,创建对应的函数对象
  * 对局部数据进行预处理
  * 形参变量 ==》 赋值(实参)  ==>添加为执行上下文的属性
  * arguments ==> 赋值(实参列表),添加为执行上下文的属性
  * var 定义的全局变量等于undefined,添加为执行上下文的属性
  * function 声明的全局函数===》赋值(fun)，添加为执行上下文的属性
  * this ==> 赋值(调用函数的对象)

  ```js
      function fn(a1) {
          console.log(a1);
          console.log(a2);
          console.log(arguments);
          var a2 = 3
  
          a3()
          
          function a3() {
              console.log('a3');
          }
      }
  
      fn(2,3,5)
  ```

  * window 全局上下文总是在栈的底部
  * 执行一个函数就开辟一个新的栈空间(上下文环境)，当执行完时才释放所占空间
  * 栈的顶部总是当前执行的上下文环境
## 11.作用域和上下文环境的区别

## 12.闭包

* 产生闭包的条件(子函数调用了父函数的局部变量)

  * 函数嵌套 

  * 内部函数引用了外部函数的数据(变量/函数)
  
* 闭包: 包含被引用变量/函数的对象,闭包只存在于嵌套的内部函数中(chrome调试可以查看)

* 使函数内部的变量在函数执行完后,仍然存活在内存中(延长了局部变量的生命周期)

* 让函数外部可以操作(读写)到函数内部的数据(变量/函数)

* 闭包的生命周期

  * 产生: 在内部嵌套函数定义(直接声明方式),外部函数执行时产生

  * 死亡: 在嵌套的内部函数成为垃圾对象时
  
    ```js
      function fn1() {
      // 执行到此行闭包产生(函数提升,内部函数对象已经创建)
          var a=2
         function fn2() {
             a++
             console.log(a);
         }
          
         /* var fn2 = function(){
               a++
             console.log(a);
          }
         // 这种情况下,在这一行产生闭包(函数声明完)
          */
         return fn2
      }
      var f = fn1()
        f()
        f()
       f=null //闭包死亡(包含闭包的函数体成为垃圾对象)
    ```
  
    ```js
    //闭包的另一种方式
    function show(msg,time){
        setTimeout(function(){
            alert(msg)
        },time)
    }
    
    show('弹弹更健康',2000)
    
    ```
## 13.内存溢出和内存泄漏

## 14.继承的原理

```js
  function Animal(name,age) {
        this.name = name
        this.age =  age
    }

    Animal.prototype.showName = function(){
        console.log(this.name);
    }

    // 实现继承属性,本质上还不是继承
    function Person(name,age){
        Animal.call(this,name,age)
    }

    let p1 = new Person('小明',18)

    console.log(p1.name);
    console.log(p1.age);

    // 实现继承 Person的实例指向Animal的实例
    Person.prototype = new Animal()
    // 重新指定constructor
    Person.prototype.constructor = Person


    let p2 = new Person('小花',28)

    p2.showName()

```



## 15.webWorker的使用	

防止主线程阻塞,把大量的计算交给分线程(目前使用的少)

```js
  var worker = new Worker('./worker.js')

    document.getElementById('btn').onclick = function () {
        var val = document.getElementById('target').value
        worker.onmessage = function(event){
            console.log(event.data,'最后接收到的数据');
        }

        worker.postMessage(val)
        console.log('最开始的数据')
    }

```

```js
function feibolaqie(n) {
    return n<=3 ? 1 : feibolaqie(n-1)+feibolaqie(n-2)
}

var onmessage = function (event) {
    console.log(event.data,'计算前的数据');
    var number = event.data

    let result = feibolaqie(number)

    postMessage(result)

    console.log('计算后的数据')

}
```

