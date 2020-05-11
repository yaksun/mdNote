## 01.`undefined`和`null`的区别

undefined 变量定义了但未赋值

null 变量定义并赋值为null

## 02.什么时候给变量赋值为null

* 初始赋值,表明变量将要赋值为对象
* 结束前,让对象成为垃圾对象

## 03.`var a=xxx`,a内存中保存的是什么

* `xxx`是基本类型:保存的就是这个数据
* `xxx`是引用类型:保存的是对象数据的地址
* `xxx`是变量:保存的是`xxx`这个变量的内存内容(数据-》基本类型或地址-》对象数据)
* 赋值是将右边变量的内存数据拷贝给左边的变量

## 04.函数栈的理解

* 依次输出什么?有几个上下文环境?

```js
 console.log('gb:'+a)
       var a = 1
        function f1(a) {
            if(a==4){
                return
            }
            console.log('fb:'+a)
             f1(a+1)
            console.log('fe:'+a)
        }
     f1(1)

    console.log('ge:'+a)
```

## 05.变量提升的问题

```js
	// 先执行变量提升，再执行函数提升
    // function a() {
    // }
    // var a
    //
    // console.log(typeof a); // function


    // Uncaught TypeError: c is not a function
    // var c=1
    // function c(c) {
    //     console.log(c);
    //     // var c=3
    // }
    // c(2)
    
    
    if(!(b in window)){
       var b
    }
    console.log(b);  // undefined
```

## 06.作用域

```js
var x=10

function fn(){
	console.log(x)
}

function show(f){
	var x = 20
	f()
}

show(fn) // 10

```

```js
var fn1 =  function () {
        console.log(fn1)
    }

    fn1()

    var obj={
       fn2:function () {
           console.log(fn2);
       }
    }

    obj.fn2() //fn2 is not defined
```

